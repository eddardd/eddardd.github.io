---
title: 'Derivatives -- Concepts'
date: 2021-11-01
permalink: /posts/calculus/lec1
toc: True
tags:
  - calculus
  - derivatives
  - undergraduate level
---

In this post I will discuss the notion of derivative and how to compute them on different spaces, ranging from the real line, up to general metric spaces

## Introduction

In introductory calculus courses, derivatives play a central role alongside integrals. Even though the definition and intuition of a derivative in the real line is very straightforward, one can often found themselves lost in more general (and complex) spaces.

To that end, we begin by introducing different notions of spaces that are, in a certain sense, generalizations of the real line. Once these spaces are properly defined, we can understand derivatives on functions whose domain are these spaces. Whenever possible, we adopt a bottom-up approach in terms of abstraction, namely, we start with the most basic notions up to the most general.

### Vector Spaces

The most intuitive vector space is $\mathbb{R}^{2}$, whose elements are ordered tuples $\mathbf{v} = (x, y), x \in \mathbb{R}, y\in \mathbb{R}$. The elements of $\mathbb{R}^{2}$ can be visualized in the __cartesian plane__, as follows,

<p align="center">
  <img src="https://eddardd.github.io/images/PostFigures/Calculus/CartesianGrid.png"/>
</p>

Now, let us define how to add vectors.

---
**Vector Addition**

For vectors $\mathbf{u} = (x_{1}, y_{1})$ and $\mathbf{v} = (x_{2}, y_{2})$ in $\mathbb{R}^{2}$, their sum is a third vector

$$\mathbf{w} = (x_{1} + x_{2}, y_{1} + y_{2}).$$

Geometrically, this corresponds to putting $\mathbf{v}$ on the end of $\mathbf{u}$, and drawing the arrow that joins the extremes of these vectors, as shown in the Figure below,

<p align="center">
  <img src="https://eddardd.github.io/images/PostFigures/Calculus/Vector_Sum.png"/>
</p>

---

and how to multiply vectors by scalars.

---
**Multiplication by Scalar**

For a vector $\mathbf{u} = (x, y)$ and a real number $\lambda$, the multiplication $\lambda \mathbf{u}$ is a second vector $\mathbf{v}$, whose coordinates are,

$$\mathbf{v} = (\lambda x, \lambda y).$$

Geometrically, this corresponds to enlarging $\mathbf{u}$ when $\lambda > 1$, and shrinking $\mathbf{u}$ otherwise. When $\lambda < 0$, the __direction__ of $\mathbf{u}$ is reversed.

<p align="center">
  <img src="https://eddardd.github.io/images/PostFigures/Calculus/VectorMultiplication.png"/>
</p>


---

With these ideas in mind, a vector space is a set $E$, for which its elements $\mathbf{v} \in E$ are __vectors__. These spaces need to further suffice a series of __axioms__, or properties, in order to be called vector spaces. Here we give a list of these properties,

---
**Definition: Vector Spaces**

A set $E$ with operations $+:E\times E\rightarrow E$ and $\cdot:E\times\mathbb{R}\rightarrow E$ is called a vector space if and only if the following properties hold,

__Properties of the Addition Operation__
1. __Associativity:__ $\forall \mathbf{u},\mathbf{v},\mathbf{w} \in E$, $(\mathbf{u} + \mathbf{v}) + \mathbf{w} = \mathbf{u} + (\mathbf{v} + \mathbf{w})$
2. __Commutativity:__ $\forall \mathbf{u},\mathbf{v} \in E$, $\mathbf{u} + \mathbf{v} = \mathbf{v} + \mathbf{u}$
3. __Existence of Identity Element:__ $\exists \mathbf{0} \in E$ such that $\mathbf{v} + \mathbf{0} = \mathbf{v}$, $\forall \mathbf{v}$
4. __Existence of Inverse Element:__ $\forall \mathbf{v} \exists -\mathbf{v}$ such that $\mathbf{v} + (-\mathbf{v}) = \mathbf{0}$

