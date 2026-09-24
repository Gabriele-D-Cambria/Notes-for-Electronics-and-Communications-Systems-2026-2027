---
title: Analog Communications
---

# 1. Index

- [1. Index](#1-index)
- [2. Analog Communications](#2-analog-communications)
  - [2.1. Dual Side-Band Modulation - `DSB`](#21-dual-side-band-modulation---dsb)
  - [2.2. Quadrature Amplitude Modulation - `QAM`](#22-quadrature-amplitude-modulation---qam)
  - [2.3. Passband Signals](#23-passband-signals)
    - [2.3.1. Carrier Mismatch in Complex Baseband representation](#231-carrier-mismatch-in-complex-baseband-representation)
  - [2.4. Angle Modulation](#24-angle-modulation)
    - [2.4.1. Frequency Modulation - `FM`](#241-frequency-modulation---fm)

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

## 2.1. Dual Side-Band Modulation - `DSB`

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

## 2.2. Quadrature Amplitude Modulation - `QAM`

Several amplitude-modulation schemes can be obtained by modifying how the
carrier and the side-bands are transmitted.

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
  sub-carriers at different frequencies.

<div class="grid2">
<div class="">

Using _sine_ and _cosine_ we can transmit two orthogonal carriers in the same frequency:

$$
s_{QM}(t) = A_c m_I(t) \cos{(2\pi f_c t)} - A_c m_Q(t) \sin{(2\pi f_c t)}
$$

At the receiver, coherent mixing with the same carriers shifts the desired
components to baseband, while the cross-component and the remaining
high-frequency terms are centered around $2f_C$, and can be filtered out by
Low-Pass Filters (`LPF`):

$$
\begin{matrix}
    \hat{m}_I(t) = A_c m_I(t) & & \hat{m}_Q(t) = A_c m_Q(t)
\end{matrix}
$$

</div>
<div class="">
<img class="80" src="./images/analog-comms/qam-trans-recieve.png">
</div>
</div>

Therefore, the quadrature-modulated signal is described by two real baseband components
that can be combined into one complex signal:

$$
\tilde{s}_{QM}(t) = A_c m_I(t) + jA_c m_Q(t)
$$

The physical **passband** waveform is recovered as just:

$$
  s_{QM}(t) = \text{Re}\Set{\tilde{s}(t)e^{j2\pi f_c t}}
$$

## 2.3. Passband Signals

The majority of communication systems are _passband systems_.

A passband signal $s(t)$ has its spectrum concentrated around a
non-zero carrier frequency $f_c$, and, in most communication systems, these
signals are also _narrowband_ $(f_c \gg B)$.

Any passband signal $s(t)$ can be represented as:

$$
\begin{align*}
s(t) &= \text{Re}\Set{\tilde{s}(t)e^{j2\pi f_ct}} \\
     &= s_I(t) \cos{(2\pi f_ct)} - s_Q(t) \sin{(2\pi f_ct)}
\end{align*}
$$

Where $\tilde{s}(t) = s_I(t) + js_Q(t)$ is called **Complex Envelope** of the
signal $s(t)$, where $s_I(t) = \text{Re}\Set{\tilde{s}(t)}$ and
$s_Q(t) = \text{Im}\Set{\tilde{s}(t)}$.

The complex envelop is an equivalent baseband representation of a passband signal.

Employing the baseband equivalent has several benefits:

- **Easier to study**: it removes the effects of the carrier frequency from
  the signal model.
- **Better Numerically simulated**: since the bandwidth is lower, also the
  sampling rate, which must be at least _twice the bandwidth_, is lower,
  lowering the computation complexity
- **Basis for Digital Implementation for Passband Communication Systems**.

In the following image we can see a comparison between the **Passband Model**
and the **Equivalent Baseband Model**.

<img class="50" src="./images/analog-comms/passband-vs-eq-baseband-model.png">

### 2.3.1. Carrier Mismatch in Complex Baseband representation

In a practical receiver, the phase $\phi_{LO}$ and the frequency $f_{LO}$ of
the local oscillator **may not be exactly equal** to the phase $\phi_c$ and
frequency $f_c$ of the received carrier:

$$
\begin{matrix}
  \Delta f = f_c - f_{LO} & & \Delta \phi = \phi_c - \phi_{LO}
\end{matrix}
$$

Therefore, the received complex envelope takes this form:

$$
\tilde{v}_{off}(t)  = \tilde{v}(t)e^{j(2\pi \Delta ft + \Delta \phi)}
$$

In complex baseband, a phase offset causes a _fixed rotation_, whereas a
frequency offset causes a _progressive rotation_.

## 2.4. Angle Modulation

We said that a passband signal can be represented as:

$$
  s(t) = A(t)\cos{(2\pi f_c t + \phi_m(t))}
$$

Accordingly, we can convey different informations by varying the **amplitude**
$A(t)$ and/or the information-bearing phase $\phi_m(t)$.

Since the instantaneous frequency is the _time derivative of the total phase_,
we can write:

$$
\begin{align*}
  f_i(t) &= \frac{1}{2\pi} \frac{d}{dt} (2\pi f_c t + \phi_m(t)) \\
         &= f_c + \frac{1}{2\pi} \frac{d}{dt} \phi_m(t) \\
\end{align*}
$$

This highlight the fact that changing the phase **also produces a time-varying
instantaneous frequency**.

More in general we will see that _angle modulation_ includes _phase
modulation_ and _frequency modulation_.

### 2.4.1. Frequency Modulation - `FM`

In _frequency modulation_, the _instantaneous frequency deviation_ is linearly
proportional to the message:

$$
\Delta f_i(t) = f_i(t) - f_c = \frac{1}{2\pi}\frac{d}{dt} \phi_m(t) = k_fm(t)
$$

Since $\frac{d \phi_m(t)}{dt} = 2\pi k_f m(t)$, it follows that the
information-bearing phase term is **the integral of the message**:

$$
  \phi_m(t) = 2\pi \int_{-\infty}^t{k_f m(\tau)\;d\tau}
$$

All said, the passband `FM` modulated signal is:

$$
  s_{FM}(t) = A_c \cos{\Biggl(2\pi f_c t + 2\pi k_f \int_{-\infty}^t{m(\tau)\;d\tau}\Biggr)}
$$

The complex envelope of a `FM` signal is:

$$
  \tilde{s}_{FM}(t) = A_c e^{j2\pi k_f \int_{-\infty}^t{m(\tau);d\tau}} = A_c e^{j\phi_m(t)}
$$

Since it is true that $\vert \tilde{s}_{FM}(t) | = A_c$, we can say that
**Frequency Modulation is a _constant-envelope_ modulation**.

The maximum frequency deviation is then defined as:

$$
\begin{align*}
\Delta f &= \max_t \{\vert \Delta f_i (t) \vert \} \\
         &= k_f \max_t \{\vert \Delta f_i (t) \vert \}
\end{align*}
$$
