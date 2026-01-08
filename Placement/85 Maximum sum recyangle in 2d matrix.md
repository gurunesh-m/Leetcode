# Maximum Sum Rectangle in 2D Matrix

## Problem Information
- **Problem Type:** Classic Interview Problem (GeeksforGeeks / Company Interviews)
- **Difficulty:** Hard
- **Related LeetCode:** 363 (Max Sum Rectangle No Larger Than K)
- **Topics:** Dynamic Programming, Kadane's Algorithm, Matrix, Prefix Sum

---

## Problem Statement

Given a 2D matrix, find the maximum sum rectangle (submatrix) within it. A submatrix is defined as any rectangular region obtained by selecting contiguous rows and columns from the original matrix.

**Note:** This is different from LeetCode 1975 (Maximum Matrix Sum with flipping operations). This is about finding maximum sum submatrix.

---

## Examples

### Example 1
```
Input: matrix = [
    [1,   2,  -1, -4, -20],
    [-8, -3,   4,  2,   1],
    [3,   8,  10,  1,   3],
    [-4, -1,   1,  7,  -6]
]

Output: 29

Explanation: 
The maximum sum rectangle is:
    [4,  2]
    [10, 1]
    [1,  7]
Sum = 4 + 2 + 10 + 1 + 1 + 7 = 29
```

### Example 2
```
Input: matrix = [
    [2, 1, -3, -4, 5],
    [0, 6, 3, 4, 1],
    [2, -2, -1, 4, -5],
    [-3, 3, 1, 0, 3]
]

Output: 18

Explanation:
The maximum sum rectangle includes most of row 2.
```

---

## Core Intuition

This problem brilliantly combines two powerful concepts:

1. **Kadane's Algorithm (1D)**: Finds maximum sum subarray in O(n) time
2. **Column Compression**: Reduces 2D problem to multiple 1D problems

### The Key Insight

**"Convert 2D matrix problem into multiple 1D array problems!"**

By fixing left and right column boundaries, we can compress all rows between these boundaries into a 1D array, where each element is the sum of elements in that row between the fixed columns. Then apply Kadane's algorithm!

---

## Approach 1: Brute Force (6 Nested Loops)

### Algorithm
Check every possible rectangle by iterating through all combinations of:
- Top row (i)
- Bottom row (j)  
- Left column (k)
- Right column (l)

For each rectangle, calculate sum using two more loops.

### Implementation
```java
public class MaximumMatrixSumBruteForce {
    public int maxSum(int[][] matrix) {
        int rows = matrix.length;
        int cols = matrix[0].length;
        int maxSum = Integer.MIN_VALUE;
        
        // Try all possible rectangles
        for (int top = 0; top < rows; top++) {
            for (int bottom = top; bottom < rows; bottom++) {
                for (int left = 0; left < cols; left++) {
                    for (int right = left; right < cols; right++) {
                        // Calculate sum of this rectangle
                        int currentSum = 0;
                        for (int i = top; i <= bottom; i++) {
                            for (int j = left; j <= right; j++) {
                                currentSum += matrix[i][j];
                            }
                        }
                        maxSum = Math.max(maxSum, currentSum);
                    }
                }
            }
        }
        
        return maxSum;
    }
}
```

### Complexity Analysis
- **Time Complexity:** O(n³ × m³) = O(n³m³) where n = rows, m = cols
  - O(n²) to choose top and bottom rows
  - O(m²) to choose left and right columns
  - O(nm) to calculate sum of each rectangle
- **Space Complexity:** O(1)

### Why This Fails
Way too slow! For a 100×100 matrix, this would require 10^12 operations.

---

## Approach 2: Prefix Sum Optimization

### Algorithm
Use 2D prefix sum to calculate rectangle sums in O(1) time.

### Implementation
```java
public class MaximumMatrixSumPrefixSum {
    public int maxSum(int[][] matrix) {
        int rows = matrix.length;
        int cols = matrix[0].length;
        int maxSum = Integer.MIN_VALUE;
        
        // Build prefix sum matrix
        int[][] prefix = new int[rows + 1][cols + 1];
        for (int i = 1; i <= rows; i++) {
            for (int j = 1; j <= cols; j++) {
                prefix[i][j] = matrix[i-1][j-1] 
                             + prefix[i-1][j] 
                             + prefix[i][j-1] 
                             - prefix[i-1][j-1];
            }
        }
        
        // Try all rectangles
        for (int top = 0; top < rows; top++) {
            for (int bottom = top; bottom < rows; bottom++) {
                for (int left = 0; left < cols; left++) {
                    for (int right = left; right < cols; right++) {
                        // Calculate sum using prefix sum
                        int sum = prefix[bottom+1][right+1]
                                - prefix[top][right+1]
                                - prefix[bottom+1][left]
                                + prefix[top][left];
                        maxSum = Math.max(maxSum, sum);
                    }
                }
            }
        }
        
        return maxSum;
    }
}
```

### Complexity Analysis
- **Time Complexity:** O(n²m²)
  - O(nm) for prefix sum
  - O(n²m²) for checking all rectangles
