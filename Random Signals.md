---
title: Random Signals
---

# 1. Index

<!--toc:start-->

- [1. Index](#1-index)
- [2. Random Signals](#2-random-signals)
  - [2.1. First Order Distribution](#21-first-order-distribution)
  - [2.2. Finite Dimensional Distribution](#22-finite-dimensional-distribution)
  - [2.3. Mean and Autocorrelation](#23-mean-and-autocorrelation)
    - [2.3.1. Stationarity](#231-stationarity)
  - [2.4. Power Spectral Density](#24-power-spectral-density)
  - [2.5. Second-Order Cyclostationarity](#25-second-order-cyclostationarity)
  <!--toc:end-->

# 2. Random Signals

As long as our information-bearing waveform are affected by _noise_, _interference_,
_fading_, and other random phenomena, we must treat them as _random signals_.

A **Stochastic Process** $X(t)$ is a _family of random variables_ indexed by time.

Starting from the process we get:

- A **Realization** $X(t, \xi_0)$ for a fixed outcome $\xi_0$
- A **Random Variable** $X(t_0, \xi)$ for a fixed time $t_0$

Stochastic processes can be classified according to two different aspects:

- **Parameter space**: we call them _discrete_ or _continuous_ depending on
  whether the index set is discrete $n \in \Z$ or continuous $t \in \R$.
- **State space**: we call them _discrete_ or _continuous_ depending on
  whether the state space is discrete $x \in \Z$ or continuous $x \in \R$.

In digital communication we often deal between _discrete-time models_ and
_continuous-time models_.

## 2.1. First Order Distribution

For a _real-valued_ stochastic process $X(t)$, fixing $t = t_0$ will produce
the random variable.

We call a **Fisrt Order Cumulative Distribution Function** (**CDF**) the
function $F_{X(t_0)}(x)$ defined as the \_probability that $X(t)$ does not exceed
$x$:

$$
\large
F_{X(t_0)}(x) = P\Set{X(t_0) \le x}
$$

When the CDF is differentiable, we define the **Fisrt Order Probability
Density Function** (**PDF**) as:

$$
\large
f_{X(t_0)}(x) = \frac{d}{dx} F_{X(t_0)}(x)
$$

## 2.2. Finite Dimensional Distribution

A process is fully characterized by **joint distributions** of samples taken
at any finite set of times.

The simplest example is when we work with _Complex RVs_, which characteristics are
dependent to the joint statistics of both its _real_ and _imaginary_ parts.

The _joint_ distribution is defined as follows:

$$
F_{X(t_1),\dots,X(t_n)}(x_1, \dots, x_n) = P\Set{X(t_1)\le x_1, \dots, X(t_n)
\le x_n}
$$

The different RVs can be **Independent** by one another, meaning that the behaviour
of one does not influence the others.
Under this hypothesis, the joint distribution can be factorized into the **product
of the marginal distribution**:

$$
\begin{matrix}
  F_{X(t_1),\dots,X(t_n)}(x_1, \dots, x_n) = \prod_i{F_{X(t_i)}(x_i)} \\
  f_{X(t_1),\dots,X(t_n)}(x_1, \dots, x_n) = \prod_i{f_{X(t_i)}(x_i)}
\end{matrix}
$$

## 2.3. Mean and Autocorrelation

When dealing with RVs there two useful and interesting properties we can deal with
are the **mean** and the **autocorrelation**.

The **Mean** is the average value of the process at time $t$. It is exactly the
_expectation_:

$$
\mu_X(t) = E\Set{X(t)} = \int_{-\infty}^{+\infty}{x \cdot f_{X(t)}(x)\;dx}
$$

The **autocorrelation** is a measure of the **statistical similarity** of two samples
of the same process, meaning how the signal changes in time.

$$
R_{XX}(t_1, t_2) = E\Set{X(t_2)X^\ast(t_1)} = \iint_{-\infty}^{+\infty}
  {x_1^\ast \cdot x_2 \cdot f_{X(t_1)X(t_2)}(x_1,x_2)\;dx_1d_x2}
$$

### 2.3.1. Stationarity

We can model our RVs by how it changes in time:

- **Strict Stationarity**: All its distributions are **time-invariant** to a common
  time shift.
- **First-Order Stationarity**: the one-time distribution, a.k.a. the _mean
  value_, is _time-invariant_. &emsp; $\mu_X(t) = \mu \rArr f_X(x)
  \not{\propto} t$
- **Second-Order Stationarity**: the joint distribution of _any pair of
  samples_ is _time-invariant_. &emsp; $f_{X(t_1), X(t_2)}(x_1,x_2) = g(x_1,
  x_2, t_2-t_1)$

The _strict stationarity_ is difficult to evaluate, so we introduce the **Wide-Sense
Stationarity** (**WSS**) when _second-order_ summaries are _time-invariant_:

$$
\begin{align*}
E\Set{X(t)} &= \mu_X \\
R_{XX} &= E\Set{X(t_2)X^\ast(t_1)} = R_{XX}(t_2-t_1)
\end{align*}
$$

In particular we can define $\tau = t_2 - t_1$, then we can rewrite the autocorrelation
as:

$$
R_{XX}(t, t+\tau) = R_{XX}(\tau) = E\Set{X(t + \tau), X^\ast(\tau)}
$$

## 2.4. Power Spectral Density

For _zero-mean WSS processes_ fast temporal variations generally imply a **short
correlation time**, whereas slowly varying processes retain correlation over **longer
lags**.

This means that the faster a process varies, the faster its autocorrelation goes
to zero, and vice-versa.

If the autocorrelation is a deterministic value bond to how fast a RV changes,
we can use **Fourier Transform** to describe how _power_ is distributed over
frequency, getting **Power Spectral Density** (**PSD**) (_Wiener-Khinchin_
theorem):

$$
S_{XX}(f) = \int{R_{XX}(\tau)e^{-j2\pi f\tau}\;d\tau}
$$

Power and Power Density are related:

$$
P_X = R_{XX}(0) = \int{S_{XX}(f)\;df}
$$

## 2.5. Second-Order Cyclostationarity

Some communication signals are not WSS, because their statistics vary
**periodically** with the absolute time.

A process is _second-order cyclostationary_ with period $T_0$ if:

$$
\begin{align*}
  \mu_X(t+t_T) = \mu_X(t) \\
  R_{XX}(t+T_0, \tau) = R_{XX}(t, \tau) \\
\end{align*}
$$

This time dependence can be removed by _averaging over one period_.

$$
\begin{align*}
  \overline{R}_{XX}(\tau) &= \frac{1}{T_0}\int_0^{T_0}{R_{XX}(t, \tau)\;dt} \\
  \overline{S}_{XX}(\tau) &= \frac{1}{T_0}\int{\overline{R}_{XX}(\tau) \cdot
    e^{-j2\pi f\tau}\;d\tau} \\
\end{align*}
$$

This type of process are important for us because both `PAM` and `QAM`
waveforms are generally second-order cyclostationary with period $T$.
