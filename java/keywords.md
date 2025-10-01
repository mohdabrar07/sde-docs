# Comprehensive Guide to Java Keywords

Java has 51 reserved keywords that have special meaning in the language. Here's a complete guide with real-world examples for each:

## 1. Access Modifiers

### `public`
Accessible from anywhere.
```java
public class BankAccount {
    public void deposit(double amount) {
        // Anyone can call this method
    }
}
```

### `private`
Accessible only within the same class.
```java
public class CreditCard {
    private String cardNumber; // Hidden from outside access
    private String cvv;
    
    public boolean validatePayment(String number, String cvv) {
        return this.cardNumber.equals(number) && this.cvv.equals(cvv);
    }
}
```

### `protected`
Accessible within the same package and subclasses.
```java
public class Employee {
    protected double salary; // Accessible to subclasses like Manager
}

class Manager extends Employee {
    public void giveRaise() {
        salary += 5000; // Can access protected field
    }
}
```

## 2. Class, Method, and Variable Modifiers

### `abstract`
Defines incomplete classes or methods that must be implemented by subclasses.
```java
abstract class Vehicle {
    abstract void start(); // No implementation
}

class Car extends Vehicle {
    @Override
    void start() {
        System.out.println("Car engine starting...");
    }
}
```

### `static`
Belongs to the class rather than instances.
```java
public class MathUtils {
    // Shared across all instances
    public static final double PI = 3.14159;
    
    public static int add(int a, int b) {
        return a + b;
    }
}

// Usage: MathUtils.add(5, 3); // No object needed
```

### `final`
Makes variables constant, prevents method overriding, or class inheritance.
```java
// Constant value
public class Configuration {
    public static final String API_KEY = "abc123";
}

// Prevent overriding
class Parent {
    public final void criticalOperation() {
        // Cannot be overridden
    }
}

// Prevent inheritance
final class SecurityManager {
    // Cannot be extended
}
```

### `synchronized`
Ensures thread-safe access to methods or blocks.
```java
public class BankAccount {
    private double balance = 1000;
    
    // Only one thread can execute this at a time
    public synchronized void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
        }
    }
}
```

### `volatile`
Ensures variable visibility across threads.
```java
public class SensorReader {
    // Always read from main memory, not thread cache
    private volatile boolean stopReading = false;
    
    public void stopSensor() {
        stopReading = true;
    }
}
```

### `transient`
Excludes fields from serialization.
```java
public class User implements Serializable {
    private String username;
    private transient String password; // Won't be saved to file
}
```

### `native`
Indicates method is implemented in native code (C/C++).
```java
public class SystemInfo {
    // Implementation in C/C++
    public native int getCPUCount();
}
```

### `strictfp`
Ensures consistent floating-point calculations across platforms.
```java
public strictfp class ScientificCalculator {
    public double calculate(double x, double y) {
        return x * y; // Consistent results everywhere
    }
}
```

## 3. Control Flow Keywords

### `if`, `else`
Conditional execution.
```java
public class DiscountCalculator {
    public double applyDiscount(double price, boolean isMember) {
        if (isMember) {
            return price * 0.9; // 10% discount
        } else {
            return price;
        }
    }
}
```

### `switch`, `case`, `default`
Multi-way branch statement.
```java
public class OrderProcessor {
    public void processOrder(String status) {
        switch (status) {
            case "PENDING":
                System.out.println("Waiting for payment");
                break;
            case "PAID":
                System.out.println("Processing shipment");
                break;
            case "SHIPPED":
                System.out.println("Out for delivery");
                break;
            default:
                System.out.println("Unknown status");
        }
    }
}
```

### `for`
Loop with initialization, condition, and increment.
```java
public class EmailSender {
    public void sendBulkEmails(List<String> emails) {
        for (int i = 0; i < emails.size(); i++) {
            sendEmail(emails.get(i));
        }
        
        // Enhanced for loop
        for (String email : emails) {
            sendEmail(email);
        }
    }
}
```

