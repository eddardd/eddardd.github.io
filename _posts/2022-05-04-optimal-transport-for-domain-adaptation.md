---
layout: post
title: Optimal Transport for Domain Adaptation
date: 2022-05-04
description: How to adapt a CNN between different domains using Optimal Transport.
tags: optimal-transport domain-adaptation transfer-learning tutorial
categories: tutorials
giscus_comments: false
related_posts: false
toc:
  beginning: true
---

The idea of this post is to give a deeper feel for **how** to perform Optimal Transport (OT) for Domain Adaptation. We follow [Courty et al. [1]](https://arxiv.org/abs/1507.00504), who first proposed using OT to adapt models in an unsupervised way. We won't dive into how to *solve* OT problems — for that, the [Python Optimal Transport (POT)](https://pythonot.github.io/) library does the heavy lifting and the textbook by [Peyré and Cuturi [2]](https://arxiv.org/abs/1803.00567) is the canonical reference. The full notebook with executable code is on [GitHub](https://github.com/eddardd/my-personal-blog/blob/master/_notebooks/2022-05-04-Optimal-Transport-for-Transfer-Learning.ipynb).

## Setup

```python
import ot
import numpy as np
import matplotlib.pyplot as plt
from tqdm import tqdm

import torch
from torchinfo import summary
from torchvision import datasets, transforms
from torchvision.utils import make_grid

from sklearn.metrics import accuracy_score
from sklearn.neighbors import KNeighborsClassifier

device = 'cpu'
```

## Loading the datasets

For the transfer learning task, we use **MNIST [3]** and **USPS [4]** — both are handwritten-digit datasets, very similar to each other, with white digits on a black background. Our goal is to adapt a model trained on MNIST so that it correctly classifies USPS digits.

This particular adaptation problem has been studied extensively (e.g. [5, 6]). The point here isn't to chase state-of-the-art numbers — it's to walk through the OT mechanics. We preprocess each image as follows:

1. Convert each pixel from `uint8 [0, 255]` to `float32` in `[0, 1]` via `transforms.ToTensor()`.
2. Resize to $$32 \times 32$$. Note: this introduces resampling artifacts, especially for USPS (originally $$16 \times 16$$).
3. Replicate across the 3 RGB channels.

```python
T = transforms.Compose([
    transforms.ToTensor(),
    transforms.Resize((32, 32))
])

src_dataset = datasets.MNIST(root='./.tmp', train=True, transform=T, download=True)
src_loader = torch.utils.data.DataLoader(src_dataset, batch_size=256, shuffle=True)

tgt_dataset = datasets.USPS(root='./.tmp', train=True, transform=T, download=True)
tgt_loader = torch.utils.data.DataLoader(tgt_dataset, batch_size=256, shuffle=True)
```

A quick visual comparison:

<div class="row justify-content-center">
  <div class="col-md-10">
    {% include figure.liquid loading="eager" path="assets/img/posts/ot-domain-adaptation/samples.png" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

Even though the two datasets *look* similar, they're noticeably different. MNIST digits are centered on the $$32 \times 32$$ grid; USPS digits tend to fill it. The CNN cares about that, even though a human wouldn't.

This is the classic **covariate shift** phenomenon: $$P_{S}(X) \neq P_{T}(X)$$. The marginal feature distribution changes across domains, and the statistical properties of the data shift in ways the source-trained classifier can't anticipate.

The plan from here:

* Train a CNN feature extractor on MNIST.
* Measure its performance on USPS (the **baseline**).
* Use Optimal Transport to enhance USPS performance.

## A pretrained feature extractor

We use the classic [LeNet5 [6]](https://en.wikipedia.org/wiki/LeNet) architecture, originally designed for MNIST, implemented with PyTorch's [`Module` API](https://pytorch.org/docs/stable/generated/torch.nn.Module.html).

```python
class LeNet5(torch.nn.Module):
    def __init__(self, n_channels=3):
        super().__init__()
        self.n_channels = n_channels

        self.feature_extractor = torch.nn.Sequential(
            torch.nn.Conv2d(in_channels=n_channels, out_channels=6, kernel_size=5),
            torch.nn.ReLU(),
            torch.nn.MaxPool2d(stride=2, kernel_size=2),
            torch.nn.Conv2d(in_channels=6, out_channels=16, kernel_size=5),
            torch.nn.ReLU(),
            torch.nn.MaxPool2d(stride=2, kernel_size=2),
        )

        self.class_discriminator = torch.nn.Sequential(
            torch.nn.Linear(16 * 5 * 5, 120),
            torch.nn.ReLU(),
            torch.nn.Linear(120, 84),
            torch.nn.ReLU(),
            torch.nn.Linear(84, 10),
            torch.nn.Softmax(dim=1),
        )

    def forward(self, x):
        y = self.feature_extractor(x)
        h = y.view(-1, 16 * 5 * 5)
        features = self.class_discriminator[:-2](h)
        predicted_labels = self.class_discriminator(h)
        return features, predicted_labels

model = LeNet5(n_channels=1)
```

LeNet5 has **61,706 trainable parameters** — small enough to train on CPU in a few minutes. We minimize cross-entropy with one-hot labels:

$$
\mathcal{L}(y, \hat{y}) = -\dfrac{1}{n}\sum_{i=1}^{n}\sum_{j=1}^{K}y_{ij}\log \hat{y}_{ij}
$$

with batch size $$n = 256$$ and $$K = 10$$ classes. Training for 10 epochs with Adam (`lr=1e-3`) brings source-domain accuracy to **~98.4%** by the last epoch.

```python
criterion = torch.nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)

for it in range(10):
    for x, y in tqdm(src_loader):
        optimizer.zero_grad()
        x, y = x.to(device), y.to(device)
        y = torch.nn.functional.one_hot(y, num_classes=10).float()
        _, yhat = model(x)
        loss = criterion(yhat, y)
        loss.backward()
        optimizer.step()
```

### Measuring the baseline

The **naive baseline** in domain adaptation is: don't adapt — just apply the source-trained classifier to the target. Following [1], we evaluate using a 1-NN classifier on the CNN's features. Build $$H_{S} \in \mathbb{R}^{n_{S} \times 84}$$ from the source features and analogously $$H_T$$ for the target:

$$h_{S}^{i} = \phi(x_{S}^{i})$$

where $$\phi$$ is the convolutional feature extractor.

```python
def extract_features(loader):
    H, Y = [], []
    for x, y in tqdm(loader):
        with torch.no_grad():
            h, _ = model(x)
            H.append(h)
        Y.append(y)
    return torch.cat(H, dim=0), torch.cat(Y)

Hs, Ys = extract_features(src_loader)
Ht, Yt = extract_features(tgt_loader)

# 1-NN: pick each target sample's nearest source feature, then transfer its label
C = torch.cdist(Hs, Ht, 2) ** 2
ind_opt = C.argmin(dim=0)
Yp = Ys[ind_opt]

print(accuracy_score(Yt, Yp))
# 0.7151
```

So **the baseline accuracy on USPS is 71.51%**. A successful adaptation needs to do better than that.

## Optimal Transport

### Background

Optimal Transport [7] is a mathematical theory about *moving mass at minimum effort*. Its modern formulation goes back to [Gaspard Monge](https://en.wikipedia.org/wiki/Gaspard_Monge); [Leonid Kantorovich](https://en.wikipedia.org/wiki/Leonid_Kantorovich) recast it in the 20th century as a linear program (and won a Nobel Prize for related work).

What makes OT useful in ML — and especially transfer learning — is that you can think of probability distributions as distributions of mass. OT then becomes a framework for **manipulating probability distributions**: warping one into another, comparing them, or aligning them.

Let $$P_{S}(X)$$ and $$P_{T}(X)$$ be the (unknown) source and target feature distributions. From samples we approximate them empirically:

$$\hat{P}_{S}(x) = \dfrac{1}{n_{S}}\sum_{i=1}^{n_{S}}\delta(\mathbf{x} - \mathbf{x}_{S}^{i})$$

where $$\delta$$ is the [Dirac delta](https://en.wikipedia.org/wiki/Dirac_delta_function). For data matrices $$\mathbf{X}_{S} \in \mathbb{R}^{n_{S} \times d}$$ and $$\mathbf{X}_{T} \in \mathbb{R}^{n_{T} \times d}$$, OT seeks a **transportation plan** $$\pi \in \mathbb{R}^{n_{S} \times n_{T}}$$ specifying how much mass moves from $$\mathbf{x}_{S}^{i}$$ to $$\mathbf{x}_{T}^{j}$$. The plan must conserve mass:

$$
\sum_{i=1}^{n_{S}}\pi_{ij} = \dfrac{1}{n_{T}} \quad \text{and} \quad \sum_{j=1}^{n_{T}}\pi_{ij} = \dfrac{1}{n_{S}}.
$$

Given a transport cost $$c(\cdot, \cdot)$$, we minimize total effort:

$$E(\pi) = \sum_{i=1}^{n_{S}}\sum_{j=1}^{n_{T}}\pi_{ij}\,c(\mathbf{x}_{S}^{i}, \mathbf{x}_{T}^{j}).$$

This is a **linear program** — its cost and constraints are linear in $$\pi_{ij}$$. But it's a *huge* one: the number of variables grows with the product of sample counts. Naively solving this on deep-learning-scale datasets is infeasible.

### Entropic regularization

Following [Cuturi [8]](https://arxiv.org/abs/1306.0895), we add an entropic regularization term to make the problem tractable:

$$E(\pi) = \sum_{i=1}^{n_{S}}\sum_{j=1}^{n_{T}}\pi_{ij}\,c(\mathbf{x}_{S}^{i}, \mathbf{x}_{T}^{j}) + \epsilon\sum_{i=1}^{n_{S}}\sum_{j=1}^{n_{T}}\pi_{ij}\log\pi_{ij}$$

Two effects: (i) the LP becomes smooth, yielding a smooth $$\pi$$; (ii) it can be solved with fast matrix-scaling algorithms (Sinkhorn iterations). In practice, this often *also* improves adaptation performance.

### From plan to mapping

$$\pi$$ tells us how much mass moves between samples, but not where to *map* a specific source point. That's what the **barycentric mapping** does:

$$T_{\pi}(\mathbf{x}_{S}^{i}) = \arg\min_{\mathbf{x} \in \mathbb{R}^{d}} \sum_{j=1}^{n_{T}}\pi_{ij}\,c(\mathbf{x}, \mathbf{x}_{T}^{j}).$$

For squared-Euclidean cost, $$c(\mathbf{x}_{S}^{i}, \mathbf{x}_{T}^{j}) = \lVert \mathbf{x}_{S}^{i} - \mathbf{x}_{T}^{j} \rVert_{2}^{2}$$, this has a **closed form**:

$$T_{\pi}(\mathbf{X}_{S}) = n_{S}\,\pi\,\mathbf{X}_{T}.$$

But $$T_{\pi}$$ is only defined on the source samples used to fit $$\pi$$. For our datasets, that would be a $$60{,}000 \times 7{,}291$$ plan — possible, but slow and storage-heavy. The fix from [Ferradans et al. [9]](https://arxiv.org/abs/1307.5551) is to fit $$T_{\pi}$$ on a representative *subsample*, then extend to new points by:

$$T_{\pi}(\mathbf{x}) = T_{\pi}(\mathbf{x}_{S}^{i_{\star}}) + \mathbf{x} - \mathbf{x}_{S}^{i_{\star}}$$

where $$i_{\star}$$ is the index of the nearest neighbor of $$\mathbf{x}$$ in $$\mathbf{X}_{S}$$.

### Fitting the barycentric mapping

We extract 10 batches (2,560 samples) from each loader and fit a Sinkhorn transport with POT.

```python
def take_batches(loader, k=10):
    H, Y = [], []
    for i, (x, y) in enumerate(loader):
        if i == k: break
        with torch.no_grad():
            h, _ = model(x)
            H.append(h)
            Y.append(y)
    return torch.cat(H, dim=0).numpy(), torch.cat(Y, dim=0).numpy()

_Hs, _Ys = take_batches(src_loader)
_Ht, _Yt = take_batches(tgt_loader)

otda = ot.da.SinkhornTransport(reg_e=1e-2, norm='max')
otda.fit(Xs=_Hs, ys=None, Xt=_Ht, yt=None)
```

Visualizing the resulting plan:

```python
plt.imshow(np.log(otda.coupling_ + 1e-12), cmap='Reds')
```

<div class="row justify-content-center">
  <div class="col-md-6">
    {% include figure.liquid loading="lazy" path="assets/img/posts/ot-domain-adaptation/coupling-unsorted.png" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

Hard to see structure here because samples aren't sorted by class. Intuitively, samples within the same class should be close (1s in MNIST closer to 1s in USPS than to 8s), so we'd expect $$\pi$$ to be **class-sparse** — a notion introduced in [1]:

$$\pi_{ij} \neq 0 \iff y_{S}^{i} = y_{T}^{j}.$$

We didn't *enforce* this (we fit $$\pi$$ without using labels), but if we sort rows and columns by label, we can check whether it emerges naturally:

```python
plt.imshow(np.log(otda.coupling_[_Ys.argsort(), :][:, _Yt.argsort()] + 1e-12), cmap='Reds')
```

<div class="row justify-content-center">
  <div class="col-md-6">
    {% include figure.liquid loading="lazy" path="assets/img/posts/ot-domain-adaptation/coupling-sorted.png" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

The plan is approximately class-sparse — the features alone are informative enough to induce this property.

### Why class sparsity matters

Look at where each source sample $$\mathbf{x}_{S}^{i}$$ gets mapped:

$$\hat{\mathbf{x}}_{S}^{i} = \sum_{j=1}^{n_{T}}(n_{S}\pi_{ij})\,\mathbf{x}_{T}^{j}.$$

Letting $$\alpha_{j} = n_{S}\pi_{ij}$$, we have $$\sum_{j}\alpha_{j} = 1$$ and $$\alpha_{j} \geq 0$$. So $$\hat{\mathbf{x}}_{S}^{i}$$ lies inside the [convex hull](https://en.wikipedia.org/wiki/Convex_hull) of the target samples that receive mass from $$\mathbf{x}_{S}^{i}$$.

Worst case: if all those targets belong to a class $$k_{j}$$ different from $$\mathbf{x}_{S}^{i}$$'s true class, the source point gets mapped into the *wrong* region of decision space — and adaptation hurts more than it helps. That's why class sparsity matters: when it holds, the barycentric image of a source 1 stays near other 1s, not near 8s.

### Transporting and evaluating

Final step: extract features from source samples, transport them to the target, then run 1-NN as before.

$$\hat{h}_{S}^{i} = T_{\pi}(\phi(\mathbf{x}_{S}^{i}))$$

```python
THs, Ys = [], []
for xs, ys in tqdm(src_loader):
    with torch.no_grad():
        hs, _ = model(xs)
        hs = torch.from_numpy(otda.transform(hs.numpy()))
        THs.append(hs)
    Ys.append(ys)
THs = torch.cat(THs, dim=0)
Ys = torch.cat(Ys, dim=0)

# Mini-batched 1-NN to avoid OOMing the distance matrix
Yp = torch.zeros_like(Yt)
batch = 64
for i in tqdm(range((len(Ht) + batch - 1) // batch)):
    ht = Ht[i * batch:(i + 1) * batch]
    C = torch.cdist(THs, ht, 2) ** 2
    Yp[i * batch:(i + 1) * batch] = Ys[C.argmin(dim=0)]

print(accuracy_score(Yt, Yp))
# 0.7860
```

So **adapted accuracy is 78.60% — up from a 71.51% baseline**. About 7 percentage points, or a ~10% relative improvement. Modest, but it confirms the mechanism works.

## Where to go next

OT has reshaped a lot of the transfer-learning landscape over the last decade. If this whetted your appetite:

* **Inducing structure (e.g. classes) in OT maps:** [1, 6]
* **Issues with extending barycentric mappings:** [6, 10]
* **OTDA on joint distributions:** [11, 12]
* **Multi-source domain adaptation:** [13, 14]

## References

[1] N. Courty, R. Flamary, D. Tuia, and A. Rakotomamonjy. *Optimal transport for domain adaptation*. IEEE TPAMI 39(9):1853–1865, 2016.

[2] G. Peyré and M. Cuturi. *Computational optimal transport: With applications to data science*. Foundations and Trends in Machine Learning 11(5-6):355–607, 2019.

[3] Y. LeCun, B. Boser, J. S. Denker, et al. *Backpropagation applied to handwritten zip code recognition*. Neural Computation 1(4):541–551, 1989.

[4] J. J. Hull. *A database for handwritten text recognition research*. IEEE TPAMI 16(5):550–554, 1994.

[5] Y. Ganin, E. Ustinova, H. Ajakan, et al. *Domain-adversarial training of neural networks*. JMLR 17(1):2096–2030, 2016.

[6] V. Seguy, B. B. Damodaran, R. Flamary, N. Courty, A. Rolet, and M. Blondel. *Large-scale optimal transport and mapping estimation*. arXiv:1711.02283, 2017.

[7] C. Villani. *Optimal transport: old and new*. Springer, 2009.

[8] M. Cuturi. *Sinkhorn distances: lightspeed computation of optimal transport*. NeurIPS, 2013.

[9] S. Ferradans, N. Papadakis, G. Peyré, and J.-F. Aujol. *Regularized discrete optimal transport*. SIAM Journal on Imaging Sciences 7(3):1853–1882, 2014.

[10] M. Perrot, N. Courty, R. Flamary, and A. Habrard. *Mapping estimation for discrete optimal transport*. NeurIPS, 2016.

[11] N. Courty, R. Flamary, A. Habrard, and A. Rakotomamonjy. *Joint distribution optimal transportation for domain adaptation*. NeurIPS, 2017.

[12] B. B. Damodaran, B. Kellenberger, R. Flamary, D. Tuia, and N. Courty. *DeepJDOT: Deep joint distribution optimal transport for unsupervised domain adaptation*. ECCV, 2018.

[13] T. Nguyen, T. Le, H. Zhao, Q. H. Tran, T. Nguyen, and D. Phung. *MOST: Multi-source domain adaptation via optimal transport for student-teacher learning*. UAI, 2021.

[14] E. F. Montesuma and F. M. N. Mboula. *Wasserstein Barycenter for Multi-Source Domain Adaptation*. CVPR, 2021.
