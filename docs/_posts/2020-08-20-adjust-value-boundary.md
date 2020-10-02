---
title: "Adjust Value Boundary Percentage"
---

Convert the boundary of a percentage or value from one to another, using two old bounds and applying the new ones in the given formula:

```
newValue = (((oldValue - oldMinimum) * (newMaximum - newMinimum)) / (oldMaximum - oldMinimum)) + newMinimum
```

**Example:**
* Convert `x = 62` from old boundary of 52-100 to be bound to 0-100.


`x = (((62 - 52) * (100 - 0)) / (100 - 52)) + 0`