- **Space Complexity:** O(nm) for prefix sum array

### Still Not Optimal
Better than brute force, but we can do even better!

---

## Approach 3: Optimized Solution (Kadane's Algorithm + Column Compression)

### Algorithm

**Step 1:** Fix left and right column boundaries  
**Step 2:** For each row, sum elements between left and right columns  
**Step 3:** Apply Kadane's algorithm on this 1D array to find max subarray  
**Step 4:** Track overall maximum

### Implementation
```java
public class Maximummatrixsum {
    public int maxSum(int[][] matrix) {
        int rows = matrix.length;
        int cols = matrix[0].length;
        int maxSum = Integer.MIN_VALUE;

        // Fix left column boundary
        for (int left = 0; left < cols; left++) {
            // Array to store row sums between left and right
            int[] temp = new int[rows];
            
            // Expand right column boundary
            for (int right = left; right < cols; right++) {
                // Add current column to temp array
                for (int i = 0; i < rows; i++) {
                    temp[i] += matrix[i][right];
                }
                
                // Apply Kadane's algorithm on temp
                maxSum = Math.max(maxSum, kadane(temp));
            }
        }

        return maxSum;
    }

    private int kadane(int[] arr) {
        int maxSoFar = arr[0];
        int maxEndingHere = arr[0];

        for (int i = 1; i < arr.length; i++) {
            maxEndingHere = Math.max(arr[i], maxEndingHere + arr[i]);
            maxSoFar = Math.max(maxSoFar, maxEndingHere);
        }

        return maxSoFar;
    }
}
```

### Complexity Analysis
- **Time Complexity:** O(n × m²)
  - O(m²) for fixing left and right columns
  - O(n) for updating temp array per column
  - O(n) for Kadane's algorithm
  - Total: O(m²) × O(n) = O(nm²)
- **Space Complexity:** O(n) for temp array

---

## Algorithm Flow with Example

Let's trace through a smaller example:

```
Matrix:
    [1,   2,  -1]
    [-8, -3,   4]
    [3,   8,  10]
```

### Iteration Details

**Left = 0, Right = 0:**
```
temp = [1, -8, 3]  (column 0)
kadane(temp) = 3
maxSum = 3
```

**Left = 0, Right = 1:**
```
temp = [1+2, -8+(-3), 3+8] = [3, -11, 11]
kadane(temp) = 11
maxSum = 11
```

**Left = 0, Right = 2:**
```
temp = [3+(-1), -11+4, 11+10] = [2, -7, 21]
kadane(temp) = 21
maxSum = 21
```

**Left = 1, Right = 1:**
```
temp = [2, -3, 8]  (reset, column 1)
kadane(temp) = 8
maxSum = 21
```

**Left = 1, Right = 2:**
```
temp = [2+(-1), -3+4, 8+10] = [1, 1, 18]
kadane(temp) = 20
maxSum = 21
```

**Left = 2, Right = 2:**
```
temp = [-1, 4, 10]  (reset, column 2)
kadane(temp) = 14
maxSum = 21
```

**Final Answer: 21**

The rectangle is:
```
[2,  -1]
[-3,  4]
[8,  10]
```

---

## Understanding Kadane's Algorithm

Kadane's algorithm solves the **Maximum Subarray Problem** in O(n) time.

### The Core Idea

At each position, decide:
- **Start fresh** from current element (if previous sum is negative)
- **Extend** the current subarray (if previous sum is positive)

### Visual Example

Array: `[-2, 1, -3, 4, -1, 2, 1, -5, 4]`

```
Index:  0   1   2   3   4   5   6   7   8
Value: -2   1  -3   4  -1   2   1  -5   4
        ↓   ↓   ↓   ↓   ↓   ↓   ↓   ↓   ↓
MaxEnd: -2  1  -2   4   3   5   6   1   5
MaxSoFar:-2  1   1   4   4   5   6   6   6
```

**Explanation:**
- At index 1: Start fresh (1 > -2+1)
- At index 3: Start fresh (4 > -2+4)
- At index 4-6: Extend (positive sums)
- At index 7: Extend but don't update max
- Final max = 6 (subarray [4, -1, 2, 1])

### Kadane's Algorithm Code
```java
private int kadane(int[] arr) {
    int maxSoFar = arr[0];        // Global maximum
    int maxEndingHere = arr[0];   // Maximum ending at current position
    
    for (int i = 1; i < arr.length; i++) {
        // Decide: start fresh or extend
        maxEndingHere = Math.max(arr[i], maxEndingHere + arr[i]);
        
        // Update global max
        maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
    
    return maxSoFar;
}
```

---

## Key Concepts & Thinking Process

### 1. Why Column Compression Works

Think of it as "squashing" the matrix:

```
Original 3x3:          After compressing columns 0-1:
[1   2  -1]           [3]   (1+2)
[-8 -3   4]    →      [-11] (-8-3)
[3   8  10]           [11]  (3+8)
```

