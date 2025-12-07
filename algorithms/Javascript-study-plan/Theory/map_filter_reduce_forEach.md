# **map()**

**Purpose:** Transform every element and return a *new* array.

**Input:** array
**Output:** new array (same length)

**Example:**
Convert everything to uppercase, double numbers, etc.

```js
const nums = [1, 2, 3];
const doubled = nums.map(n => n * 2);
console.log(doubled);
```

Output:

```
[2, 4, 6]
```

Same length, transformed values.

If you’re not returning something inside the callback, you’re using it wrong.

---

# **filter()**

**Purpose:** Keep only the elements that pass a condition.

**Input:** array
**Output:** new array (same or smaller length)

**Example:**
Grab only even numbers, only adults, only non-null values.

```js
const nums = [1, 2, 3, 4, 5];
const evens = nums.filter(n => n % 2 === 0);
console.log(evens);
```

Output:

```
[2, 4]
```

Only the ones that meet the condition survive.

If you’re not returning a boolean in the callback, congrats, you just misused it.

---

# **reduce()**

**Purpose:** Crush the whole array into a *single value*.

**Input:** array
**Output:** one thing (number, object, string, whatever)

**Example:**
Sum all numbers, count stuff, convert an array into an object.

```js
const nums = [1, 2, 3, 4];
const sum = nums.reduce((acc, n) => acc + n, 0);
console.log(sum);
```

Output:

```
10
```

Everything crushed into one result.

If you’re using reduce for things map or filter could do, you’re overcomplicating life.

---

# **forEach()**

**Purpose:** Do something for each element, return **nothing**.

**Input:** array
**Output:** undefined
(Yes, undefined. It’s unapologetically useless for chaining.)

**Example:**
Log data, update UI, mutate another array.


```js
const fruits = ["apple", "banana", "mango"];
fruits.forEach(f => console.log("I ate", f));
```

Output:

```
I ate apple
I ate banana
I ate mango
```
Runs the function on each item, returns nothing, just side effects.

If you're expecting a return value, you’re confusing it with map because your brain is tired.

---

# **Summary table**

| Method      | Returns      | Changes array length? | Use case                            |
| ----------- | ------------ | --------------------- | ----------------------------------- |
| **map**     | New array    | No                    | Transform values                    |
| **filter**  | New array    | Yes (maybe)           | Keep certain values                 |
| **reduce**  | Single value | Not applicable        | Summaries, totals, conversions      |
| **forEach** | Nothing      | No                    | Side-effects (logging, DOM updates) |


