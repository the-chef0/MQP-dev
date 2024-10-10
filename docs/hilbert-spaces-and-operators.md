# Hilbert spaces and operators

Recommended reference: {cite:t}`griffiths`, in particular Section 3.1.

So far, we have been considering vectors in an $n$-dimensional real or
complex space.  In quantum mechanics, one encounters
infinite-dimensional vector spaces as well.  In particular, the state
of a quantum system is represented mathematically by a (unit) vector
in a *Hilbert space*.

## Vector spaces

As is customary in quantum physics and quantum algorithms, we use
*bra-ket notation* for vectors in Hilbert spaces.

:::{prf:definition} Vector space

A (real or complex) *vector space* consists of

- a set of vectors, denoted by symbols like $\ket\alpha$
- a distinguished vector $\mathbf{0}$ called the *zero vector*[^zero]
- a way of *adding vectors*, i.e. given two vectors $\ket\alpha$ and
  $\ket\beta$ we can obtain a third vector
  $\ket\alpha+\ket\beta$
- a way of *multiplying a vector by a scalar*: given a scalar $c$ (a
  real or complex number, depending on whether we consider a real or
  complex vector space) and a vector $\ket\alpha$ we can obtain a
  vector $c\ket\alpha$

such that a number of properties hold:

- $0\ket\alpha=\mathbf{0}$
- $1\ket\alpha=\ket\alpha$
- $\mathbf{0}+\ket\alpha=\ket\alpha$
- $\ket\alpha+\ket\beta=\ket\beta+\ket\alpha$
- $(\ket\alpha+\ket\beta)+\ket\gamma=\ket\alpha+(\ket\beta+\ket\gamma)$
- $c(\ket\alpha+\ket\beta)=c\ket\alpha+c\ket\beta$
- $(c+d)\ket\alpha = c\ket\alpha + d\ket\alpha$
- $c(d\ket\alpha)=(cd)\ket\alpha$

:::

[^zero]: We avoid denoting the zero vector by $\ket0$ because this
    symbol is often used for a distinguished unit vector, as in
    {prf:ref}`finite-dimensional`.

:::{prf:definition} Basis, dimension
A *basis* for a vector space is a collection of vectors
$(\ket{e_i})_i$ such that every vector can be written in exactly one
way as a linear combination of the $\ket{e_i}$.  The number of
vectors in a basis is called the *dimension* of the space.
:::

As in [](basis-transformations), any two bases contain the same number
of vectors, so the dimension does not depend on the choice of bases.

:::{prf:example}
:label: finite-dimensional

The simplest examples are the $n$-dimensional vector spaces $\RR^n$
and $\CC^n$ with coordinatewise addition and scalar multiplication.

An important special case is the two-dimensional space $\CC^2$.  In
bra-ket notation, various symbols are used for the standard basis
vectors $\binom{1}{0}$ and $\binom{0}{1}$ in this space, such as

- $\ket+$ and $\ket-$;
- $\ket0$ and $\ket1$ (in the context of quantum computing);
- $\ket\uparrow$ and $\ket\downarrow$ (in the context of spins).
:::

## Hermitian inner products and Hilbert spaces

:::{prf:definition} Hermitian inner product
A *Hermitian inner product* assigns to vectors $\ket\alpha$ and
$\ket\beta$ a complex number $\braket{\alpha|\beta}$ such that the
following properties hold:

- $\braket{\alpha|\beta}=\overline{\braket{\beta|\alpha}}$;
- $\braket{\alpha|\alpha}\ge0$, and $\braket{\alpha|\alpha}=0$ holds
  precisely when $\ket\alpha=\mathbf{0}$ (note that
  $\braket{\alpha|\alpha}$ is a real number because of the first
  property);
- $\bra\alpha(\ket\beta+\ket\gamma) = \braket{\alpha|\beta} +
  \braket{\alpha|\gamma}$;
- $\bra\alpha(c\ket\beta)=c(\braket{\alpha|\beta})$;
:::

:::{prf:definition} Norm of a vector
The *norm* or *length* of a vector $\ket\alpha$ in a Hilbert space
is defined as the real number

$$
\|\alpha\|=\sqrt{\braket{\alpha|\alpha}}.
$$
:::

:::{note}

The properties above imply that for any scalar $c$, the inner product
of $c\ket{\alpha}$ with $\ket\beta$ equals $\bar
c\braket{\alpha|\beta}$.  In other words, while the inner product is
(complex) linear in its second argument, it is only *conjugate linear*
in its first argument.  This is the usual convention in physics.  It
differs from the convention in mathematics, where inner products are
usually linear in the first argument and conjugate linear in the
second argument.

