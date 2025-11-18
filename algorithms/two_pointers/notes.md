# Two Pointers Algorithm: Complete Master Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Core Concepts](#core-concepts)
3. [When to Use Two Pointers](#when-to-use-two-pointers)
4. [Pattern Classifications](#pattern-classifications)
5. [Implementation Strategies](#implementation-strategies)
6. [Common Problem Types](#common-problem-types)
7. [Time and Space Complexity](#time-and-space-complexity)
8. [Problem-Solving Framework](#problem-solving-framework)
9. [Advanced Techniques](#advanced-techniques)
10. [Practice Problems](#practice-problems)

---

## Introduction

The Two Pointers technique is a powerful algorithmic pattern that uses two references (pointers) to traverse a data structure, typically arrays or linked lists. This approach often reduces time complexity from O(n²) to O(n) and eliminates the need for additional data structures.

### Key Benefits
- **Efficiency**: Reduces nested loops to single-pass solutions
- **Space Optimization**: Usually O(1) extra space
- **Simplicity**: Intuitive logic and clean code
- **Versatility**: Applicable to many problem domains

---

## Core Concepts

### What Are Pointers?

In the context of this algorithm, "pointers" are indices or references that traverse through data structures. They can:
- Move in the same direction (both left-to-right)
- Move in opposite directions (one from start, one from end)
- Move at different speeds (fast and slow)

### Fundamental Principles

1. **Reduction of Search Space**: Each pointer movement eliminates possibilities
2. **Invariant Maintenance**: Pointers maintain specific relationships or conditions
3. **Single Pass**: Most problems solved in one traversal
4. **Decision Making**: At each step, decide which pointer(s) to move based on conditions

---

## When to Use Two Pointers

### Strong Indicators

✅ **Use Two Pointers When:**
- Working with **sorted arrays** or lists
- Need to find **pairs or triplets** with specific properties
- Looking for **subarrays** or **subsequences** meeting criteria
- Problem involves **palindromes** or **mirror patterns**
- Need to **remove duplicates** or **partition** data in-place
- Comparing elements from **both ends** of structure
- Problem has **O(n²)** brute force that can be optimized
- Working with **linked lists** and need to find cycles or middle

❌ **Avoid When:**
- Data is **unsorted** and sorting would be too expensive
- Need to access **random** elements frequently
- Problem requires **backtracking** or **recursive exploration**
- Multiple passes are unavoidable

### Problem Keywords to Watch For
- "Sorted array"
- "Find pair/triplet"
- "Remove duplicates"
- "Reverse"
- "Palindrome"
- "Container/trap water"
- "Sliding window" (related pattern)
- "In-place"

---

## Pattern Classifications

### 1. Opposite Direction (Converging)

Pointers start at opposite ends and move toward each other.

**Use Cases:**
- Finding pairs in sorted arrays
- Palindrome verification
- Container with most water problems

**Visual:**
```
[1, 2, 3, 4, 5, 6, 7, 8]
 ↑                    ↑
left               right
```

### 2. Same Direction (Parallel/Chasing)

Both pointers start from the same end and move in the same direction.

**Use Cases:**
- Removing duplicates
- Fast/slow cycle detection
- Partitioning arrays

**Visual:**
```
[1, 2, 3, 4, 5, 6, 7, 8]
 ↑  ↑
slow fast
```

### 3. Sliding Window (Variable/Fixed Size)

Two pointers define a window that expands or contracts.

**Use Cases:**
- Longest substring problems
- Maximum sum subarray
- Anagrams in strings

**Visual:**
```
[1, 2, 3, 4, 5, 6, 7, 8]
    ↑     ↑
  start  end
```

### 4. Fast and Slow (Tortoise and Hare)

One pointer moves faster than the other.

**Use Cases:**
- Cycle detection in linked lists
- Finding middle of linked list
- Removing nth node from end

**Visual:**
```
LinkedList: 1 → 2 → 3 → 4 → 5
            ↑       ↑
          slow    fast (moves 2x speed)
```

---

## Implementation Strategies

### Strategy 1: Opposite Direction Pattern

**Template:**

```python
# Python
def opposite_direction(arr):
    left = 0
    right = len(arr) - 1
    
    while left < right:
        # Process current state
        if condition_met(arr[left], arr[right]):
            # Found answer
            return result
        elif should_move_left(arr[left], arr[right]):
            left += 1
        else:
            right -= 1
    
    return default_result
```

```javascript
// JavaScript
function oppositeDirection(arr) {
    let left = 0;
    let right = arr.length - 1;
    
    while (left < right) {
        // Process current state
        if (conditionMet(arr[left], arr[right])) {
            // Found answer
            return result;
        } else if (shouldMoveLeft(arr[left], arr[right])) {
            left++;
        } else {
            right--;
        }
    }
    
    return defaultResult;
}
```

```java
// Java
public class Solution {
    public ResultType oppositeDirection(int[] arr) {
        int left = 0;
        int right = arr.length - 1;
        
        while (left < right) {
            // Process current state
            if (conditionMet(arr[left], arr[right])) {
                // Found answer
                return result;
            } else if (shouldMoveLeft(arr[left], arr[right])) {
                left++;
            } else {
                right--;
            }
        }
        
        return defaultResult;
    }
}
```

**Example: Two Sum in Sorted Array**

```python
# Python
def two_sum_sorted(nums, target):
    left, right = 0, len(nums) - 1
    
    while left < right:
        current_sum = nums[left] + nums[right]
        
        if current_sum == target:
            return [left, right]
        elif current_sum < target:
            left += 1  # Need larger sum
        else:
            right -= 1  # Need smaller sum
    
    return [-1, -1]
```

```javascript
// JavaScript
function twoSumSorted(nums, target) {
    let left = 0, right = nums.length - 1;
    
    while (left < right) {
        const currentSum = nums[left] + nums[right];
        
        if (currentSum === target) {
            return [left, right];
        } else if (currentSum < target) {
            left++;  // Need larger sum
        } else {
            right--;  // Need smaller sum
        }
    }
    
    return [-1, -1];
}
```

```java
// Java
public int[] twoSumSorted(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    
    while (left < right) {
        int currentSum = nums[left] + nums[right];
        
        if (currentSum == target) {
            return new int[]{left, right};
        } else if (currentSum < target) {
            left++;  // Need larger sum
        } else {
            right--;  // Need smaller sum
        }
    }
    
    return new int[]{-1, -1};
}
```

### Strategy 2: Same Direction Pattern

**Template:**

```python
# Python
def same_direction(arr):
    slow = 0
    
    for fast in range(len(arr)):
        if condition_met(arr[fast]):
            # Process element at fast
            arr[slow] = arr[fast]
            slow += 1
    
    return slow  # or arr[:slow]
```

```javascript
// JavaScript
function sameDirection(arr) {
    let slow = 0;
    
    for (let fast = 0; fast < arr.length; fast++) {
        if (conditionMet(arr[fast])) {
            // Process element at fast
            arr[slow] = arr[fast];
            slow++;
        }
    }
    
    return slow;  // or arr.slice(0, slow)
}
```

```java
// Java
public int sameDirection(int[] arr) {
    int slow = 0;
    
    for (int fast = 0; fast < arr.length; fast++) {
        if (conditionMet(arr[fast])) {
            // Process element at fast
            arr[slow] = arr[fast];
            slow++;
        }
    }
    
    return slow;
}
```

**Example: Remove Duplicates from Sorted Array**

```python
# Python
def remove_duplicates(nums):
    if not nums:
        return 0
    
    slow = 0
    
    for fast in range(1, len(nums)):
        if nums[fast] != nums[slow]:
            slow += 1
            nums[slow] = nums[fast]
    
    return slow + 1
```

```javascript
// JavaScript
function removeDuplicates(nums) {
    if (nums.length === 0) return 0;
    
    let slow = 0;
    
    for (let fast = 1; fast < nums.length; fast++) {
        if (nums[fast] !== nums[slow]) {
            slow++;
            nums[slow] = nums[fast];
        }
    }
    
    return slow + 1;
}
```

```java
// Java
public int removeDuplicates(int[] nums) {
    if (nums.length == 0) return 0;
    
    int slow = 0;
    
    for (int fast = 1; fast < nums.length; fast++) {
        if (nums[fast] != nums[slow]) {
            slow++;
            nums[slow] = nums[fast];
        }
    }
    
    return slow + 1;
}
```

### Strategy 3: Sliding Window Pattern

**Template:**

```python
# Python
def sliding_window(arr):
    left = 0
    result = 0
    window_state = initialize_state()
    
    for right in range(len(arr)):
        # Expand window - add arr[right]
        update_state(window_state, arr[right])
        
        # Contract window while invalid
        while not is_valid(window_state):
            remove_from_state(window_state, arr[left])
            left += 1
        
        # Update result with current window
        result = max(result, right - left + 1)
    
    return result
```

```javascript
// JavaScript
function slidingWindow(arr) {
    let left = 0;
    let result = 0;
    let windowState = initializeState();
    
    for (let right = 0; right < arr.length; right++) {
        // Expand window - add arr[right]
        updateState(windowState, arr[right]);
        
        // Contract window while invalid
        while (!isValid(windowState)) {
            removeFromState(windowState, arr[left]);
            left++;
        }
        
        // Update result with current window
        result = Math.max(result, right - left + 1);
    }
    
    return result;
}
```

```java
// Java
public int slidingWindow(int[] arr) {
    int left = 0;
    int result = 0;
    StateType windowState = initializeState();
    
    for (int right = 0; right < arr.length; right++) {
        // Expand window - add arr[right]
        updateState(windowState, arr[right]);
        
        // Contract window while invalid
        while (!isValid(windowState)) {
            removeFromState(windowState, arr[left]);
            left++;
        }
        
        // Update result with current window
        result = Math.max(result, right - left + 1);
    }
    
    return result;
}
```

**Example: Longest Substring Without Repeating Characters**

```python
# Python
def length_of_longest_substring(s):
    char_set = set()
    left = 0
    max_length = 0
    
    for right in range(len(s)):
        # Contract window until no duplicates
        while s[right] in char_set:
            char_set.remove(s[left])
            left += 1
        
        # Add current character
        char_set.add(s[right])
        max_length = max(max_length, right - left + 1)
    
    return max_length
```

```javascript
// JavaScript
function lengthOfLongestSubstring(s) {
    const charSet = new Set();
    let left = 0;
    let maxLength = 0;
    
    for (let right = 0; right < s.length; right++) {
        // Contract window until no duplicates
        while (charSet.has(s[right])) {
            charSet.delete(s[left]);
            left++;
        }
        
        // Add current character
        charSet.add(s[right]);
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

```java
// Java
public int lengthOfLongestSubstring(String s) {
    Set<Character> charSet = new HashSet<>();
    int left = 0;
    int maxLength = 0;
    
    for (int right = 0; right < s.length(); right++) {
        // Contract window until no duplicates
        while (charSet.contains(s.charAt(right))) {
            charSet.remove(s.charAt(left));
            left++;
        }
        
        // Add current character
        charSet.add(s.charAt(right));
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

### Strategy 4: Fast and Slow Pattern

**Template:**

```python
# Python - Linked List
def fast_slow_linked_list(head):
    slow = head
    fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if meets_condition(slow, fast):
            return result
    
    return default_result
```

```javascript
// JavaScript - Linked List
function fastSlowLinkedList(head) {
    let slow = head;
    let fast = head;
    
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        
        if (meetsCondition(slow, fast)) {
            return result;
        }
    }
    
    return defaultResult;
}
```

```java
// Java - Linked List
public ResultType fastSlowLinkedList(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        
        if (meetsCondition(slow, fast)) {
            return result;
        }
    }
    
    return defaultResult;
}
```

**Example: Detect Cycle in Linked List**

```python
# Python
class ListNode:
    def __init__(self, val=0):
        self.val = val
        self.next = None

def has_cycle(head):
    if not head:
        return False
    
    slow = head
    fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:
            return True
    
    return False
```

```javascript
// JavaScript
class ListNode {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
    }
}

function hasCycle(head) {
    if (!head) return false;
    
    let slow = head;
    let fast = head;
    
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        
        if (slow === fast) {
            return true;
        }
    }
    
    return false;
}
```

```java
// Java
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; }
}

public boolean hasCycle(ListNode head) {
    if (head == null) return false;
    
    ListNode slow = head;
    ListNode fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        
        if (slow == fast) {
            return true;
        }
    }
    
    return false;
}
```

---

## Common Problem Types

### 1. Pair/Triplet Finding

**Problem:** Find two/three numbers that sum to target.

**Approach:**
- Sort the array first (if not sorted)
- Use opposite direction pointers
- For triplets, fix one element and use two pointers for the rest

**Example: Three Sum**

```python
# Python
def three_sum(nums):
    nums.sort()
    result = []
    
    for i in range(len(nums) - 2):
        # Skip duplicates for first element
        if i > 0 and nums[i] == nums[i-1]:
            continue
        
        left, right = i + 1, len(nums) - 1
        
        while left < right:
            total = nums[i] + nums[left] + nums[right]
            
            if total == 0:
                result.append([nums[i], nums[left], nums[right]])
                
                # Skip duplicates for second element
                while left < right and nums[left] == nums[left+1]:
                    left += 1
                # Skip duplicates for third element
                while left < right and nums[right] == nums[right-1]:
                    right -= 1
                
                left += 1
                right -= 1
            elif total < 0:
                left += 1
            else:
                right -= 1
    
    return result
```

```javascript
// JavaScript
function threeSum(nums) {
    nums.sort((a, b) => a - b);
    const result = [];
    
    for (let i = 0; i < nums.length - 2; i++) {
        // Skip duplicates for first element
        if (i > 0 && nums[i] === nums[i-1]) continue;
        
        let left = i + 1, right = nums.length - 1;
        
        while (left < right) {
            const total = nums[i] + nums[left] + nums[right];
            
            if (total === 0) {
                result.push([nums[i], nums[left], nums[right]]);
                
                // Skip duplicates
                while (left < right && nums[left] === nums[left+1]) left++;
                while (left < right && nums[right] === nums[right-1]) right--;
                
                left++;
                right--;
            } else if (total < 0) {
                left++;
            } else {
                right--;
            }
        }
    }
    
    return result;
}
```

```java
// Java
public List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> result = new ArrayList<>();
    
    for (int i = 0; i < nums.length - 2; i++) {
        // Skip duplicates for first element
        if (i > 0 && nums[i] == nums[i-1]) continue;
        
        int left = i + 1, right = nums.length - 1;
        
        while (left < right) {
            int total = nums[i] + nums[left] + nums[right];
            
            if (total == 0) {
                result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                
                // Skip duplicates
                while (left < right && nums[left] == nums[left+1]) left++;
                while (left < right && nums[right] == nums[right-1]) right--;
                
                left++;
                right--;
            } else if (total < 0) {
                left++;
            } else {
                right--;
            }
        }
    }
    
    return result;
}
```

### 2. Palindrome Problems

**Problem:** Check if string is palindrome or find longest palindrome.

**Approach:**
- Use opposite direction pointers
- Compare characters from both ends
- Move inward until they meet

**Example: Valid Palindrome**

```python
# Python
def is_palindrome(s):
    # Clean string: lowercase and alphanumeric only
    cleaned = ''.join(c.lower() for c in s if c.isalnum())
    
    left, right = 0, len(cleaned) - 1
    
    while left < right:
        if cleaned[left] != cleaned[right]:
            return False
        left += 1
        right -= 1
    
    return True
```

```javascript
// JavaScript
function isPalindrome(s) {
    // Clean string: lowercase and alphanumeric only
    const cleaned = s.toLowerCase().replace(/[^a-z0-9]/g, '');
    
    let left = 0, right = cleaned.length - 1;
    
    while (left < right) {
        if (cleaned[left] !== cleaned[right]) {
            return false;
        }
        left++;
        right--;
    }
    
    return true;
}
```

```java
// Java
public boolean isPalindrome(String s) {
    // Clean string: lowercase and alphanumeric only
    String cleaned = s.toLowerCase().replaceAll("[^a-z0-9]", "");
    
    int left = 0, right = cleaned.length() - 1;
    
    while (left < right) {
        if (cleaned.charAt(left) != cleaned.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }
    
    return true;
}
```

### 3. Container/Water Problems

**Problem:** Find maximum area or volume that can be contained.

**Approach:**
- Use opposite direction pointers
- Calculate area/volume at each step
- Move pointer with smaller height (greedy choice)

**Example: Container With Most Water**

```python
# Python
def max_area(height):
    left, right = 0, len(height) - 1
    max_water = 0
    
    while left < right:
        # Calculate current area
        width = right - left
        current_height = min(height[left], height[right])
        current_area = width * current_height
        max_water = max(max_water, current_area)
        
        # Move pointer with smaller height
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
    
    return max_water
```

```javascript
// JavaScript
function maxArea(height) {
    let left = 0, right = height.length - 1;
    let maxWater = 0;
    
    while (left < right) {
        // Calculate current area
        const width = right - left;
        const currentHeight = Math.min(height[left], height[right]);
        const currentArea = width * currentHeight;
        maxWater = Math.max(maxWater, currentArea);
        
        // Move pointer with smaller height
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }
    
    return maxWater;
}
```

```java
// Java
public int maxArea(int[] height) {
    int left = 0, right = height.length - 1;
    int maxWater = 0;
    
    while (left < right) {
        // Calculate current area
        int width = right - left;
        int currentHeight = Math.min(height[left], height[right]);
        int currentArea = width * currentHeight;
        maxWater = Math.max(maxWater, currentArea);
        
        // Move pointer with smaller height
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }
    
    return maxWater;
}
```

### 4. Partitioning/Segregation

**Problem:** Partition array based on a condition (e.g., even/odd, colors, pivot).

**Approach:**
- Use same direction pointers
- Slow pointer tracks position for next valid element
- Fast pointer explores array

**Example: Move Zeros to End**

```python
# Python
def move_zeros(nums):
    slow = 0  # Position for next non-zero
    
    for fast in range(len(nums)):
        if nums[fast] != 0:
            # Swap non-zero element to slow position
            nums[slow], nums[fast] = nums[fast], nums[slow]
            slow += 1
    
    return nums
```

```javascript
// JavaScript
function moveZeros(nums) {
    let slow = 0;  // Position for next non-zero
    
    for (let fast = 0; fast < nums.length; fast++) {
        if (nums[fast] !== 0) {
            // Swap non-zero element to slow position
            [nums[slow], nums[fast]] = [nums[fast], nums[slow]];
            slow++;
        }
    }
    
    return nums;
}
```

```java
// Java
public void moveZeros(int[] nums) {
    int slow = 0;  // Position for next non-zero
    
    for (int fast = 0; fast < nums.length; fast++) {
        if (nums[fast] != 0) {
            // Swap non-zero element to slow position
            int temp = nums[slow];
            nums[slow] = nums[fast];
            nums[fast] = temp;
            slow++;
        }
    }
}
```

### 5. Linked List Problems

**Problem:** Find middle, detect cycles, remove nth from end.

**Approach:**
- Use fast and slow pointers
- Fast moves 2x speed of slow
- When fast reaches end, slow is at middle

**Example: Find Middle of Linked List**

```python
# Python
def find_middle(head):
    slow = fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    return slow  # Middle node
```

```javascript
// JavaScript
function findMiddle(head) {
    let slow = head, fast = head;
    
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    return slow;  // Middle node
}
```

```java
// Java
public ListNode findMiddle(ListNode head) {
    ListNode slow = head, fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    return slow;  // Middle node
}
```

---

## Time and Space Complexity

### Typical Complexities

| Pattern | Time Complexity | Space Complexity | Notes |
|---------|----------------|------------------|-------|
| Opposite Direction | O(n) | O(1) | Single pass from both ends |
| Same Direction | O(n) | O(1) | Single pass, in-place |
| Sliding Window | O(n) | O(k) | k = window state size |
| Fast/Slow | O(n) | O(1) | May traverse list twice |
| With Sorting | O(n log n) | O(1) or O(n) | Depends on sort algorithm |

### Complexity Analysis Examples

**Two Sum (Sorted Array):**
- Time: O(n) - single pass with two pointers
- Space: O(1) - only pointer variables

**Three Sum:**
- Time: O(n²) - O(n) for outer loop × O(n) for two pointers
- Space: O(1) - ignoring output space

**Longest Substring Without Repeating:**
- Time: O(n) - each character visited at most twice
- Space: O(min(n, m)) - where m is charset size

---

## Problem-Solving Framework

### Step-by-Step Approach

#### 1. Identify the Pattern

Ask yourself:
- Is the data sorted or can it be sorted?
- Do I need to find pairs/triplets?
- Am I looking for a subarray/substring?
- Is this about partitioning or removing elements?
- Does the problem involve a linked list?

#### 2. Choose the Right Variant

Match problem characteristics to pattern:
- **Sorted + pairs** → Opposite direction
- **In-place modification** → Same direction
- **Substring/subarray** → Sliding window
- **Linked list cycles** → Fast/slow

#### 3. Define Pointer Initialization

Decide starting positions:
```python
# Opposite direction
left, right = 0, len(arr) - 1

# Same direction
slow, fast = 0, 0

# Sliding window
start, end = 0, 0

# Fast/slow linked list
slow = fast = head
```

#### 4. Define Movement Logic

Determine when to move each pointer:
- What condition triggers left/slow movement?
- What condition triggers right/fast movement?
- Should both move simultaneously?

#### 5. Define Termination Condition

When should the loop stop?
```python
# Opposite direction
while left < right:

# Same direction
for fast in range(len(arr)):

# Sliding window
while end < len(arr):

# Fast/slow
while fast and fast.next:
```

#### 6. Handle Edge Cases

Consider:
- Empty input
- Single element
- All elements same
- No valid answer exists

### Decision Tree for Pattern Selection

```
Start
  │
  ├─ Is data sorted?
  │   ├─ Yes → Looking for pairs/sum?
  │   │         ├─ Yes → Opposite Direction
  │   │         └─ No → Check other factors
  │   └─ No → Can you sort it efficiently?
  │            ├─ Yes → Consider sorting + two pointers
  │            └─ No → Check if same direction works
  │
  ├─ Need to modify array in-place?
  │   └─ Yes → Same Direction
  │
  ├─ Finding substring/subarray with property?
  │   └─ Yes → Sliding Window
  │
  └─ Working with linked list?
      └─ Yes → Fast/Slow
```

---

## Advanced Techniques

### 1. Three Pointers

Sometimes problems require three simultaneous pointers.

**Example: Dutch National Flag (Sort Colors)**

```python
# Python - Sort array of 0s, 1s, 2s
def sort_colors(nums):
    low, mid, high = 0, 0, len(nums) - 1
    
    while mid <= high:
        if nums[mid] == 0:
            nums[low], nums[mid] = nums[mid], nums[low]
            low += 1
            mid += 1
        elif nums[mid] == 1:
            mid += 1
        else:  # nums[mid] == 2
            nums[mid], nums[high] = nums[high], nums[mid]
            high -= 1
    
    return nums
```

```javascript
// JavaScript
function sortColors(nums) {
    let low = 0, mid = 0, high = nums.length - 1;
    
    while (mid <= high) {
        if (nums[mid] === 0) {
            [nums[low], nums[mid]] = [nums[mid], nums[low]];
            low++;
            mid++;
        } else if (nums[mid] === 1) {
            mid++;
        } else {  // nums[mid] === 2
            [nums[mid], nums[high]] = [nums[high], nums[mid]];
            high--;
        }
    }
    
    return nums;
}
```

```java
// Java
public void sortColors(int[] nums) {
    int low = 0, mid = 0, high = nums.length - 1;
    
    while (mid <= high) {
        if (nums[mid] == 0) {
            int temp = nums[low];
            nums[low] = nums[mid];
            nums[mid] = temp;
            low++;
            mid++;
        } else if (nums[mid] == 1) {
            mid++;
        } else {  // nums[mid] == 2
            int temp = nums[mid];
            nums[mid] = nums[high];
            nums[high] = temp;
            high--;
        }
    }
}
```

### 2. Pointers with Different Speeds

**Example: Remove Nth Node From End of List**

```python
# Python
def remove_nth_from_end(head, n):
    dummy = ListNode(0)
    dummy.next = head
    fast = slow = dummy
    
    # Move fast n steps ahead
    for _ in range(n):
        fast = fast.next
    
    # Move both until fast reaches end
    while fast.next:
        fast = fast.next
        slow = slow.next
    
    # Remove nth node
    slow.next = slow.next.next
    
    return dummy.next
```

```javascript
// JavaScript
function removeNthFromEnd(head, n) {
    const dummy = new ListNode(0);
    dummy.next = head;
    let fast = dummy, slow = dummy;
    
    // Move fast n steps ahead
    for (let i = 0; i < n; i++) {
        fast = fast.next;
    }
    
    // Move both until fast reaches end
    while (fast.next) {
        fast = fast.next;
        slow = slow.next;
    }
    
    // Remove nth node
    slow.next = slow.next.next;
    
    return dummy.next;
}
```

```java
// Java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode fast = dummy, slow = dummy;
    
    // Move fast n steps ahead
    for (int i = 0; i < n; i++) {
        fast = fast.next;
    }
    
    // Move both until fast reaches end
    while (fast.next != null) {
        fast = fast.next;
        slow = slow.next;
    }
    
    // Remove nth node
    slow.next = slow.next.next;
    
    return dummy.next;
}
```

### 3. Combining Two Pointers with Hash Map

For problems requiring both O(n) lookup and two-pointer logic.

**Example: Subarray Sum Equals K**

```python
# Python
def subarray_sum(nums, k):
    count = 0
    current_sum = 0
    sum_freq = {0: 1}  # Handle subarrays starting from index 0
    
    for num in nums:
        current_sum += num
        
        # Check if (current_sum - k) exists
        if current_sum - k in sum_freq:
            count += sum_freq[current_sum - k]
        
        # Add current sum to map
        sum_freq[current_sum] = sum_freq.get(current_sum, 0) + 1
    
    return count
```

```javascript
// JavaScript
function subarraySum(nums, k) {
    let count = 0;
    let currentSum = 0;
    const sumFreq = new Map([[0, 1]]);  // Handle subarrays from index 0
    
    for (const num of nums) {
        currentSum += num;
        
        // Check if (currentSum - k) exists
        if (sumFreq.has(currentSum - k)) {
            count += sumFreq.get(currentSum - k);
        }
        
        // Add current sum to map
        sumFreq.set(currentSum, (sumFreq.get(currentSum) || 0) + 1);
    }
    
    return count;
}
```

```java
// Java
public int subarraySum(int[] nums, int k) {
    int count = 0;
    int currentSum = 0;
    Map<Integer, Integer> sumFreq = new HashMap<>();
    sumFreq.put(0, 1);  // Handle subarrays from index 0
    
    for (int num : nums) {
        currentSum += num;
        
        // Check if (currentSum - k) exists
        if (sumFreq.containsKey(currentSum - k)) {
            count += sumFreq.get(currentSum - k);
        }
        
        // Add current sum to map
        sumFreq.put(currentSum, sumFreq.getOrDefault(currentSum, 0) + 1);
    }
    
    return count;
}
```

### 4. Binary Search + Two Pointers

Combine binary search efficiency with two-pointer validation.

**Example: Find K Closest Elements**

```python
# Python
def find_closest_elements(arr, k, x):
    # Binary search for insertion point
    left, right = 0, len(arr) - k
    
    while left < right:
        mid = (left + right) // 2
        # Compare distances from x
        if x - arr[mid] > arr[mid + k] - x:
            left = mid + 1
        else:
            right = mid
    
    return arr[left:left + k]
```

```javascript
// JavaScript
function findClosestElements(arr, k, x) {
    // Binary search for insertion point
    let left = 0, right = arr.length - k;
    
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        // Compare distances from x
        if (x - arr[mid] > arr[mid + k] - x) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return arr.slice(left, left + k);
}
```

```java
// Java
public List<Integer> findClosestElements(int[] arr, int k, int x) {
    // Binary search for insertion point
    int left = 0, right = arr.length - k;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        // Compare distances from x
        if (x - arr[mid] > arr[mid + k] - x) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    List<Integer> result = new ArrayList<>();
    for (int i = left; i < left + k; i++) {
        result.add(arr[i]);
    }
    return result;
}
```

### 5. Backtracking with Two Pointers

For problems requiring exploration of multiple possibilities.

**Example: Trapping Rain Water**

```python
# Python
def trap_rain_water(height):
    if not height:
        return 0
    
    left, right = 0, len(height) - 1
    left_max, right_max = height[left], height[right]
    water = 0
    
    while left < right:
        if left_max < right_max:
            left += 1
            left_max = max(left_max, height[left])
            water += left_max - height[left]
        else:
            right -= 1
            right_max = max(right_max, height[right])
            water += right_max - height[right]
    
    return water
```

```javascript
// JavaScript
function trapRainWater(height) {
    if (height.length === 0) return 0;
    
    let left = 0, right = height.length - 1;
    let leftMax = height[left], rightMax = height[right];
    let water = 0;
    
    while (left < right) {
        if (leftMax < rightMax) {
            left++;
            leftMax = Math.max(leftMax, height[left]);
            water += leftMax - height[left];
        } else {
            right--;
            rightMax = Math.max(rightMax, height[right]);
            water += rightMax - height[right];
        }
    }
    
    return water;
}
```

```java
// Java
public int trapRainWater(int[] height) {
    if (height.length == 0) return 0;
    
    int left = 0, right = height.length - 1;
    int leftMax = height[left], rightMax = height[right];
    int water = 0;
    
    while (left < right) {
        if (leftMax < rightMax) {
            left++;
            leftMax = Math.max(leftMax, height[left]);
            water += leftMax - height[left];
        } else {
            right--;
            rightMax = Math.max(rightMax, height[right]);
            water += rightMax - height[right];
        }
    }
    
    return water;
}
```

---

## Practice Problems

### Beginner Level

1. **Two Sum II - Input Array Is Sorted**
   - Pattern: Opposite Direction
   - Difficulty: Easy
   - Key: Adjust pointers based on sum comparison

2. **Valid Palindrome**
   - Pattern: Opposite Direction
   - Difficulty: Easy
   - Key: Skip non-alphanumeric characters

3. **Remove Duplicates from Sorted Array**
   - Pattern: Same Direction
   - Difficulty: Easy
   - Key: Slow pointer tracks unique position

4. **Merge Sorted Array**
   - Pattern: Opposite Direction (from end)
   - Difficulty: Easy
   - Key: Fill from right to avoid overwrites

5. **Move Zeroes**
   - Pattern: Same Direction
   - Difficulty: Easy
   - Key: Swap non-zero elements forward

### Intermediate Level

6. **3Sum**
   - Pattern: Opposite Direction + Loop
   - Difficulty: Medium
   - Key: Fix one element, two pointers for rest

7. **Container With Most Water**
   - Pattern: Opposite Direction
   - Difficulty: Medium
   - Key: Move pointer with smaller height

8. **Longest Substring Without Repeating Characters**
   - Pattern: Sliding Window
   - Difficulty: Medium
   - Key: Expand right, contract left on duplicates

9. **Find the Duplicate Number**
   - Pattern: Fast/Slow (Floyd's Cycle Detection)
   - Difficulty: Medium
   - Key: Treat array as linked list

10. **Sort Colors**
    - Pattern: Three Pointers
    - Difficulty: Medium
    - Key: Dutch National Flag algorithm

11. **Minimum Size Subarray Sum**
    - Pattern: Sliding Window
    - Difficulty: Medium
    - Key: Expand until valid, then contract

12. **Linked List Cycle II**
    - Pattern: Fast/Slow
    - Difficulty: Medium
    - Key: Find meeting point, then find start

### Advanced Level

13. **Trapping Rain Water**
    - Pattern: Opposite Direction
    - Difficulty: Hard
    - Key: Track max heights from both sides

14. **Minimum Window Substring**
    - Pattern: Sliding Window + Hash Map
    - Difficulty: Hard
    - Key: Expand until valid, contract to minimize

15. **Longest Duplicate Substring**
    - Pattern: Binary Search + Sliding Window
    - Difficulty: Hard
    - Key: Binary search on length, validate with window

16. **Subarrays with K Different Integers**
    - Pattern: Sliding Window
    - Difficulty: Hard
    - Key: Count(at most K) - Count(at most K-1)

17. **Median of Two Sorted Arrays**
    - Pattern: Binary Search + Two Pointers
    - Difficulty: Hard
    - Key: Partition arrays for equal halves

### Problem-Solving Tips

**For Each Problem:**

1. **Read Carefully**: Identify constraints and requirements
2. **Draw Examples**: Visualize with small test cases
3. **Identify Pattern**: Match to one of the four main patterns
4. **Plan Before Coding**: Outline pointer movements
5. **Handle Edge Cases**: Empty, single element, all same
6. **Test Incrementally**: Verify with multiple test cases
7. **Optimize**: Look for unnecessary operations

---

## Common Pitfalls and How to Avoid Them

### 1. Off-by-One Errors

**Problem:** Incorrect loop conditions or pointer updates.

```python
# ❌ Wrong
while left <= right:  # May process same element twice
    
# ✅ Correct
while left < right:  # Stops when pointers meet
```

### 2. Not Handling Duplicates

**Problem:** Results include duplicate triplets/pairs.

```python
# ❌ Wrong - doesn't skip duplicates
for i in range(len(nums)):
    # ... two pointer logic

# ✅ Correct - skip duplicates
for i in range(len(nums)):
    if i > 0 and nums[i] == nums[i-1]:
        continue
    # ... two pointer logic
```

### 3. Forgetting to Sort

**Problem:** Two pointers require sorted data but array isn't sorted.

```python
# ❌ Wrong - using two pointers on unsorted array
def two_sum(nums, target):
    left, right = 0, len(nums) - 1
    # This won't work correctly!

# ✅ Correct - sort first if needed
def two_sum_sorted(nums, target):
    nums.sort()  # Or ensure input is sorted
    left, right = 0, len(nums) - 1
```

### 4. Incorrect Window Expansion/Contraction

**Problem:** Not maintaining valid window state.

```python
# ❌ Wrong - doesn't maintain valid window
while right < len(arr):
    add_to_window(arr[right])
    if not valid():
        left += 1  # Only moves once!

# ✅ Correct - contract until valid
while right < len(arr):
    add_to_window(arr[right])
    while not valid():  # Keep contracting
        remove_from_window(arr[left])
        left += 1
```

### 5. Infinite Loops in Linked Lists

**Problem:** Not checking for null pointers.

```python
# ❌ Wrong - crashes on odd-length lists
while fast.next:  # What if fast is None?
    fast = fast.next.next

# ✅ Correct - check both conditions
while fast and fast.next:
    fast = fast.next.next
```

### 6. Modifying Array While Iterating

**Problem:** Unexpected behavior when array changes during iteration.

```python
# ❌ Wrong - modifying during iteration
for i in range(len(arr)):
    if condition:
        arr.pop(i)  # Changes indices!

# ✅ Correct - use two pointers
slow = 0
for fast in range(len(arr)):
    if keep_condition(arr[fast]):
        arr[slow] = arr[fast]
        slow += 1
```

---

## Optimization Techniques

### 1. Early Termination

Stop processing when answer is found or impossible.

```python
# Early termination in 3Sum
if nums[i] > 0:
    break  # All remaining numbers are positive
```

### 2. Skip Redundant Work

Avoid processing duplicate elements.

```python
# Skip duplicates efficiently
while left < right and nums[left] == nums[left + 1]:
    left += 1
```

### 3. Precomputation

Calculate values before pointer movement.

```python
# Precompute prefix sums for range queries
prefix_sum = [0]
for num in nums:
    prefix_sum.append(prefix_sum[-1] + num)
```

### 4. Reverse Thinking

Sometimes iterating backwards is more efficient.

```python
# Merge sorted arrays from end to avoid overwrites
while p1 >= 0 and p2 >= 0:
    if nums1[p1] > nums2[p2]:
        nums1[p] = nums1[p1]
        p1 -= 1
    else:
        nums1[p] = nums2[p2]
        p2 -= 1
    p -= 1
```

---

## Testing Strategies

### Essential Test Cases

1. **Empty Input**: `[]`
2. **Single Element**: `[1]`
3. **Two Elements**: `[1, 2]`
4. **All Same**: `[1, 1, 1, 1]`
5. **Already Sorted**: `[1, 2, 3, 4]`
6. **Reverse Sorted**: `[4, 3, 2, 1]`
7. **No Solution**: When target can't be found
8. **Multiple Solutions**: Verify all are found
9. **Minimum Values**: `[INT_MIN, ...]`
10. **Maximum Values**: `[..., INT_MAX]`

### Debugging Checklist

- [ ] Are pointers initialized correctly?
- [ ] Is the loop condition correct (`<` vs `<=`)?
- [ ] Are both pointers updated properly?
- [ ] Are edge cases handled?
- [ ] Is the array sorted when required?
- [ ] Are duplicates handled if needed?
- [ ] Is the window state maintained correctly?
- [ ] Are null checks in place for linked lists?

---

## Comparison with Other Techniques

| Technique | Time | Space | Use Case |
|-----------|------|-------|----------|
| Two Pointers | O(n) | O(1) | Sorted data, pairs, in-place |
| Hash Map | O(n) | O(n) | Unsorted data, any lookup |
| Binary Search | O(log n) | O(1) | Sorted, single element search |
| Sliding Window | O(n) | O(k) | Contiguous subarrays |
| Dynamic Programming | O(n²) | O(n) | Overlapping subproblems |

### When to Choose Two Pointers Over Others

**Choose Two Pointers:**
- Data is sorted or can be sorted efficiently
- Need O(1) space complexity
- Looking for pairs/relationships between elements
- In-place modification required

**Choose Hash Map Instead:**
- Data cannot be sorted
- Need O(1) lookup for arbitrary keys
- Order doesn't matter

**Choose Sliding Window Instead:**
- Need to track contiguous subarray state
- Window size varies dynamically
- Optimizing substring/subarray problems

---

## Real-World Applications

1. **Database Query Optimization**: Merge operations on sorted result sets
2. **Memory Management**: Compacting fragmented memory blocks
3. **Network Packet Processing**: Finding pairs of request-response packets
4. **Data Deduplication**: Removing duplicate entries efficiently
5. **String Matching**: Finding patterns in DNA sequences
6. **Computer Graphics**: Polygon intersection detection
7. **Financial Analysis**: Finding trading opportunities in sorted price data
8. **Image Processing**: Edge detection and contour finding

---

## Conclusion

The Two Pointers technique is a fundamental algorithmic pattern that every programmer should master. Its elegance lies in its simplicity and efficiency.

### Key Takeaways

✅ **Four Main Patterns**: Opposite direction, same direction, sliding window, fast/slow
✅ **When to Use**: Sorted data, pairs/triplets, in-place operations, linked lists
✅ **Complexity**: Usually O(n) time with O(1) space
✅ **Versatility**: Applicable to arrays, strings, and linked lists
✅ **Foundation**: Understanding this builds intuition for more complex algorithms

### Next Steps

1. **Practice Daily**: Solve 2-3 problems using different patterns
2. **Recognize Patterns**: Train yourself to identify when two pointers apply
3. **Optimize**: Try converting O(n²) solutions to O(n) using two pointers
4. **Combine Techniques**: Learn to use with binary search, hash maps, etc.
5. **Teach Others**: Explaining concepts solidifies understanding

### Resources for Further Learning

- LeetCode Two Pointers Tag
- AlgoExpert Two Pointers Section
- Cracking the Coding Interview - Chapter 7
- Elements of Programming Interviews - Arrays Chapter

---

## Quick Reference Cheat Sheet

```python
# Template Selection Guide

# 1. OPPOSITE DIRECTION (Converging)
left, right = 0, len(arr) - 1
while left < right:
    if condition: return result
    elif move_left: left += 1
    else: right -= 1

# 2. SAME DIRECTION (Chasing)
slow = 0
for fast in range(len(arr)):
    if condition: arr[slow] = arr[fast]; slow += 1

# 3. SLIDING WINDOW
left = 0
for right in range(len(arr)):
    expand_window(right)
    while invalid(): contract_window(left); left += 1

# 4. FAST/SLOW (Linked List)
slow = fast = head
while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
    if slow == fast: return True
```

**Remember**: The key to mastering two pointers is recognizing the pattern, not memorizing code!
