---
title: Basics
---


### Main Functions

Function that you'll be using the most is `study()` which is mandatory for any script.

Afterwards `input()` in order to able to change script settings on the fly without editing the script itself.

### Built-in function limitations

Currently the way functions receive their properties is a mix between named and unnamed values.

For instance, you can write `input("test", 123)` but you can't write `input(title="test", value=123)`
but as for most optional arguments, this is is different. 

_(e.g you can write `input("test", 123, min=0)` but not `input("test", 123, 0)` because LitScript doesn't know what that last 0 refers to.)_

### How to Plot on Chart

Plotting something on the chart is fairly straightforward:

```js
plot(open, color=#FFF)
```

You can find various other plot drawing functions [here](/docs/reference#functions+plot) such as [histogram](/docs/reference#functions+histogram), [shapes](/docs/reference#functions+plotshape), [fill](/docs/reference#functions+fill), and more.

### Available Data

#### Price

Returns candle values (timeseries), values are affected by **heiken ashi** when enabled

```js
open, high, low, close
```

The following averages are also available:
```js
ohlc4, hlc3, hl2
```

##### Basic Example

```js
//@version=1
study("Price Candles", overlay=false)
plotcandle(open, high, low, close) // candle plots
```

#### Volume

Returns volume buy/sell values (timeseries)

```js
vbuy, vsell
```

##### Basic Example

```js
//@version=1
study("Volume", overlay=false)
var v = (vbuy + vsell)
histogram(v, color = close<open ? color.red : color.green)
```

##### Volume Delta Example

```js
//@version=1
study("Volume Delta", overlay=false)
var vd = (vbuy - vsell)
histogram(vd, color = vd<0 ? color.red : color.green)
```

##### CVD (Cumulative Volume Delta) Example

```js
//@version=1
study("CVD (Cumulative Volume Delta)")
seq vd = vbuy-vsell
var cvd = cum(vd)
plot(cvd,color=#00ff00)
```

#### Bids & Asks

##### Bid/Ask Sum

Returns a sum of bid/ask values

The range depends on the heatmap definition ( SD or HD heatmap )

* SD heatmap max range is 220% of the closing price
* HD heatmap max range is 120% of the closing price

```js
//@version=1
study("[example] Bid/Ask Sum")
plot(bid_sum(),color=#AF0)
plot(ask_sum(),color=#F30)
```

##### Bid/Ask Sum Range ( Percent )

Returns a sum of bid/ask sum of a specific range
( ⚠ This feature is experimental and slow )

* `range` — Range in %, (e.g. `bid_sum(120)` 120% range of its current closing price)

```js
//@version=1
study("[example] Bid/Ask Sum 15% Range")
plot(bid_sum(15),color=#AF0)
plot(ask_sum(15),color=#F30)
```

##### Bid/Ask Sum Absolute Range

Returns a sum of bid/ask sum of a specific price range
( ⚠ This feature is experimental, slightly faster than percent equivalent )

* `price_min` — Min value
* `price_max` — Max value

```js
//@version=1
study("[example] Bid/Ask Sum Absolute range (4000 from current price)")
plot(ask_sum(0,close+4000),color=#AF0)
plot(bid_sum(close-4000,0),color=#F30)
```


##### Bid/Ask Sum Example

```js
//@version=1
study("Bid/Ask Range Sum Bands")

const divide = input("Divide by", 7)
plots[10] asks, bids

for (i=1;i<=10;i++) append(asks,ask_sum((i*i)/divide), darken(#FFAA00,i*10))
for (i=1;i<=10;i++) append(bids,-bid_sum((i*i)/divide), darken(#AAFFAA,i*10))

plot(asks)
plot(bids)

plot(ask_sum())
plot(-bid_sum())

plot(0, color=#333) // middle line
```

##### Bid/Ask Highest Size Price

Returns price where the bid/ask is at its highest size

```js
//@version=1
study("[example] Bid/Ask Highest Size Price")
plot(ask_highest(),color=#F30)
plot(bid_highest(),color=#AF0)
```

##### Bid/Ask Spread Price

Returns price where cumulative bid/ask size starting from current price reaches the specified total threshold size

* `size` — Total threshold size

```js
//@version=1
study("[example] Bid/Ask Spread")
plot(ask_spread(100000),color=#F30)
plot(bid_spread(100000),color=#AF0)
```

#### Open Interest

Returns open interest candle values (timeseries)

⚠ **Open Interest** only available to **subscribers**.

Open Interest is also available as a built-in layer so you don't have to code one.

```js
oi_open, oi_high, oi_low, oi_close
```

The following averages are also available:
```js
oi_ohlc4, oi_hlc3, oi_hl2
```

Example: 
```js
//@version=1
study("Open Interest in LitScript", overlay=false)
plotcandle(oi_open, oi_high, oi_low, oi_close)
```

#### Liquidations 

Returns ask or bid liquidations (timeseries)

⚠ **Liquidations** only available to **subscribers**.

```js
liq_ask, liq_bid
```

#### Funding and Mark Price

Returns funding, funding_predicted or mark_price (timeseries)

⚠ **Funding** only available to **subscribers**.

```js
funding, funding_predicted, mark_price
```

### Array/Matrix Plots

Array/matrix plots are special kind of plots, helps avoid writing unnecessary bloat in your code.

In order to use them, you have to define an array, use append, then use plot to draw them.

⚠ max array limit is 64 plots

Examples: 

```js
//@version=1
plots[2] array// size is a positive value, represents the amount of charts to create
append(array, open, #FF00FF) // fill the first chart's array data
append(array, close, #FF00FF) // fill the first chart's array data
plot(array) //plot the 
```

```js
//@version=1
study("array overlaid example", overlay=true)
plots[32] levelsA // array allows us to generate 32 charts easily
plots[10] levelsB

// simple for loop
for (i=0;i<32;i++) {
  append(levelsA, sma(open,i*2)+i*20, lighten(#509,i*5))  // append is how we fill the data for arrays plots
}

for (i=0;i<100;i+=10) { // step by 10
  append(levelsB, sma(open,i*2)-i*20, darken(#B09,i*2))
}

plot(levelsA)
plot(levelsB)
```
