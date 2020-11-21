---
title: Colors
---

### Color Basics

#### Color definition

Colors can be defined using 3 character hexadecimal notation (e.g. `#F0b`), 6 characters (e.g. `#FF00BB`) or using builtin static color names (e.g. `color.magenta`)


#### List of static colors

All built-in colors should start with `color.` e.g. `color.blue`

```js
black, white, red, orange, lime, blue, yellow, cyan, magenta, silver, gray, maroon, olive, green, purple, teal, navy
```

### Color blending

#### Color functions

```js
brightness(color, amount) // adjusts color's brightness by a value ( 0-255 )
darken(color, amount) // alias to brightness(color, -amount)
lighten(color, amount) // alias to brightness(color, amount)
blend(colorA, colorB, amount) //  blends between two colors by amount ( 0.0-1.0 )
```

#### Example

```js
//@version=1
study("MyScript")
var mycolor = blend(#f00,#0f0,(open-open[1])/10)
plot(open, linewidth=10, color=mycolor)
```