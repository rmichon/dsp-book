# Chapter 3: Modulation Synthesis

In this Chapter we will explain some classic modulation techniques.
Modulation is the alteration of the amplitude, frequency and /or phase of an 
oscillator in accordance with another signal. The original oscillator is called 
the carrier signal, while the modulating signal is called the modulator signal.
In this Chapter we will examine three forms of modulations: Ring Modulation (RM), 
Amplitude Modulation (AM), and Frequency Modulation (FM).

## Ring modulation

Ring modulation is simply the multiplication of two signals. Ring modulation 
has been extensively used by the German composer [Karlheinz Stockhausen](TODO). 
It has also been used in the implementation of the first movie completely 
designed with electronic sounds: [Forbidden Planet](TODO) (1956). Ring 
modulation has also been used extensively in video games. As an example, the 
sound chip of [Commodore 64](TODO) had ring modulation implemented, which was 
used in sound effects for several games.
<!-- RM: that's a lot of "been used..." -->

The algorithm of ring modulation can be summarized as follows:

TODO: insert ring mod diagram here

In order to understand what happens to the spectrum of a sound when we 
multiply two signals, let’s consider the simple case of multiplying two 
cosinewaves:

$$y(t) = \cos(\omega_1 t) \cos (\omega_2 t)$$ 

<!-- Why cosine and not sine? -->

where $\omega_1$ and $\omega_2$ represent the frequencies of two sinewaves.

Using trigonometric identities, the following equation holds:

$$y(t) = \cos(\omega_1 t) \cos (\omega_2 t) = \frac {1}{2}[ \cos((\omega_1-\omega_1)  t)) +  \cos((\omega_1+ \omega_1)  t))$$ 

As an example, if we consider two cosinewaves whose frequencies are 
$\omega_1 = 300$ Hz and $\omega_2 = 100$ Hz respectively, multiplying them 
provides the sum of two sinewaves whose frequencies are 200 Hz and 400 Hz 
respectively, and whose amplitude is the half of the original amplitude.

<!-- RM: respectively... Also, can we really say that w1 and w2 are frequencies
in Hz while they're supposed to be radian frequencies? -->

A Faust implementation of the previous equation could be:

<!-- faust-run -->
```
import("stdfaust.lib");
f1 = hslider("f1",300,20,1000,0.1);
f2 = hslider("f2",100,20,1000,0.1);
process = os.osc(f1)*os.osc(f2);
```
<!-- /faust-run -->		

Note that the modulation could be applied to an input signal simply by making 
the following modification:

<!-- faust-run -->
```
import("stdfaust.lib");
modFreq = hslider("Modulation Frequency",300,20,1000,0.1);
process = *(os.osc(modFreq));
```
<!-- /faust-run -->		

### Tremolo Effect

Tremolo is defined as the variation of the amplitude of a signal. A tremolo 
effect can be obtained with a ring modulator, but having the frequency of the 
modulator signal below 20 Hz.

A Faust implementation of a simple tremolo effect could be:

<!-- faust-run -->
```
import("stdfaust.lib");
modFreq = hslider("Modulation Frequency",8,0.001,20,0.1);
process = *(os.osc(modFreq));
```
<!-- /faust-run -->		

<!-- RM: do we want to do some scaling on the signal of the sine oscillator to
prevent the phase inversion? If yes, should we comment that in the body of the
text and add some math? -->

## Amplitude Modulation

In the ring modulation equation the frequency of the carrier signal is not 
present anymore in the resulting sound. In order to avoid this, the ring 
modulation equation can be modified. Amplitude modulation is mathematically 
expressed as:

$$y(t) = \cos(\omega_1 t) (\cos (\omega_2 t)+1)$$ 

Using again trigonometric identities, the result of such multiplication 
becomes:

$$y(t) =  \frac {1}{2}[ \cos((\omega_1-\omega_1)  t)) +  \cos((\omega_1+ \omega_1)  t)) + \cos(\omega_1 t) $$ 

