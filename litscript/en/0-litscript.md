---
title: LitScript Manual (v1)
---

### Before You Start

#### Intro

The idea of **LitScript** is to be as close to [PineScript](https://www.tradingview.com/pine-script-docs/en/v4/index.html), we want to make the experience of switching from one to another as effortless as possible.
It's intended to be a resource for people who already have some familiarity with PineScript or any other basic programming languages.

**If you wish to help us improve these docs, then please reach us out on Discord.**

#### Version 1

At this moment by default LitScript uses an old version, and these docs are aimed for `version=1`

Please write `//@version=1` in front of every script you make.

You can find a small [upgrade guide here on our blog](/blog/post/6/LitScript_Update)

### Quickstart Script

Same quickstart script as PineScript.

```js
//@version=1
study("My Script")
var fast = 12, slow = input("Slow", 26)
var fastMA = ema(close, fast)
var slowMA = ema(close, slow)
seq macd = fastMA - slowMA
var signal = sma(macd, 9)
plot(macd, color=color.blue)
plotshape(signal, shape.up, 10, color=color.yellow)
```