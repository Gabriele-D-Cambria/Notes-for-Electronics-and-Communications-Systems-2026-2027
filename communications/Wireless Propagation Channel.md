---
title: Wireless Propagation Channel
---

# 1. Index

<!--toc:start-->

- [1. Index](#1-index)
- [2. Wireless Propagation Channel](#2-wireless-propagation-channel)
  - [2.1. Space-Wave Phenomena](#21-space-wave-phenomena)
    - [2.1.1. Large-Scale Fading](#211-large-scale-fading)
      - [2.1.1.1. Path Loss](#2111-path-loss)
      - [2.1.1.2. Shadowing](#2112-shadowing)
    - [2.1.2. Small-Scale Fading: Multipath Propagation](#212-small-scale-fading-multipath-propagation)
      - [2.1.2.1. Reyleigh Fading](#2121-reyleigh-fading)
      - [2.1.2.2. Time Dispersion](#2122-time-dispersion)
    - [2.1.3. Coherence Bandwidth](#213-coherence-bandwidth)
      - [2.1.3.1. Example: two-ray channel mode](#2131-example-two-ray-channel-mode)
    - [2.1.4. Inter Symbol Interference](#214-inter-symbol-interference)
    - [2.1.5. BER Benchmark](#215-ber-benchmark)
      - [2.1.5.1. Additive White Gaussian Noise Channel](#2151-additive-white-gaussian-noise-channel)
      - [2.1.5.2. Flat Fading Channel](#2152-flat-fading-channel)
      - [2.1.5.3. Multipath Fading Channel](#2153-multipath-fading-channel)
    - [2.1.6. Time Varying Channel](#216-time-varying-channel)
      - [2.1.6.1. Doppler Shift](#2161-doppler-shift)
      - [2.1.6.2. Channel's Coherence Time](#2162-channels-coherence-time)
    - [2.1.7. Clarke Jakes Doppler Spectrum](#217-clarke-jakes-doppler-spectrum)
      - [2.1.7.1. Spectral Broadening of a Sinusoidal Tone](#2171-spectral-broadening-of-a-sinusoidal-tone)
    - [2.1.8. Doppler Spectrum](#218-doppler-spectrum)
      - [2.1.8.1. Fading Channel Example](#2181-fading-channel-example)
  - [2.2. Small Scale Fading Recap](#22-small-scale-fading-recap)
  <!--toc:end-->

# 2. Wireless Propagation Channel

Radio signals do not propagate in the same way at all frequencies.

Depending on frequency and environment, different propagation mechanisms become
dominant.

<div class="grid2">
<div class="">

We classify the propagation mechanisms into three categories:

- **Ground-wave propagation**: at low frequencies, radio waves can propagate close
  to the Earth's surface, partially following its curvature. This mechanism
  is particularly relevant in the LF $(<300kHz)$ and MF $(300kHz-3MHz)$ bands and
  can provide coverage **beyond the geometric horizon**.
- **Sky-wave propagation**: At HF $(3MHz-30MHz)$ frequencies, the ionosphere
  can **refract radio waves back** toward the Earth. Multiple ionospheric
  returns may allow communication over distances of hundreds or thousands of kilometres.
- **Space-wave propagation**: From VHF upward $(>30MHz)$, propagation is
  mainly associated with direct and reflected waves travelling through the lower
  atmosphere. Communication is therefore primarily limited by geometry,
  obstacles, and the surrounding propagation environment.

</div>
<div class="">
<img class="40" src="./images/wpc/propagation_mechanisms.png"
    alt="Propagation Mechanisms">
</div>
</div>

Most modern wireless systems operate from `VHF` to _millimeter-wave
frequencies_, where space-wave propagation is dominant.

In this regime, the received signal is typically the combination of:

- **Direct path**
- **Multiple interactions due to environmental objects**

These propagation effects can cause signal variations over _different spatial scales_
leading to the distinction between _large-scale_ and _small-scale_ fading.

> ![note]
> When defining _large_, _comparable_ and _small_ we are always referring to spatial
> dimensions relative to the carrier wavelength $\lambda$.

## 2.1. Space-Wave Phenomena

When dealing with _space-wave propagation_, several phenomena can be observed:
_transmission_, _reflection_, _refraction_, _diffraction_, _absorption_ and
_scattering_.

For our purpose, we will account just for:

- **Reflection**: occurs when a wave encounters a large surface, producing
  reflected and transmitted components.
- **Diffraction**: occurs around sharp edges or obstacles, allowing the wave
  to reach regions beyond direct line of sight.
- **Scattering**: occurs on small objects or rough surfaces, redistributing
  the wave energy in several directions.

<img class="" src="./images/wpc/space_wave_phenomena.png"
    alt="Space-Wave Phenomena">

The received signal is the combination of the multiple propagation paths, meaning
that it will suffer from both **large-scale fading** (distance larger than the wavelength)
and **small-scale fading** (distances in order of the wavelength).

The first are typically due to **path loss** and **shadowing**, while the
second are due to **multipath propagation** (and **Doppler effects** when
there is mobility).

### 2.1.1. Large-Scale Fading

Large-scale fading describes variations of the local average received power
over distances **much larger than the wavelength**.

It is mainly modeled trough **path loss** and **shadowing**.

To understand it, we shall first understand what happens to signal when it propagates
in free space from a transmitter.

Supposing that the transmitter is isotropic, and radiates the power $P_t$
uniformly in all directions of a spherical surface.

Hence, the power flux density at a distance $d$ from the transmitter is given by:

$$
S(d) = \frac{P_tG_t}{4\pi d^2} \quad [W/m^2]
$$

Where $G_t$ is the gain of the transmitting antenna.

A receiving antenna will collect power proportional to its **effective aperture**:

$$
A = \frac{G_r\lambda^2}{4\pi}
$$

Thus, the received power at a distance $d$ will be:

$$
P_r = S(d) A = P_t G_t G_r \cdot \frac{\lambda^2}{16\pi^2 d^2}
$$

This means that, in free space, the received powers decreases as $\frac{1}{d^2}$.

For example, supposing we have $\lambda = 0.1m (3GHz)$ , $G_t = G_r = 1$:

- $d = 1m \to 0.006\%\to-42dB$
- $d = 10m \to 0.00006\%\to-62dB$

#### 2.1.1.1. Path Loss

We can then introduce **path-loss** as a measure of _the average propagation
attenuation for a Tx-Rx distance_ $d$:

$$
P_r(d) = \frac{P_tG_tG_r}{L(d)} \quad [W]
$$

In $dB$ this relation cha be expressed as:

$$
  L_{dB}(d) = P_{t,dB} + G_{t,dB} + G_{r,dB} - P_{r,dB}(d)
$$

By this definition, in free-space the path-loss is:

$$
\begin{CD}
{
  P_r = P_t G_t G_r \left(\frac{\lambda}{4\pi d}\right)^2
}
@>>>
{
  L(d) = \left(\frac{4\pi d}{\lambda}\right)^2
}
\end{CD}
$$

In a generic propagation environment, we can average the received power and the
path-loss over a distance $d$ as:

$$
\begin{align*}
  P_r(d) &\approx P_r(d_0) \cdot \left(\frac{d_0}{d}\right)^{n} \\
  L(d) &\approx L(d_0) \cdot \left(\frac{d}{d_0}\right)^{n}
\end{align*}
$$

Where $d_0$ is the **reference distance** obtained from either the _free-space_
equations or from _measurements_.

Instead, $n$ is the **path-loss exponent**, which depends on the environment. Some
typical values are:

<div class="flexbox" markdown="1">

|           Environment            | Path Loss Exponent<br>$n$ |
| :------------------------------: | :-----------------------: |
|            Free space            |             2             |
|    Urban area cellular radio     |         2.7 – 3.5         |
| Urban area cellular (obstructed) |            3–5            |
|    In-building line-of-sight     |         1.6 – 1.8         |
|      Obstructed in-building      |            4–6            |
|     Obstructed in-factories      |            2–3            |

</div>

For fixed antenna gains, the free-space path loss increases with the
**square of the carrier frequency**:

$$
L_{FS,dB}(f_c, d) = 20 \cdot \log_{10}{\left(\frac{4\pi d f_c}{c}\right)}
\quad [dB]
$$

Atmospheric gases can introduce an additional frequency-selective attenuation
$\gamma(f_c) [dB/km]$, which becomes particularly relevant at millimetre-wave
frequencies:

$$
L_{atm,dB}(f_c, d) = \gamma(f_c) \cdot d \quad [dB]
$$

The total path loss for free-space propagation with atmospheric absorption is then:

$$
L_{tot,dB}(f_c, d) = L_{FS,dB}(f_c, d) + L_{atm,dB}(f_c, d) \quad [dB]
$$

<img class="" src="./images/wpc/atmospheric_path_loss.png"
    alt="Atmospheric Path Loss">

When projecting cell size for a wireless system, this means that
at its edges the path-loss may exceed $100dB$.

For fixed antenna gains and link budget, higher carrier frequencies result in
higher free-space path loss and smaller coverage radius, meaning that:

- **Low-frequency** bands are better suited for _wide-area_ coverage.
- **Higher-frequency** bands are commonly used in _smaller cells_ to provide
  high capacity.

<img class="30" src="./images/wpc/path_loss_frequency.png"
    alt="Path Loss vs Frequency">

#### 2.1.1.2. Shadowing

Two points at the same Tx-Rx distance $d$ have the same _distance-dependent path-loss_.
However, they might experience _different local average received powers_.

This happens because of **Shadowing**, which accounts for random variations
of the local average received power around the _distance-dependent path-loss model_.

In $dB$, the shadowing $A_S$ is commonly modeled as a **zero-mean Gaussian
Random Variable**:

$$
p(A_S) = \frac{1}{\sqrt{2\pi}\sigma_S} \cdot e^{-\frac{A_S^2}{2\sigma_S^2}}
$$

Where $\sigma_S$ is the _standard deviation in decibels_ (typically a few $dB$).

Equivalently, the shadowing factor is **log-normally distributed** in linear scale.

This means, that the received power at a distance $d$ can be expressed as:

$$
P_{r,dB}(d) = \overline{P}_{r,dB}(d) + X_S(d) + X_F(d)
$$

<figure class="75">
<img class="60" src="./images/wpc/received-power-variation-over-distance.png"
    alt="Received Power Variation over Distance">
<figcaption>

We will see _Small-Scale Fading_ $X_F(d)$ in the next section.

</figcaption>
</figure>

### 2.1.2. Small-Scale Fading: Multipath Propagation

> [!IMPORTANT]
> From now on, all signals and channel responses are represented
> in equivalent complex baseband, unless otherwise stated.

In the equivalent complex-baseband representation, multiple replicas of the
transmitted signal reach the receiver through different propagation paths, with
each replica experiencing a different _attenuation_ $\alpha_l$, _phase shift_ $\phi_l$
and _propagation delay_ $\tau_l$.

<img class="" src="./images/wpc/multipath_propagation.png"
    alt="Multipath Propagation">

Hence, the overall signal $y(t)$:

$$
y(t) = \sum_{l=0}^{L-1} {\alpha_l \cdot e^{j\phi_l} \cdot s(t-\tau_l)}
$$

For a static, or quasi-static, propagation environment, the equivalent
complex-baseband channel can be modeled as an **LTI filter**, knowing
that $y(t) = s(t) \otimes h(t)$:

$$
h(\tau) = \sum_{l=0}^{L-1} {\alpha_l \cdot e^{j\phi_l} \cdot \delta(\tau-\tau_l)}
$$

We have made some assumption in this model, for example that the channel is
**linear** and **time-invariant**, other that **noiseless**.

A _resolvable multipath component_ represents a group of propagation
contributions with **very similar delays**, observed by the receiver as a **single
delay component**.

The complex gain $h$ of one such component results from the _superposition of
many scattered contributions_ with different random phases:

$$
h = \sum_{m=0}^{M-1} {\alpha_m \cdot e^{j\theta_m}}
$$

When many independent scattered contribution add and _no dominant contribution
is present_, the in-phase and quadrature components of $h$ can be modeled as
a _zero-mean Gaussian random variable_, following the **Central Limit Theorem**:

$$
h = h_I + jh_Q \qquad h_I, h_Q \sim \mathcal{N}(0, \sigma_h^2)
$$

Meaning that we can model the sum of all the scattered contributions as a
**complex Gaussian random variable**:

$$
  h \cdot \delta(t-\tau)
$$

#### 2.1.2.1. Reyleigh Fading

When no dominant propagation path is present, the received signal envelope
$r = \vert h \vert$ follows a **Rayleigh distribution**:

$$
f_R(r) = \frac{r}{\sigma_h^2} \cdot e^{-\frac{r^2}{2\sigma_h^2}}, \quad r \ge 0
$$

The channel phase $\phase{h}$ is **uniformly distributed** in $[0, 2\pi)$.

<figure class="75">
<img class="50" src="./images/wpc/rayleigh_fading.png"
    alt="Rayleigh Fading">
<figcaption>

Having a Rayleigh-distributed envelope means that when received signal amplitude
is near zero, we have a non-negligible probability of deep fades, which can cause
**severe signal degradation**.

</figcaption>
</figure>

The channel power gain $g = \vert h \vert^2 = r^2$ is instead **exponentially
distributed**:

$$
f_G(g) = \frac{1}{2\sigma_h^2} \cdot e^{-\frac{g}{2\sigma_h^2}}, \quad g \ge 0
$$

<img class="40" src="./images/wpc/rayleigh_fading_power.png"
    alt="Rayleigh Fading Power">

The consideration in the image above can be proved:

$$
\begin{align*}
  E\Set{r^2} &= E\Set{\vert h \vert ^2} \\
    &= E\Set{h_I^2 + h_Q^2} \\
    &= E\Set{h_I^2} + E\Set{h_Q^2} \\
    &= \sigma_h^2 + \sigma_h^2 \\
    &= 2\sigma_h^2
\end{align*}
$$

#### 2.1.2.2. Time Dispersion

Different resolvable multipath components arrive at the receiver
**with different delays**, spreading the channel response over time.

The **Power Delay Profile (PDP)** describes how the average received power is
distributed over the propagation delays $\tau_l$:

$$
  P(\tau_l) = E\Set{\vert h_{tau_l} \vert^2}
$$

Thus, the channel's time dispersion will be characterized by the **Root Mean
Square (RMS) Delay Spread** $\sigma_\tau$. (i.e., the standard deviation of
the PDP)

<img class="" src="./images/wpc/power_delay_profile.png"
    alt="Power Delay Profile">

For a discrete PDP, delay statistics are computed by _weighting each resolvable
component_ by its average power.

Defining $P_l = P(\tau_l)$, the normalized weights can be expressed as:

$$
p_l = \frac{P_l}{\sum_{k} P_k} \qquad \sum_{l} p_l = 1
$$

This allows us to compute the **mean delay** $\overline{\tau}$ and the
**RMS delay spread** $\sigma_\tau$, which measures the power-weighted dispersion
of the multipath delays around the mean delay:

$$
\begin{align*}
  \overline{\tau} &= \sum_{l} p_l \cdot \tau_l \\
  \sigma_\tau &= \sqrt{\sum_{l} p_l \cdot (\tau_l - \overline{\tau})^2}
\end{align*}
$$

### 2.1.3. Coherence Bandwidth

In the frequency domain, convolution with the multipath channel
becomes multiplication:

$$
  Y(f) = S(f)H(f)
$$

Where $Y(f)$ is the frequency response of the received signal, $S(f)$ the one
of the transmitted signal, and $H(f)$ the channel frequency response.

If we had a single path component, the channel frequency response would be **flat**:

$$
h(t) = \alpha \cdot e^{j\phi} \cdot \delta(t-\tau) \quad \Leftrightarrow \quad
H(f) = \alpha \cdot e^{j\phi} \cdot e^{-j2\pi f\tau} \quad \rightarrow \quad
\vert H(f) \vert = \alpha
$$

But since we now take into consideration to have multiple resolvable multipath
components, the channel frequency response must take into account all of them:

$$
H(f) = \sum_{l=0}^{L-1} {\alpha_l \cdot e^{j\phi_l} \cdot e^{-j2\pi f\tau_l}}
$$

Thus, different propagation delays will produce different _frequency-dependent
phase shifts_, whose superposition makes both the magnitude and phase of
$H(f)$ vary with frequency.

We define the **coherence bandwidth** $B_c$ the frequency interval
over which _the channel frequency response **remains strongly correlated**_.

$$
B_c \approx \frac{1}{5\sigma_\tau} \quad [Hz]
$$

> [!WARNING]
> Note that this relation is an **approximation**. The numerical factor
> depends on the _correlation criterion_.

If our signal has a bandwidth $B$:

- $B \ll B_c$: the channel is **frequency-flat**. This means that all frequency
  components of the signal experience the same channel gain and phase shift.
- $B \ge B_c$: the channel is **frequency-selective**. This means that different
  frequency components of the signal experience different channel gains and phase
  shifts.

<img class="40" src="./images/wpc/coherence_bandwidth.png"
    alt="Coherence Bandwidth">

We can also see this relation in the _time domain_, taking in relevance the delay
spread $\sigma_\tau$ of the channel impulse response and the overall transmission
duration $T$:

$$
\begin{align*}
  \sigma_\tau \ll T &\Leftrightarrow B_c\gg B && \text{(frequency-flat)} \\
  \sigma_\tau \ge T &\Leftrightarrow B_c\le B && \text{(frequency-selective)}
\end{align*}
$$

The RMS delay spread depends strongly on the propagation environment and geometry.

A delay spread of $5\;\mu s$ corresponds to the coherence bandwidth of $B_c
\approx 40\;kHz$.

Typical approximated values of RMS delay spread and coherence bandwidth for
different propagation environments are the following:

<div class="flexbox" markdown="1">

|     Environment     | Typical $\sigma_\tau$ $\mu s$ | Approx. $B_c$ $MHz$ |
| :-----------------: | :---------------------------: | :-----------------: |
| **Rural/open area** |         $0.05 - 0.3$          |      $0.7 - 4$      |
|  **Indoor office**  |         $0.01 - 0.1$          |       $2- 20$       |
| **Urban Microcell** |           $0.1 - 1$           |      $0.2 - 2$      |
| **Urban Macrocell** |            $1 - 5$            |    $0.04 - 0.2$     |

</div>

#### 2.1.3.1. Example: two-ray channel mode

<div class="grid2">
<div class="">

Suppose we have a channel impulse response:

$$
h(t) = \alpha_0 \cdot e^{j\phi_0} \cdot \delta(t-\tau_0) +
\alpha_1 \cdot e^{j\phi_1} \cdot \delta(t-\tau_1) \qquad \tau_1 > \tau_0
$$

Choosing the first arrival as the delay and phase reference, we can set:

$$
\begin{cases}
  \tau_0 = 0 & \phi_0 = 0 \\
  \tau_1 = \tau & \phi_1 = \phi
\end{cases}
$$

To isolate the effect of the _relative delay_, we can also set the phase
shift of the second path to zero: $\phi = 0$.

</div>
<div class="">
<img class="80" src="./images/wpc/two_ray_channel_model.png"
    alt="Two-Ray Channel Model">
</div>
</div>

This means that the channel impulse response can be expressed as:

$$
h(t) = \alpha_0 \cdot \delta(t) + \alpha_1 \cdot \delta(t-\tau) \quad
\Leftrightarrow \quad H(f) = \alpha_0 + \alpha_1 \cdot e^{-j2\pi f\tau}
$$

Having $alpha_0 = 1$ and $alpha_1 = 0.9$, we can calculate the mean value
of the delay spread:

$$
\begin{align*}
  \overline{\tau} &= \sum_{l=0}^1{p_l \cdot \tau_l} \\
    &= p_0 \cdot \tau_0 + p_1 \cdot \tau_1 \\
    &= p_0 \cdot \tau_0 + (1 - p_0) \cdot \tau_1 \\
    &= \frac{1}{1+0.9^2} \cdot 0 + (\frac{0.9^2}{1+0.9^2}) \cdot \tau \\
    &= 0.45 \cdot \tau  = p_1 \tau
\end{align*}
$$

As for the RMS delay spread, we can calculate it as:

$$
\begin{align*}
  \sigma_\tau &= \sqrt{\sum_{l=0}^1{p_l \cdot (\tau_l - \overline{\tau})^2}} \\
    &= \sqrt{p_0 \cdot (\tau_0 - \overline{\tau})^2 + p_1 \cdot (\tau_1 -
\overline{\tau})^2} \\
    &= \sqrt{p_0 \cdot (0 - p_1\tau)^2 + p_1 \cdot (\tau - p_1\tau)^2} \\
    &= \sqrt{p_0 \cdot (p_1\tau)^2 + p_1 \cdot (p_1 \tau)^2} \\
    &= \tau \cdot \sqrt{p_0 p_1^2 + p_1 p_0^2} \\
    &= \tau \cdot \sqrt{p_0 p_1 (p_1 + p_0)} \\
    &= \tau \cdot \sqrt{p_0 p_1} \\
    &= 0.5 \cdot \tau
\end{align*}
$$

We can then calculate the relative coherence bandwidth for
several values of $\tau$:

<div class="flexbox" markdown="1">

| Relative Delay |    Norm. Delay Spread    | Norm. Coherence Bandwidth  |
| :------------: | :----------------------: | :------------------------: |
|  $\tau=0.1T$   | ${\sigma_\tau/ T}= 0.05$ |  ${B_c\cdot T}\approx 4$   |
|    $\tau=T$    | ${\sigma_\tau/ T}= 0.5$  | ${B_c\cdot T}\approx 0.4$  |
|  $\tau=1.5T$   | ${\sigma_\tau/ T}= 0.75$ | ${B_c\cdot T}\approx 0.27$ |
|   $\tau=4T$    |  ${\sigma_\tau/ T}= 2$   | ${B_c\cdot T}\approx 0.1$  |

</div>

### 2.1.4. Inter Symbol Interference

As we saw when we were talking about the _baseband transmission model_,
when sending a signal, after transmit filtering, propagation and receive filtering,
the effective pulse shape $g(t)$ is:

$$
g(t) = g_t(t) \otimes h(t) \otimes g_r(t) = g_N(t) \otimes h(t)
$$

Neglecting the effect of noise, the receive signal can then be expressed as:

$$
x(t) = \sum_i {c_i \cdot g(t-iT)}
$$

Meaning that the received signal is a **superposition of delayed and scaled
transmitted pulses**.

If we sample at $t = mT$, the decision variable $x[m]$ is:

$$
\begin{align*}
  x[m] &= \sum_i {c_i \cdot g(mT-iT)} \\
    &= c_m \cdot g(0) + \sum_{i\neq m} {c_i \cdot g((m-i)T)} \\
    &= c_m + \sum_{i\neq m} {c_i \cdot g((m-i)T)}
\end{align*}
$$

We call the second term **Inter-Symbol Interference (ISI)**, which is the
interference due to other symbols in the same transmission.

In **multipath channels** the zero-ISI condition of the Nyquist criterion
is **_generally not satisfied_**:

$$
g(kT) = \sum_{l=0}^{L-1} {\alpha_l \cdot e^{j\phi_l} \cdot g_N(kT-\tau_l)} \neq
0
$$

This is because the interference is not due to the transmission of other
signal, but to the multipath propagation of the same signal.

### 2.1.5. BER Benchmark

#### 2.1.5.1. Additive White Gaussian Noise Channel

With a AWGN channel, the decision variable
is:

$$
x[m] = c_m + n[m]
$$

If _no fading_ and _no ISI_ are present, the BER (Bit Error Rate) is a function
of the SNR (Signal-to-Noise Ratio), as we saw before in the _baseband transmission
model_.

With a **constant channel gain** $\vert h[m] \vert| = 1$ we do not
impact the SNR.

<img class="40" src="./images/wpc/ber_awgn.png"
    alt="BER vs SNR for AWGN Channel">

#### 2.1.5.2. Flat Fading Channel

In a **flat fading channel**, assuming coherent detection and phase
compensation, the decision variable is:

$$
x[m] = \alpha c_m + n[m]
$$

The channel amplitude $\alpha$ is a **Rayleigh distributed RV**, normalized
such that $E\Set{\alpha^2} = 1$.

The mean error probability is thus obtained by averaging the error probability over
the channel:

$$
\begin{align*}
  P_e &= \int_0^{\infty} P(e \vert \alpha)\cdot p(\alpha) d\alpha \\
      &= \int_0^{\infty} Q\left(\frac{\alpha}{\sigma}\right) \cdot p(\alpha) d\alpha
\end{align*}
$$

This type of channel does not introduce ISI, but **random instantaneous SNR** due
to the fading amplitude $\alpha$.

<img class="40" src="./images/wpc/ber_flat_fading.png"
    alt="BER vs SNR for Flat Fading Channel">

Assuming a coherent detection and phase compensation, in a 2-PAM the decision
variable is:

$$
x = \alpha a + n
$$

The PDF of $\alpha$ is:

$$
p(\alpha) = \frac{\alpha}{\sigma^2} \cdot e^{-\frac{\alpha^2}{2\sigma^2}},
\quad \alpha \ge 0
$$

If we normalize such that $\sigma^2 = \frac{1}{2}$, we have:

$$
E\Set{\alpha^2} = 2\sigma^2 = 1 \qquad \Rightarrow \quad
p(\alpha) = 2\alpha \cdot e^{-\alpha^2}, \quad \alpha \ge 0
$$

This means that, for a given value of $\alpha$, the conditional
error probability for a 2-PAM is:

$$
P(e\vert \alpha) = Q\left(\alpha\sqrt{\frac{2E_s}{N_0}}\right)
$$

Hence, the error probability:

$$
\begin{align*}
  P_e &= \int_0^{\infty} P(e \vert \alpha)\cdot p(\alpha) d\alpha \\
      &= \int_0^{\infty} Q\left(\alpha\sqrt{\frac{2E_s}{N_0}}\right) \cdot
        2\alpha e^{-\alpha^2} d\alpha
\end{align*}
$$

Since $2\alpha e^{-\alpha^2} = -\frac{d}{d\alpha} e^{-\alpha^2}$, we can integrate
by parts.

We first have to compute the derivative of the Q-function:

$$
\begin{align*}
  Q(x) &= \frac{1}{\sqrt{2\pi}} \int_x^{\infty} e^{-\frac{t^2}{2}} \\
       &\Downarrow \\
  \frac{d}{dx} Q(x) &= -\frac{1}{\sqrt{2\pi}} e^{-\frac{x^2}{2}}
\end{align*}
$$

Solving the integral, and replacing $\mu = \sqrt{\frac{2E_s}{N_0}}$, we have:

$$
\begin{align*}
  P(e) &= -e^{-\alpha^2} Q(\mu \alpha) \Big\vert_0^{\infty} -
\int_0^{\infty} {\left(-e^{-\alpha^2}\right) \cdot \left(-\frac{\mu}{\sqrt{2\pi}}
e^{-\frac{\mu^2 \alpha^2}{2}}\right) d\alpha} \\
  &= \left(0 + \frac{1}{2}\right) - \frac{\mu}{\sqrt{2\pi}}
\int_0^{\infty} {e^{-\frac{\alpha^2(2 + \mu^2)}{2}} d\alpha} \\
\end{align*}
$$

Substituting $\beta = \alpha \sqrt{\left(2 + \mu^2\right)} \to d\alpha =
\frac{1}{\sqrt{\left(2 + \mu^2\right)}} d\beta$, we have:

$$
\begin{align*}
  P(e) &= \frac{1}{2} - \frac{\mu}{\sqrt{2\pi(2 + \mu^2)}} \frac{1}{\sqrt{2\pi}}
\int_0^{\infty} {e^{-\frac{\beta^2}{2}} d\beta} \\
  &= \frac{1}{2} - \frac{\mu}{\sqrt{2 + \mu^2}} \cdot \frac{1}{\sqrt{2\pi}}
\frac{\sqrt{2\pi}}{2} \\
  &= \frac{1}{2} \left(1 - \frac{\mu}{\sqrt{2 + \mu^2}}\right) \\
  &= \frac{1}{2} \left(1 - \sqrt{\frac{E_s/N_0}{1 + E_s/N_0}}\right)
\end{align*}
$$

For large $x$ we can use the approximation $\sqrt{\frac{x}{1+x}} \approx
1 - \frac{1}{2x}$, which means that for **large SNR values** the probability
of error can be approximated as:

$$
\begin{align*}
  P(e) &\approx\frac{1}{2}\left(1 - \left(1 - \frac{1}{2E_s/N_0}\right)\right) \\
       &= \frac{1}{4E_s/N_0} \\[2em]
  (P(e))_{dB} &= 10 \cdot \log_{10}{\left(\frac{1}{4E_s/N_0}\right)} \\
    &= -10 \cdot \log_{10}{\left(E_s/N_0\right)} - 10 \cdot \log_{10}{4} \\
    &= -10 \cdot \log_{10}{\left(4E_s/N_0\right)} \\
    &\approx -\log_{10}{\left(E_s/N_0\right)}
\end{align*}
$$

Meaning that the error probability decreases **inversely proportional to the SNR**
(in dB scale).

#### 2.1.5.3. Multipath Fading Channel

With a frequency selective channel, the decision variable is:

$$
x[m] = g(0)c_m + \sum_{i\neq m} {g((m-i)T)c_i} + n[m]
$$

If we do not take any countermeasures, the ISI term will dominate the error
probability, producing an **irreducible error floor**.

<img class="40" src="./images/wpc/ber_multipath_fading.png"
    alt="BER vs SNR for Multipath Fading Channel">

### 2.1.6. Time Varying Channel

If the transmitter, receiver or surrounding scatterers are moving, the propagation
paths change with time, making the channel **time varying**.

Large-scale fading is typically assumed constant over the observation interval,
thus its omitted.

Over a sufficiently short observation interval, the path delays can be regarded
as **approximately constant**, while the complex path gains vary with time:

$$
h(t, \tau) = \sum_{l=0}^{L-1} {\alpha_l(t) \cdot e^{j\phi_l(t)}
\cdot \delta(\tau-\tau_l)}
$$

Thus, the channel output observed at time $t$ is:

$$
y(t) = \int{h(t,\tau)s(t-\tau)\;d\tau} =
  \sum_{l=0}^{L-1} \alpha_l(t) e^{k\phi_l(t)}s(t-\tau_l)
$$

A simple example of a setup where path varies is when the source or
the environment is moving, for example like when an ambulance going past us.

#### 2.1.6.1. Doppler Shift

<div class="grid2">
<div class="">

Knowing the distance between the two observer as $d$, and the speed
of the ambulance as $v$, the time it takes for it to go from the
first observer to the second is $t = {d \over v}$.

At time $t_0$ the ambulance is at the position of the first
observer, who immediately hears the siren. However, the second
observer will only hear the sound at $t_0 + \frac{d}{v_s}$,
where $v_s = 343\;m/s \approx 1200\;km/h$ is the _speed of sound_.

At $t_0 + t$ the ambulance will be at the position of the second
observer, which will immediately hear the sound.
The first one will hear it after it has traveled back to him, at
$t_0 + t + \frac{d}{v_s}$.

This means that the duration of the siren will be:

- $t - \frac{d}{v_s}$ for the second observer
- $t + \frac{d}{v_s}$ for the first observer
- $t$ for the driver of the ambulance

This means that, in perspective to the driver:

- The first observer will experiment a **lower observed frequency**
- The second observer will experiment an **higher observed frequency**

</div>
<div class="">
<img class="80" src="./images/wpc/ambulance-doppler-effect.png"
      alt="Ambilance Doppler Effect">
</div>
</div>

In wireless propagation, this effect is called **Doppler Shift**.

Considering a receiver moving away from a fixed transmitter of
a sinusoidal signal $s(t) = \sin(2\pi f_t)$ at constant radial
velocity $v$.

After $t$ seconds, the receiver will be at distance $d(t) = vt$ from
the transmitter, corresponding to a propagation delay of:

$$
\tau(t) = \frac{vt}{c}
$$

Considering the propagation delay $\tau(t)$, the received signal will be:

$$
\begin{align*}
  y(t) &= \sin(2\pi f_c (t - \tau(t))) \\
       &= \sin(2\pi (f_ct - f_ct \frac{v}{c})) \\
       &= \sin(2\pi t (f_c - f_c \frac{v}{c})) \\
       &= \sin(2\pi (f_c + f_D)t) \\
       &= \sin(2\pi f_r t) \\
\end{align*}
$$

Where $f_D$ is the **Doppler Shift**, and can be expressed:

$$
  f_D = -\frac{f_c v}{c} = - \frac{v}{\lambda}
$$

> [!NOTE]
> The convention is that:
>
> - If the receiver is **moving away**, the **phase accumulates more slowly**, hence
>   the _Doppler Shift_ is positive.
> - If the receiver is **moving towards**, the **phase accumulates more rapidly**,
>   hence the _Doppler Shift_ is negative.

However, we have seen that a signal does not arrive from one single direction,
but from a spread, each shifted of $\theta_l$.

Each of this component will experience _doppler shift_:

$$
f_{D,l} = -\frac{v}{\lambda} \cos{(\theta_l)} \le f_{D,max} = \frac{v}{\lambda}
$$

A resolvable multipath component may contain many scattered contributions
with _similar delays_ but difference angles of arrival.

The mobility itself produces a range of Doppler Shifts, which spectrum is called
**Doppler Spread**:

- **Maximum Negative Shift**: $\theta_l = 0 \quad \Rightarrow f_{D,l} = -\frac{v}{\lambda}$
- **No Doppler Shift**: $\theta_l = \frac{\pi}{2} \quad \Rightarrow f_{D,l} = 0$
- **Maximum Positive Shift**: $\theta_l = \pi \quad \Rightarrow f_{D,l} = \frac{v}{\lambda}$

#### 2.1.6.2. Channel's Coherence Time

The superposition of many scattered contributions, each with a different
Doppler shift, makes the complex gain **vary randomly with time**.

In particular, we can model each channel gain as a **stochastic process**,
where the doppler shift appears as a _time-varying phase rotation_:

$$
h_l(t) = \sum_{m} a_{l,m} e^{j(2\pi f_{D,l,m}t + \phi_{l,m})}
$$

Over a suitable observation interval, the fading process
can be modeled as **Wide-Sense-Stationary (WSS)**.

Therefore, **its autocorrelation depends _only on the time difference_**
$\Delta t$.

Under two-dimensional isotropic scattering, the _Clarke-Jakes model_ gives:

$$
R_h(\Delta t) = \underbrace{J_0\left(2\pi f_{D, max} \Delta t\right)}_
\text{Bessel Functions}
$$

The **channel's Coherence Time** $T_c$ is the time interval over which
the channel _remains significantly_ correlated and can be regarded
as **approximately constant**:

$$
T_c \approx \frac{1}{2f_D} = \frac{c}{2f_c v} = \frac{\lambda}{2v}
$$

Faster motion, or higher carrier frequency result in shorter coherence time.

In terms of distance, the **coherence distance** is:

$$
d_c = vT_c \approx v\frac{c}{2f_c v} = \frac{\lambda}{2}
$$

> [!WARNING]
> Both equations provided for $T_c$ and $d_c$ are **_APPROXIMATIONS_**.

### 2.1.7. Clarke Jakes Doppler Spectrum

<div class="grid2">
<div class="">

For a WSS fading process, the Doppler power spectral density
is the _Fourier Transform of the temporal autocorrelation_:

$$
\begin{align*}
  S_h(f) &= \int{R_h(\Delta t) e^{-j2\pi f \Delta t}\;d\Delta t} \\
  &= \begin{cases}
    \frac{1}{\pi f_{D,max}} \left(\sqrt{1 - \left(\frac{f}{f_{D,max}}\right)^2}
    \right)^{-1} & \vert f \vert \le f_{D,max} \\[4em]
    0 & \vert f \vert > f_{D,max}
\end{cases}
\end{align*}
$$

Meaning that, as we can see on the right, the fading spectrum is
confined to: $f \in [-f_{D,max}, f_{D,max}]$.

Therefore, mobility spreads the channel energy over a range of
Doppler frequencies.

</div>
<div class="">
<img class="60" src="./images/wpc/clarke-jackes-doppler-spectrum.png"
      alt="Clarke Jackes Doppler Spectrum">
</div>
</div>

#### 2.1.7.1. Spectral Broadening of a Sinusoidal Tone

Considering a complex sinusoidal tone $s(t) = e^{2\pi f_0 t}$.

After propagation through a **flat time-varying channel**:

$$
y(t) = h(t)st) = h(t)e^{2j\pi f_0 t}
$$

Its _Power Spectral Density_ of this signal will then be $S_y(f) = S_h(f - f_0)$

<figure class="90">
<img class="50" src="./images/wpc/spectral-broad-sinus-tone.png"
      alt="Spectral Broadening of a Sinusoidal Tone">
<figcaption>

Since the Doppler spectrum is confined to $\vert f \vert \le f_{D,max}$ the
convolution spreads the transmitted spectrum by up to $f_{D,max}$ on each
side.

</figcaption>
</figure>

If the transmitted signal occupied a frequency interval $f \in [f_1, f_2]$ (i.e.,
has a band of $B = f_2 - f_1$), the received spectrum further extends:

$$
  f_1 - f_{D,max} \le f \le f_2 + f_{D, max} \qquad \Rightarrow \qquad
  B_y = B + 2f_{D, max}
$$

Thus, the effect of mobility is a **broadening of the signal spectrum**.

In a frequency-selective time-varying channel, each resolvable delay
component has its own time-varying complex gain:

$$
h(t, \tau) = \sum_{l=0}^{L-1}{H_l(t) \delta(\tau - \tau_l)}
$$

Where $h_l(t) = \alpha_l(t) e^{j\phi_l(t)}$.

Over a sufficiently short observation interval, the delays
$\tau_l$ are approximately constant, while the tap coefficients
$h_l(t)$ vary with time.

Under WSS isotropic-scatternig assumptions, each tap undergoes
_Clarke-Jakes fading_, so the overall channel combines both **Doppler
Broadening** with **Multipath Frequency Selectivity**.

### 2.1.8. Doppler Spectrum

Doppler spread is a measure of the **spectral broadening** caused by
motion.

If the _baseband signal bandwidth_ $B_s \gg f_{D,max}$ the channel **varies
slowly** compared with the signal time scale. We refer it as **slow fading**.

On the other hand, if $B_s \le f_{D,max}$ the channel **may vary significantly**
over the signal time scale. We refer to is as **fast fading**.

Severe fast fading may make reliable reception impossible without sufficiently
fast channel estimation and tracking.

Similar considerations can be made in terms of symbol duration

- A channel is **slow fading** if $T_c \gg T$
- A channel is **fast fading** if $T_c \le T$

#### 2.1.8.1. Fading Channel Example

Consider a transmission at $f_c = 2.1\;GHz$ in a suburban area with RMS
delay spread $\sigma = 2\;\mu s$ to a user moving at a speed $v = 90\;km/h
= 25\;m/s$.
The signal bandwidth is $B = 2\;MHz$, meaning that symbol time can be
approximated as $T \sim 1/B_s = 500\;ns$.

The maximum Doppler shift is:

$$
  f_{D,max} = \frac{f_c v}{c} = \dots = 175\;Hz
$$

Meaning that the coherence time will be approximately:

$$
  T_c \approx \frac{1}{2f_{D,max}} = 3\;ms
$$

The channel coherence bandwidth id $B_c \approx \frac{1}{5\sigma_\tau} = 100\;kHz$.

We can say that:

- The channel is slow $(B \gg f_{D,max} \Leftrightarrow T \ll T_c)$
- It is **frequently selective** $(B \ge B_c \Leftrightarrow T \le \sigma_\tau)$

## 2.2. Small Scale Fading Recap

<img class="" src="./images/wpc/small-scale-fading-recap.png"
      alt="Small Scale Fading Recap">