### `while`
Loop that continues while condition is true.
```java
public class FileDownloader {
    public void download(InputStream input) {
        int data;
        while ((data = input.read()) != -1) {
            // Process data
        }
    }
}
```

### `do`
Loop that executes at least once.
```java
public class LoginSystem {
    public void promptLogin(Scanner scanner) {
        String password;
        do {
            System.out.print("Enter password: ");
            password = scanner.nextLine();
        } while (!isValidPassword(password));
    }
}
```

### `break`
Exits a loop or switch statement.
```java
public class SearchEngine {
    public String findUser(List<User> users, String name) {
        for (User user : users) {
            if (user.getName().equals(name)) {
                return user.getEmail();
                // break; // implicitly breaks due to return
            }
        }
        return null;
    }
}
```

### `continue`
Skips to the next iteration.
```java
public class DataProcessor {
    public void processRecords(List<Record> records) {
        for (Record record : records) {
            if (record.isInvalid()) {
                continue; // Skip invalid records
            }
            processRecord(record);
        }
    }
}
```

### `return`
Exits a method and optionally returns a value.
```java
public class Calculator {
    public int divide(int a, int b) {
        if (b == 0) {
            return 0; // Early return
        }
        return a / b;
    }
}
```

## 4. Exception Handling

### `try`, `catch`, `finally`
Handle exceptions gracefully.
```java
public class FileProcessor {
    public String readFile(String path) {
        FileReader reader = null;
        try {
            reader = new FileReader(path);
            // Read file contents
            return "File content";
        } catch (FileNotFoundException e) {
            System.out.println("File not found: " + path);
            return null;
        } catch (IOException e) {
            System.out.println("Error reading file");
            return null;
        } finally {
            // Always executes, even if exception occurs
            if (reader != null) {
                try {
                    reader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

### `throw`
Explicitly throws an exception.
```java
public class AgeValidator {
    public void validateAge(int age) {
        if (age < 18) {
            throw new IllegalArgumentException("Must be 18 or older");
        }
    }
}
```

### `throws`
Declares exceptions a method might throw.
```java
public class DatabaseConnection {
    public void connect(String url) throws SQLException {
        // May throw SQLException
        DriverManager.getConnection(url);
    }
}
```

## 5. Class and Object Keywords

### `class`
Defines a class.
```java
public class Customer {
    private String name;
    private String email;
}
```

### `interface`
Defines a contract that classes must implement.
```java
public interface PaymentProcessor {
    void processPayment(double amount);
    boolean refund(String transactionId);
}

class StripeProcessor implements PaymentProcessor {
    public void processPayment(double amount) {
        // Stripe-specific implementation
    }
    
    public boolean refund(String transactionId) {
        // Refund logic
        return true;
    }
}
```

### `extends`
Creates a subclass (inheritance).
```java
public class Animal {
    public void eat() {
        System.out.println("Eating...");
    }
}

public class Dog extends Animal {
    public void bark() {
        System.out.println("Woof!");
    }
}
```

### `implements`
Implements an interface.
```java
public class ArrayList<E> implements List<E> {
    // Provides implementation for List interface
}
```

### `enum`
Defines a fixed set of constants.
```java
public enum OrderStatus {
    PENDING, PROCESSING, SHIPPED, DELIVERED, CANCELLED;
}

// Usage
public class Order {
    private OrderStatus status = OrderStatus.PENDING;
    
    public void ship() {
        status = OrderStatus.SHIPPED;
    }
}
```

### `this`
References the current object.
```java
public class Person {
    private String name;
    
    public Person(String name) {
        this.name = name; // Distinguish field from parameter
    }
    
    public Person clone() {
        return this; // Return current object
    }
}
```

### `super`
References the parent class.
```java
public class Vehicle {
    protected String brand;
    
    public Vehicle(String brand) {
        this.brand = brand;
    }
}

public class Car extends Vehicle {
    private int doors;
    
    public Car(String brand, int doors) {
        super(brand); // Call parent constructor
        this.doors = doors;
    }
    