__Properties of the Multiplication by Scalar Operation__
1. __Compatibility:__ $\forall \alpha,\beta \in \mathbb{R}, \mathbf{u} \in E$, $\alpha(\beta \mathbf{v}) = \alpha\beta\mathbf{v}$
2. __Existence of Identity Element:__ $\exists 1 \in \mathbb{R}$ such that $1\mathbf{v} = \mathbf{v}$
3. __Distributivity of Multiplication w.r.t. Vector Addition:__ $\forall \alpha \in \mathbb{R},\mathbf{u},\mathbf{v} \in E$, $\alpha(\mathbf{u} + \mathbf{v}) = \alpha \mathbf{u} + \alpha \mathbf{v}$.
4. __Distributivity of Multiplication w.r.t. Scalar Addition:__ $\forall \alpha,\beta \in \mathbb{R}, \mathbf{u} \in E$, $(\alpha+\beta)\mathbf{u} = \alpha \mathbf{u} + \beta \mathbf{u}$.

---

As you can see, the definition of a vector space has much more to do with the vector addition and multiplication by scalar opertions than with the elements of $E$ themselves. It is often very tedious to prove that a set $E$ is a vector space, but you can note that for $\mathbb{R}^{2}$, the properties hold from the properties of real numbers. More complicated spaces have more involved proofs.

### Euclidean Spaces

As $\mathbb{R}^{2}$ represents a way to identify points on a plane, $\mathbb{R}^{3}$ allows us to identify points in space. This is done by using three cordinates $\mathbf{p} = (x, y, z)$. The most general case correspond to $\mathbb{R}^{n}$, where $n$ is any positive integer. These spaces are generally called Euclidean Spaces. Points in an Euclidean space are defined as follows,

$$\mathbf{x} = (x_{1},\cdots,x_{n})$$

that is, they have $n$ coordinates. For that reason, they are called $n-$dimensional spaces. As we did with the Cartesian plane, sum of vectors in Euclidean spaces can be defined as follows,

$$\mathbf{x} + \mathbf{y} = (x_{1} + y_{1},\cdots,x_{n}+y_{n})$$

that is, one has coordinate-wise sum. This is also valid for scalar multiplication,

$$\lambda \mathbf{x} = (\lambda x_{1}, \cdots, \lambda x_{2})$$

As follows, let us define two new operations on vectors: the inner product, the norm and the metric.

#### Inner Product

---
**Definition: Inner Product**

An inner product is a function $\langle \cdot, \cdot \rangle:E\times E\rightarrow\mathbb{R}$ on a vector space $E$ that suffices the following properties,

1. __Linearity in the first argument:__ $\forall\mathbf{u}, \mathbf{v}, \mathbf{w} \in E$, $\lambda \in \mathbb{R}$,
   $$\langle \mathbf{u} + \lambda\mathbf{v},\mathbf{w}\rangle = \langle \mathbf{u},\mathbf{w}\rangle + \lambda \langle \mathbf{v},\mathbf{w}\rangle$$
2. __Symmetry:__ $\forall \mathbf{u},\mathbf{v}$, $\langle\mathbf{u},\mathbf{v}\rangle = \langle\mathbf{v},\mathbf{u}\rangle$
3. __Positive Definiteness:__ $\forall \mathbf{u}$, $\langle\mathbf{u},\mathbf{u}\rangle \geq 0$, the equality holds if and only if $\mathbf{u} = \mathbf{0}$.

A vector space equiped with an inner product is denoted as $(E,\langle\cdot,\cdot\rangle)$, and is called an __inner product space__.

---

For grounding this definition, let us give an example

---
**Example: Inner Product in Euclidean Spaces**

Let $E = \mathbb{R}^{n}$. The natural notion of inner product is given by the __dot product__,

$$\langle \mathbf{x},\mathbf{y} \rangle = \mathbf{x}\cdot\mathbf{y} = \mathbf{x}^{T}\mathbf{y} = \sum_{i=1}^{n}x_{i}y_{i}$$

Next, we verify the three properties of the inner product,

__Property 1__

$$\langle \mathbf{x} + \mathbf{z},\mathbf{y} \rangle = \sum_{i=1}^{n}(x_{i}+z_{i})y_{i} = \sum_{i=1}^{n}x_{i}y_{i}+\sum_{i=1}^{n}z_{i}y_{i}$$

__Property 2__

$$\langle \mathbf{x},\mathbf{y} \rangle = \sum_{i=1}^{n}x_{i}y_{i} = \sum_{i=1}^{n}y_{i}x_{i} = \langle \mathbf{y},\mathbf{x} \rangle$$

__Property 3__

$$\langle \mathbf{x},\mathbf{x} \rangle = \sum_{i=1}^{n}x_{i}^{2} \geq 0$$

since this previous sum is a sum of non-negative terms. Note that it is only zero if $x_{i} = 0$ $\forall i$.