:::

:::{prf:definition} Hilbert space

A *Hilbert space* is a complex vector space $\HH$ equipped with a
Hermitian inner product $\braket{\enspace|\enspace}$ and satisfying a
condition called *completeness*.
:::

We will not make the notion of completeness precise in these notes.
Roughly speaking, it means that we do not have to worry too much about
convergence of sequences or series in our Hilbert space, and there is
a notion of infinite linear combinations.

:::{prf:example}
:label: inner-product-example

We take $\HH=\CC^n$ and define an inner product by

$$
\braket{\alpha|\beta} = \sum_{i=1}^n\bar\alpha_i\beta_i.
$$

Check for yourself that this satisfies all the properties of a
Hermitian inner product.
:::

:::{prf:example}

Consider the space $L^2(\RR)$ of square-integrable functions on the
real line, i.e.\ functions $f\colon\RR\to\CC$ such that the integral
$\int_{-\infty}^\infty|f(x)|^2 dx$ exists and is finite.  This is a
Hilbert space with inner product

$$
\braket{f|g} = \int_{-\infty}^\infty \bar f(x)g(x)dx.
$$

:::

:::{prf:example}

Consider the space $\ell^2$ of sequences $z=(z_0,z_1,\ldots)$ such
that

$$
\sum_{n=0}^\infty |z_n|^2<\infty.
$$

Then $\ell^2$ is a Hilbert space with inner product

$$
\braket{w|z}=\sum_{n=0}^\infty \bar w_n z_n.
$$

:::

## The dual Hilbert space

With any Hilbert space $\HH$, we can associate a *dual* Hilbert space
$\HH^*$.  In the bra-ket formalism used in quantum mechanics, for
every “ket vector” $\ket\alpha$ in $\HH$ we have a dual “bra vector”
$\bra\alpha$ in $\HH^*$.  This correspondence has the following
properties:

| vector in $\HH$         | corresponding vector in $\HH^*$ |
|-------------------------|---------------------------------|
| ket vector $\ket\alpha$ | bra vector $\bra\alpha$         |
| $\ket\alpha+\ket\beta$  | $\bra\alpha+\bra\beta$          |
| $c\ket\alpha$           | $\bar c\bra\alpha$              |

In mathematical language, $\HH^*$ is the space of continuous linear
maps $\HH\to\CC$.  The bra vector $\bra\alpha$ then represents the
linear map that sends $\ket\beta$ to $\braket{\alpha|\beta}$.

:::{prf:example} The dual of $\CC^n$

In the case $\HH=\CC^n$ we can identify $\HH^*$ with $\CC^n$ as well;
a vector $\ket\alpha$ in $\CC^n$ is then simply the same vector.
However, it can also be convenient to distinguish $\HH$ from $\HH^*$
by denoting a ket vector $\ket\alpha$ as a column vector and the
corresponding bra vector $\bra\alpha$ by the conjugate transpose of
$\ket\alpha$, so $\bra\alpha$ is the row vector whose entries are the
complex conjugates of those of $\alpha$.  In $\CC^2$, for example, we
have

$$
\ket\alpha=\begin{pmatrix}1\\ i\end{pmatrix}\Longrightarrow
\bra\alpha=\begin{pmatrix}1& -i\end{pmatrix}.
$$

This allows us to view $\bra\alpha$ as a linear map sending
$\ket\beta$ to the scalar obtained by multiplying the row vector
$\bra\alpha$ by the column vector $\ket\beta$.

:::

:::{note}
If you know about continuous linear maps of Hilbert spaces, here is a
useful connection between the mathematical definition of $\HH^*$ and
the bra-ket formalism: the *Riesz representation theorem* states that
every continuous linear map $\HH\to\CC$ is of the form
$\braket{\alpha|\enspace}$ for some $\alpha$ in $\HH$.
:::

## Orthonormal bases

:::{prf:definition} Orthonormal basis

An *orthonormal basis* of a Hilbert space $\HH$ is a collection of
vectors $(\ket{e_i})_{i\in I}$, where $I$ is some index set, such that

1. the system is orthonormal: $\braket{e_i|e_j} = \begin{cases}
   1&\quad\text{if }i=j,\\ 0&\quad\text{if }i\ne j;\end{cases}$
2. the system spans $\HH$: every vector in $\HH$ can be written
   as a (possibly infinite) linear combination of the $\ket{e_i}$.

:::

:::{prf:example}
:label: fourier-orthonormal

