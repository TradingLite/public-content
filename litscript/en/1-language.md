---
title: Language Basics
---

### Differences & Limitations

LitScript is still in experimental phase, issues may arise and syntax could change in the
future. 

* Syntax is very close to JavaScript, so scopes and curly braces are necessary
* [Conditional Statements](#Conditional_Statements), [Iterators, Loops](#Iterators_Loops) require scopes and are javascript-like
* [Input functions](/docs/reference#functions+input) have slightly different parameters
* [Plot functions](/docs/reference#functions+plot) have slightly different parameters
* [Array/Matrix plot support](#Array_Matrix_Plots)
* [Functions](#Function_Declarations_experimental) are still experimental
* [Built-in function list](/docs/reference) is not complete yet, so please tell us if there's anything missing.
* We have various new [data sources](#Available_Data)

### Variable Definitions

##### Simple variable definition

You can define a variable using the keyword `var` in front of your variables.

```js
var myValue = 5
```

##### Series/Sequence definition

You can define a series variable, although we call them sequences in LitScript.

* ⚠ RULE OF THUMB: If you don't know if you need to use this, just simply use **var** unless compiler complains

```js
seq series = open + 10
```

This sequence can be access it by writing `series` (which is equivalent to `series[0]`) or `series[1]` if you want to lookup previous value.

##### Global variable definition

You can define a variable that persists between bars, this allows for a more advanced algorithms.

```js
//@version=1
global total_counter=0, realtime=0

if (barstate.isnew) total_counter++
plot(total_counter,color=#AF0)

if (barstate.isrealtime) realtime++
plotshape(total_counter, shape.circle, realtime)
```

##### Multiple definitions on a single line

Multiple definitions can be stacked on one line using a comma

```js
seq seriesA = open, seriesB = close, something = 99
var test = 10, something = 20
```

##### Constants

Constants are values that are defined once and can no longer be redefined afterwards.

```js
const myValue = 5
```

##### Self-referencing values

In order to self reference you need to first define then set it to something.

```js
seq test = 0
test = test + 1
```

##### **NaN / na**

NaN (Not A Number) values are supported, written as `na`


### Conditional Statements

##### **if else** statements

```js
if (condition1) {
} else if (condition2) {
} else { }
```

Scopes are optional for single line statements. e.g.

```js
if (test==2) test=1
else test=2
```

##### Shorthand conditional expressions

You can use ternary conditional operators

```js
var test = condition ? true : false
```

you can also use `iff()` function

```js
var test = iff(condition,true,false)
```


### Iterators & Loops
It is pretty close to JavaScript, with some minor differences and restrictions

* ⚠ Loops are limited to 1000 iterations for your own safety.

##### **for()**

```js
for(i=0;i<10;i++) { // simple loop to 10
    /* code */ 
}

for(j=1;j>0;j-=0.05) { // reverse loop with fractions
    /* code */ 
}  
```

##### **while()**

```js
while(condition) { 
    /* code */ 
}
```
##### **switch()**

```js
switch(somevariable){ // switch statements !
    case 0:
        /* code */
        break
    case 1:
    case 2:
        /* code */
        break
    default:
        /* code */
        break
}
```



### Function Declarations ( experimental )

Explicit variable type definition for functions

* ⚠ Functions are still experimental, so bugs may arise
* ⚠ Defining series variables INSIDE a function might not work as expected

##### Single line functions

```js
//@version=1
study("My Script")
func zigzag(val:seq) => val[2] == 0 ? 50 : 0
seq test = 0
test = zigzag(test)
plot(test, color=#0ff)
```

##### Multi-line functions

```js
//@version=1
study("My Script")
func mipmap(val:seq){
    var temp = val[1]
    if (temp == -10)
        temp = -20
    else
        temp = -10
    return temp
}
seq hiya = 0
hiya = mipmap(hiya)
plot(hiya, color=#ff0)
```

##### Function type declaration issues

⚠ Note that if you're passing a series variable, you have to use `:seq` as shown in the following example

```js
func MyFunc(mySeriesVar:seq, myVar) => mySeriesVar[1] * myVar
```


### Operators


| operators | alternative | description |
| --- | --- | --- |
| `or` | `\|\|` | Or
| `and` | `&&` | And
| `==` | **none** | Equal to
| `!=` | **none** | Not equal to
| `+` `-` `*` `/` | `+=` `-=` `*=` `/=` | Simple arithmetic operators
| `++` `--` | **none** | Increase / decrease value by one 
| `%` `\|` `~` `^` | **none** | Various other advanced operators ( similar to JavaScript )
