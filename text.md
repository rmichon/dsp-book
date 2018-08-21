# Chapter 1: Introduction
Digital sound processing is the discipline where digital signals are manipulated through different operations. 
All these operations will be explained and implemented using Faust.

##What is sound?
Sound is a form of energy, created when air molecules propagate in patterns called waves. Such waves of rapidly varying pressure are caused by vibrating objects. As an example, when a person plucks a guitar string, as the string moves in one direction, it pushes on nearby air molecules, causing them to move closer together. This creates a small region of high pressure on one side of the string and low pressure on the opposite side. As the string moves in the opposite direction, the areas of high and low pressure reverse. This compression and rarefaction of air molecules occurs periodically.
The frequency $f$ of a sound is defined as the number of oscillations per second. It is measured in Hertz (abbreviated as Hz). The amplitude of a sound, measured in decibel (db), represents the size of such variations.
Sound waves are captured by using a transducer such as a microphone. A microphone converts acoustical waves into electrical waves.
Figure 1 shows a sinewave or sinusoid in time domain, i.e., in a time versus amplitude axes, in the bottom part. The frequency of this sinewave is 2.5 Hz. The sinewave is a periodic wave, which means that it regularly repeats over time.
The period is given by $ 1 / f$.

Figure 1 shows a sinewave in time domain.
<img src="img/sine.svg" class="mx-auto d-block">
Figure 1: time domain representation of a sinewave.


A frequency domain or spectrum representation shows the frequency content of a sound versus the amplitude. The individual frequency components are called harmonics (if they are integer multiple of the fundamental frequency) or partials.
Figure XX shows the spectrum of the same sinusoid.


When representing a sound digitally, the soundwave is sampled at
regular intervals by using an analog to digital converter (ADC),
which produces numbers which represent the value of each sample.

The sampling rate is defined as the number of samples per seconds.
As an example, to obtain compact disc quality the sampling rate is
set to 44.100 Hz, or 44.1 kHz. This means that each second of sound
is represented by 44.100 samples.

When the goal is to listen to a digitally reproduced sound, the
operation called digital to analog conversion (DAC) needs to be
performed. A digital to analog converter reconstructs the sound from
its samples. Computer sound cards have both ADC to input sounds and
DAC to output them.



# Chapter 2: Sound synthesis
## Additive synthesis

Additive synthesis is based on the principle that any complex waveform can be created by summing a finite number of sinewaves. This idea derives from the Fourier theorem, which states that any complex sound can be decomposed as the sum of its elementary components, which are sinewaves (also called sinusoids or pure tones).
As an example, Figure 1 shows the time and frequency domain representation of a square wave. The diagram on the center represents the time domain (top) and frequency domain (bottom) or spectrogram of a square wave.
The diagram on the right side shows what is known as spectrogram. The spectrogram is a representation time versus frequency of a signal. The amplitude is represented by the greyscale in which the wave is represented. The darker the mark, the higher the amplitude at that specific frequency.

Figure XX shows the block diagram of an additive synthesizer. In it, four sinewaves are summed together.


<img src="img/additive.svg" class="mx-auto d-block">

##Amplitude envelopes
An amplitude envelope determines how the amplitude of a sound evolves over time.
A typical example of amplitude envelopes is the so-called ADSR envelope, which stands for Attack, Decay, Sustain and Release.
A schematic structure of an ADSR envelope is shown in Figure XXX.

<img src="img/adsr.svg" class="mx-auto d-block">

## Granular synthesis

A grain of sound is a brief acoustical event, with a duration near the treshold of human perception (usually between 10 and 60 ms). The idea behind granular synthesis is the creation of complex sound events by combining thousands of grains of sound over time. In granular synthesis, a grain of sound is therefore considered as a building block for the creation of more complex sonic events.

The idea of having sound grains was proposed by the British physicist Dennis Gabor in 1947. Gabor suggested the idea of a quantum of sound, as a perceptual indivisible unit of information. All macro-level phenomena are based on the sound quantum.
Granular synthesis was first suggested as a computer music technique for producing complex sounds by Iannis Xenakis and Curtis Roads.
Granular synthesis is based on the production of a high density of small acoustic events called grains that are typically in the range of 10-60 ms.
To enable smooth transitions between grains, each grain has an amplitude envelope. In Gabor’s original conception, the amplitude envelope is a bell shaped curve generated by the Gaussian method.

###Parameters of granular synthesis
The control parameters of a granular synthesizer are the following:

####Grain size:
 length of the grain in milliseconds.
#### Grain shape. 
The grain itself may come from a sinewave, pulse wave, synthesis techniques such as FM synthesis or sampled sounds.
Envelope shape.
####Grain spacing over time.

####Grain density: 
number of grains per unit of time.
Typical grain densities range from several hundred to several thousand grains per second.
#### High-level grain organization
When organized over time, grains can be placed in a synchronous or asynchronous way.

#### Synchronous granular synthesis
In synchronous granular synthesis, grains are placed at equally spaced positions. This creates a periodicity which provides a sensation of frequency. For example, if grains are placed every 10 ms, a 100 Hz frequency will be heard.
A technique designed for the synthesis of tones with one or more resonances, i.e. this technique is suitable for imitating natural instruments and voices. All the grains are regularly placed over time.

#### Asynchronous granular synthesis
In asynchronous granular synthesis, grains are scattered randomly over a user-determined duration and with user-determined density and frequency content (depending on the waveform used in the grains).
### Implementation
Granular synthesis can be implemented generating small sound events and placing them over time as if in a score. Lots of parameters are involved. Their choice and organisation is what makes granular synthesis more or less interesting.

#### Time stretching
In synchronous granular synthesis, it is possible to create time stretching effects by repeating the grains is a longer (or shorter) amount of time, without varying their distance.
#### Pitch shifting
In synchronous granular synthesis, it is possible to create pitch shifting effects by changing the space between the grains.

## Modulation synthesis

###Ring modulation

###Amplitude modulation
###Frequency modulation


#Chapter 3: Delay based effects
## Basic delay
A basic delay simply plays an input sound after a specific amount of time.
Mathematically speaking, a delay can be expressed as:
$$y[n] = x[n -N]$$
where $n$ represents the current time in samples and $N$ represents the delay time in samples.

<img src="img/delay.svg" class="mx-auto d-block">

### Echo

### Flanger
###LFO
### Chorus
### Overdrive and clipping
### Tremolo

### Feedback delay
### Doppler effect




#Chapter 4: Filters
#Chapter 5: Spatial sound

