# brintness
### brintness is an integer brightness/lightness/darkness calculation

This is part of an experiment in estimating a perceived brightness while remaining in integer math and using bitshifts to maximize performance.

This repo is based on [this Gist by Andrew Somers](https://gist.github.com/Myndex/04dd7d3143806ad050bb946d667e889f#file-brintness-md), discussing a computationally fast/efficient, integer-based, perceptually semi-uniform lightness/brightness calculation.

## The Issue

The traditional means to determine the perceived lightness or brightness for a given color value is to first normalize R, G, and B from 0-255 to 0.0-1.0, linearize the values via exponent or more exotic methods (we assume colors are in a gamma encoded color space, such as sRGB), and then after linearizing, creating a linear luminance value by applying coefficients to each of the R, G, B values, adding them, and then finally applying an exponent or more exotic math to find a predicted lightness value.

This is computationally expensive. And even then, we generally miss factors such as the HK effect, and the above as described does not consider the importance of context. In other words, we may say "this is the accurate way" and yet it still lacks in accuracy.

### The Unbearable Lightness of Perception

So if the commonly accepted methods lack inherent accuracy do to disregarding certain factors, and given that RGB color spaces are often encoded with a gamma or transfer curve of some type, which while different than some lightness curves, still "in the ballpark" in terms of perception.

And let's not forget that the human vision system has its own built-in gain control that makes measuring lightness perception a frustrating task that is still a matter of emerging science.

### How Fast Does Red Weigh?

Light in the world follows simple linear math. That is, if you have 100 photons of light, and triple it, you then have 300 photons of light. Human vision does not perceive light linearly however, a given change in light value will result in a larger or smaller change in perceived lightness, depending on a number of contextual factors.

And, light does not have a "color", as color is only a perception of our vision system. But light does have different wavelengths or frequencies, like musical notes on a piano for want of an analogy. But also, human vision is most sensitive to a very narrow range of "middle notes", the middle wavelengths we identify as green, with sensitivity rapidly dropping off for shorter (blue) or longer (red) wavelengths.

> [!NOTE]  
> So to model the mixing of light, we often want to be in a linear space, but when we want to predict how we see a color or lightness, we want to be in a space that is curved per our perception in the given context.

Among the implications is that each of the red, green, and blue primaries in our display are weighted differently based on an averaged visual sensitivity to each, so that `#ffffff` i.e. equal values of R,G & B is white or grey. Because these weights are being applied to light sources, they should ideally be applied in a linear space. If you apply spectral weighting to values that are gamma or TRC encoded, you'll get some errors, most noticeable in the middle ranges.

See the [Gist by Andrew Somers](https://gist.github.com/Myndex/04dd7d3143806ad050bb946d667e889f#file-brintness-md) that discusses the topic in more detail. More code will be added to this repo as it becomes available.
