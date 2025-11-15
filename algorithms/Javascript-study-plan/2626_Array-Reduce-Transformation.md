# 2626. Array Reduce Transformation

## Problem Summary

You’re given:

* An array of numbers `nums`,
* A reducer function `fn`,
* An initial value `init`.

Your task is to manually simulate how `Array.prototype.reduce` works.

The workflow looks like this:

* Start with a running value `val = init`.
* For each element in `nums`, update:
  `val = fn(val, nums[i])`.
* After processing every element, return `val`.

If `nums` is empty, simply return `init`.

## Key Concepts

### 1. Reduction Behavior

A *reduce* operation transforms an entire array into one single value. You’re not looping for the sake of looping — you’re feeding the output of the previous computation into the next one.

This kind of behavior
**“don’t think in steps. Think in behaviors.”**
really matters in algorithm design. You’re modeling how data transforms, not just how many loops you run.

### 2. Accumulator Pattern

`init` acts as your accumulator.
Each iteration mutates the accumulator using:

```
init = fn(init, nums[i])
```

This is a common pattern in:

* prefix sums
* aggregations
* combiners
* dynamic programming transitions
* functional programming

### 3. Avoid Dead Variables

In your original solution, you left an unused `result = []`. Removing unused variables is necessary to write clean, intention-based code.

## Final Solution

```js
var reduce = function(nums, fn, init) {
    for (let i = 0; i < nums.length; i++) {
        init = fn(init, nums[i]);
    }
    return init;
};
```
