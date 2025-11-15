

# **2635. Apply Transform Over Each Element in Array**

## **Problem Summary**

You're given:

* an array `arr`
* a function `fn`

Your job is to return a **new array** where every element is the result of calling:

```
fn(arr[i], i)
```

You can’t use the built-in `Array.map`.

This forces you to understand how JavaScript handles:

* higher-order functions
* callbacks
* passing functions as values
* how `this` behaves in prototype methods
* how the real `map` works internally

---

# **Core Concepts From This Problem**

## **1. Functions Are Values**

In JavaScript, functions are treated like normal data:

* you can store them in variables
* pass them as arguments
* return them from other functions

This flexibility allows patterns like `map`, where you pass a function that determines the transformation.

---

## **2. Callback Functions**

A callback is simply a function you give to another function so the receiving function can call it later.

Example:

```js
map(arr, fn)
```

Here `fn` is the callback. Your map doesn't know what transformation you want — it delegates that job to the callback.

---

## **3. Higher-Order Functions**

A higher-order function is any function that:

* takes another function as an argument, **or**
* returns another function
* or both

Your custom `map` is a higher-order function because it receives `fn`.

Yes, returning functions also counts, but you don’t need that here.

This is where the mindset shift hits you:

**don’t think in steps. Think in behaviors.**

---

## **4. Why JS Ignores Extra Arguments**

JavaScript doesn’t care how many arguments a function “expects.”

If `fn` accepts 1 parameter:

```js
function plusone(n) { ... }
```

But you call it with 2:

```js
fn(arr[i], i);
```

JS simply ignores the extra one.

This is why some test cases only use `(n)` while others use `(n, i)`.

---

## **5. What `Array.map` Actually Is**

`Array.prototype.map` is just a function that:

* loops over the array
* applies the callback to each value
* returns a **new array**
* doesn’t mutate the original

The signature is:

```js
map(callback(value, index, array))
```

So a function receives:

* the element
* the index
* the whole array

That pattern is exactly what you're recreating.

---

## **6. Where `this` Comes From**

The spicy part.

Inside your custom map:

```js
Array.prototype.map = function(fn) {
    // here, "this" refers to the array you called .map on
}
```

When you call:

```js
arr.map(fn)
```

JS interprets this internally as:

```js
Array.prototype.map.call(arr, fn)
```

So:

* `this` → `arr`
* `this[i]` → arr[i]
* `this.length` → arr.length

You don’t pass the array manually.
JS binds `this` based on **how the method is called**, not where it's written.

This behavior is fundamental to how prototype methods work.

---

# **All Concepts Recap (Your Cheatsheet)**

* Functions behave like data.
* Callbacks let you inject behavior into another function.
* Higher-order functions take/return functions.
* JS ignores argument count mismatches.
* `map` is a transformation mechanism, not a loop.
* Prototype methods receive the object via `this`.
* The mindset: **don’t think in steps. Think in behaviors.**

These ideas show up everywhere — React, algorithms, NodeJS, async work, basically all real JS programming.

---

# **Code Implementation**

```js
/**
 * @param {number[]} arr
 * @param {Function} fn
 * @return {number[]}
 */
Array.prototype.map = function(fn) {
    const result = [];

    for (let i = 0; i < this.length; i++) {
        result.push(fn(this[i], i, this));
    }

    return result;
};
```


