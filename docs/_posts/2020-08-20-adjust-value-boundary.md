---
title: "Adjust Value Boundary Percentage"
---

Convert the boundary of a percentage or value from one to another, using two old bounds and applying the new ones in the given formula:

### Formula
```
newValue = (((oldValue - oldMinimum) * (newMaximum - newMinimum)) / (oldMaximum - oldMinimum)) + newMinimum
```

**Example:**
* Convert `x = 62` from old boundary of 52-100 to be bound to 0-100.


`x = (((62 - 52) * (100 - 0)) / (100 - 52)) + 0`

***

## JavaScript

```js
/**
 * @param   int     number      The number to adjust.
 * @param   int     oldMin      The old minimum value.
 * @param   int     oldMax      The old maximum value.
 * @param   int     newMin      The new minimum value to convert to.
 * @param   int     newMax      The new maximum value to convert to.
 * 
 * @returns int     The original number converted to the new boundary.
*/
function convertValueToBoundary(number, oldMin, oldMax, newMin, newMax)
{
    return (((number - oldMin) * (newMax - newMin)) / (oldMax - oldMin)) + newMin
}
```

### Example

For instance, given the number `5` within a range of `0-10`, we want to increase the numerical range to be between `0-100` instead.

```nodejs
convertValueToBoundary(5, 0, 10, 0, 100) // Output -> 50
```

<div markdown="0"><a href="https://codereview.stackexchange.com/questions/269408/convert-number-from-old-range-to-new-numeric-range" class="btn">Stack Exchange Code Review</a></div>