Now we have a 1D problem: find max subarray in `[3, -11, 11]`

### 2. Why Not Row Compression?

You can! If rows >> cols, compress rows instead:
```java
// Swap the loop order
for (int top = 0; top < rows; top++) {
    int[] temp = new int[cols];
    for (int bottom = top; bottom < rows; bottom++) {
        // ... apply Kadane's on columns
    }
}
```
Time becomes O(n²m) instead of O(nm²).

### 3. The Temp Array Trick

Instead of recalculating sums when expanding right:
```java
temp[i] += matrix[i][right];  // Incrementally add
```
This is way faster than recalculating from scratch!

### 4. Edge Cases

- **All negative numbers:** Returns the largest (least negative) element
- **Single element:** Returns that element
- **Single row/column:** Reduces to 1D Kadane's

---

## Visual Representation

### Column Compression Visualization

```
Matrix (4x5):
┌─────────────────┐
│ 1  2 -1 -4 -20 │
│-8 -3  4  2   1 │
│ 3  8 10  1   3 │
│-4 -1  1  7  -6 │
└─────────────────┘

Fix left=1, right=3:
     ↓  ↓  ↓
┌─────────────────┐
│ 1│ 2 -1 -4│-20 │    temp[0] = 2-1-4 = -3
│-8│-3  4  2│  1 │ →  temp[1] = -3+4+2 = 3
│ 3│ 8 10  1│  3 │    temp[2] = 8+10+1 = 19
│-4│-1  1  7│ -6 │    temp[3] = -1+1+7 = 7
└─────────────────┘

Apply Kadane's on temp = [-3, 3, 19, 7]
Result = 29 (elements at indices 1-3)
```

### Kadane's Algorithm Visualization

![Kadane's Visualization](https://miro.medium.com/max/1400/1*gJmEbw_7JqxHfGl0H-6z-A.png)

---

## Common Pitfalls

1. ❌ **Not resetting temp array**: Must reset when left column changes
   ```java
   int[] temp = new int[rows];  // Inside left loop!
   ```

2. ❌ **Wrong loop order**: Must expand right while keeping left fixed
   ```java
   for (left...) {
       reset temp;
       for (right starting from left...) { ... }
   }
   ```

3. ❌ **Forgetting negative numbers**: Initialize to `Integer.MIN_VALUE`, not 0

4. ❌ **Index confusion**: Remember temp is indexed by rows, not columns

5. ❌ **Not understanding Kadane's**: The algorithm handles negative numbers automatically!

---

## Interview Tips

### How to Approach in an Interview

**Step 1:** Start with brute force (acknowledge it's O(n³m³))  
**Step 2:** Suggest prefix sum optimization (O(n²m²))  
**Step 3:** Introduce Kadane's algorithm for 1D case  
**Step 4:** Explain column compression technique  
**Step 5:** Combine them for O(nm²) solution  

### Key Points to Mention

1. **Why Kadane's?** "It solves max subarray in O(n), so we reduce 2D to multiple 1D problems"
2. **Column vs Row?** "Choose based on dimensions - compress the smaller one"
3. **Real-world application?** "Finding optimal rectangular regions in images, financial data analysis"

### Follow-up Questions

**Q: Can you track the actual rectangle coordinates?**  
A: Yes! Store top, bottom, left, right when updating maxSum. In Kadane's, track start and end indices.

**Q: What if we want the k largest rectangles?**  
A: Use a min-heap of size k to track top k sums.

**Q: Space optimization?**  
A: Already optimal at O(n) or O(m) depending on compression choice.

---

## Related Problems

- **LeetCode 53:** Maximum Subarray (Pure Kadane's)
- **LeetCode 363:** Max Sum Rectangle No Larger Than K (Harder variant)
- **LeetCode 85:** Maximal Rectangle (Binary matrix variant)
- **LeetCode 1074:** Number of Submatrices That Sum to Target
- **GeeksforGeeks:** Maximum sum rectangle in a 2D matrix

---

## Time Complexity Comparison

| Approach | Time | Space | Practical Use |
|----------|------|-------|---------------|
| Brute Force | O(n³m³) | O(1) | Never |
| Prefix Sum | O(n²m²) | O(nm) | Small matrices |
| Kadane's + Compression | O(nm²) | O(n) | **Best choice** |
| If rows >> cols | O(n²m) | O(m) | Tall matrices |

---

## Summary

**The Maximum Sum Rectangle problem teaches:**
- How to reduce dimensionality of problems
- The power of combining classical algorithms
- Why Kadane's algorithm is fundamental
- How incremental computation saves time

**Key Takeaway:** When facing a 2D problem, ask yourself: "Can I fix one dimension and solve multiple 1D problems?" This technique extends beyond matrices to many optimization problems.

**Remember:** 
- Kadane's = O(n) maximum subarray
- Fix columns → compress rows → apply Kadane's
- Total = O(nm²) where m is number of columns

This is a **hard problem** that tests deep algorithmic thinking. Master it, and you'll ace similar 2D optimization questions!M
