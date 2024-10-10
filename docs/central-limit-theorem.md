---
jupytext:
    formats: md:myst
    text_representation:
        extension: .md
        format_name: myst
kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

# Central limit theorem

Recommended reference: {cite:t}`wasserman`, Sections 5.3–5.4.

The *central limit theorem* is a very important result in probability
theory.  It tell us that when we have $n$ independent and identically
distributed random variables, the distribution of their average (up to
suitable shifting and rescaling) approximates a normal distribution as
$n\to\infty$.

## Sample mean

Consider a sequence of independent random variables $X_1, X_2, \ldots$
distributed according to the same distribution function (which can be
discrete or continuous).  We say $X_1, X_2, \ldots$ are *independent
and identically distributed* (or *i.i.d.*).

:::{note}
We saw the notion of independence of *two* random variables in
{numref}`def-indep`.  For a precise definition of independence of an
arbitrary number of random variables, see Section 2.9 in
{cite:t}`wasserman`.
:::

:::{prf:definition}
:label: def-sample-mean

The $n$-th *sample mean* of $X_1,X_2,\ldots$ is

$$
\overline{X}_n = \frac{1}{n}\sum_{i=1}^n X_i.
$$
:::

Note that $\overline{X}_n$ is itself again a random variable.

## The law of large numbers

As above, consider i.i.d. samples $X_1, X_2, \ldots$, say with
distribution function $f$.  We assume that $f$ has finite mean $\mu$.
The *law of large numbers* says that the $n$-th sample average is
likely to be close to $\mu$ for sufficiently large $n$.

:::{prf:theorem} Law of large numbers
For every $\epsilon>0$, the probability

$$
P(|\overline{X}_n-\mu|>\epsilon)
$$

tends to $0$ as $n\to\infty$.
:::

:::{note}
The above formulation is known as the *weak law of large numbers*;
there are also stronger versions, but the differences are not
important here.
:::

{numref}`lln` illustrates the law of large numbers for the average of
the first $n$ out of 1000 dice rolls.

```{code-cell} ipython3
:tags: [hide-input, remove-output]

import numpy as np
from matplotlib import pyplot
from myst_nb import glue

rng = np.random.default_rng()
N = 1000
n = np.arange(1, N + 1, 1)
X = rng.integers(1, 7, N)
Xbar = np.cumsum(X) / n

fig, ax = pyplot.subplots()
ax.plot((0, 1000), (3.5, 3.5))
ax.plot(n, Xbar)

ax.set_xlabel('$n$')
ax.set_ylabel('$\\overline{X}_n$')

glue("lln", fig)
```

(lln)=
:::{glue:figure} lln
Illustration of the law of large numbers: average of $n$ dice rolls as
$n\to\infty$.
:::

:::{warning}
If the mean of the distribution function $f$ does not exist, then the
law of large numbers is meaningless.  This is illustrated for the
Cauchy distribution (see {prf:ref}`cauchy-distribution`) in
{numref}`lln-cauchy`.
:::

```{code-cell} ipython3
:tags: [hide-input, remove-output]

import numpy as np
from matplotlib import pyplot
from myst_nb import glue

rng = np.random.default_rng(seed=37)
N = 1000
n = np.arange(1, N + 1, 1)
X = rng.standard_cauchy(N)
Xbar = np.cumsum(X) / n

fig, ax = pyplot.subplots()
ax.plot((0, 1000), (0, 0))
ax.plot(n, Xbar)

ax.set_xlabel('$n$')
ax.set_ylabel('$\\overline{X}_n$')

glue("lln-cauchy", fig)
```

(lln-cauchy)=
:::{glue:figure} lln-cauchy
Failure of the law of large numbers: average of $n$ samples from the
Cauchy distribution as $n\to\infty$.
:::

(central-limit-theorem)=
## The central limit theorem

Again, we consider a sequence of i.i.d. samples $X_1, X_2, \ldots$
with distribution function $f$.  We assume $f$ has finite mean $\mu$
and variance $\sigma^2$.

By the law of large numbers, the shifted sample mean

$$
\overline{X}_n-\mu = \frac{1}{n}(X_1+\cdots+X_n)-\mu
$$

has expectation 0.

:::{prf:theorem} Central limit theorem
The distribution of the sequence of random variables

$$
Y_n = \frac{\sqrt{n}}{\sigma}(\overline{X}_n-\mu)
$$

converges to the standard normal distribution as $n\to\infty$.
:::

Roughly speaking, we can write this as

$$
\overline{X}_n \approx \mu + \frac{\sigma}{\sqrt{n}}Z
$$

where $Z$ is a random variable following a standard normal
distribution, or as

$$
\overline{X}_n \longrightarrow
\mathcal{N}\biggl(\mu,\frac{\sigma}{\sqrt{n}}\biggr)
\quad\text{as }n\to\infty,
$$

where $\mathcal{N}(\mu,\sigma)$ denotes the normal distribution with
mean $\mu$ and standard deviation $\sigma$.