Consider the space $L^2([0,1])$ of square-integrable functions on the
unit interval.  An orthonormal basis for this space consists of the
functions $e_n(x)=\exp(2\pi i n x)$ for all integers $n$.  This
reflects the fact that every “nice” function on $[0,1]$ can be
expressed as a [](fourier-series).
:::

## Operators

An *operator* or *linear map* on a Hilbert space $\HH$ is a map

$$
T\colon\HH\to\HH
$$

such that

- $T(c\ket\alpha) = c(T\ket\alpha)$
- $T(\ket\alpha+\ket\beta) = T\ket\alpha+T\ket\beta$

:::{note}
It is also possible to define linear maps between different
Hilbert spaces, but we will not go into this.
:::

:::{note}
In physics, operators are often denoted with a hat, so what we call
$T$ would be written as $\hat T$.  One reason for this is to
distinguish a physical quantity (like momentum or energy) from the
corresponding mathematical operator.  Since we do not consider
physical quantities in this module, we do not use the hat notation.
:::

Operators form a vector space: the sum of two operators $T$ and $U$
is defined by the formula

$$
(T+U)\ket\alpha=T\ket\alpha+U\ket\alpha
$$

and multiplication of an operator $T$ by a scalar $c$ is defined by
the formula

$$
(cT)\ket\alpha=c(T\ket\alpha).
$$

Like matrices (but unlike vectors), operators can be *composed*: if
$T$ and $U$ are two operators, then $TU$ is the operator defined by

$$
(TU)\ket\alpha=T(U\ket\alpha).
$$

## Matrix entries

In physics, quantities of the form

$$
\bra\alpha T\ket\beta
$$

are often of special interest.  These are called *matrix entries* of
$T$, especially if $\ket\alpha$ and $\ket\beta$ are basis vectors in
some fixed orthonormal basis of $\HH$.

:::{prf:example}
Consider a linear map $A$ on $\CC^n$, viewed as a matrix.  Taking the
matrix entries as above, letting $\alpha$ and $\beta$ range over the
standard basis vectors, we obtain the usual matrix entries of $A$; see
{ref}`matrix-entry`.
:::

:::{note}

A remark on notation: you will often see notation like
$\braket{\alpha|T\beta}$.  This is literally meaningless: the operator
$T$ can be applied to vectors, which we denote by $\ket\beta$, while
the symbol $\beta$ is just a label.  However, in practice it is very
convenient and completely unambiguous to write
$\braket{\alpha|T\beta}$ instead of the strictly speaking more correct
$\bra\alpha(T\ket\beta)$.

Similarly, we will write $\braket{T\alpha|\beta}$ to mean the inner
product of $T\ket\alpha$ with $\ket\beta$.  Extending this notation to
scalars $c$, we can also write

$$
\braket{c\alpha|\beta}=\overline{\braket{\beta|c\alpha}}
=\bar c\overline{\braket{\beta|\alpha}}
=\bar c\braket{\alpha|\beta}.
$$

:::

## The adjoint of an operator

Given an operator $T$ on a Hilbert space $\HH$, there exists an
operator $T^\dagger$ with the property

$$
\braket{T^\dagger\alpha|\beta}=\braket{\alpha|T\beta}
\quad\text{for all }\ket\alpha,\ket\beta\in\HH.
$$

The operator $T^\dagger$ is called the *adjoint* of $T$.

:::{prf:property} Properties of the adjoint

For all operators $T$, $U$ and scalars $c$, we have

1. $(cT)^\dagger = \bar c T^\dagger$

2. $(TU)^\dagger = U^\dagger T^\dagger$

3. $(T^\dagger)^\dagger = T$

:::

:::{prf:example}

Take $\HH=\CC^n$ with the standard inner product.  For a matrix $A$,
we write $A^\dagger$ for the conjugate transpose of $A$.  We extend
this to vectors by viewing column vectors as $n\times 1$-matrices and
row vectors as $1\times n$-matrices.  For all
$\ket\alpha,\ket\beta\in\HH$ we then have

$$
\braket{\alpha|A|\beta}=\ket{\alpha}^\dagger A\ket\beta
=(A^\dagger\ket{\alpha})^\dagger\ket\beta
=\braket{A^\dagger\alpha|\beta}.
$$

This shows that the adjoint of the operator $A$ corresponds to the
conjugate transpose of $A$ viewed as a matrix.

:::

## Hermitian operators

:::{prf:definition} Hermitian operator

An operator $T$ on a Hilbert space $\HH$ is called *Hermitian* if it
is equal to its own adjoint, i.e.

$$
T = T^\dagger.
$$

:::

