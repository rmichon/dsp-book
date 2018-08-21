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

      