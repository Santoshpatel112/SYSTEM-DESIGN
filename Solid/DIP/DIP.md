# Dependency Inversion Principle (DIP)

## Definition

**"High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions."**

The Dependency Inversion Principle states that code should depend on abstractions (interfaces/abstract classes) rather than concrete implementations. This inverts the traditional dependency structure.

## Key Concepts

- **Depend on Abstractions**: Use interfaces instead of concrete classes
- **Inversion of Control**: High-level code doesn't create low-level objects
- **Loose Coupling**: Components are independent and interchangeable
- **Dependency Injection**: Dependencies are provided from outside

## Why DIP Matters

1. **Flexibility**: Easy to swap implementations
2. **Testability**: Mock dependencies for unit testing
3. **Maintainability**: Changes to implementations don't affect high-level code
4. **Reusability**: Components can be reused in different contexts

## Visual Workflow

```
❌ TRADITIONAL DEPENDENCY (Violation):
┌─────────────────────┐
│  High-Level Module  │
│   (Business Logic)  │
└──────────┬──────────┘
           │ depends on
           ↓
┌─────────────────────┐
│  Low-Level Module   │
│ (Implementation)    │
└─────────────────────┘

Problem: High-level tightly coupled to low-level

✅ DEPENDENCY INVERSION (Following DIP):
┌─────────────────────┐
│  High-Level Module  │
│   (Business Logic)  │
└──────────┬──────────┘
           │ depends on
           ↓
┌─────────────────────┐
│    Abstraction      │
│    (Interface)      │
└──────────┬──────────┘
           ↑ implements
           │
┌─────────────────────┐
│  Low-Level Module   │
│ (Implementation)    │
└─────────────────────┘

Benefit: Both depend on abstraction, loosely coupled
```

## Detailed Flow Diagram

```
BEFORE DIP (Tight Coupling):
┌──────────────────────────────────┐
│     OrderProcessor               │
│  ┌────────────────────────────┐ │
│  │ MySQLDatabase db = new     │ │
│  │   MySQLDatabase();         │ │
│  │ db.save(order);            │ │
│  └────────────────────────────┘ │
└──────────────────────────────────┘
         ↓ tightly coupled
┌──────────────────────────────────┐
│     MySQLDatabase                │
│  - Can't switch to MongoDB       │
│  - Hard to test                  │
│  - Changes break OrderProcessor  │
└──────────────────────────────────┘

AFTER DIP (Loose Coupling):
┌──────────────────────────────────┐
│     OrderProcessor               │
│  ┌────────────────────────────┐ │
│  │ Database db;               │ │
│  │ OrderProcessor(Database d) │ │
│  │ db.save(order);            │ │
│  └────────────────────────────┘ │
└──────────────────────────────────┘
         ↓ depends on abstraction
┌──────────────────────────────────┐
│     <<interface>>                │
│        Database                  │
│  + save(data)                    │
└──────────────────────────────────┘
         ↑                    ↑
         │                    │
    ┌────┴────┐         ┌────┴────┐
    │  MySQL  │         │ MongoDB │
    │Database │         │Database │
    └─────────┘         └─────────┘
```

## Example Scenario

### Violation Example

```java
// Low-level module
class MySQLDatabase {
    public void save(String data) {
        System.out.println("Saving to MySQL: " + data);
    }
}

// High-level module depends on concrete low-level module
class OrderProcessor {
    private MySQLDatabase database;  // ❌ Depends on concrete class
    
    public OrderProcessor() {
        this.database = new MySQLDatabase();  // ❌ Creates dependency
    }
    
    public void processOrder(String order) {
        // Business logic
        System.out.println("Processing order: " + order);
        database.save(order);
    }
}
```

**Problems**:
- `OrderProcessor` tightly coupled to `MySQLDatabase`
- Can't switch to MongoDB or PostgreSQL without changing code
- Hard to test - can't mock database
- Violates DIP - high-level depends on low-level

### Following DIP

```java
// Abstraction (interface)
interface Database {
    void save(String data);
}

// Low-level modules implement abstraction
class MySQLDatabase implements Database {
    public void save(String data) {
        System.out.println("Saving to MySQL: " + data);
    }
}

class MongoDatabase implements Database {
    public void save(String data) {
        System.out.println("Saving to MongoDB: " + data);
    }
}

class PostgreSQLDatabase implements Database {
    public void save(String data) {
        System.out.println("Saving to PostgreSQL: " + data);
    }
}

// High-level module depends on abstraction
class OrderProcessor {
    private Database database;  // ✅ Depends on interface
    
    // Dependency Injection via constructor
    public OrderProcessor(Database database) {
        this.database = database;  // ✅ Injected from outside
    }
    
    public void processOrder(String order) {
        System.out.println("Processing order: " + order);
        database.save(order);  // Works with any Database implementation
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        // Easy to switch implementations
        Database db = new MySQLDatabase();
        // Database db = new MongoDatabase();  // Just change this line!
        
        OrderProcessor processor = new OrderProcessor(db);
        processor.processOrder("Order #123");
    }
}
```

