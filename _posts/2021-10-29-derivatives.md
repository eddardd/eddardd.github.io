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

# Introduction

In introductory calculus courses, derivatives play a central role alongside integrals. Even though the definition and intuition of a derivative in the real line is very straightforward, one can often found themselves lost in more general (and complex) spaces.

To that end, we begin by introducing different notions of spaces that are, in a certain sense, generalizations of the real line. Once these spaces are properly defined, we can understand derivatives on functions whose domain are these spaces. Whenever possible, we adopt a bottom-up approach in terms of abstraction, namely, we start with the most basic notions up to the most general.

## Vector Spaces

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

## Euclidean Spaces

As $\mathbb{R}^{2}$ represents a way to identify points on a plane, $\mathbb{R}^{3}$ allows us to identify points in space. This is done by using three cordinates $\mathbf{p} = (x, y, z)$. The most general case correspond to $\mathbb{R}^{n}$, where $n$ is any positive integer. These spaces are generally called Euclidean Spaces. Points in an Euclidean space are defined as follows,

$$\mathbf{x} = (x_{1},\cdots,x_{n})$$

that is, they have $n$ coordinates. For that reason, they are called $n-$dimensional spaces. As we did with the Cartesian plane, sum of vectors in Euclidean spaces can be defined as follows,

$$\mathbf{x} + \mathbf{y} = (x_{1} + y_{1},\cdots,x_{n}+y_{n})$$

that is, one has coordinate-wise sum. This is also valid for scalar multiplication,

$$\lambda \mathbf{x} = (\lambda x_{1}, \cdots, \lambda x_{2})$$

As follows, let us define two new operations on vectors: the inner product, and the norm.

## Metric Spaces



# Derivatives

## On the Real Line

## On Euclidean Spaces

## On Metric Spaces

## Jacobian Matrix

## Hessian Matrix