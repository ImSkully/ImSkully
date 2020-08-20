---
title: "Lua: string.sub a string too long"
---

Append to the end of a string that is larger than the value of `x` where `x` is the maximum length and position of where the string should be appended.

`value = string.sub(value, 1, length - 1) .. "..." end`

**Example:**
```lua
local value = "This is a long string that needs substitution."
local maxLength = 12

if #value > maxLength then value = string.sub(value, 1, maxLength - 1) .. "..." end end
```