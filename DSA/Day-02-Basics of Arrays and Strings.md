# Day 2 – Basics of Arrays and Strings in Java

---

## 📘 Introduction

Today we dive into two of the most fundamental data structures in Java — **Arrays** and **Strings**.
These form the **building blocks** of data manipulation and logical problem-solving in programming.

Understanding arrays and strings deeply helps you:

* Store and organize data efficiently.
* Solve problems involving searching, sorting, and data transformation.
* Build logic for higher-level data structures (like linked lists, stacks, and queues).
* Handle textual data (names, inputs, words, etc.) effectively.

---

## 🧩 1. What is an Array?

An **Array** is a collection of **similar data types** stored in **contiguous memory locations**.
It is used to store multiple values of the same type in a single variable name.

### ✅ Example:

```java
int numbers[] = {10, 20, 30, 40, 50};
```

Here:

* `numbers[0] = 10`
* `numbers[1] = 20`
* `numbers[4] = 50`

---

### 💡 Key Characteristics:

1. **Fixed size:** Once an array is declared, its size cannot be changed.
2. **Same data type:** All elements must be of the same data type.
3. **Index-based access:** Each element is accessed using its index (starting from 0).
4. **Stored in contiguous memory:** Improves access time and cache performance.

---

## 🧠 2. Declaring and Initializing Arrays in Java

### 🔹 Declaration

```java
int[] arr;          // Preferred syntax
int arr2[];         // Also valid
```

### 🔹 Memory Allocation

```java
arr = new int[5];   // Creates an array of size 5
```

### 🔹 Combined Declaration & Initialization

```java
int arr[] = {1, 2, 3, 4, 5};
```

---

### 🧾 Example: Array Input & Output

```java
import java.util.*;

class ArrayDemo {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int arr[] = new int[n];

        for (int i = 0; i < n; i++) {
            arr[i] = sc.nextInt();
        }

        System.out.print("Array Elements: ");
        for (int i = 0; i < n; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```

**Input:**

```
5
10 20 30 40 50
```

**Output:**

```
Array Elements: 10 20 30 40 50
```

---

## ⚙️ 3. Memory Representation of Arrays

Imagine you declared:

```java
int arr[] = {5, 10, 15};
```

Memory representation:

| Index | Value | Address (example) |
| ----- | ----- | ----------------- |
| 0     | 5     | 1000              |
| 1     | 10    | 1004              |
| 2     | 15    | 1008              |

Each `int` takes **4 bytes** (depends on system), stored contiguously.
That’s why random access is **O(1)** — we can directly calculate address by:

```
address(arr[i]) = base_address + (i * size_of_element)
```

---

## 🔍 4. Common Array Operations

| Operation      | Description        | Time Complexity |
| -------------- | ------------------ | --------------- |
| Access element | Using index        | O(1)            |
| Search element | Linear or Binary   | O(N) / O(log N) |
| Insert element | Add at position    | O(N)            |
| Delete element | Remove by index    | O(N)            |
| Traverse       | Visit all elements | O(N)            |

---

## 🧮 Example 1: Sum of Array Elements

```java
class SumArray {
    public static void main(String[] args) {
        int arr[] = {10, 20, 30, 40, 50};
        int sum = 0;

        for (int i = 0; i < arr.length; i++) {
            sum += arr[i];
        }

        System.out.println("Sum = " + sum);
    }
}
```

**Output:**

```
Sum = 150
```

---

## ⚡ Example 2: Find Maximum and Minimum in Array

```java
class MaxMin {
    public static void main(String[] args) {
        int arr[] = {5, 9, 3, 15, 2, 10};
        int max = arr[0], min = arr[0];

        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > max)
                max = arr[i];
            if (arr[i] < min)
                min = arr[i];
        }

        System.out.println("Maximum: " + max);
        System.out.println("Minimum: " + min);
    }
}
```

---

## 🔁 5. Traversing and Printing Arrays (Enhanced For Loop)

```java
class EnhancedLoop {
    public static void main(String args[]) {
        int arr[] = {10, 20, 30, 40};
        for (int num : arr) {
            System.out.println(num);
        }
    }
}
```

---

## 🧩 6. Multi-dimensional Arrays

Arrays can be 2D, 3D, etc.

Example (Matrix form):

```java
int matrix[][] = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

**Access:**
`matrix[1][2] = 6`

---

### 💡 Print 2D Array

```java
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}
```

---

## 🧠 7. What is a String?

A **String** is a sequence of characters enclosed within double quotes.
In Java, `String` is **not a primitive data type** — it’s an **object** of the `java.lang.String` class.

Example:

```java
String s = "Hello Java";
```

---

### 🧩 How Strings Work in Java

* Strings are **immutable** — once created, they cannot be changed.
* Every modification creates a **new String object** in memory.
* String literals are stored in a **String Constant Pool (SCP)** to save memory.

---

### 🧠 Example:

```java
String s1 = "Hello";
String s2 = "Hello";
System.out.println(s1 == s2); // true (same memory in SCP)