```{code-cell} ipython3
:tags: [hide-input]

# Adapted from
# https://furnstahl.github.io/Physics-8820/notebooks/Basics/visualization_of_CLT.html
# by Dick Furnstahl (license: CC BY-NC 4.0)

from math import comb, factorial
from matplotlib import pyplot
from myst_nb import glue
import numpy as np
import plotly.graph_objects as go

mu = 0.5
sigma = 1 / np.sqrt(12)

def bates_pdf(x, n):
    # https://en.wikipedia.org/wiki/Bates_distribution
    return (n / (2 * factorial(n - 1)) *
            sum((-1)**k * comb(n, k) * (n*x - k)**(n-1) * np.sign(n*x - k) for k in range(n + 1)))

def normal_pdf(x, mu, sigma):
    return (1/(np.sqrt(2*np.pi) * sigma) *
            np.exp(-((x - mu)/sigma)**2 / 2))

# Create figure
fig = go.Figure()

# Add traces, one for each slider step
for step in np.arange(1, 21, 1):
    sigma_tilde = sigma / np.sqrt(step)
    x_min = mu - 4*sigma_tilde
    x_max = mu + 4*sigma_tilde
    x_pts = np.linspace(x_min, x_max, 200)

    # Add normal distribution at current slider step
    fig.add_trace(
        go.Scatter(
            visible=False, # Set to false by default to prevent all of them appearing on top of each other before the slider is moved
            line=dict(color="green", width=5),
            x=x_pts,
            y=normal_pdf(x_pts, mu, sigma_tilde),
            showlegend=False
        )
    )

    # Add bates distribution at current slider step
    fig.add_trace(
        go.Scatter(
            visible=False, # Set to false by default to prevent all of them appearing on top of each other before the slider is moved
            line=dict(color="red", width=5),
            x=x_pts,
            y=bates_pdf(x_pts, step),
            showlegend=False
        )
    )

# We disabled the visibility of every trace above so they don't show up all at the same time.
# Now we pick which ones we want to make visible by default, i.e. the default position of the slider.
# We added two traces per slider step, so fig.data[0] and fig.data[1] both belong to the first step,
# i.e. normal and Bates distribution at the first step will be visible by default
fig.data[0].visible = True
fig.data[1].visible = True

# For n slider steps we have 2n elements in fig.data because there is a normal and Bates
# distribution for each one. So we move in increments of 2.
steps = []
for step in range(0, len(fig.data), 2):
    step_dict = dict(
        method="update",
        args=[{"visible": [False] * len(fig.data)}],
        label=((step // 2) + 1)
    )
    step_dict["args"][0]["visible"][step:step+2] = [True, True]  # Show normal and Bates for this step
    steps.append(step_dict)

sliders = [dict(
    active=0,
    currentvalue={"prefix": "n = "},
    pad={"t": 50},
    steps=steps
)]

fig.update_layout(
    sliders=sliders,
    xaxis_title="x",
    yaxis_title="f(x)",
)

fig

glue("clt", fig)
```

:::{glue:figure} clt
Illustration of the central limit theorem for an average of $n$
uniformly random samples
:::

```{code-cell} ipython3
:tags: [hide-input, remove-output]

from math import comb, factorial
import numpy as np

from matplotlib import pyplot
from myst_nb import glue

p = 0.7

# Mean and standard deviation of our distribution
mu = p
sigma = np.sqrt(p*(1 - p))

def binomial_pmf(k, n):
    return comb(n, k) * p**k * (1 - p)**(n - k)

def normal_pdf(x, mu, sigma):
    return (1/(np.sqrt(2*np.pi) * sigma) *
            np.exp(-((x - mu)/sigma)**2 / 2))

def plot_ax(ax, n):
    """
    Plot a histogram on axis ax that shows the distribution of the
    n-th sample mean.  Add the limiting normal distribution.
    """
    sigma_tilde = sigma / np.sqrt(n)
    x_min = mu - 4*sigma_tilde
    x_max = mu + 4*sigma_tilde

    ax.set_title('$n={}$'.format(n))
    ax.set_xlabel('$x$')
    ax.set_ylabel('$f(x)$')
    ax.set_xlim(x_min, x_max)

    # plot a normal pdf with the same mu and sigma
    # divided by the sqrt of the sample size.
    x_pts = np.linspace(x_min, x_max, 200)
    y_pts = normal_pdf(x_pts, mu, sigma_tilde)
    ax.plot(x_pts, y_pts, color='gray')
    for k in range(n + 1):
        ax.add_line(pyplot.Line2D((k/n, k/n), (0, n * binomial_pmf(k, n)),
                    linewidth=2))

sample_sizes = [1, 2, 4, 8, 16, 32]

# Plot a series of graphs to show the approach to a Gaussian.
fig, axes = pyplot.subplots(3, 2, figsize=(8, 8))

for ax, n in zip(axes.flatten(), sample_sizes):
    plot_ax(ax, n)

fig.tight_layout(w_pad=1.8)

glue("clt-bern", fig)
```

:::{glue:figure} clt-bern
Illustration of the central limit theorem for an average of $n$
Bernoulli random samples (probability mass function scaled by a factor
$n$ for comparison with the probability density function of the normal
distribution)
:::

The law of large numbers and *a fortiori* the central limit theorem
are ‘well-known’ results, but their applicability relies on properties
of random variables that are not necessarily satisfied in all cases.
We already saw an example where the law of large numbers fails in
{numref}`lln-cauchy`.  Likewise, one needs to be careful when applying
the central limit theorem.  Mathematical proofs help to clarify under
what conditions these results hold and why.  Perhaps surprisingly, a
way to prove the law of large numbers and the central limit theorem is
through Fourier analysis, via the *characteristic function* introduced
in {numref}`moments-characteristic-function`.  We will not go into the
details here.

***

## Exercises

:::{exercise}
Suppose a certain species of tree has average height of 20 metres with
a standard deviation of 3 metres.  A dendrologist measures 100 of
these trees and determines their average height.  What will the mean
and standard deviation of this average height be?
:::

:::{exercise}
Suppose $X$ and $Y$ are two independent continuous random variables which follow the standard normal distribution $\mathcal{N}(0,1)$. In whatever programming language you like, compute and plot a histogram of $n$ samples from $X+Y$ for $n = 100,1000,$ and $10000$. What can you conclude about the random variable $X+Y$? Repeat the same exercise, but this time with $X/Y$. What can you conclude about the random variable $X/Y$? Think about the central limit theorem as you explain your reasoning.
:::
