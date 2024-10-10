# Taylor series

Recommended reference: {cite:t}`strang`, in particular Section 10.4.

*Taylor series* are a useful way of approximating a function by
simpler functions, namely polynomials.

## Definition and basic properties

Recall that a *power series* is like a polynomial, but with possibly
infinitely many powers of $x$:

$$
g(x) = c_0 + c_1 x + c_2 x^2 + c_3 x^3 + \cdots
$$

where the $c_i$ are scalars (usually real or complex numbers).

:::{prf:definition} Taylor series
Let $f$ be a (real or complex) function that is infinitely
differentiable at a point $a$.  The *Taylor series* of $f$ at $a$ is
the power series

$$
f(a) + f'(a) (x - a) + \frac{f''(a)}{2}(x - a)^2 + \cdots
= \sum_{n=0}^\infty \frac{f^{(n)}(a)}{n!}(x-a)^n.
$$
:::

For almost all functions $f$ that one encounters in practice, the
Taylor series of $f$ will converge to $f$.  This means that the
polynomials

$$
f(a) + f'(a) (x - a) + \frac{f''(a)}{2}(x - a)^2 + \cdots
+ \frac{f^{(n)}(a)}{n!}(x-a)^n
$$

of degree $n$ obtained by truncating the Taylor series to a finite
number of terms give successively better approximations to $f$ as
$n\to\infty$, at least for $x$ sufficiently close to $a$.  However,
there are exceptions; see the exercises.

## Important examples

(taylor-exponential)=
### The exponential function

Consider the function $f(x)=\exp(x)$.  To compute its Taylor series at
$x=0$, note that all derivatives of $f$ are equal to $f$ itself, and
take the value $1$ at $x=0$.  This gives

$$
\begin{aligned}
\exp(x) &= 1 + x + \frac{1}{2}x^2 + \frac{1}{6}x^3+\cdots\\
&= \sum_{n=0}^\infty\frac{1}{n!}x^n.
\end{aligned}
$$

### The logarithm

Now consider the function $f(x)=\log(1-x)$.  We have

$$
f'(x) = -\frac{1}{1-x}
$$

and more generally

$$
f^{(n)}(x) = -\frac{(n-1)!}{(1-x)^n}\quad\text{for all }n\ge1.
$$

This gives

$$
\begin{aligned}
\log(1-x) &= -x-\frac{1}{2}x^2-\frac{1}{3}x^3-\cdots\\
&= -\sum_{n=1}^\infty\frac{1}{n}x^n.
\end{aligned}
$$

(taylor-trig)=
### Trigonometric functions

The Taylor series of the cosine and sine functions are given by

$$
\begin{aligned}
\cos x &= \sum_{n=0}^\infty\frac{(-1)^n}{(2n)!}x^{2n},\\
\sin x &= \sum_{n=0}^\infty\frac{(-1)^n}{(2n+1)!}x^{2n+1}.
\end{aligned}
$$

This can be proved directly using the familiar expressions for the
derivatives of these functions.  It is also instructive to derive the
above formulae by taking the Taylor series of $\exp(ix)$, splitting it
into the real and imaginary parts and using Euler's formula

$$
\exp(ix) = \cos x + i\sin x.
$$

***

## Exercises

:::{exercise}
Compute the Taylor series at $x=0$ of the function

$$
f(x) = \frac{1}{1-x}.
$$
:::

:::{exercise}
Derive the Taylor series for the sine and cosine functions using the
two different methods sketched in [](taylor-trig).
:::

:::{exercise}
Consider the function

$$
f(x) = \begin{cases}
\exp(-1/x^2)& \text{if }x\ne0,\\
0& \text{if }x=0.
\end{cases}
$$

Show that the Taylor series of $f$ at $0$ is identically zero.
:::
