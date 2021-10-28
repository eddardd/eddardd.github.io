---
title: 'Calculus -- Derivatives'
date: 2021-10-28
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

An inner product is an operation $\langle \cdot, \cdot \rangle:E\times E\rightarrow\mathbb{R}$ on a vector space $E$ that suffices the following properties,

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

A norm is an operation $ ||\cdot|| : E \rightarrow \mathbb{R}$ on a vector space $E$ that suffices the following properties,

1. __Nonegativity:__ $\forall \mathbf{u} \in \mathbb{R}^{n}$, $|| \mathbf{u} || \geq 0$, the equality holding if and only if $\mathbf{u} = \mathbf{0}$.
2. __Scalar Multiplication:__ $\forall \mathbf{u}, \forall \lambda \in \mathbb{R}$, $||\lambda \mathbf{u}|| = |\lambda|\cdot||\mathbf{u}||$
3. __Triangle Inequality:__ $\forall \mathbf{u},\mathbf{v} \in E$,

$$||\mathbf{u}+\mathbf{v}|| \leq ||\mathbf{u}|| + ||\mathbf{v}||$$

A vector space equiped with a norm is denoted as $(E,||\cdot||)$, and is called a __normed space__.

---

As with the canonical inner product, one also has a canonical idea of norm in Euclidean spaces,

---
**Example: Norms in Euclidean Spaces**

Let \(E = \mathbb{R}^{n}\). The natural notion of norm is given by,

$$||\mathbf{x}|| = \sqrt{\mathbf{x}^{T}\mathbf{x}} = \sqrt{\langle\mathbf{x},\mathbf{x}\rangle} = \sum_{i=1}^{n}x_{i}^{2}$$

Next, we verify the three properties of the norm,

__Property 1__

This property is the same as Property 3 of the canonical inner product on $\mathbb{R}^{n}$.

__Property 2__

$$||\lambda\mathbf{x}|| = \sqrt{\sum_{i=1}^{n}(\lambda x_{i})^{2}} = \sqrt{\sum_{i=1}^{n}\lambda^{2}x_{i}^{2}} = |\lambda|\sqrt{\sum_{i=1}^{n}x_{i}^{2}}$$

__Property 3__

$$||\mathbf{x}+\mathbf{y}||^{2} = \sum_{i=1}^{n}(x_{i} + y_{i})^{2} = \sum_{i=1}^{n}x_{i}^{2} + \sum_{i=1}^{n}y_{i}^{2} + 2\sum_{i=1}^{n}x_{i}y_{i} = ||\mathbf{x}||^{2}+||\mathbf{y}||^{2} + 2\mathbf{x}^{T}\mathbf{y}$$

now, we use a property called Cauchy-Schwarz inequality, that is stated as follows,

$$\mathbf{x}^{T}\mathbf{y} \leq ||\mathbf{x}||\cdot||\mathbf{y}||$$

As follows,

$$||\mathbf{x}+\mathbf{y}||^{2} = ||\mathbf{x}||^{2}+||\mathbf{y}||^{2} + 2\mathbf{x}^{T}\mathbf{y} \leq ||\mathbf{x}||^{2}+||\mathbf{y}||^{2} + 2||\mathbf{x}||\cdot||\mathbf{y}|| = (||\mathbf{x}||+||\mathbf{y}||)^{2}$$

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

$$d(\mathbf{x},\mathbf{y}) = ||\mathbf{x}-\mathbf{y}|| = \sqrt{\sum_{i=1}^{n}(x_{i}-y_{i})^{2}}$$

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

### On Metric Spaces

### Jacobian Matrix

### Hessian Matrix