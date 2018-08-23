#Chapter 3: Modulation synthesis
In this Chapter we will explain some classic modulation techniques.
Modulation is the alteration of the amplitude, frequency and /or phase of an oscillator in accordance with another signal.
The original oscillator is called the carrier signal, while the modulating signal is called the modulator signal.
In this Chapter we will examine three forms of modulations: ring modulation (RM), amplitude modulation (AM) and frequency modulation (FM).

##Ring modulation
A ring modulation is simply the multiplication of two signals.
Ring modulation has been extensively used by the german composer Stockhausen. It has also been used in the implementation of the first movie completely designed with electronic sounds: Forbidden Planet (1956). Ring modulation has also been used extensively in video games. As an example, the sound chip of Commodore 64 had ring modulation implemented, which was used in sound effects for several games.

A block diagram of a ring modulator is shown in Figure XXX.

In order to understand what happens to the spectrum of a sound when we multiply to signals, let’s consider the simple case of multiplying two cosinewaves, as shown in Equation 1. In Equation 1, $\omega_1$ and $\omega_2$ represent the frequencies of two sinewaves.

$$y(t) = \cos(\omega_1 t) \cos (\omega_2 t)$$ 

Using trigonometric identities, the following equation holds:
$$y(t) = \cos(\omega_1 t) \cos (\omega_2 t) = \frac {1}{2}[ \cos((\omega_1-\omega_1)  t)) +  \cos((\omega_1+ \omega_1)  t))$$ 

As an example, if we consider two cosinewaves whose frequencies are $\omega_1 = 300$ Hz and $\omega_2 = 100$ Hz respectively, multiplying them provides the sum of two sinewaves whose frequencies are 200 Hz and 400 Hz respectively, and whose amplitude is the half of the original amplitude.

#### Tremolo effect

Tremolo is defined as the variation of the amplitude of a signal. A tremolo effect can be obtained with a ring modulator, but having the frequency of the modulator signal below 20 Hz.

##Amplitude modulation
In the ring modulation equation the frequency of the carrier signal is not present anymore in the resulting sound. In order to avoid this, the ring modulation equation can be modified. Amplitude modulation is mathematically expressed as:
$$y(t) = \cos(\omega_1 t) (\cos (\omega_2 t)+1)$$ 
Using again trigonometric identities, the result of such multiplication becomes:
$$y(t) =  \frac {1}{2}[ \cos((\omega_1-\omega_1)  t)) +  \cos((\omega_1+ \omega_1)  t)) + \cos(\omega_1 t) $$ 

##Frequency modulation
Frequency modulation (FM) is a synthesis technique invented by John Chowning at Stanford University in California.

TODO: Question: how do we put references? --!>
References
[1]   J. Chowning. The synthesis of complex audio spectra by means of frequency modulation. Journal of the Audio Engineering Society, 21(7):526–534, 1973.


The FM patent was licenced from Stanford University to Yamaha, and allowed the creation of the DX 7, the most successful synthesizer in history.
FM synthesis was the most popular synthesis techniques in the 80s, especially for its ability to create several complex sounds with very little computational power.

The main idea behind frequency modulation is the fact that the frequency of a carrier oscillator can be modulated by another oscillator. Mathematically speaking, given two cosine waves:

$$
y_1(t) = a_1 \cos(\omega_1 t)
y_2(t) = a_2 \cos(\omega_2 t)
$$

FM can be expressed as:

$$y = \cos((\omega_1 + a_2 cos (\omega_2 t)) t)
$$

This equation therefore represents a cosine wave whose frequency is modulated by another cosine wave. The result is a very complex spectrum able to generate not only few sidebands like amplitude and ring modulation, but an infinite number of sidebands.

More precisely, John Chowning discovered that with frequency modulation it is possible to produce sidebands given by the following equation:
$$
\omega_s = \omega_c \pm n \omega_n
$$
where $n$ is an integer, $\omega_c$ is the carrier's frequency and $\omega_m$ is the modulation frequency.

Equation 2 simply expresses the fact that each sideband lies at a frequency which is equal to the carrier frequency plus or minus an integer multiple of the modulator frequency.

In theory, since $n$ can take any integer value, applying frequency modulation produces an infinite series of sidebands. In practice this is not the case, as we will see later.

The calculation of the amplitude of sidebands is a quite complex task. We need to introduce a new term named $\beta$, known as the modulation index. In the frequency modulation equation, $\beta$ represents $a_2$, the amplitude of the modulator. $\beta$ is defined as:
$$
\beta = \frac {\Delta \omega_c}{\omega_n}
$$

where $\Delta \omega_c$
is the variation of the carrier frequency (the amount of variations of the carrier frequency from its unmodulated frequency).

###Bandwidth

The bandwidth of a signal can be defined as the range of frequencies occupied by a given signal. For example, a signal composed by two sinusoids, one at 100 Hz and one at 200 Hz has a bandwidth of 100 Hz. On the other end, a signal composed by the sum of two sinusoids, one at frequency 100 Hz and another at frequency 400 Hz has a bandwidth of 300 Hz.
Exercise: Two cosine waves at frequencies 300 Hz and 500 Hz are ring modulated. What is the bandwidth of the resulting signal?
For frequency modulation, there is a rule of thumb which states the following:
$$ B = 2 \omega_m 1 + \beta $$

### The C:M ratio
The C:M ratio expresses the relative frequencies of carrier and modulator signal. An an example, if the carrier frequency is 200 Hz and the modulator frequency is 100 Hz, then the C:M ratio is 2.

In frequency modulation, for any given carrier frequency the frequencies of the upper sidebands lie at C+M, C+2M, C+3M,..and so on, while the lower sidebands lie at C-M,C-2M, and so on.

Lets consider the example in which C:M = 1:1.
Let’s assume that both carrier and modulator frequency are placed at 100 Hz. This means that the upper sidebands will be at 200 Hz, 300 Hz, 400 Hz, and so on, while the lower sidebands will be at 0 Hz, -100 Hz,...and so on.
What does the concept of a negative frequency mean? A negative frequency is equivalent to its corresponding positive frequency, but remapped to the positive axes. When summing the same negative and positive frequency, if they have the same amplitude they will cancel each others.

In FM synthesis the amplitude of the sidebands is not the same, so the issue of cancellation of sidebands does not occur. In the case of FM synthesis, the calculation of the amplitude of each sideband requires the use of Bessel functions.



##Musical examples

To be written.

