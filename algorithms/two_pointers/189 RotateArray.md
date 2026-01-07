# 189. Rotate Array

## Problem

Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative.

---

## Core Idea (Tell it straight)

Rotating right by `k` is the same as:

1. Reverse the entire array.
2. Reverse the first `k` elements.
3. Reverse the remaining `n - k` elements.

This works because reversing rearranges the order in a controlled way without extra space.

Time complexity is O(n). Space complexity is O(1).

---

## Java Solution (In-place, No Extra Memory)

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        k = k % n; // handle large k

        reverse(nums, 0, n - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, n - 1);
    }

    static void reverse(int[] nums, int l, int r) {
        while (l < r) {
            nums[l] = nums[l] + nums[r];
            nums[r] = nums[l] - nums[r];
            nums[l] = nums[l] - nums[r];
            l++;
            r--;
        }
    }
}
```

Note: The swap uses arithmetic instead of a temp variable. It works but can overflow for extreme values. A temp variable is safer in production.

---

## Step-by-Step Flow (Example)

Input:

```
nums = [1,2,3,4,5,6,7]
k = 3
```

Step 1: Normalize k

```
k = 3 % 7 = 3
```

Step 2: Reverse the whole array

```
[7,6,5,4,3,2,1]
```

Step 3: Reverse first k elements (0 to 2)

```
[5,6,7,4,3,2,1]
```

Step 4: Reverse remaining elements (3 to 6)

```
[5,6,7,1,2,3,4]
```

Final result:

```
[5,6,7,1,2,3,4]
```

---

## JavaScript Solution

```javascript
var rotate = function(nums, k) {
    const n = nums.length;
    k = k % n;

    reverse(nums, 0, n - 1);
    reverse(nums, 0, k - 1);
    reverse(nums, k, n - 1);
};

function reverse(nums, l, r) {
    while (l < r) {
        const temp = nums[l];
        nums[l] = nums[r];
        nums[r] = temp;
        l++;
        r--;
    }
}
```

---

## Python Solution

```python
class Solution:
    def rotate(self, nums, k):
        n = len(nums)
        k = k % n

        self.reverse(nums, 0, n - 1)
        self.reverse(nums, 0, k - 1)
        self.reverse(nums, k, n - 1)

    def reverse(self, nums, l, r):
        while l < r:
            nums[l], nums[r] = nums[r], nums[l]
            l += 1
            r -= 1
```

---

## Why This Approach Is Preferred

* No extra array
* Linear time
* Clean logic once you understand the reverse trick

If you don’t know this pattern, you’ll overcomplicate it. Once you do, this becomes a one-minute problem.
