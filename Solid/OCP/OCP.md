# Open/Closed Principle (OCP)

## Definition

**"Software entities (classes, modules, functions) should be open for extension but closed for modification."**

The Open/Closed Principle states that you should be able to add new functionality to existing code without changing the existing code itself. This is achieved through abstraction and polymorphism.

## Key Concepts

- **Open for Extension**: You can add new behavior or functionality
- **Closed for Modification**: Existing code should not be changed
- **Use Abstraction**: Interfaces and abstract classes enable extension
- **Polymorphism**: Different implementations can be used interchangeably

## Why OCP Matters

1. **Stability**: Existing tested code remains unchanged
2. **Flexibility**: Easy to add new features without breaking existing ones
3. **Reduced Risk**: No need to modify and retest working code
4. **Scalability**: System can grow without becoming fragile

## Example Explanation

### Violation (OCPViolated.java)

In the violated example, the `ShoppingCartStorage` class has **multiple concrete methods** for different storage types:

```java
class ShoppingCartStorage {
    void saveToSQLDatabase() {
        System.out.println("Saving to SQL DB...");
    }
    
    void saveToMongoDatabase() {
        System.out.println("Saving to Mongo DB...");
    }
    
    void saveToFile() {
        System.out.println("Saving to File...");
    }
}
```

**Problems**:
- Adding a new storage type (e.g., Redis, Cloud) requires **modifying** this class
- Each new storage method adds more code to the same class
- Violates OCP because the class is not closed for modification
- Risk of breaking existing functionality when adding new features
- Class grows larger with each new storage type

### Following OCP (OCPFollowed.java)

The corrected version uses an **interface** to allow extension without modification:

```java
// Abstract interface - defines the contract
interface Persistence {
    void save(ShoppingCart cart);
}

// Concrete implementations - each is a separate class
class SQLPersistence implements Persistence {
    public void save(ShoppingCart cart) {
        System.out.println("Saving to SQL DB...");
    }
}

class MongoPersistence implements Persistence {
    public void save(ShoppingCart cart) {
        System.out.println("Saving to MongoDB...");
    }
}

class FilePersistence implements Persistence {
    public void save(ShoppingCart cart) {
        System.out.println("Saving to a file...");
    }
}
```

**Benefits**:
- **Open for Extension**: Add new storage types by creating new classes
- **Closed for Modification**: Existing classes remain unchanged
- No need to modify `Persistence` interface or existing implementations
- Each storage type is independent and isolated
- Easy to test each implementation separately

### Adding New Functionality

Want to add Redis storage? Just create a new class:

```java
class RedisPersistence implements Persistence {
    public void save(ShoppingCart cart) {
        System.out.println("Saving to Redis...");
    }
}
```

**No existing code needs to change!**

## Real-World Analogy

Think of a power outlet:
- **Interface** (Persistence): The standard outlet shape
- **Implementations**: Different devices (phone charger, laptop, lamp)

You can plug in new devices without modifying the outlet. The outlet is **closed for modification** but **open for extension** (new devices).

## Design Patterns That Support OCP

1. **Strategy Pattern**: Different algorithms implementing the same interface
2. **Template Method**: Base class with extension points
3. **Decorator Pattern**: Adding behavior without modifying original class
4. **Factory Pattern**: Creating objects without specifying exact classes

## When to Apply OCP

- When you anticipate multiple variations of behavior
- When adding features frequently
- When you want to avoid touching stable, tested code
- When different implementations share a common contract

## How to Achieve OCP

1. **Identify what varies**: Find the parts that change frequently
2. **Create abstractions**: Define interfaces or abstract classes
3. **Depend on abstractions**: Use interfaces instead of concrete classes
4. **Extend through inheritance**: Create new implementations without modifying existing ones

## Summary

OCP is about **designing for change**. By using abstractions and polymorphism, you create systems that can grow and evolve without breaking existing functionality. New features are added through new code, not by modifying old code.
