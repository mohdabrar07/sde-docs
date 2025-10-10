# Data Types in Java - Essential Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Primitive Data Types](#primitive-data-types)
3. [Reference Data Types](#reference-data-types)
4. [Type Conversion](#type-conversion)
5. [Wrapper Classes](#wrapper-classes)

---

## Introduction

Java has **two categories** of data types:
- **Primitive Types**: 8 basic built-in types (byte, short, int, long, float, double, char, boolean)
- **Reference Types**: Objects, arrays, interfaces

Java is **strongly-typed** - every variable must have a declared type.

---

## Primitive Data Types

### 1. Integer Types

#### byte
- Size: 8 bits (1 byte)
- Range: -128 to 127
- Default: 0

```java
byte age = 25;
byte temperature = -10;
```

#### short
- Size: 16 bits (2 bytes)
- Range: -32,768 to 32,767
- Default: 0

```java
short year = 2024;
short population = 15000;
```

#### int
- Size: 32 bits (4 bytes)
- Range: -2,147,483,648 to 2,147,483,647
- Default: 0
- **Most commonly used integer type**

```java
int count = 1000;
int salary = 50000;

// Different number formats
int decimal = 100;
int binary = 0b1100100;      // Binary (Java 7+)
int hex = 0x64;              // Hexadecimal
int readable = 1_000_000;    // Underscores for readability
```

#### long
- Size: 64 bits (8 bytes)
- Range: -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807
- Default: 0L
- **Requires 'L' suffix for large values**

```java
long bigNumber = 9876543210L;
long timestamp = System.currentTimeMillis();
long population = 7_800_000_000L;
```

### 2. Floating-Point Types

#### float
- Size: 32 bits (4 bytes)
- Precision: ~6-7 decimal digits
- Default: 0.0f
- **Requires 'f' suffix**

```java
float price = 19.99f;
float pi = 3.14159f;
float distance = 1.5e6f;  // Scientific notation
```

#### double
- Size: 64 bits (8 bytes)
- Precision: ~15 decimal digits
- Default: 0.0d
- **Default choice for decimals**

```java
double salary = 75000.50;
double pi = 3.141592653589793;
double avogadro = 6.022e23;
```

### 3. Character Type

#### char
- Size: 16 bits (2 bytes)
- Range: 0 to 65,535 (Unicode)
- Default: '\u0000'

```java
char letter = 'A';
char digit = '5';
char unicode = '\u0041';  // Unicode for 'A'

// Escape sequences
char newline = '\n';
char tab = '\t';
char backslash = '\\';
char quote = '\"';
```

### 4. Boolean Type

#### boolean
- Values: true or false only
- Default: false

```java
boolean isActive = true;
boolean hasAccess = false;
boolean isEven = (10 % 2 == 0);  // true

// Cannot use integers as boolean
// boolean flag = 1;  // ERROR
```

### Summary Table

| Type    | Size     | Range                        | Default  | Example         |
|---------|----------|------------------------------|----------|-----------------|
| byte    | 1 byte   | -128 to 127                  | 0        | byte b = 100;   |
| short   | 2 bytes  | -32,768 to 32,767            | 0        | short s = 1000; |
| int     | 4 bytes  | -2 billion to 2 billion      | 0        | int i = 50000;  |
| long    | 8 bytes  | -9 quintillion to 9 quint... | 0L       | long l = 100L;  |
| float   | 4 bytes  | 6-7 decimal digits           | 0.0f     | float f = 3.14f;|
| double  | 8 bytes  | 15 decimal digits            | 0.0d     | double d = 3.14;|
| char    | 2 bytes  | 0 to 65,535                  | '\u0000' | char c = 'A';   |
| boolean | ~1 bit   | true or false                | false    | boolean b = true;|

---

## Reference Data Types

Reference types store memory addresses of objects.

### String

```java
String name = "John";
String greeting = new String("Hello");

// Common methods
int length = name.length();
char firstChar = name.charAt(0);
String upper = name.toUpperCase();
String sub = name.substring(0, 2);
```

### Arrays

```java
// Declaration and initialization
int[] numbers = {1, 2, 3, 4, 5};
String[] names = new String[10];

// Access elements
int first = numbers[0];
numbers[1] = 10;

// Multi-dimensional
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6}
};
```

### Classes and Objects

```java
class Student {
    String name;
    int age;
}

Student student = new Student();
student.name = "Alice";
student.age = 20;
```

### Null Values

```java
String str = null;  // Valid for reference types
// int num = null;  // ERROR: primitives cannot be null
```

---

## Type Conversion

### Widening (Automatic)

From smaller to larger type - no data loss:

```java
// Conversion path: byte → short → int → long → float → double

byte b = 10;
int i = b;        // Automatic
long l = i;       // Automatic
float f = l;      // Automatic
double d = f;     // Automatic

// In expressions
int x = 10;
double y = 5.5;
double result = x + y;  // x auto-converted to double
```

### Narrowing (Manual)

From larger to smaller type - possible data loss:

```java
// Requires explicit casting

double d = 9.78;
int i = (int) d;        // 9 (decimal part lost)

long l = 1000L;
int i2 = (int) l;

float f = 10.5f;
byte b = (byte) f;      // 10

// Data loss example
int large = 130;
byte small = (byte) large;  // -126 (overflow)
```

### Type Promotion

```java
// byte, short, char promoted to int in expressions
byte a = 10;
byte b = 20;
// byte result = a + b;  // ERROR
int result = a + b;      // Correct

// Mixed types
int x = 10;
long y = 20L;
long result2 = x + y;    // Result is long
```

---

## Wrapper Classes

Object versions of primitive types.

### Primitive → Wrapper

| Primitive | Wrapper Class |
|-----------|---------------|
| byte      | Byte          |
| short     | Short         |
| int       | Integer       |
| long      | Long          |
| float     | Float         |
| double    | Double        |
| char      | Character     |
| boolean   | Boolean       |

### Creating Wrappers

```java
// valueOf() - preferred method
Integer obj1 = Integer.valueOf(100);
Double obj2 = Double.valueOf(10.5);
Boolean obj3 = Boolean.valueOf(true);

// From String
Integer num = Integer.valueOf("123");
Double decimal = Double.valueOf("45.67");
```

### Autoboxing and Unboxing

```java
// Autoboxing: primitive → wrapper (automatic)
Integer num = 10;  // Auto-conversion

// Unboxing: wrapper → primitive (automatic)
Integer wrapper = 100;
int primitive = wrapper;  // Auto-conversion

// In collections
ArrayList<Integer> list = new ArrayList<>();
list.add(5);           // Autoboxing
int value = list.get(0);  // Unboxing
```

### Common Methods

```java
// Integer
int max = Integer.MAX_VALUE;
int min = Integer.MIN_VALUE;
int parsed = Integer.parseInt("123");
String str = Integer.toString(100);

// Double
double maxDouble = Double.MAX_VALUE;
double parsed = Double.parseDouble("3.14");
boolean isNaN = Double.isNaN(0.0 / 0.0);

// Character
boolean isDigit = Character.isDigit('5');
boolean isLetter = Character.isLetter('A');
char upper = Character.toUpperCase('a');
```

### Comparison Pitfall

```java
// WRONG: Don't use == for wrapper comparison
Integer a = 128;
Integer b = 128;
System.out.println(a == b);  // false (different objects)

// CORRECT: Use .equals()
System.out.println(a.equals(b));  // true

// Exception: -128 to 127 are cached
Integer x = 100;
Integer y = 100;
System.out.println(x == y);  // true (cached values)
```

---

## Key Points to Remember

### 1. Default Values
```java
// Instance variables get default values
class Example {
    int num;        // 0
    double dec;     // 0.0
    boolean flag;   // false
    char ch;        // '\u0000'
    String str;     // null
}

// Local variables must be initialized
void method() {
    int x;
    // System.out.println(x);  // ERROR: not initialized
}
```

### 2. Literals
```java
int i = 100;        // int literal
long l = 100L;      // long literal (L required)
float f = 10.5f;    // float literal (f required)
double d = 10.5;    // double literal
char c = 'A';       // char literal (single quotes)
String s = "Hello"; // String literal (double quotes)
boolean b = true;   // boolean literal
```

### 3. Special Values
```java
// Infinity
double inf = 1.0 / 0.0;  // Infinity
double ninf = -1.0 / 0.0; // -Infinity

// NaN (Not a Number)
double nan = 0.0 / 0.0;
System.out.println(nan == nan);  // false!
System.out.println(Double.isNaN(nan));  // true
```

### 4. Type Selection Guide
- **byte**: Save memory in large arrays
- **short**: Rarely used
- **int**: Default choice for integers
- **long**: Values > 2 billion
- **float**: Rarely used (use double instead)
- **double**: Default choice for decimals
- **char**: Single characters
- **boolean**: True/false flags

### 5. Best Practices
```java
// Use appropriate type
byte age = 25;           // Good
int age = 25;            // Also fine

// Use wrapper classes in collections
ArrayList<Integer> list = new ArrayList<>();  // Correct
// ArrayList<int> list = new ArrayList<>();   // ERROR

// Compare wrapper objects with .equals()
Integer a = 200;
Integer b = 200;
if (a.equals(b)) { }     // Correct
// if (a == b) { }       // Wrong (compares references)

// Avoid unnecessary boxing/unboxing
int sum = 0;
for (int i = 0; i < 1000; i++) {
    sum += i;  // Good (all primitives)
}

// Check for null before unboxing
Integer num = null;
// int x = num;  // NullPointerException!
if (num != null) {
    int x = num;  // Safe
}
```

---

## Quick Examples

```java
// Complete example demonstrating types
public class DataTypeDemo {
    public static void main(String[] args) {
        // Primitives
        int age = 25;
        double salary = 50000.50;
        char grade = 'A';
        boolean isEmployed = true;
        
        // Reference types
        String name = "John Doe";
        int[] scores = {85, 90, 78, 92};
        
        // Type conversion
        int x = 10;
        double y = x;  // Widening
        int z = (int) 10.5;  // Narrowing
        
        // Wrapper classes
        Integer num = 100;  // Autoboxing
        int primitive = num;  // Unboxing
        
        // Output
        System.out.println("Age: " + age);
        System.out.println("Salary: " + salary);
        System.out.println("Grade: " + grade);
        System.out.println("Employed: " + isEmployed);
        System.out.println("Name: " + name);
    }
}
```

---