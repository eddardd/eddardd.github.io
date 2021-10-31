---
title: 'Gradients in Generalized Coordinates'
date: 2021-11-01
permalink: /posts/calculus/graduate/lec1
toc: True
tags:
  - gradients
  - graduate level
---

Note that, previously, we assumed that the gradient is written with respect to the canonical basis in $\mathbb{R}^{n}$. This implies that,

$$\nabla f = \sum_{i=1}^{n}\dfrac{\partial f}{\partial x_{i}}\mathbf{e}_{i}$$

this is only true when the basis vectors $\mathbf{e}_{i}$ do not change with $\mathbf{x}$. The next example shows how the gradient vector can be calculated for an alternative basis.

---
**Example: Graidents in Polar Coordinates**

For a vector $\mathbf{u} = (x, y)$, the polar representation can be written in terms of the length $r = \lVert\mathbf{u}\lVert$ and the angle $\theta$. The relationship between $(x,y)$ and $(r,\theta)$ is given below,

$$\begin{cases}x=r\cos\theta\\ y=r\sin\theta\end{cases}\implies\begin{cases}r=\sqrt{x^{2}+y^{2}}\\ \tan\theta=y/x\end{cases}$$

Note that with these alternative representation we can map vectors in cartesian coordinates to vectors in polar coordinates. For instance, let us denote $\mathbf{u}_{c} = (1, 1)$ to the vector in cartesian coordenates. One has,

$$r = \sqrt{1^{2} + 1^{2}} = \sqrt{2}$$

and,

$$\tan\theta = 1/1 = 1 \implies \theta = \pi/4$$

that is, $\mathbf{u}_p$, the same vector but in polar coordinates, is $\mathbf{u}_p = (\sqrt{2}, \pi/4)$. Now, it is important to notice how $x$ and $y$ affect the basis vectors. Let $e_{r}$ be the radial basis vector, and $e_{\theta}$ be the angular basis vector. For different points in space we have different vectors, as

$$e_{r}(x, y) = (\sqrt{x^{2} + y^{2}}, 0),$$

and,

$$e_{\theta}(x, y) = (0, \text{atan}\,y/x).$$

This is shown in the figure below, where the same point is shown in two different coordinate systems, with the basis vectors highlighted.

<p align="center">
  <img src="https://eddardd.github.io/images/PostFigures/Calculus/CoordinateSystems.png"/>
</p>

Now we have the ingredients for computing the gradient of $f$ under a change of basis $\phi(x, y) = (r(x, y), \theta(x, y))$,

$$\nabla f = \dfrac{\partial f}{\partial x}\mathbf{e}_{x} + \dfrac{\partial f}{\partial y}\mathbf{e}_{\mathbf{y}}$$

so,

$$\begin{cases}\dfrac{\partial f}{\partial x} = \dfrac{\partial f}{\partial r}\dfrac{\partial r}{\partial x} + \dfrac{\partial f}{\partial \theta}\dfrac{\partial \theta}{\partial x}\\ \dfrac{\partial f}{\partial y} = \dfrac{\partial f}{\partial r}\dfrac{\partial r}{\partial y} + \dfrac{\partial f}{\partial \theta}\dfrac{\partial \theta}{\partial y}\end{cases}$$

Note that applying this chain rule alone will only change the coordinates of $\nabla f$ in the cartesian coordinate system, that is,

$$\nabla f = \biggr(\dfrac{\partial f}{\partial r}\dfrac{\partial r}{\partial x}+\dfrac{\partial f}{\partial \theta}\dfrac{\partial \theta}{\partial x}\biggr)\mathbf{e}_{x} + \biggr(\dfrac{\partial f}{\partial r}\dfrac{\partial r}{\partial x}+\dfrac{\partial f}{\partial \theta}\dfrac{\partial \theta}{\partial x}\biggr)\mathbf{e}_{\mathbf{y}}$$

on the other hand we want $\nabla f = g_{r}\mathbf{e}_{r} + g_{\theta}\mathbf{e}_{\theta}$. As follows, we need to __transform the basis vectors__ using the Jacobian matrix,

$$\begin{cases}\mathbf{e}_{x} = \dfrac{\partial r}{\partial x}\mathbf{e}_{r} + \dfrac{\partial \theta}{\partial x}\mathbf{e}_{\theta}\\ \mathbf{e}_{y} = \dfrac{\partial r}{\partial y}\mathbf{e}_{r} + \dfrac{\partial \theta}{\partial y}\mathbf{e}_{\theta}\end{cases}$$

this implies that, the radial direction component,

$$g_{r} = \dfrac{\partial f}{\partial r}\biggr(\dfrac{\partial r}{\partial x}\biggr)^{2}  + \dfrac{\partial f}{\partial r}\dfrac{\partial \theta}{\partial x}\dfrac{\partial r}{\partial x} + \dfrac{\partial f}{\partial r}\biggr(\dfrac{\partial r}{\partial y}\biggr)^{2}  + \dfrac{\partial f}{\partial r}\dfrac{\partial \theta}{\partial y}\dfrac{\partial r}{\partial y}$$

likewise, in the angular component,

$$g_{\theta} = \dfrac{\partial f}{\partial \theta}\biggr(\dfrac{\partial \theta}{\partial x}\biggr)^{2}  + \dfrac{\partial f}{\partial \theta}\dfrac{\partial \theta}{\partial x}\dfrac{\partial r}{\partial x} + \dfrac{\partial f}{\partial \theta}\biggr(\dfrac{\partial \theta}{\partial y}\biggr)^{2}  + \dfrac{\partial f}{\partial \theta}\dfrac{\partial \theta}{\partial y}\dfrac{\partial r}{\partial y}$$

---