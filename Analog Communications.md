---
title: Analog Communications
---

## 1. Index

# 2. Analog Communications

Information signals, such as speech and audio, are typically generated at
_baseband_, which is a non-zero signal centered at $f = 0$:

$$
  m(t) \Leftrightarrow M(f), \quad \text{concentrated around } f = 0
$$

Thus, in order to avoid noise between different signals, wireless transmission
requires each signal to be moved to an appropriate radio-frequency band.
This process is called **modulation**, and it allows us to:

- Translate baseband information to a suitable radio-frequency band;

- Enable efficient radiation and reception with practical antennas;

- Allow multiple services and users to share the radio spectrum.

## Dual SideBand Modulation - `DSB`

It's the simplest form of analog modulation.

It can translate a baseband message $m(t)$ to a radio-frequency band by
**multiplying it by a sinusoidal carrier**:

$$
s_{DSB}(t) = A_cm(t)\cdot \cos{(2\pi f_c t)}
$$

The $m(t)$ controls the amplitude and sign of the carrier.

Multiplication by a sinusoidal carrier creates two shifted copies of the
baseband spectrum around $f_c$ and $-f_c$.

This can be proved by analyzing the Fourier's transformations theorems and definitions:

$$
\begin{matrix}
  \cos{(2\pi f_c t)} \Leftrightarrow \frac{1}{2}[\delta(f - f_c) +
    \delta(f + f_c)] \\
  S_{DSB}(f) = \frac{A}{2}[M(f - f_c) + M(f + f_c)]
\end{matrix}
$$

Neglecting the effect related to noise and propagation channels, the recovery
of $m(t)$ from $s_{DSB}(t)$ (or _demodulation_) is possible with **coherent detection**.
The receiver has to multiply the signal by a locally generated carrier with
_the same frequency and phase_ of the transmitter's carrier:

$$
  u(t) = 2s_{DSB}\cos{(2\pi f_c t)} = A_c m(t) + A_c m(t) \cos{(4\pi f_c t)}
$$

To recover the original signal $m(t)$ we just have to filter with a
_Band-Pass-Filter_ (`BPF`) the higher
frequencies from $u(t)$.

## Quadrature Amplitude Modulation - `QAM`

Several amplitude-modulation schemes can be obtained by modifying how the
carrier and the sidebands are transmitted.

Rather than reviewing all analog amplitude-modulation schemes, on this course
we focus on **Quadrature Modulation**.

This type of modulation allows two independent messages to share the same
frequency band without interfering by **modulating two orthogonal carriers**.

The use of this technique naturally introduces the in-phase $I(t)$ and
quadrature $Q(t)$ components, using the same principles that digital QAM and many
modern communication system use.

Two signals $x_1(t)$ and $x_2(t)$ are _orthogonal_ over an interval $T$ if:

$$
\int_{t_0}^{t_0 + T}{x_1(t) x_2^\ast(t)\;dt} = 0
$$

The simplest example of two orthogonal signals over any integer number $k$ of
periods $(T = kT_c = \frac{k}{f_c})$
that carry the same frequency are _sine_ and _cosine_ with a phase difference
of 90°:

$$
  \int_{t_0}^{t_0 + T}{\cos{(2\pi f_c t)}\sin{(2\pi f_c t)}\;dt} = 0
$$

Since the signal are orthogonal, they can _overlap in time and frequency_ and
still be separated by an appropriate receiver:

- **Quadrature Modulation**: uses the orthogonality between sine and cosine.

- **Orthogonal Frequency Dual Modulation**: uses the orthogonality between
  subcarriers at different frequencies.

Using _sine_ and _cosine_ we can transmit two orthogonal carriers in the same frequency:

$$
s_{QM}(t) = A_c m_I(t) \cos{(2\pi f_c t)} - A_c m_Q(t) \sin{(2\pi f_c t)}
$$
