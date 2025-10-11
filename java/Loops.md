# Comprehensive Guide to Loops in Java

## Table of Contents
- [Introduction](#introduction)
- [Types of Loops](#types-of-loops)
- [For Loop](#for-loop)
- [While Loop](#while-loop)
- [Do-While Loop](#do-while-loop)
- [Enhanced For Loop (For-Each)](#enhanced-for-loop-for-each)
- [Loop Control Statements](#loop-control-statements)
- [Nested Loops](#nested-loops)
- [Infinite Loops](#infinite-loops)
- [Loop Performance & Best Practices](#loop-performance--best-practices)
- [Common Patterns & Examples](#common-patterns--examples)
- [Interview Questions](#interview-questions)

---

## Introduction

Loops are fundamental control structures in Java that allow you to execute a block of code repeatedly based on a condition. They are essential for iterating through collections, processing data, and implementing algorithms efficiently.

**Key Concepts:**
- Iteration: Repeating a set of instructions
- Condition: Boolean expression that controls loop execution
- Loop variable: Variable that changes with each iteration
- Termination: Condition that ends the loop

---

## Types of Loops

Java provides four main types of loops:

1. **for loop** - Used when the number of iterations is known
2. **while loop** - Used when the number of iterations is unknown
3. **do-while loop** - Executes at least once before checking condition
4. **enhanced for loop (for-each)** - Used for iterating over arrays and collections

---

## For Loop

The for loop is the most commonly used loop when you know how many times you want to iterate.

### Syntax
```java
for (initialization; condition; update) {
    // code to be executed
}
```

### Basic Examples

**Example 1: Simple Counter**
```java
for (int i = 0; i < 5; i++) {
    System.out.println("Iteration: " + i);
}
// Output: 0, 1, 2, 3, 4
```

**Example 2: Reverse Loop**
```java
for (int i = 10; i > 0; i--) {
    System.out.println(i);
}
// Countdown from 10 to 1
```

**Example 3: Step by Multiple Values**
```java
for (int i = 0; i <= 20; i += 5) {
    System.out.println(i);
}
// Output: 0, 5, 10, 15, 20
```

### Advanced For Loop Patterns

**Multiple Variables**
```java
for (int i = 0, j = 10; i < j; i++, j--) {
    System.out.println("i: " + i + ", j: " + j);
}
```

**Empty Sections**
```java
int i = 0;
for (; i < 5; ) {
    System.out.println(i);
    i++;
}
```

**Array Iteration**
```java
int[] numbers = {10, 20, 30, 40, 50};
for (int i = 0; i < numbers.length; i++) {
    System.out.println("Index " + i + ": " + numbers[i]);
}
```

---

## While Loop

The while loop executes as long as the condition is true. It's ideal when the number of iterations is not predetermined.

### Syntax
```java
while (condition) {
    // code to be executed
}
```

### Examples

**Example 1: Basic While Loop**
```java
int count = 0;
while (count < 5) {
    System.out.println("Count: " + count);
    count++;
}
```

**Example 2: User Input Validation**
```java
Scanner scanner = new Scanner(System.in);
int number = 0;
while (number <= 0) {
    System.out.print("Enter a positive number: ");
    number = scanner.nextInt();
}
System.out.println("You entered: " + number);
```

**Example 3: Reading Data**
```java
String input = "";
while (!input.equals("quit")) {
    System.out.print("Enter command (or 'quit' to exit): ");
    input = scanner.nextLine();
    System.out.println("You entered: " + input);
}
```

**Example 4: Sum Until Condition**
```java
int sum = 0;
int num = 1;
while (sum < 100) {
    sum += num;
    num++;
}
System.out.println("Sum: " + sum + ", Numbers added: " + (num - 1));
```

---

## Do-While Loop

The do-while loop guarantees at least one execution of the loop body before checking the condition.

### Syntax
```java
do {
    // code to be executed
} while (condition);
```

### Examples

**Example 1: Menu System**
```java
Scanner scanner = new Scanner(System.in);
int choice;
do {
    System.out.println("\n--- Menu ---");
    System.out.println("1. Option A");
    System.out.println("2. Option B");
    System.out.println("3. Exit");
    System.out.print("Choose: ");
    choice = scanner.nextInt();
    
    switch (choice) {
        case 1:
            System.out.println("Option A selected");
            break;
        case 2:
            System.out.println("Option B selected");
            break;
        case 3:
            System.out.println("Exiting...");
            break;
        default:
            System.out.println("Invalid choice");
    }
} while (choice != 3);
```

**Example 2: Input Validation**
```java
int age;
do {
    System.out.print("Enter your age (1-120): ");
    age = scanner.nextInt();
} while (age < 1 || age > 120);
System.out.println("Valid age: " + age);
```

**Example 3: At Least Once Execution**
```java
int x = 10;
do {
    System.out.println("This executes even though x >= 10");
    x++;
} while (x < 10);
```

---

## Enhanced For Loop (For-Each)

The enhanced for loop simplifies iteration over arrays and collections.

### Syntax
```java
for (Type variable : collection) {
    // code to be executed
}
```

### Examples

**Example 1: Array Iteration**
```java
String[] fruits = {"Apple", "Banana", "Cherry", "Date"};
for (String fruit : fruits) {
    System.out.println(fruit);
}
```

**Example 2: ArrayList Iteration**
```java
ArrayList<Integer> numbers = new ArrayList<>();
numbers.add(10);
numbers.add(20);
numbers.add(30);

for (int num : numbers) {
    System.out.println(num);
}
```

**Example 3: 2D Array Iteration**
```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

for (int[] row : matrix) {
    for (int value : row) {
        System.out.print(value + " ");
    }
    System.out.println();
}
```

**Example 4: Collection Processing**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
for (String name : names) {
    System.out.println("Hello, " + name);
}
```

### Limitations of For-Each Loop
- Cannot modify elements (for primitive arrays)
- Cannot access index
- Cannot iterate backwards
- Cannot iterate multiple collections simultaneously

---

## Loop Control Statements

Loop control statements alter the normal flow of loop execution.

### Break Statement

Terminates the loop immediately.

```java
for (int i = 0; i < 10; i++) {
    if (i == 5) {
        break; // Exit loop when i is 5
    }
    System.out.println(i);
}
// Output: 0, 1, 2, 3, 4
```

**Break with Label**
```java
outer:
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (i == 1 && j == 1) {
            break outer; // Break outer loop
        }
        System.out.println(i + "," + j);
    }
}
```

### Continue Statement

Skips the current iteration and moves to the next one.

```java
for (int i = 0; i < 10; i++) {
    if (i % 2 == 0) {
        continue; // Skip even numbers
    }
    System.out.println(i);
}
// Output: 1, 3, 5, 7, 9
```

**Continue with Label**
```java
outer:
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (j == 1) {
            continue outer; // Continue outer loop
        }
        System.out.println(i + "," + j);
    }
}
```

### Return Statement

Exits from the current method.

```java
public static int findElement(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) {
            return i; // Exit method and return index
        }
    }
    return -1; // Not found
}
```

---

## Nested Loops

Nested loops are loops inside other loops, commonly used for multi-dimensional data structures.

### Examples

**Example 1: Multiplication Table**
```java
for (int i = 1; i <= 10; i++) {
    for (int j = 1; j <= 10; j++) {
        System.out.printf("%4d", i * j);
    }
    System.out.println();
}
```

**Example 2: Pattern Printing**
```java
// Right triangle
for (int i = 1; i <= 5; i++) {
    for (int j = 1; j <= i; j++) {
        System.out.print("* ");
    }
    System.out.println();
}

// Output:
// *
// * *
// * * *
// * * * *
// * * * * *
```

**Example 3: Matrix Operations**
```java
int[][] matrix1 = {{1, 2}, {3, 4}};
int[][] matrix2 = {{5, 6}, {7, 8}};
int[][] result = new int[2][2];

for (int i = 0; i < 2; i++) {
    for (int j = 0; j < 2; j++) {
        result[i][j] = matrix1[i][j] + matrix2[i][j];
    }
}
```

**Example 4: Finding Prime Numbers**
```java
for (int num = 2; num <= 100; num++) {
    boolean isPrime = true;
    for (int i = 2; i <= Math.sqrt(num); i++) {
        if (num % i == 0) {
            isPrime = false;
            break;
        }
    }
    if (isPrime) {
        System.out.print(num + " ");
    }
}
```

---

## Infinite Loops

Infinite loops run indefinitely until explicitly terminated.

### Examples

**For Loop**
```java
for (;;) {
    System.out.println("Infinite loop");
    // Needs break or return to exit
}
```

**While Loop**
```java
while (true) {
    System.out.println("Infinite loop");
    // Needs break or return to exit
}
```

**Practical Use Case**
```java
while (true) {
    String input = scanner.nextLine();
    if (input.equals("exit")) {
        break;
    }
    processInput(input);
}
```

---

## Loop Performance & Best Practices

### Performance Tips

**1. Minimize Work in Loop Condition**
```java
// Bad
for (int i = 0; i < list.size(); i++) {
    // size() called every iteration
}

// Good
int size = list.size();
for (int i = 0; i < size; i++) {
    // size() called once
}
```

**2. Use Appropriate Loop Type**
```java
// For known iterations
for (int i = 0; i < 10; i++) { }

// For collections
for (String item : collection) { }

// For unknown iterations
while (condition) { }
```

**3. Avoid Unnecessary Computations**
```java
// Bad
for (int i = 0; i < array.length; i++) {
    int temp = expensiveCalculation();
    array[i] = temp;
}

// Good
int temp = expensiveCalculation();
for (int i = 0; i < array.length; i++) {
    array[i] = temp;
}
```

### Best Practices

1. **Use meaningful variable names**
   ```java
   for (int studentIndex = 0; studentIndex < students.length; studentIndex++)
   ```

2. **Keep loop bodies short and focused**
3. **Avoid modifying loop variables inside the loop**
4. **Use break and continue judiciously**
5. **Initialize loop variables close to loop**
6. **Prefer for-each loop for simple iterations**

---

## Common Patterns & Examples

### Pattern 1: Searching
```java
public static int linearSearch(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) {
            return i;
        }
    }
    return -1;
}
```

### Pattern 2: Accumulation
```java
int sum = 0;
for (int num : numbers) {
    sum += num;
}
double average = (double) sum / numbers.length;
```

### Pattern 3: Filtering
```java
List<Integer> evenNumbers = new ArrayList<>();
for (int num : numbers) {
    if (num % 2 == 0) {
        evenNumbers.add(num);
    }
}
```

### Pattern 4: Transformation
```java
String[] upperCaseNames = new String[names.length];
for (int i = 0; i < names.length; i++) {
    upperCaseNames[i] = names[i].toUpperCase();
}
```

### Pattern 5: Validation
```java
boolean isValid = true;
for (int num : numbers) {
    if (num < 0) {
        isValid = false;
        break;
    }
}
```

---

## Interview Questions

### Question 1: What's the difference between while and do-while?
**Answer:** While loop checks condition before execution; do-while checks after, guaranteeing at least one execution.

### Question 2: Can you modify an array in a for-each loop?
**Answer:** You can modify object properties but cannot reassign array elements or change the array itself.

### Question 3: What happens if you forget to increment in a while loop?
**Answer:** It creates an infinite loop if the condition never becomes false.

### Question 4: When should you use break vs return?
**Answer:** Break exits the loop; return exits the entire method.

### Question 5: How do you iterate backwards?
```java
for (int i = array.length - 1; i >= 0; i--) {
    System.out.println(array[i]);
}
```

---

## Summary

Loops are essential for repetitive tasks in Java. Choose the right loop type based on your needs:
- **for**: Known iterations, need index access
- **while**: Unknown iterations, condition-based
- **do-while**: Must execute at least once
- **for-each**: Simple iteration over collections
