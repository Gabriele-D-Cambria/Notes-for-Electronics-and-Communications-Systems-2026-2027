---
title: Digital Communications
---

# 1. Index

<!--toc:start-->

- [1. Index](#1-index)
- [2. Digital Communications](#2-digital-communications)
  - [2.1. Pulse Amplitude Modulation (PAM)](#21-pulse-amplitude-modulation-pam)
    - [2.1.1. Symbol Mapping](#211-symbol-mapping)
    - [2.1.2. Power Spectral Density](#212-power-spectral-density)
    <!--toc:end-->

# 2. Digital Communications

The passband signal in a communication system takes the following form:

$$
s(t) = Re\Set{\tilde{s}(t)e^{j2\pi f_ct}} =
  s_I \cos{(2\pi f_c t)} - s_Q \sin{(2\pi f_c t)}
$$

In _analog modulation_, the complex envelope $\tilde{s}(t)$ is determined by the
continuous-valued message signal $m(t)$.

Instead, in _digital modulation_ it is generate from a **discrete sequence of
information symbols**.

## 2.1. Pulse Amplitude Modulation (PAM)

The **Pulse Amplitude Modulation** (`PAM`) is obtained by:

1. Mapping the bists $d_k$ to **information symbols** $a_i$
1. Generating a continuous-tie waveform by _pulse shaping_ the symbol
   sequence with a _transmit filter_ of _impulsive response_ $g_T(t)$.

<img class="" src="./images/digital-comms/pam-scheme.png" alt="PAM scheme">

Since each information symbol represents $q$ bits, the relationship between
_bit-duration_ $T_b$ and _symbol-interval_ $T$ is:

$$
  T = q \cdot T_b
$$

The signal $\tilde{s}_{PAM}(t)$ is a **real baseband signal** that has the same
modulation structure as an _analog DSB signal_.
Its modulating signal is the _pulse-shaped symbol sequence_:

$$
\tilde{s}_{PAM}(t) = \sum_m{a_m \cdot g_T(t - mT)}
$$

The baseband PAM signal can be later translated to any carrier frequency just like
any other analog signal:

$$
s_{PAM}(t) = Re\Set{\tilde{s}_{PAM}(t)e^{j2\pi f_ct}} =
  \tilde{s}_{PAM}(t) \cos{(2\pi f_c t)}
$$

From now on, unless otherwise stated, all signal are represented by their _complex
baseband equivalent_, simplifying the notation as to:

$$
s_{PAM}(t) := \tilde{s}_{PAM}(t) = \sum_m{a_m \cdot g_T(t - mT)}
$$

### 2.1.1. Symbol Mapping

The **mapper** is a device that associates a sequence of $q$ bits $d_k$ to a single
symbol $a_m \in A \subset \R$.

We call the collections of all the possible $M$ symbols a **Constellation**.

Using _fixed-length binary mapping_, the constellation contains $M = 2^q$ symbols.

Usually, we use constellations that are _symmetric to $0$_. This way if we have
equi-probable symbols, the expectation will be $E \Set{a_m} = 0$.

Using larger constellations means representing more bits per symbol, therefore
providing _higher spectral efficiency_.

However, for a fixed minimum distance between adjacent symbols, increasing $M$ increases
the _average symbol energy_, meaning we will need **more band**.

Given that the bit duration is $T_b$, the rate will be $R_B = \frac{1}{T_b}$.

In an interval $T$, if each symbol represents $q$ bits:

$$
  T = q T_b = T_b \cdot \log_2{M}
$$

Accordingly, the symbol rate $R_s$ will be:

$$
  R_s = \frac{1}{T} = \frac{1}{qT_b} = \frac{R_b}{q} = \frac{R_b}{\log_2{M}}
$$

To optimize also error recovery, we can introduce **Gray Mapping**,
constellations that yield two properties:

1. They are symmetric to $0$
2. Adjacent symbol map sequence of bits different for **exactly one bit**.

<figure class="">
<img class="100" src="./images/digital-comms/gray-map-example.png"
      alt="Gray Mapping Examples">
<figcaption>

Some example of **Gray Mapping**

</figcaption>
</figure>

### 2.1.2. Power Spectral Density

The symbol sequence $\Set{a_m}$ is modelled as a **stationary discrete-time**,
**discrete-valued** random process with independent symbols.

Since the symbols are aligned with a _periodic time grid_, the resulting
_continuous-time_ PAM signal is **cyclostationary** with period $T$.

The _time-averaged_ PSD of its complex envelope $s(t)$ is:

$$
  S_s(f) = \frac{1}{T} S_a(f) \cdot \vert G_T(f) \vert^2
$$

Where $S_a(f)$ is the PSD of $a_m$ and $G_T(f)$ is the **frequency response** of
the filter $g_T(t)$.