---

#### Norm

Note that this is __a__ inner product, other may be defined in $\mathbb{R}^{n}$ as well. This is also called __canonical__ inner product.

---
**Definition: Norm**

A norm is a function on a vector space $E$ that suffices the following properties,

1. __Nonegativity:__ $\forall \mathbf{u} \in \mathbb{R}^{n}$
2. __Scalar Multiplication:__

$$\lambda \in \mathbb{R}, \mathbf{u} \in E, \lVert\lambda\mathbf{u}\lVert = |\lambda\lVert|\mathbf{u}\lVert$$

3. __Triangle Inequality:__ $\forall \mathbf{u},\mathbf{v} \in E$,

$$\lVert\mathbf{u}+\mathbf{v}\lVert \leq \lVert\mathbf{u}\lVert + \lVert\mathbf{v}\lVert$$

A vector space equiped with a norm is called a __normed space__.

---

As with the canonical inner product, one also has a canonical idea of norm in Euclidean spaces,

---
**Example: Norms in Euclidean Spaces**

Let $E = \mathbb{R}^{n}$. The natural notion of norm is given by,

$$\lVert\mathbf{x}\lVert = \sqrt{\mathbf{x}^{T}\mathbf{x}} = \sqrt{\langle\mathbf{x},\mathbf{x}\rangle} = \sum_{i=1}^{n}x_{i}^{2}$$

Next, we verify the three properties of the norm,

__Property 1__

This property is the same as Property 3 of the canonical inner product on $\mathbb{R}^{n}$.

__Property 2__

$$\lVert\lambda\mathbf{x}\lVert = \sqrt{\sum_{i=1}^{n}(\lambda x_{i})^{2}} = \sqrt{\sum_{i=1}^{n}\lambda^{2}x_{i}^{2}} = |\lambda|\sqrt{\sum_{i=1}^{n}x_{i}^{2}}$$

__Property 3__

$$\lVert\mathbf{x}+\mathbf{y}\lVert^{2} = \sum_{i=1}^{n}(x_{i} + y_{i})^{2} = \sum_{i=1}^{n}x_{i}^{2} + \sum_{i=1}^{n}y_{i}^{2} + 2\sum_{i=1}^{n}x_{i}y_{i} = \lVert\mathbf{x}\lVert^{2}+\lVert\mathbf{y}\lVert^{2} + 2\mathbf{x}^{T}\mathbf{y}$$

now, we use a property called Cauchy-Schwarz inequality, that is stated as follows,

$$\mathbf{x}^{T}\mathbf{y} \leq \lVert\mathbf{x}\lVert\cdot\lVert\mathbf{y}\lVert$$

As follows,

$$\lVert\mathbf{x}+\mathbf{y}\lVert^{2} = \lVert\mathbf{x}\lVert^{2}+\lVert\mathbf{y}\lVert^{2} + 2\mathbf{x}^{T}\mathbf{y} \leq \lVert\mathbf{x}\lVert^{2}+\lVert\mathbf{y}\lVert^{2} + 2\lVert\mathbf{x}\lVert\cdot\lVert\mathbf{y}\lVert = (\lVert\mathbf{x}\lVert+\lVert\mathbf{y}\lVert)^{2}$$

taking the square root on both sides yields the desired inequality.

---

#### Metric

---
**Definition: Metric**

A metric $d:E\times E\rightarrow \mathbb{R}$ is a function between vectors in $E$ such that,

1. __Identity of indiscernibles:__ $\forall \mathbf{u},\mathbf{v} \in E$, $d(\mathbf{u}, \mathbf{v}) = 0 \iff \mathbf{u} = \mathbf{v}$,
2. __Symmetry:__ $\forall \mathbf{u}, \mathbf{v}$, $d(\mathbf{u}, \mathbf{v}) = d(\mathbf{v}, \mathbf{u})$.
3. __Triangle Inequality:__  $\forall \mathbf{u},\mathbf{v},\mathbf{w}$, $d(\mathbf{u},\mathbf{v}) \leq d(\mathbf{u},\mathbf{w}) + d(\mathbf{w},\mathbf{v})$.

A vector space equiped with a metric $d$ is denoted $(E, d)$, and is called a __metric space__.

---

As before we build our intuition in the case of Euclidean spaces.

---
**Example: Metrics in Euclidean Spaces**

Let $E = \mathbb{R}^{n}$. The natural notion of metric is given by,