The corresponding Faust implementation could be:

<!-- faust-run -->
```
import("stdfaust.lib");
f1 = hslider("f1",300,20,1000,0.1);
f2 = hslider("f2",100,20,1000,0.1);
process = os.osc(f1)*(os.osc(f2)+1)*0.5;
```
<!-- /faust-run -->		

Note that the whole process must be multiplied by 0.5 here to restrain the
range of the generated signal between -1 and 1 to prevent clicking.

## Frequency Modulation

Frequency modulation (FM) is a synthesis technique 
[invented by John Chowning](#chowning-fm) at Stanford University in California.

The FM patent was licensed from Stanford University to Yamaha, and allowed the 
creation of the [DX7](TODO), the most successful synthesizer in history.
FM synthesis was the most popular synthesis techniques in the 80s, especially 
for its ability to create several complex sounds with very little computational 
power.

The main idea behind frequency modulation is the fact that the frequency of a 
carrier oscillator can be modulated by another oscillator. Mathematically 
speaking, given two cosine waves:

$$
y_1(t) = a_1 \cos(\omega_1 t) \\
y_2(t) = a_2 \cos(\omega_2 t)
$$

FM can be expressed as:

$$y = \cos((\omega_1 + a_2 cos (\omega_2 t)) t)$$

<!-- RM: where did a1 go? -->

The corresponding Faust implementation could look like:

<!-- faust-run -->
```
import("stdfaust.lib");
f1 = hslider("[0]f1",300,20,1000,0.1);
a1 = hslider("[1]a1",1,0,1,0.01);
f2 = hslider("[2]f2",100,20,1000,0.1);
a2 = hslider("[3]a2",1,0,100,0.01);
process = a1*os.osc(f1 + a2*os.osc(f2));
```
<!-- /faust-run -->	

This equation therefore represents a cosine wave whose frequency is modulated 
by another cosine wave. The result is a very complex spectrum able to generate 
not only a few sidebands like amplitude and ring modulation, but an infinite 
number of sidebands.

<!-- RM: I don't think we mentioned the concept of sidebands in the AM section,
I think we should. -->

More precisely, John Chowning discovered that with frequency modulation it is 
possible to produce sidebands given by the following equation:

$$
\omega_s = \omega_c \pm n \omega_n
$$

where $n$ is an integer, $\omega_c$ is the carrier's frequency and $\omega_m$ 
is the modulation frequency.

<!-- wm or wn? ;) -->

This equation simply expresses the fact that each sideband lies at a frequency 
which is equal to the carrier frequency plus or minus an integer multiple of 
the modulator frequency.

In theory, since $n$ can take any integer value, applying frequency modulation 
produces an infinite series of sidebands. In practice this is not the case, as 
we will see later.

The calculation of the amplitude of sidebands is a quite complex task. We need 
to introduce a new term named $\beta$, known as the modulation index. In the 
frequency modulation equation, $\beta$ represents $a_2$, the amplitude of the 
modulator. $\beta$ is defined as:

$$
\beta = \frac {\Delta \omega_c}{\omega_n}
$$

where $\Delta \omega_c$
is the variation of the carrier frequency (the amount of variations of the carrier frequency from its unmodulated frequency).

### Bandwidth

The bandwidth of a signal can be defined as the range of frequencies occupied 
by a given signal. For example, a signal composed by two sinusoids, one at 
100 Hz and one at 200 Hz has a bandwidth of 100 Hz. On the other hand, a signal 
composed by the sum of two sinusoids, one at frequency 100 Hz and another at 
frequency 400 Hz has a bandwidth of 300 Hz.

**Exercise:** Two cosine waves at frequencies 300 Hz and 500 Hz are 
ring-modulated. What is the bandwidth of the resulting signal? For frequency 
modulation, there is a rule of thumb which states the following:

