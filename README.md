# color [![Build Status](https://travis-ci.org/Qix-/color.svg?branch=master)](https://travis-ci.org/Qix-/color)

> JavaScript library for immutable color conversion and manipulation with support for CSS color strings.

```js
var color = Color('#7743CE').alpha(0.5).lighten(0.5);
console.log(color.hsl().string());  // 'hsla(262, 59%, 81%, 0.5)'

console.log(color.cmyk().round().array());  // [ 16, 25, 0, 8, 0.5 ]

console.log(color.ansi256().object());  // { ansi256: 183, alpha: 0.5 }
```

## Install
```console
$ npm install color
```

## Usage
```js
var Color = require('color');
```

### Constructors
```js
var color = Color('rgb(255, 255, 255)')
var color = Color({r: 255, g: 255, b: 255})
var color = Color.rgb(255, 255, 255)
var color = Color.rgb([255, 255, 255])
```

Set the values for individual channels with `alpha`, `red`, `green`, `blue`, `hue`, `saturationl` (hsl), `saturationv` (hsv), `lightness`, `whiteness`, `blackness`, `cyan`, `magenta`, `yellow`, `black`

String constructors are handled by [color-string](https://www.npmjs.com/package/color-string)

### Getters
```js
color.hsl();
```
Convert a color to a different space (`hsl()`, `cmyk()`, etc.).

```js
color.object(); // {r: 255, g: 255, b: 255}
```
Get a hash of the color value. Reflects the color's current model (see above).

```js
color.rgb().array()  // [255, 255, 255]
```
Get an array of the values with `array()`. Reflects the color's current model (see above).

```js
color.rgbNumber() // 16777215 (0xffffff)
```
Get the rgb number value.

```js
color.hex() // #ffffff
```
Get the hex value.

```js
color.red()       // 255
```
Get the value for an individual channel.

### CSS Strings
```js
color.hsl().string()  // 'hsl(320, 50%, 100%)'
```

Calling `.string()` with a number rounds the numbers to that decimal place. It defaults to 1.

### Luminosity
```js
color.luminosity();  // 0.412
```
The [WCAG luminosity](http://www.w3.org/TR/WCAG20/#relativeluminancedef) of the color. 0 is black, 1 is white.

```js
color.contrast(Color("blue"))  // 12
```
The [WCAG contrast ratio](http://www.w3.org/TR/WCAG20/#contrast-ratiodef) to another color, from 1 (same color) to 21 (contrast b/w white and black).

```js
color.isLight();  // true
color.isDark();   // false
```
Get whether the color is "light" or "dark", useful for deciding text color.

### Manipulation
```js
color.negate()         // rgb(0, 100, 255) -> rgb(255, 155, 0)

color.lighten(0.5)     // hsl(100, 50%, 50%) -> hsl(100, 50%, 75%)
color.lighten(0.5)     // hsl(100, 50%, 0)   -> hsl(100, 50%, 0)
color.darken(0.5)      // hsl(100, 50%, 50%) -> hsl(100, 50%, 25%)
color.darken(0.5)      // hsl(100, 50%, 0)   -> hsl(100, 50%, 0)

color.lightness(50)    // hsl(100, 50%, 10%) -> hsl(100, 50%, 50%)

color.saturate(0.5)    // hsl(100, 50%, 50%) -> hsl(100, 75%, 50%)
color.desaturate(0.5)  // hsl(100, 50%, 50%) -> hsl(100, 25%, 50%)
color.grayscale()      // #5CBF54 -> #969696

color.whiten(0.5)      // hwb(100, 50%, 50%) -> hwb(100, 75%, 50%)
color.blacken(0.5)     // hwb(100, 50%, 50%) -> hwb(100, 50%, 75%)

color.fade(0.5)     // rgba(10, 10, 10, 0.8) -> rgba(10, 10, 10, 0.4)
color.opaquer(0.5)     // rgba(10, 10, 10, 0.8) -> rgba(10, 10, 10, 1.0)

color.rotate(180)      // hsl(60, 20%, 20%) -> hsl(240, 20%, 20%)
color.rotate(-90)      // hsl(60, 20%, 20%) -> hsl(330, 20%, 20%)

color.mix(Color("yellow"))        // cyan -> rgb(128, 255, 128)
color.mix(Color("yellow"), 0.3)   // cyan -> rgb(77, 255, 179)

// chaining
color.green(100).grayscale().lighten(0.6)
```

### Manipulation

Implementation of Porter Duff source over compositing operator as per [WCAG Compositing and Blending](https://www.w3.org/TR/compositing-1/#porterduffcompositingoperators_srcover).

```js
const transparentRed = 'rgba(255, 59, 48, 0.67)';
const transparentBlue = 'rgba(0, 122, 255, 0.65)';
const transparentYellow = 'rgb(255, 204, 0, 0.69)';

Color('white').overlay(Color(transparentRed)).rgb().string(0); // 'rgb(255, 124, 116)'

Color('black').overlay(Color(transparentRed)).rgb().string(0); // 'rgb(171, 40, 32)'

Color(transparentRed).contrast(Color('white')); // 3.547137959249039

Color('white').overlay(Color(transparentRed)).contrast(Color('white')); // 2.508157178805863

Color('black').overlay(Color(transparentRed)).contrast(Color('white')); // 6.893229382289893);

Color('black').overlay(Color(transparentBlue)).rgb().string(0); // 'rgb(0, 79, 166)'

Color('white').overlay(Color(transparentBlue)).rgb().string(0); // 'rgb(89, 169, 255)'

Color(transparentBlue).contrast(Color('white')); // 4.016975780478911

Color('white').overlay(Color(transparentBlue)).contrast(Color('white')); // 2.466814448364806

Color('black').overlay(Color(transparentBlue)).contrast(Color('white')); // 7.847914944366607

Color('white').overlay(Color(transparentYellow)).overlay(Color(transparentRed)).hex(); // '#FF703A'

Color('white').overlay(Color(transparentRed)).overlay(Color(transparentYellow)).hex() // '#FFB324'
```

## Propers
The API was inspired by [color-js](https://github.com/brehaut/color-js). Manipulation functions by CSS tools like Sass, LESS, and Stylus.
