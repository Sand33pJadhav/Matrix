# Matrix
Falling Characters Effect Developed using JavaScript


<img width="1190" alt="Screenshot 2022-03-04 at 1 25 38 AM" src="https://user-images.githubusercontent.com/26808820/156642691-464146f0-2217-41e6-99d1-1350fe915225.png">


# Single Responsibility Principle (SRP)

* The SRP states that a class should have only one reason to change, meaning it should have only one responsibility.

```java
// Bad example: Violating SRP
class Employee {
    String name;
    double salary;
    
    void saveToDatabase() {
        // Code to save employee to the database
    }
    
    void generateReport() {
        // Code to generate an employee report
    }
}
```

* In the improved example, we've separated the responsibilities. Now, the `Employee` class only represents an employee, while `EmployeeDatabaseManager` handles database operations, and `ReportGenerator` handles report generation.

```java
// Good example: Following SRP
class Employee {
    String name;
    double salary;
}

class EmployeeDatabaseManager {
    void saveToDatabase(Employee employee) {
        // Code to save employee to the database
    }
}

class ReportGenerator {
    void generateReport(Employee employee) {
        // Code to generate an employee report
    }
}
```


# Open-Closed Principle (OCP)

* The OCP states that a class should be open for extension but closed for modification. This means you can add new features by extending the class, rather than modifying its existing code.

```java
// Bad example: Violating OCP
class Shape {
    String type;
    
    double calculateArea() {
        if (type.equals("circle")) {
            // Calculate circle area
        } else if (type.equals("square")) {
            // Calculate square area
        }
        // More conditions for other shapes
    }
}
```

* In this example, if we want to add a new shape (e.g., triangle), we have to modify the Shape class, which violates the OCP.


```java
// Good example: Following OCP
interface Shape {
    double calculateArea();
}

class Circle implements Shape {
    double radius;
    
    @Override
    public double calculateArea() {
        // Calculate circle area
    }
}

class Square implements Shape {
    double side;
    
    @Override
    public double calculateArea() {
        // Calculate square area
    }
}

```

* In the improved example, we use interfaces and create separate classes for each shape. If we want to add a new shape, we simply create a new class implementing the `Shape` interface.


# Liskov Substitution Principle (LSP)

* Objects of a superclass should be able to be replaced with objects of a subclass without affecting the correctness of the program. This principle ensures that subtypes can be used in place of their base types.


* In this example, a `Square` is a subclass of `Rectangle`, but it violates the LSP because setting the width and height of a square behaves differently than setting them for a rectangle.

```java
// Bad example: Violating LSP
class Rectangle {
    int width;
    int height;
    
    void setWidth(int width) {
        this.width = width;
    }
    
    void setHeight(int height) {
        this.height = height;
    }
    
    int calculateArea() {
        return width * height;
    }
}

class Square extends Rectangle {
    @Override
    void setWidth(int width) {
        this.width = width;
        this.height = width;
    }
    
    @Override
    void setHeight(int height) {
        this.width = height;
        this.height = height;
    }
}

```

* In this improved example, both `Rectangle` and `Square` inherit from a common `Shape` class, and they each implement their own `calculateArea()` method appropriately.

```java
// Good example: Following LSP
abstract class Shape {
    abstract int calculateArea();
}

class Rectangle extends Shape {
    int width;
    int height;
    
    @Override
    int calculateArea() {
        return width * height;
    }
}

class Square extends Shape {
    int side;
    
    @Override
    int calculateArea() {
        return side * side;
    }
}

```


# Interface Segregation Principle (ISP)

* A client should not be forced to depend on interfaces it doesn't use. In other words, it's better to have multiple specific interfaces than one general-purpose interface.

```java
// Bad Example
interface Worker {
    void work();
    void eat();
}

class Human implements Worker {
    void work() {
        // Human working logic
    }
    void eat() {
        // Human eating logic
    }
}

class Robot implements Worker {
    void work() {
        // Robot working logic
    }
    void eat() {
        // Robot eating logic which is not required
    }
}

// Good Example
interface Worker {
    void work();
}

interface Eater {
    void eat();
}

class Robot implements Worker {
    void work() {
        // Robot working logic
    }
}

class Human implements Worker, Eater {
    void work() {
        // Human working logic
    }
    void eat() {
        // Human eating logic
    }
}

```

# Dependency Inversion Principle (DIP)

* High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details; details should depend on abstractions. This promotes decoupling and flexibility.


* In this example, `NotificationService` directly depends on concrete implementations (`EmailService` and `SMSService`). This violates the Dependency Inversion Principle because the high-level module (`NotificationService`) depends on low-level modules (`EmailService` and `SMSService`).

```java
class EmailService {
    public void sendEmail(String to, String message) {
        // Logic to send email
    }
}

class SMSService {
    public void sendSMS(String phoneNumber, String message) {
        // Logic to send SMS
    }
}

class NotificationService {
    private EmailService emailService;
    private SMSService smsService;

    public NotificationService() {
        this.emailService = new EmailService();
        this.smsService = new SMSService();
    }

    public void sendNotification(String recipient, String message, String channel) {
        if (channel.equalsIgnoreCase("email")) {
            emailService.sendEmail(recipient, message);
        } else if (channel.equalsIgnoreCase("sms")) {
            smsService.sendSMS(recipient, message);
        }
    }
}

```

* In this modified example, we've introduced an interface `MessageService` that both `EmailService` and `SMSService` implement. Then, `NotificationService` depends on `MessageService` (an abstraction), rather than concrete implementations. This adheres to the Dependency Inversion Principle, as both high-level and low-level modules depend on abstractions (`MessageService`). This allows for easier swapping of different notification services without modifying `NotificationService`.


```java
interface MessageService {
    void sendMessage(String recipient, String message);
}

class EmailService implements MessageService {
    @Override
    public void sendMessage(String to, String message) {
        // Logic to send email
    }
}

class SMSService implements MessageService {
    @Override
    public void sendMessage(String phoneNumber, String message) {
        // Logic to send SMS
    }
}

class NotificationService {
    private MessageService messageService;

    public NotificationService(MessageService messageService) {
        this.messageService = messageService;
    }

    public void sendNotification(String recipient, String message) {
        messageService.sendMessage(recipient, message);
    }
}

```