$$ B = 2 \omega_m 1 + \beta $$

### The C:M Ratio

The C:M ratio expresses the relative frequencies of carrier and modulator 
signal. An an example, if the carrier frequency is 200 Hz and the modulator 
frequency is 100 Hz, then the C:M ratio is 2.

The previous Faust example could be modified to reflect this change:

<!-- faust-run -->
```
import("stdfaust.lib");
f1 = hslider("[0]f1",300,20,1000,0.1);
a1 = hslider("[1]a1",1,0,1,0.01);
cm = hslider("[2]CM",1,1,10,0.1);
a2 = hslider("[3]a2",1,0,100,0.01);
process = a1*os.osc(f1 + a2*os.osc(f1*cm));
```
<!-- /faust-run -->	

In frequency modulation, for any given carrier frequency the frequencies of the 
upper sidebands lie at C+M, C+2M, C+3M,..and so on, while the lower sidebands 
lie at C-M,C-2M, and so on.

Lets consider the example in which C:M = 1:1. Let’s assume that both carrier 
and modulator frequency are placed at 100 Hz. This means that the upper 
sidebands will be at 200 Hz, 300 Hz, 400 Hz, and so on, while the lower 
sidebands will be at 0 Hz, -100 Hz,...and so on.

*What does the concept of a negative frequency mean?* A negative frequency is 
equivalent to its corresponding positive frequency, but remapped to the 
positive axes. When summing the same negative and positive frequency, if they 
have the same amplitude they will cancel each others.

In FM synthesis the amplitude of the sidebands is not the same, so the issue of 
cancellation of sidebands does not occur. In the case of FM synthesis, the 
calculation of the amplitude of each sideband requires the use of 
[Bessel functions](TODO).

<!-- RM: I think this section could be a little bit more developed (e.g., do
your students know what Bessel functions are?). It'd be nice to have some
spectra figures, etc. -->

## Musical examples

To be written.

# Faust Codes

## Additive Synthesizer

<!-- faust-run -->
```
import("stdfaust.lib");

addSynth(f) = par(i,N,oscillator(i)) :> /(N)
with{
  N = 4; // number of iterations
  oscillator(i) = os.osc(f*fRatio)
  with{
	fRatio = hgroup("[0]Add Synth",vslider("Freq Ratio %i",1+i,1,10,0.01));
  };
};

// "MIDI" Parameters
freq = hslider("[1]freq",440,50,2000,0.01);
gain = hslider("[2]gain",1,0,1,0.01);
gate = button("[3]gate");

process = addSynth(freq)*gain*gate;
```
<!-- /faust-run -->			

`N` changes the number of oscillators...

## ADSR Envelope

TODO

## Decaying Exponential

Not really sure how you wanted to implement this, but that seemed like a good
way to do it... :)

<!-- faust-run -->
```
import("stdfaust.lib");

exDecEnv(t60,gate) = gate : normOnePole(pole)
with{
  normOnePole(s) = *(1.0 - s) : + ~ *(s);
  pole = exp(-1.0/(t60*ma.SR));
};

freq = hslider("[1]freq",440,50,2000,0.01);

envelope = hgroup("[2]Envelope",exDecEnv(t60,gate)*gain)
with{
  t60 = hslider("[0]t60[style:knob]",0.1,0.001,5,0.001);
  // "MIDI" Parameters
  gain = hslider("[1]gain[style:knob]",1,0,1,0.01);
  gate = button("[2]gate");
};

process = os.sawtooth(freq)*envelope;		
```
<!-- /faust-run -->	

## ADSR With Exponential Segments

Example of ADSR envelope with exponential segments applied to a sawtooth wave
oscillator.

