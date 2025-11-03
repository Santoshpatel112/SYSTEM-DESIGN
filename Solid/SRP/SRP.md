# Single Responsibility Principle (SRP)

## Definition

**"A class should have only one reason to change."**

The Single Responsibility Principle states that every class should have only one responsibility or job. A class should focus on doing one thing and doing it well. If a class has multiple responsibilities, changes to one responsibility may affect or break the other responsibilities.

## Key Concepts

- **One Responsibility**: Each class should handle only one part of the functionality
- **Separation of Concerns**: Different concerns should be separated into different classes
- **Easier Maintenance**: When a class has a single responsibility, it's easier to understand, test, and modify
- **Reduced Coupling**: Classes with single responsibilities are less dependent on each other

## Why SRP Matters

1. **Maintainability**: Changes to one responsibility don't affect others
2. **Testability**: Easier to write unit tests for focused classes
3. **Reusability**: Single-purpose classes can be reused in different contexts
4. **Readability**: Code is easier to understand when each class has a clear purpose

## Example Explanation

### Violation (SRPViolated.java)

In the violated example, the `ShoppingCart` class has **three responsibilities**:

1. **Cart Management**: Adding products and calculating totals
2. **Printing**: Displaying invoice information
3. **Persistence**: Saving data to database

```java
class ShoppingCart {
    // Responsibility 1: Cart management
    public void addProduct(Product p) { ... }
    public double calculateTotal() { ... }
    
    // Responsibility 2: Printing (VIOLATION)
    public void printInvoice() { ... }
    
    // Responsibility 3: Database operations (VIOLATION)
    public void saveToDatabase() { ... }
}
```

**Problems**:
- If printing format changes, we need to modify `ShoppingCart`
- If database logic changes, we need to modify `ShoppingCart`
- Hard to test each responsibility independently
- The class has multiple reasons to change

### Following SRP (SRPFollowed.java)

The corrected version separates responsibilities into **three distinct classes**:

1. **ShoppingCart**: Only manages cart operations
2. **ShoppingCartPrinter**: Only handles printing/display
3. **ShoppingCartStorage**: Only handles database persistence

```java
// Responsibility 1: Cart management only
class ShoppingCart {
    public void addProduct(Product p) { ... }
    public double calculateTotal() { ... }
}

// Responsibility 2: Printing only
class ShoppingCartPrinter {
    public void printInvoice() { ... }
}

// Responsibility 3: Storage only
class ShoppingCartStorage {
    public void saveToDatabase() { ... }
}
```

**Benefits**:
- Each class has one clear purpose
- Changes to printing don't affect cart logic
- Changes to storage don't affect printing
- Each class can be tested independently
- Easy to add new storage or printing methods without touching cart logic

## Real-World Analogy

Think of a restaurant:
- **Chef** (ShoppingCart): Prepares food
- **Waiter** (ShoppingCartPrinter): Serves and presents food to customers
- **Accountant** (ShoppingCartStorage): Records transactions

Each person has one job. You wouldn't ask the chef to also handle accounting and serving customers!

## When to Apply SRP

- When a class is doing too many things
- When changes in one area require modifying the class
- When a class is hard to name because it does multiple things
- When testing requires mocking multiple unrelated dependencies

## Summary

SRP is about **focus and clarity**. Each class should do one thing well, making your code more maintainable, testable, and easier to understand.