It is clear from the definition that all real symmetric matrices are
Hermitian.

:::{prf:example}
The matrix

$$
\begin{pmatrix}
3& 2+i& 4i\\
2-i& 1& 1-i\\
-4i& 1+i& 3
\end{pmatrix}
$$

is Hermitian.
:::

The following two results are of fundamental importance in quantum
mechanics.  Together, they explain why in the mathematical formalism
of quantum mechanics, Hermitian operators correspond to physical
observables.

::::{prf:theorem}

The eigenvalues of a Hermitian operator $T$ are real.

:::{prf:proof}
Suppose $\lambda$ is an eigenvalue of $T$ and $\ket\alpha$ is a
corresponding eigenvector.  Then we have

$$
\lambda\braket{\alpha|\alpha}=\braket{\alpha|T\alpha}
=\braket{T\alpha|\alpha}=\braket{\lambda\alpha|\alpha}
=\bar\lambda\braket{\alpha|\alpha}.
$$

Since $\braket{\alpha|\alpha}$ is non-zero, we can divide by it and
obtain $\lambda=\bar\lambda$.
:::

::::

:::{prf:theorem} Spectral theorem

If $T$ is a Hermitian operator on a Hilbert space $\HH$, then there
exists an orthonormal basis of $\HH$ that consists of eigenvectors for
$T$.

:::

:::{note}
Besides the fact that they encode observables, Hermitian operators
also appear in quantum physics as *density operators*, or *density
matrices*, which describe mixed states (ensembles of quantum systems)
rather than individual (‘pure’) quantum states.  Density operators
play a central role in the theory of entanglement and in quantum
information theory; we will not discuss them further here.
:::

## Unitary operators

:::{prf:definition} Unitary operator

An operator $T$ on a Hilbert space $\HH$ is called *unitary* if the
adjoint $T^\dagger$ is a two-sided inverse of $T$, i.e.

$$
TT^\dagger = \id\quad\text{and}\quad T^\dagger T=\id.
$$

:::

Alternatively, $T$ is unitary whenever $T$ preserves inner products;
see {ref}`unitary-inner-product` for a precise statement.

:::{prf:example}
For any angle $\phi$, the matrix

$$
\begin{pmatrix}
\cos\phi& \sin\phi\\
i\sin\phi& -i\cos\phi
\end{pmatrix}
$$

is unitary.
:::

