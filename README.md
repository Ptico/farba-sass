[![SWUbanner](https://raw.githubusercontent.com/vshymanskyy/StandWithUkraine/main/banner-direct-single.svg)](https://stand-with-ukraine.pp.ua/)

# farba-sass

(ukrainian: фарба - paint)

Pure SASS color convertion and manipulation.

## Usage

General usage example, with fallback for browsers without CSS Color 4 support

```scss
@use "farba/color" as farba;

// For now it's impossible to set alpha through the / notation
// this will change in dart-sass 2.0
$color: farba.create(okhsv-srgb 264deg 100% 48% 80%);

.test {
  color: farba.to-srgb($color); // rgba(0, 25, 127, 0.8)
  color: farba.to-css($color); // oklab(0.296359113 -0.0175930932 -0.1673871009 / 0.8)
}
```

Conversions example:

```scss
// Create a color from any SASS compatible color representation:
// https://sass-lang.com/documentation/values/colors
$baseColor: farba.from(rebeccapurple);
$color: farba.to(hsluv, $baseColor);
```

**Important:**

For some colors (okhsl/okhsv for example), unitless values would be threated as numbers in a defined range (0 to 1 for example), not percents, and angles could be threated as revolutions or radians, not degrees. So it's highly recommended to use define corresponding units.
All the CSS unit types are supported, including [angle](https://developer.mozilla.org/en-US/docs/Web/CSS/angle)

## Colors

* **okhsl-srgb**/**okhsv-srgb** - [Announcement](https://bottosson.github.io/posts/colorpicker/), [Color picker](https://bottosson.github.io/misc/colorpicker/)

An HSL/HSV-like, perceptually uniform colors, based on Oklab color space.

Args: `<angle>, <percentage|number(0..1)>, <percentage|number(0..1)>`

`to-css()` outputs to `oklab()`

* **hsluv**/**hpluv** - [Website](https://www.hsluv.org)

An HSL-like, perceptually uniform color, based on CIE LUV

Args: `<angle>, <percentage>, <percentage>`

`to-css()` outputs to `rgb()`

* **okhsl-p3**/**okhsv-p3**

**Experimental**: an okhsl/okhsv recalculated to use wider P3 gamut instead of sRGB.

* **srgb**

Standard RGB used in web development for decades

Args: `<number(0..255)|percentage>, <number(0..255)|percentage>, <number(0..255)|percentage>`

* `cielab`, `cielch`, `oklab`, `oklch` is planned

## Color manipulation

Coming soon

## Technical information

Most of the color conversions is done through CIE XYZ with D65 white point.
Oklab/LMS matrices have been updated based on [this thread](https://github.com/w3c/csswg-drafts/issues/6642) and may be slightly different than the current CSS. However, in most cases the displayed color would be the same.

There is some room for performance improvements by adding conversion shorcuts, but I would prefer to keep the code as universal and simple as possible.