$$d(\mathbf{x},\mathbf{y}) = \lVert\mathbf{x}-\mathbf{y}\lVert = \sqrt{\sum_{i=1}^{n}(x_{i}-y_{i})^{2}}$$

Next, we verify the three properties of the metric,

__Property 1__

Let $\mathbf{x} = \mathbf{y}$,

$$d(\mathbf{x},\mathbf{x}) = \sqrt{\sum_{i=1}^{n}(x_{i}-x_{i})^{2}} = \sqrt{\sum_{i=1}^{n}0} = 0$$

On the other hand, let $d(\mathbf{x},\mathbf{y}) = 0$,

$$\sqrt{\sum_{i=1}^{n}(x_{i}-y_{i})^{2}} = 0 \iff \sum_{i=1}^{n}(x_{i}-y_{i})^{2} = 0$$

note that this is a summation over non-negative terms. Thus,

$$\forall i, x_{i} - y_{i} = 0 \implies x_{i} = y_{i}$$

__Property 2__

For $\mathbf{x}, \mathbf{y}$,

$$d(\mathbf{x}, \mathbf{y}) = \sqrt{\sum_{i=1}^{n}(x_{i}-y_{i})^{2}} = \sqrt{\sum_{i=1}^{n}(y_{i}-x_{i})^{2}} = d(\mathbf{y}, \mathbf{x})$$

__Property 3__

For $\mathbf{x}, \mathbf{y}$ and $\mathbf{z}$,

$$d(\mathbf{x},\mathbf{y}) = \sqrt{\sum_{i=1}^{n}(x_{i}-y_{i})^{2}} = \sqrt{\sum_{i=1}^{n}(x_{i} - z_{i} + z_{i} - y_{i})^{2}} \leq \sqrt{\sum_{i=1}^{n}(x_{i}-z_{i})^{2}} + \sqrt{\sum_{i=1}^{n}(z_{i}-x_{i})^{2}}$$

which, as you may notice, the final term is $d(\mathbf{x}, \mathbf{z}) + d(\mathbf{z}, \mathbf{y})$.

---

## Derivatives

### On the Real Line

### On Euclidean Spaces

When $X \subset \mathbb{R}^{n}$, we say that $f:X\rightarrow\mathbb{R}$ is a (scalar) function of various variables. In
this case, the variables are $x_{1}, \cdots, x_{n}$. Let $\mathbf{x} = [\mathbf{x}_{1},\cdots,\mathbf{x}_{n}]$, the notion of derivative of $f$ is not unique. Let us start with __partial derivatives__,

#### Partial Derivatives

The definition of partial derivative is,

$$\dfrac{\partial f}{\partial x_{i}} = \lim_{h\rightarrow 0}\dfrac{f(x_{1},\cdots,x_{i}+h,\cdots,x_{n}) - f(x_{1},\cdots,x_{i},\cdots,x_{n})}{h},$$

note that this is equivalent to varying the i-th variable, while holding the other variables constant. That is why we call this notion of derivative "partial". Let us now illustrate partial derivatives of a function on $\mathbb{R}^{2}$.

---
**Example: Partial Derivatives on $\mathbb{R}^{2}$**

Let $f(x, y) = x^{2} + y^{2}$. The graph of this function can be seen below,

<p align="center">
  <img src="https://eddardd.github.io/images/PostFigures/Calculus/Surf3d.png"/>
</p>

If you find any of the two variables (e.g. x or y), you end up with a quadratic function. Let us illustrate for a fixed $y_{0}$,

$$g_{y_{0}}(x) = f(x, y_{0}) = x^{2} + y_{0}^{2}$$

thus,

$$\dfrac{\partial f}{\partial x} = \dfrac{dg}{dx} = 2x$$

the same reasoning may be applied for $x = x_{0}$ fixed, yielding $\partial f/\partial y = 2y$.

---

Note that we may illustrate the notion of partial derivative using basis vectors as well. For an Euclidean space of dimension $n$, is canonical basis is given by $B = \{\mathbf{e}_{i}\}_{i=1}^{n}$, where,

$$e_{ij} = \begin{cases} 1 & \text{if } j=i\\ 0 & \text{otherwise} \end{cases}$$

As follows, the partial derivative can be written as,

$$\dfrac{\partial f}{\partial x_{i}} = \lim_{h\rightarrow 0}\dfrac{f(\mathbf{x} + h\mathbf{e}_{i}) - f(\mathbf{x})}{h}$$