**Benefits**:
- `OrderProcessor` depends on `Database` interface, not concrete class
- Easy to switch database implementations
- Easy to test with mock database
- High-level and low-level both depend on abstraction
- Follows DIP perfectly

## Dependency Injection Types

```
1. Constructor Injection (Recommended):
   class Service {
       Service(Dependency dep) { ... }
   }

2. Setter Injection:
   class Service {
       setDependency(Dependency dep) { ... }
   }

3. Interface Injection:
   interface Injectable {
       void inject(Dependency dep);
   }
```

## Real-World Analogy

Think of electrical outlets:
- **Bad Design**: Appliances hardwired to specific power source
  - Can't move appliances
  - Can't replace power source
  
- **Good Design**: Appliances use standard plug (interface)
  - Plug into any outlet
  - Outlets provide power (implementation)
  - Appliance doesn't care about power source details

DIP is like using standard plugs instead of hardwiring!

## DIP Benefits

| Benefit | Description |
|---------|-------------|
| **Flexibility** | Easy to swap implementations |
| **Testability** | Mock dependencies for testing |
| **Maintainability** | Changes isolated to implementations |
| **Reusability** | Components work in different contexts |
| **Scalability** | Add new implementations without changing existing code |

## Common DIP Violations

1. **Direct Instantiation**: Creating dependencies with `new` keyword
2. **Concrete Dependencies**: Depending on concrete classes instead of interfaces
3. **Static Dependencies**: Using static methods/classes
4. **Tight Coupling**: High-level modules knowing low-level details

## How to Apply DIP

1. **Identify Dependencies**: Find where classes depend on concrete implementations
2. **Create Abstractions**: Define interfaces for dependencies
3. **Inject Dependencies**: Pass dependencies from outside (constructor/setter)
4. **Depend on Interfaces**: Use interface types, not concrete types
5. **Use DI Frameworks**: Spring, Guice, Dagger for automatic injection

## DIP and Design Patterns

1. **Factory Pattern**: Creates objects without specifying concrete classes
2. **Strategy Pattern**: Algorithms depend on interface
3. **Observer Pattern**: Subjects depend on observer interface
4. **Adapter Pattern**: Adapts interfaces for compatibility

## Testing with DIP

```java
// Easy to test with mock
class MockDatabase implements Database {
    public void save(String data) {
        System.out.println("Mock save: " + data);
    }
}

// Test
@Test
public void testOrderProcessor() {
    Database mockDb = new MockDatabase();
    OrderProcessor processor = new OrderProcessor(mockDb);
    processor.processOrder("Test Order");
    // Verify mock was called
}
```

## DIP vs IoC vs DI

- **DIP (Dependency Inversion Principle)**: Design principle - depend on abstractions
- **IoC (Inversion of Control)**: Design pattern - framework controls flow
- **DI (Dependency Injection)**: Technique - inject dependencies from outside

```
DIP (Principle)
    ↓ achieved by
IoC (Pattern)
    ↓ implemented using
DI (Technique)
```

## When to Apply DIP

- When classes depend on concrete implementations
- When you need to swap implementations
- When testing requires mocking
- When building frameworks or libraries
- When code needs to be flexible and maintainable

## Quick Checklist

✅ **Good DIP Design**:
- Depend on interfaces/abstractions
- Inject dependencies from outside
- No `new` keyword for dependencies in business logic
- Easy to swap implementations

❌ **DIP Violations**:
- Creating dependencies with `new`
- Depending on concrete classes
- Hard-coded implementations
- Difficult to test

## Summary

DIP is about **inverting dependencies**. Instead of high-level code depending on low-level code, both should depend on abstractions. This creates flexible, testable, and maintainable systems where implementations can be easily swapped.

**Remember**: "Depend on abstractions, not concretions!"

## Code Evolution

```
Stage 1: Tight Coupling
class A { B b = new B(); }

Stage 2: Dependency Inversion
interface IB { }
class B implements IB { }
class A { IB b; A(IB b) { this.b = b; } }

Stage 3: Dependency Injection
// Framework injects dependencies automatically
@Inject
class A { IB b; }
```

DIP is the foundation of modern frameworks like Spring, Angular, and .NET Core!
