# Traversal in Array

**Last Updated:** 19 Feb, 2025

Traversal in an array refers to the process of accessing each element in the array sequentially, typically to perform a specific operation, such as searching, sorting, or modifying the elements.

Traversing an array is essential for a wide range of tasks in programming, and the most common methods of traversal include iterating through the array using loops like for, while, or foreach. Efficient traversal plays a critical role in algorithms that require scanning or manipulating data.

## Examples

**Input:** arr[] = [10, 20, 30, 40, 50]  
**Output:** "10 20 30 40 50 "  
**Explanation:** Just traverse and print the numbers.

**Input:** arr[] = [7, 8, 9, 1, 2]  
**Output:** "7 8 9 1 2 "  
**Explanation:** Just traverse and print the numbers.

**Input:** arr[] = [100, 200, 300, 400, 500]  
**Output:** "100 200 300 400 500 "  
**Explanation:** Just traverse and print the numbers.

## Types of Array Traversal

There are mainly two types of array traversal:

### 1. Linear Traversal

Linear traversal is the process of visiting each element of an array sequentially, starting from the first element and moving to the last element. During this traversal, each element is processed (printed, modified, or checked) one after the other, in the order they are stored in the array. This is the most common and straightforward way of accessing the elements of an array.

```java
public class Main {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5};
        int n = arr.length;

        System.out.print("Linear Traversal: ");
        for(int i = 0; i < n; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
    }
}
```

**Output:**
```
Linear Traversal: 1 2 3 4 5 
```

**Time Complexity:** O(n)  
**Auxiliary Space:** O(1)

### 2. Reverse Traversal

Reverse traversal is the process of visiting each element of an array starting from the last element and moving towards the first element. This method is useful when you need to process the elements of an array in reverse order. In this type of traversal, you begin from the last index (the rightmost element) and work your way to the first index (the leftmost element).

```java
class Main {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5};
        int n = arr.length;

        System.out.print("Reverse Traversal: ");
        for (int i = n - 1; i >= 0; i--) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
    }
}
```

**Output:**
```
Reverse Traversal: 5 4 3 2 1 
```

**Time Complexity:** O(n)  
**Auxiliary Space:** O(1)

## Methods of Array Traversal

### 1. Using For Loop

A for loop is a control structure that allows you to iterate over an array by specifying an initial index, a condition, and an update to the index after each iteration. It is the most common and efficient way to traverse an array because the loop can be easily controlled by setting the range of the index. This method is ideal when you know the number of elements in the array beforehand or when you need to access each element by its index.

```java
public class Main {
    public static void main(String[] args) {
        int[] arr = {10, 20, 30, 40, 50};
        int n = arr.length;

        System.out.print("Traversal using for loop: ");
        for(int i = 0; i < n; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
    }
}
```

**Output:**
```
Traversal using for loop: 10 20 30 40 50 
```

**Time Complexity:** O(n)  
**Auxiliary Space:** O(1)

### 2. Using While Loop

A while loop is another method to traverse an array, where the loop continues to execute as long as the specified condition is true. The key difference with the for loop is that the loop control is more manual, and the counter or index is updated inside the loop body rather than automatically. The while loop can be useful when the number of iterations isn't predetermined or when the loop may need to stop based on a condition other than the array size.

```java
public class Main {
    public static void main(String[] args) {
        int[] arr = {10, 20, 30, 40, 50};
        int n = arr.length;
        int i = 0;

        System.out.print("Traversal using while loop: ");
        while(i < n) {
            System.out.print(arr[i] + " ");
            i++;
        }
        System.out.println();
    }
}
```

**Output:**
```
Traversal using while loop: 10 20 30 40 50 
```

**Time Complexity:** O(n)  
**Auxiliary Space:** O(1)

### 3. Using Foreach Loop (Range-based For Loop)

The foreach loop, also known as a range-based for loop, simplifies array traversal by directly accessing each element of the array without needing to manually manage the index. It is often used for its simplicity and readability when you just need to access or process each item in the array.

```java
public class Main {
    public static void main(String[] args) {
        int[] arr = {10, 20, 30, 40, 50};

        System.out.print("Traversal using foreach loop: ");
        for(int value : arr) {
            System.out.print(value + " ");
        }
        System.out.println();
    }
}
```

**Output:**
```
Traversal using foreach (range-based for) loop: 10 20 30 40 50 
```

**Time Complexity:** O(n)  
**Auxiliary Space:** O(1)

## Applications of Array Traversal

Array traversal plays a key role in many operations where we need to access or modify the elements of an array. Two of the most common applications are searching elements and modifying elements.

### 1. Searching Elements

Traversal is often used to search for an element in an array. By visiting each element of the array sequentially, we can compare each element with the target value to determine if it exists.

**Example:** If you have an array of numbers, and you need to find if a specific number is present in the array, array traversal would involve checking each element one by one until a match is found or the entire array has been searched.

```java
public class Main {
    public static void main(String[] args) {
        int[] arr = {10, 20, 30, 40, 50};
        int target = 30;
        boolean found = false;

        // Linear search using traversal
        for(int i = 0; i < arr.length; i++) {
            if(arr[i] == target) {
                found = true;
                break;
            }
        }

        if(found) {
            System.out.println("Element found!");
        } else {
            System.out.println("Element not found!");
        }
    }
}
```

**Output:**
```
Element found!
```

**Time Complexity:** O(n)  
**Auxiliary Space:** O(1)

### 2. Modifying Elements

Array traversal is also used to modify the elements of the array. This could involve updating values, changing the order, or applying some mathematical operations to each element. For example, you can multiply every element of an array by a constant, or increase each value by 1.

**Example:** If you want to increase each element in an array by 5, array traversal allows you to visit each element and modify it.

```java
public class Main {
    public static void main(String[] args) {
        int[] arr = {10, 20, 30, 40, 50};
        int n = arr.length;

        // Modifying elements using traversal (increasing each by 5)
        for(int i = 0; i < n; i++) {
            arr[i] += 5;
        }

        // Print modified array
        System.out.print("Modified array: ");
        for(int i = 0; i < n; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
    }
}
```

**Output:**
```
Modified array: 15 25 35 45 55 
```

**Time Complexity:** O(n)  
**Auxiliary Space:** O(1)