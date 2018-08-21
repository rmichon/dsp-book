# Chapter

## Section

### Subsection

Regular code example:

```
y = x;
```

Inlined code example: `y = x;`.

Compilable Faust code example:

<!-- faust-run -->
```
process = _~+(1);
```
<!-- /faust-run -->

and its corresponding differential equation:

$$y(n) = y(n-1) + 1$$

That could also be inlined: $y(n) = y(n-1) + 1$.

Example of a figure:

<img src="img/diagiteration.svg" class="mx-auto d-block">

Link to [the next section](#the-next-section).

## The Next Section

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
			
      
      