It follows immediately from the definition that the identity operator
on a Hilbert space (sending every vector to itself) is unitary, and if
$T$ is a unitary operator, then its inverse $T^{-1}$ (which equals
$T^\dagger$) is also unitary.  Similarly, if $T$ and $U$ are unitary
operators, then so is the product $TU$.  This means that the
collection of all unitary operators on a Hilbert space $\HH$ has the
mathematical structure of a
[group](https://en.wikipedia.org/wiki/Group_(mathematics)).

## Linear subspaces and projection operators

:::{prf:definition} Linear subspace
A *linear subspace* of a vector space $V$ is a set of vectors $W$
contained in $V$ such that the following properties hold:

1. The zero vector of $V$ lies in $W$.

2. For all vectors $w$ and $w'$ in $W$, the sum $w+w'$ is also in $W$.

3. For every vector $w$ in $W$ and every scalar $\lambda$, the scalar
   multiple $\lambda w$ is also in $W$.
:::

In quantum physics, linear subspaces are important because they encode
quantum states that a physical state can collapse to when a
measurement is performed.  To make this more precise, we need the
notions of *orthogonal complement* of a subspace and *orthogonal
projection* onto a subspace.

:::{prf:definition} Orthogonal complement
Consider a Hilbert space $\HH$ and a linear subspace $W$ of $\HH$.
The *orthogonal complement* of $W$ in $\HH$ is the set of all vectors
$\ket{v}$ in $\HH$ that are orthogonal to all of $W$, i.e. such that

$$
\braket{v|w}=0\quad\text{for all }w\in W.
$$
:::

It is not hard to verify that the orthogonal complement of a linear
subspace of $\HH$ is again a linear subspace; see
{ref}`exercise-orthogonal`.

Within a Hilbert space $\HH$, we will only consider linear subspaces
$W$ that satisfy the completeness condition and are therefore
themselves Hilbert spaces.  In this case, one can show that every
vector $\ket\alpha\in\HH$ can be decomposed uniquely as

$$
\ket\alpha = \ket{\alpha}_W + \ket{\alpha}_{W^\perp}
\quad\text{with}\quad\ket{\alpha}_W\text{ in }W\quad\text{and}\quad
\ket{\alpha}_{W^\perp}\text{ in }W^\perp.
$$

:::{prf:definition} Orthogonal projection
Given a Hilbert space $\HH$ and a linear subspace $W$ of $\HH$, the
*orthogonal projection* of $\HH$ onto $W$ is the map that sends every
vector $\ket\alpha$ to $\ket\alpha_W$.
:::

Orthogonal projections onto linear subspaces turn out to be important
examples of linear maps.

## Exponential of an operator

Recall (see [](taylor-exponential)) that the exponential function can
be characterised by the Taylor series

$$
\exp(x) = \sum_{n=0}^\infty\frac{1}{n!}x^n.
$$

In quantum physics, it is frequently useful to apply this formula not
just to (real or complex) numbers, but to operators as well.  One
reason is that there is an important application to differential
equations.

:::{prf:definition} Exponential of an operator
If $T$ is an operator on a Hilbert space $\HH$, then the *exponential*
of $T$ is the operator

$$
\exp(T) = \sum_{n=0}^\infty\frac{1}{n!}T^n.
$$
:::

In the case where $\HH=\CC^n$, an operator $T$ corresponds to an
$n\times n$-matrix $A$.  The same formula as above then defines the
*matrix exponential* $\exp(A)$ of $A$.

:::{warning}
Since operators do not commute in general, the exponential does *not*
have the property $\exp(T+U)=\exp(T)\exp(U)$ as one might expect from
the case of ordinary exponentials.
:::

We have seen in {prf:ref}`diffeq-exp` that the solution of the
differential equation

$$
\dot x(t) = \lambda x(t),\quad x(0)=x_0
$$

is given by

$$
x(t) = x_0 \exp(\lambda t)
$$

Using the operator exponential, we can solve the higher-dimensional
variant

$$
\frac{d}{dt}\ket{x(t)} = T\ket{x(t)},\quad \ket{x(0)}=\ket{x_0}.
$$

Namely, the solution is

$$
\ket{x(t)} = \exp(Tt)\ket{x_0}.
$$

***

## Exercises

:::{exercise}
Check that the inner product defined in
{prf:ref}`inner-product-example` is indeed a Hermitian inner product
on $\CC^n$.
:::

:::{exercise}
:label: matrix-entry

Consider a matrix $A=(A_{i,j})_{i,j=1}^n$ and the standard basis
$(\ket{e_i})_{i=1}^n$ of $\CC^n$.  Check that the matrix entry
$\braket{e_i|A|e_j}$ is simply the actual matrix entry $A_{i,j}$.
:::

:::{exercise}
:label: unitary-inner-product

Show that an operator $T$ is unitary precisely when it satisfies
$\braket{T\alpha|T\beta}=\braket{\alpha|\beta}$ for all vectors
$\ket\alpha$, $\ket\beta$.
:::

:::{exercise}
:label: cauchy-schwarz

The *Cauchy–Schwarz inequality* states that for vectors
$\ket\alpha,\ket\beta$ in a Hilbert space we have

$$
\bigl|\braket{\alpha|\beta}\bigr|^2\le
\braket{\alpha|\alpha}\braket{\beta|\beta}.
$$

Show this using the basic properties of the inner product.
:::

:::{exercise}

The *triangle inequality* states that for vectors
$\ket\alpha,\ket\beta$ in a Hilbert space we have

$$
\|\alpha+\beta\|\le\|\alpha\|+\|\beta\|.
$$

Derive the triangle inequality from the Cauchy–Schwarz inequality.
:::

:::{exercise}
Check that the *Pauli matrices*

$$
\sigma_x = \begin{pmatrix}0& 1\\1& 0\end{pmatrix},\quad
\sigma_y = \begin{pmatrix}0& -i\\i& 0\end{pmatrix},\quad
\sigma_z = \begin{pmatrix}1& 0\\0& -1\end{pmatrix}
$$

are both Hermitian and unitary.
:::

:::{exercise}
:label: exercise-orthogonal

Consider a linear subspace $W$ of a Hilbert space $\HH$.  Show that
the orthogonal complement $W^\perp$ is again a linear subspace.
:::

:::{exercise}
Suppose $D$ is a *diagonal* $n\times n$-matrix, viewed as an operator
on the space $\CC^n$.  Find an expression for the matrix exponential
$\exp(D)$.
:::

:::{exercise}
Consider a diagonal matrix $D$ and an invertible matrix $P$.  Show
that that the matrix exponential of $PDP^{-1}$ satisfies

$$
\exp(PDP^{-1}) = P\exp(D)P^{-1}.
$$
:::