#### Directional Derivative

Using the previous definition for the partial derivative, note that we may as well differentiate $f$ with respect to any vector $\mathbf{v} \in \mathbb{R}^{n}$. This is defined as follows,

$$\dfrac{\partial f}{\partial \mathbf{v}} = \lim_{h\rightarrow 0}\dfrac{f(\mathbf{x} + h\mathbf{v}) - f(\mathbf{x})}{h}$$

This highlights the difference between derivatives on the real line, and on Euclidean spaces: in the former, you can only differentiate in one direction (the real line), while in the latter, there are infinitely many directions to differentiate. These different directions are all related to the partial derivatives, as $\mathbf{v}$ can be decomposed using the canonical basis.

For now, we set this definition aside, and concentrate on the Gradient vector, a notion that will allow us to easily calculate directional derivatives.

#### The Gradient

Note that, as you have multiple partial derivatives, it is sometimes useful to have an aggregated notion of how the function varies as well. To that end, we introduce the Gradient vector, defined as follows,

$$\nabla f = \biggr[\dfrac{\partial f}{\partial x_{1}},\cdots,\dfrac{\partial f}{\partial x_{n}}\biggr] \in \mathbb{R}^{n}$$

that is, for a function of $n$ variables, the gradient is a vector of dimension $n$ as well. In some texts, you might also see the following notation,

$$\dfrac{\partial f}{\partial \mathbf{x}} = \biggr[\dfrac{\partial f}{\partial x_{1}},\cdots,\dfrac{\partial f}{\partial x_{n}}\biggr] \in \mathbb{R}^{n}$$

which highlights the fact that we are differentiating $f$ with respect to the whole vector. This, however, can create a misunderstanding with the notation of directional derivative (which is a number, not a vector), introduced in the previous section. Thus, $\nabla f$ is more clear than $\partial f/ \partial \mathbf{x}$.

Now, let us illustrate how the gradient vector works,

---
**Example: Gradients on $\mathbb{R}^{2}$**

Let $f(x, y) = x^{2} + y^{2}$. The partial derivatives of $f$ are,

$$\begin{cases}\dfrac{\partial f}{\partial x} = 2x,\\\dfrac{\partial f}{\partial y} = 2y,\end{cases}$$

thus the gradient of $f$ is the vector $\nabla f(x, y) = (2x, 2y)$. Note that this is a __vector field__, namely, a function that takes input vectors, and attributes output vectors, $\nabla f:\mathbb{R}^{2}\rightarrow\mathbb{R}^{2}$. You can see the gradient vector at various points in $\mathbb{R}^{2}$, and how it relates to the value of $f$ in the figure below,

<p align="center">
  <img src="https://eddardd.github.io/images/PostFigures/Calculus/Grad2d.png"/>
</p>

Here, note a few things, First, the gradient points in the direction of maximum increase in $f$. Second, the gradient vectors are orthogonal to the level sets. Indeed, let $\gamma$ be a curve in $\mathbb{R}^{2}$ parametrized by a parameter $\lambda$, such that,

$$g(\lambda) = f(\gamma(\lambda)) = k$$

differentiating with respect to $\lambda$ yields,

$$\dfrac{d g}{d \lambda} = \sum_{i=1}^{n}\dfrac{\partial f}{\partial x_{i}}\dfrac{\partial x_{i}}{\lambda} = \langle \nabla f, \dot{\gamma}\rangle$$

on the other hand, since $g$ is a constant function, its derivative is zero. Hence, $\nabla f$ is perpendicular to the tangent vector $\dot{\gamma}$.

---

Next we illustrate the connection between the gradient vector and the directional derivative,

---
**Example: Gradients and the Directional Derivative**

Let $f(\mathbf{x})$ for $\mathbf{x} \in \mathbb{R}^{n}$. Let $\mathbf{v}$ be an arbitrary vector in $\mathbb{R}^{n}$. Let $g:\mathbb{R}\rightarrow\mathbb{R}$ be the function,

$$g(h) = f(\mathbf{x}+h\mathbf{v}).$$

Note that the derivative of $g$ at $0$ is,

$$\dfrac{dg}{dh}(0) = \lim_{h\rightarrow 0}\dfrac{g(h) - g(0)}{h} = \lim_{h\rightarrow 0} \dfrac{f(\mathbf{x}+h\mathbf{v})-f(\mathbf{x})}{h} = \dfrac{\partial f}{\partial \mathbf{v}}$$