    @Override
    public String toString() {
        return super.toString() + " with " + doors + " doors";
    }
}
```

### `new`
Creates new objects.
```java
public class ShoppingCart {
    public void addItem(String product) {
        Item item = new Item(product); // Create new object
        ArrayList<Item> items = new ArrayList<>(); // Create collection
    }
}
```

### `instanceof`
Checks if an object is an instance of a class.
```java
public class AnimalShelter {
    public void makeSound(Animal animal) {
        if (animal instanceof Dog) {
            ((Dog) animal).bark();
        } else if (animal instanceof Cat) {
            ((Cat) animal).meow();
        }
    }
}
```

## 6. Package Keywords

### `package`
Declares the package for a class.
```java
package com.company.ecommerce.payment;

public class PaymentGateway {
    // Class belongs to payment package
}
```

### `import`
Imports classes from other packages.
```java
import java.util.ArrayList;
import java.util.List;
import java.time.*;

public class EventManager {
    private List<LocalDateTime> events = new ArrayList<>();
}
```

## 7. Primitive Data Types

### `boolean`
True or false value.
```java
public class SecurityCheck {
    private boolean isAuthenticated = false;
    
    public boolean login(String username, String password) {
        if (authenticate(username, password)) {
            isAuthenticated = true;
            return true;
        }
        return false;
    }
}
```

### `byte`
8-bit integer (-128 to 127).
```java
public class ImageProcessor {
    public void processPixels(byte[] imageData) {
        // Image data stored as bytes
        for (byte pixel : imageData) {
            // Process each pixel
        }
    }
}
```

### `short`
16-bit integer (-32,768 to 32,767).
```java
public class SensorData {
    private short temperature; // -32768 to 32767
    
    public void recordTemperature(short temp) {
        this.temperature = temp;
    }
}
```

### `int`
32-bit integer.
```java
public class InventorySystem {
    private int stockCount = 0;
    
    public void addStock(int quantity) {
        stockCount += quantity;
    }
}
```

### `long`
64-bit integer for large numbers.
```java
public class FinancialSystem {
    private long accountBalance; // For large amounts in cents
    private long timestamp = System.currentTimeMillis();
}
```

### `float`
32-bit floating-point number.
```java
public class WeatherStation {
    private float temperature = 72.5f; // Note the 'f' suffix
    
    public float celsiusToFahrenheit(float celsius) {
        return (celsius * 9/5) + 32;
    }
}
```

### `double`
64-bit floating-point number (default for decimals).
```java
public class ShoppingCart {
    private double totalPrice = 0.0;
    
    public void addItem(double price) {
        totalPrice += price;
    }
    
    public double calculateTax(double rate) {
        return totalPrice * rate;
    }
}
```

### `char`
Single 16-bit Unicode character.
```java
public class TextProcessor {
    public boolean isVowel(char letter) {
        char lower = Character.toLowerCase(letter);
        return lower == 'a' || lower == 'e' || lower == 'i' 
            || lower == 'o' || lower == 'u';
    }
}
```

### `void`
Indicates a method returns no value.
```java
public class Logger {
    public void logMessage(String message) {
        System.out.println("[LOG] " + message);
        // No return value
    }
}
```

## 8. Special Keywords

### `null`
Represents no object reference.
```java
public class UserService {
    public User findUser(int id) {
        User user = database.query(id);
        if (user == null) {
            System.out.println("User not found");
            return null;
        }
        return user;
    }
}
```

### `assert`
Tests assumptions during development.
```java
public class MathOperations {
    public int divide(int a, int b) {
        assert b != 0 : "Divisor cannot be zero";
        return a / b;
    }
}
// Enable with: java -ea YourProgram
```

## 9. Unused/Reserved Keywords

### `goto`
Reserved but not used in Java (considered harmful in structured programming).

### `const`
Reserved but not used; use `final` instead.

---

## Real-World Complete Example

Here's a comprehensive example using multiple keywords:

```java
package com.ecommerce.system;

import java.util.*;
import java.io.Serializable;