String s3 = new String("Hello");
System.out.println(s1 == s3); // false (new object in heap)
```

---

## 🧮 8. Common String Operations

| Operation       | Example               | Description                |
| --------------- | --------------------- | -------------------------- |
| `length()`      | `s.length()`          | Returns length             |
| `charAt()`      | `s.charAt(2)`         | Returns character at index |
| `substring()`   | `s.substring(2,5)`    | Extracts substring         |
| `equals()`      | `s1.equals(s2)`       | Compares content           |
| `toUpperCase()` | Converts to uppercase |                            |
| `toLowerCase()` | Converts to lowercase |                            |
| `concat()`      | `s1.concat(s2)`       | Joins two strings          |

---

### Example:

```java
class StringExample {
    public static void main(String args[]) {
        String s1 = "Java";
        String s2 = "Programming";

        System.out.println(s1.length());
        System.out.println(s1.concat(" " + s2));
        System.out.println(s1.toUpperCase());
        System.out.println(s1.charAt(2));
    }
}
```

---

## 🔄 9. String Comparison

```java
String a = "Data";
String b = "Data";
String c = new String("Data");

System.out.println(a == b);      // true (same SCP object)
System.out.println(a.equals(b)); // true (content same)
System.out.println(a == c);      // false (different memory)
System.out.println(a.equals(c)); // true (content same)
```

---

## 🧩 10. String Immutability Explained

```java
String s = "Code";
s.concat("World");
System.out.println(s);
```

**Output:**

```
Code
```

Because `concat()` creates a **new string**, but `s` still refers to the old one.

If we do:

```java
s = s.concat("World");
System.out.println(s);
```

Output:

```
CodeWorld
```

---

## 🧠 11. Mutable Strings — StringBuilder & StringBuffer

| Class           | Mutable | Thread-Safe       | Speed    |
| --------------- | ------- | ----------------- | -------- |
| `String`        | ❌ No    | ✅ Yes (immutable) | Slower   |
| `StringBuilder` | ✅ Yes   | ❌ No              | Fastest  |
| `StringBuffer`  | ✅ Yes   | ✅ Yes             | Moderate |

---

### Example using StringBuilder

```java
class BuilderDemo {
    public static void main(String args[]) {
        StringBuilder sb = new StringBuilder("Hello");
        sb.append(" Java");
        sb.insert(0, "Welcome ");
        System.out.println(sb);
    }
}
```

Output:

```
Welcome Hello Java
```

---

## ⚡ 12. Conversion Between Strings and Arrays

### String → Char Array

```java
String s = "Java";
char ch[] = s.toCharArray();
```

### Char Array → String

```java
char arr[] = {'J', 'a', 'v', 'a'};
String s = new String(arr);
```

---

## 🔍 13. Example: Count Vowels and Consonants in String

```java
class VowelCount {
    public static void main(String args[]) {
        String s = "DataStructures";
        s = s.toLowerCase();
        int v = 0, c = 0;

        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if ("aeiou".indexOf(ch) != -1)
                v++;
            else if (ch >= 'a' && ch <= 'z')
                c++;
        }

        System.out.println("Vowels: " + v);
        System.out.println("Consonants: " + c);
    }
}
```

---

## 🧮 14. Time and Space Complexity Summary

| Operation   | Array | String |
| ----------- | ----- | ------ |
| Access      | O(1)  | O(1)   |
| Search      | O(N)  | O(N)   |
| Insert      | O(N)  | O(N)   |
| Delete      | O(N)  | O(N)   |
| Concatenate | —     | O(N)   |
| Copy        | O(N)  | O(N)   |

---

## 💼 15. Real-World Applications

### Arrays:

* Managing user data (IDs, scores, ages)
* Mathematical computations (matrix operations)
* Database record storage
* Data analytics (statistics, sums, averages)

### Strings:

* Storing usernames, passwords
* Processing input/output
* Text processing, search, and NLP
* URL handling and encoding

---

## ⚠️ 16. Common Mistakes

1. Accessing array out of bounds → `ArrayIndexOutOfBoundsException`
2. Forgetting immutability of strings
3. Using `==` instead of `.equals()` for comparing strings
4. Forgetting to reassign string after using `concat()`
5. Declaring wrong array size

---

## 🧭 17. Interview Questions

1. What is the difference between `String`, `StringBuilder`, and `StringBuffer`?
2. Why are arrays stored in contiguous memory?
3. How does immutability of strings affect performance?
4. How do you reverse a string without using built-in functions?
5. Write a program to find the second largest element in an array.

---

## 🧑‍💻 18. Practice Problems

1. Find the largest and smallest number in an array.
2. Reverse an array without using extra space.
3. Check if a string is a palindrome.
4. Count frequency of each character in a string.
5. Merge two sorted arrays into one.
6. Find duplicate characters in a string.
7. Rotate an array by `k` positions.
8. Remove vowels from a string.

---

## ✅ 19. Summary

| Concept       | Key Point                                    |
| ------------- | -------------------------------------------- |
| Array         | Fixed size, same type, fast access           |
| String        | Sequence of characters, immutable            |
| StringBuilder | Mutable and fast for modifications           |
| Access time   | O(1) for both arrays and strings             |
| Search        | O(N) unless optimized                        |
| Usage         | Arrays → numerical data; Strings → text data |

---

## 🚀 20. What’s Next?

In **Day 3**, we’ll explore:

* **Searching Algorithms (Linear Search and Binary Search)**
* Step-by-step logic with dry runs
* How to calculate mid index safely in Binary Search

---
