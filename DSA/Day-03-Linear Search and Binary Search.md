## Linear Search Algorithm

Given an array, arr[] of n integers, and an integer element x, find whether element x is present in the array. Return the index of the first occurrence of x in the array, or -1 if it doesn't exist.

### Examples

**Input:** arr[] = [1, 2, 3, 4], x = 3  
**Output:** 2  
**Explanation:** There is one test case with array as [1, 2, 3 4] and element to be searched as 3. Since 3 is present at index 2, the output is 2.

**Input:** arr[] = [10, 8, 30, 4, 5], x = 5  
**Output:** 4  
**Explanation:** For array [10, 8, 30, 4, 5], the element to be searched is 5 and it is at index 4. So, the output is 4.

**Input:** arr[] = [10, 8, 30], x = 6  
**Output:** -1  
**Explanation:** The element to be searched is 6 and its not present, so we return -1.

### Implementation

```java
class GFG {
    public static int search(int arr[], int N, int x)
    {
        for (int i = 0; i < N; i++) {
            if (arr[i] == x)
                return i;
        }
        return -1;
    }

    public static void main(String args[])
    {
        int arr[] = { 2, 3, 4, 10, 40 };
        int x = 10;

        int result = search(arr, arr.length, x);
        if (result == -1)
            System.out.print("Element is not present in array");
        else
            System.out.print("Element is present at index " + result);
    }
}
```

**Output:**
```
Present at Index 3
```

### Time and Space Complexity

**Time Complexity:**
- **Best Case:** O(1)
- **Worst Case:** O(N)
- **Average Case:** O(N)

**Auxiliary Space:** O(1)

### Applications

- Unsorted Lists: When we have an unsorted array or list, linear search is most commonly used to find any element in the collection.
- Small Data Sets: Linear Search is preferred over binary search when we have small data sets
- Searching Linked Lists: In linked list implementations, linear search is commonly used to find elements within the list. Each node is checked sequentially until the desired element is found.
- Simple Implementation: Linear Search is much easier to understand and implement as compared to Binary Search or Ternary Search.

### Advantages

- Linear search can be used irrespective of whether the array is sorted or not. It can be used on arrays of any data type.
- Does not require any additional memory.
- It is a well-suited algorithm for small datasets.

### Disadvantages

- Linear search has a time complexity of O(N), which in turn makes it slow for large datasets.
- Not suitable for large arrays.

### When to use Linear Search Algorithm?

- When we are dealing with a small dataset.
- When you are searching for a dataset stored in contiguous memory.

---

## Binary Search Algorithm

Binary Search is a searching algorithm that operates on a sorted or monotonic search space, repeatedly dividing it into halves to find a target value or optimal answer in logarithmic time O(log N).

### Conditions to Apply Binary Search

To apply Binary Search algorithm:
- The data structure must be sorted.
- Access to any element of the data structure should take constant time.

### Algorithm Steps

1. Divide the search space into two halves by finding the middle index "mid".
2. Compare the middle element of the search space with the key.
3. If the key is found at middle element, the process is terminated.
4. If the key is not found at middle element, choose which half will be used as the next search space.
   - If the key is smaller than the middle element, then the left side is used for next search.
   - If the key is larger than the middle element, then the right side is used for next search.
5. This process is continued until the key is found or the total search space is exhausted.

### How Binary Search Works

Consider an array arr[] = {2, 5, 8, 12, 16, 23, 38, 56, 72, 91}, and the target = 23.

### Iterative Implementation

```java
class GFG {
  
    static int binarySearch(int arr[], int x) {
        int low = 0, high = arr.length - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (arr[mid] == x)
                return mid;

            if (arr[mid] < x)
                low = mid + 1;

            else
                high = mid - 1;
        }

        return -1;
    }

    public static void main(String args[]) {
        int arr[] = { 2, 3, 4, 10, 40 };
        int x = 10;
        int result = binarySearch(arr, x);
        if (result == -1)
            System.out.println("Element is not present in array");
        else
            System.out.println("Element is present at index " + result);
    }
}
```

**Output:**
```
Element is present at index 3
```

### Recursive Implementation

```java
class GFG {
    
    static int binarySearch(int arr[], int low, int high, int x) {
        if (high >= low) {
            int mid = low + (high - low) / 2;

            if (arr[mid] == x)
                return mid;

            if (arr[mid] > x)
                return binarySearch(arr, low, mid - 1, x);

            return binarySearch(arr, mid + 1, high, x);
        }

        return -1;
    }

    public static void main(String args[])
    {
        int arr[] = { 2, 3, 4, 10, 40 };
        int n = arr.length;
        int x = 10;
        int result = binarySearch(arr, 0, n - 1, x);
        if (result == -1)
            System.out.println("Element is not present in array");
        else
            System.out.println("Element is present at index " + result);
    }
}
```

**Output:**
```
Element is present at index 3
```

### Complexity Analysis

**Time Complexity:**
- **Best Case:** O(1)
- **Average Case:** O(log N)
- **Worst Case:** O(log N)

**Auxiliary Space:** O(1), If the recursive call stack is considered then the auxiliary space will be O(log N).

### Applications

- Searching in sorted arrays
- Finding first/last occurrence or closest match in a sorted array
- Database indexing — Used in B-trees and similar structures for fast data lookup.
- Debugging in version control — Tools like git bisect use binary search to isolate faulty commits.
- Network routing & IP lookup — Efficiently find routing entries in tables sorted by address ranges.
- File systems & libraries — Fast search through sorted directories or symbol tables.
- Gaming/graphics — Collision detection or ray tracing using sorted spatial data.
- Machine learning tuning — Efficient hyperparameter search (e.g., learning rate, thresholds).
- Optimization problems & competitive programming — Solve boundary-value challenges by narrowing search space.
- Advanced data structures — Binary search trees, self-balancing BSTs, and fractional cascading rely on search logic.

---

**Last Updated:** October 23, 2025