// Complete E-commerce Order System
public final class OrderManagementSystem {
    
    // Constants
    private static final double TAX_RATE = 0.08;
    private static final int MAX_ITEMS = 100;
    
    // Static counter for all orders
    private static int orderCount = 0;
    
    // Instance variables
    private volatile boolean isProcessing = false;
    private List<OrderItem> items;
    
    // Enum for order status
    public enum Status {
        PENDING, CONFIRMED, SHIPPED, DELIVERED, CANCELLED
    }
    
    // Inner class for order items
    public static class OrderItem implements Serializable {
        private String productId;
        private int quantity;
        private double price;
        private transient String tempData; // Won't be serialized
        
        public OrderItem(String productId, int quantity, double price) {
            this.productId = productId;
            this.quantity = quantity;
            this.price = price;
        }
        
        public double getTotal() {
            return quantity * price;
        }
    }
    
    // Abstract base for payment methods
    public abstract static class PaymentMethod {
        protected abstract boolean processPayment(double amount);
    }
    
    // Concrete payment implementation
    public static class CreditCardPayment extends PaymentMethod {
        private String cardNumber;
        
        public CreditCardPayment(String cardNumber) {
            super(); // Call parent constructor
            this.cardNumber = cardNumber;
        }
        
        @Override
        protected boolean processPayment(double amount) {
            if (amount <= 0) {
                throw new IllegalArgumentException("Amount must be positive");
            }
            
            try {
                // Simulate payment processing
                synchronized(this) {
                    // Thread-safe payment
                    System.out.println("Processing payment: $" + amount);
                    return true;
                }
            } catch (Exception e) {
                return false;
            } finally {
                System.out.println("Payment attempt completed");
            }
        }
    }
    
    // Constructor
    public OrderManagementSystem() {
        this.items = new ArrayList<>();
        orderCount++;
    }
    
    // Add item with validation
    public synchronized void addItem(String productId, int quantity, 
                                    double price) throws Exception {
        
        if (quantity <= 0) {
            throw new IllegalArgumentException("Quantity must be positive");
        }
        
        if (items.size() >= MAX_ITEMS) {
            throw new Exception("Maximum items reached");
        }
        
        OrderItem item = new OrderItem(productId, quantity, price);
        
        // Check if item already exists
        boolean found = false;
        for (OrderItem existing : items) {
            if (existing instanceof OrderItem && 
                existing.productId.equals(productId)) {
                found = true;
                break;
            }
        }
        
        if (!found) {
            items.add(item);
        }
    }
    
    // Calculate total with tax
    public double calculateTotal() {
        double subtotal = 0.0;
        
        for (int i = 0; i < items.size(); i++) {
            OrderItem item = items.get(i);
            subtotal += item.getTotal();
        }
        
        return subtotal * (1 + TAX_RATE);
    }
    
    // Process order with different payment methods
    public boolean checkout(PaymentMethod payment) {
        if (items == null || items.isEmpty()) {
            return false;
        }
        
        isProcessing = true;
        
        try {
            double total = calculateTotal();
            
            if (payment.processPayment(total)) {
                System.out.println("Order confirmed!");
                return true;
            } else {
                System.out.println("Payment failed");
                return false;
            }
            
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        } finally {
            isProcessing = false;
        }
    }
    
    // Main method to demonstrate
    public static void main(String[] args) {
        OrderManagementSystem order = new OrderManagementSystem();
        
        try {
            // Add items
            order.addItem("LAPTOP001", 1, 999.99);
            order.addItem("MOUSE001", 2, 29.99);
            
            // Create payment method
            PaymentMethod payment = new CreditCardPayment("1234-5678-9012-3456");
            
            // Checkout
            boolean success = order.checkout(payment);
            
            if (success) {
                System.out.println("Total orders processed: " + orderCount);
            }
            
        } catch (Exception e) {
            System.err.println("Error: " + e.getMessage());
        }
    }
}
```

This comprehensive guide covers all 51 Java keywords with practical, real-world examples that demonstrate their usage in actual programming scenarios!