<!-- faust-run -->
```
import("stdfaust.lib");

adsre(attT60,decT60,susLvl,relT60,gate) = envelope with {
  ugate = gate>0;
  samps = ugate : +~(*(ugate)); // ramp time in samples
  attSamps = int(attT60 * ma.SR);
  target = select2(ugate,0.0,select2(samps<attSamps,susLvl*float(ugate),ugate));
  t60 = select2(ugate,relT60, select2(samps<attSamps,decT60,attT60));
  pole = ba.tau2pole(t60/6.91);
  envelope = target : si.smooth(pole);
};

freq = hslider("[1]freq",440,50,2000,0.01);

envelope = hgroup("[2]Envelope",adsre(att,dec,gain,rel,gate))
with{
  att = hslider("[0]Attack[style:knob]",1,0.001,5,0.001);
  dec = hslider("[1]Decay[style:knob]",1,0.001,5,0.001);
  rel = hslider("[2]Release[style:knob]",1,0.001,5,0.001);
  // "MIDI" Parameters
  gain = hslider("[3]gain[style:knob]",1,0,1,0.01);
  gate = button("[4]gate");
};

process = os.sawtooth(freq)*envelope;	
```
<!-- /faust-run -->	

## Distortion

<!-- faust-run -->
```
import("stdfaust.lib");

cubicnl(drive,offset) = *(pregain) : +(offset) : clip(-1,1) : cubic
with {
  pregain = pow(10.0,2*drive);
  clip(lo,hi) = min(hi) : max(lo);
  cubic(x) = x - x*x*x/3;
};

drive = hslider("[0]Drive[style:knob]",0.5,0,1,0.01);
offset = hslider("[1]Offset[style:knob]",0,-1,1,0.01);

process = hgroup("Distortion",cubicnl(drive,offset));
```
<!-- /faust-run -->				

We could improve this by adding a DC blocker too.

## Compressor

TODO: this one hasn't been tested but it seems to work. We should talk about 
how deep we wanna go in terms of not using pre-written functions. All in all,
it seems that we can't just through Faust examples at student's face: it needs
to be contextualized and build up progressively.

<!-- faust-run -->
```
import("stdfaust.lib");

compressor(ratio,thresh,att,rel,kneeAtt,gain) = _ <: _*(ampFollower(att,rel) : 
  ba.linear2db : outminusindb : kneesmooth : visualizer : ba.db2linear)*gain 
with{
  normOnePole(p,x) = x * (1.0 - p) : (+ : max(x,_)) ~ *(p);
  attPole = ba.tau2pole(att);
  relPole = ba.tau2pole(rel);
  kneePole = ba.tau2pole(kneeAtt);
  ampFollower(att,rel) = abs : normOnePole(attPole) : normOnePole(relPole);
  outminusindb(level) = max(level-thresh,0)*(1/ratio-1);
  kneesmooth = normOnePole(kneePole);
  visualizer = hbargraph("[1]Compressor Level [unit:dB]",-50,10);
};

envelopesGroup(x) = hgroup("[0]Envelope",x);
att = envelopesGroup(hslider("[0]Attack [style:knob][unit:ms]",20,0,500,0.1)*0.001);
rel = envelopesGroup(hslider("[1]Release [style:knob][unit:ms]",20,0,500,0.1)*0.001);
kneeAtt = envelopesGroup(hslider("[2]Knee Attack [style:knob][unit:ms]",10,0,250,0.1)*0.001);
thresh = hslider("[2]Threshold [unit:dB]",-30,-60,4,0.1);
ratio = hslider("[3]Ratio",1,1,10,0.01);
makeUpGain = hslider("[4]Makeup Gain [unit:dB]",40,-96,96,0.1) : ba.db2linear;

process = vgroup("Compressor",compressor(ratio,thresh,att,rel,kneeAtt,makeUpGain));
```
<!-- /faust-run -->				
			
# References

<div id="chowning-fm">J. Chowning. The synthesis of complex audio spectra by 
means of frequency modulation. Journal of the Audio Engineering Society, 
21(7):526–534, 1973.</div> 

 
      