Thus, we may find an expression for $\partial f/\partial \mathbf{v}$ by taking the limit on the derivative of $g$,

$$\dfrac{\partial f}{\partial \mathbf{v}} = \lim_{h\rightarrow 0}\dfrac{dg}{dh} = \sum_{i=1}^{n}\dfrac{\partial f}{\partial x_{i}}\dfrac{d x_{i}}{d h} = \sum_{i=1}^{n}\dfrac{\partial f}{\partial x_{i}}v_{i} = \langle \nabla f, \mathbf{v} \rangle,$$

now, considering the generalized law of cosines, one has that,

$$\lVert\mathbf{a}-\mathbf{b}\lVert^{2} = \lVert\mathbf{a}\lVert^{2} + \lVert\mathbf{b}\lVert^{2} - 2\lVert\mathbf{a}\lVert\lVert\mathbf{b}\lVert\cos\theta$$

where $\theta$ is the angle between vectors $\mathbf{a}$ and $\mathbf{b}$. On the other hand,

$$\lVert\mathbf{a}-\mathbf{b}\lVert^{2} = \langle \mathbf{a}-\mathbf{b},\mathbf{a}-\mathbf{b}\rangle = \lVert\mathbf{a}\lVert^{2} + \lVert\mathbf{b}\lVert - 2\langle \mathbf{a}, \mathbf{b}\rangle$$

equating these two terms yields,

$$\langle \mathbf{a},\mathbf{b}\rangle = \lVert\mathbf{a}\lVert\lVert\mathbf{b}\lVert\cos\theta$$

which implies that,

$$\langle \nabla f, \mathbf{v}\rangle = \lVert\nabla f\lVert|\mathbf{v}\lVert\cos\theta$$

assuming, without loss of generality, that $\lVert \mathbf{v} \lVert = 1$, that is, we are simply taking derivatives on the direction of $\mathbf{v}$, we see that the expression,

$$\dfrac{\partial f}{\partial \mathbf{v}} = \langle \nabla f, \mathbf{v}\rangle = \lVert\nabla f\lVert\cos\theta$$

which is maximum when $\theta = 0$, that is, when $\mathbf{v}$ points in the deirection of $\nabla f$. Thus, the gradient points in the direction of maximum increase in the function $f$.

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

that is, $\mathbf{u}_{p}$, the same vector but in polar coordinates, is $\mathbf{u}_{p} = (\sqrt{2}, \pi/4)$. Now, it is important to notice how $x$ and $y$ affect the basis vectors. Let $e_{r}$ be the radial basis vector, and $e_{\theta}$ be the angular basis vector. For different points in space we have different vectors, as

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

similarly,

$$\begin{cases}\mathbf{e}_{x} = \dfrac{\partial r}{\partial x}\mathbf{e}_{r} + \dfrac{\partial \theta}{\partial x}\mathbf{e}_{\theta}\\ \mathbf{e}_{y} = \dfrac{\partial r}{\partial y}\mathbf{e}_{r} + \dfrac{\partial \theta}{\partial y}\mathbf{e}_{\theta}\end{cases}$$

this implies that, the radial direction component,

$$\dfrac{\partial f}{\partial r}\biggr(\dfrac{\partial r}{\partial x}\biggr)^{2}  + \dfrac{\partial f}{\partial r}\dfrac{\partial \theta}{\partial x}\dfrac{\partial r}{\partial x} + \dfrac{\partial f}{\partial r}\biggr(\dfrac{\partial r}{\partial y}\biggr)^{2}  + \dfrac{\partial f}{\partial r}\dfrac{\partial \theta}{\partial y}\dfrac{\partial r}{\partial y}$$

likewise, in the angular direction,

$$\dfrac{\partial f}{\partial \theta}\biggr(\dfrac{\partial \theta}{\partial x}\biggr)^{2}  + \dfrac{\partial f}{\partial \theta}\dfrac{\partial \theta}{\partial x}\dfrac{\partial r}{\partial x} + \dfrac{\partial f}{\partial \theta}\biggr(\dfrac{\partial \theta}{\partial y}\biggr)^{2}  + \dfrac{\partial f}{\partial \theta}\dfrac{\partial \theta}{\partial y}\dfrac{\partial r}{\partial y}$$

---

### On Metric Spaces

### Jacobian Matrix

## Applications in Signal Processing

### Time Series

### Images

### Graphs