# Interface Segregation Principle (ISP)

## Definition

**"No client should be forced to depend on methods it does not use."**

The Interface Segregation Principle states that it's better to have many small, specific interfaces rather than one large, general-purpose interface. Clients should not be forced to implement interfaces they don't use.

## Key Concepts

- **Small Interfaces**: Create focused, specific interfaces
- **Client-Specific**: Design interfaces based on client needs
- **No Fat Interfaces**: Avoid bloated interfaces with many methods
- **Role Interfaces**: Each interface represents a specific role or capability

## Why ISP Matters

1. **Flexibility**: Clients only depend on what they need
2. **Decoupling**: Reduces dependencies between components
3. **Maintainability**: Changes to one interface don't affect unrelated clients
4. **Clear Contracts**: Each interface has a clear, focused purpose

## Visual Workflow

```
❌ VIOLATION:
┌─────────────────────────────┐
│   Worker Interface          │
│  ┌─────────────────────┐   │
│  │ work()              │   │
│  │ eat()               │   │
│  │ sleep()             │   │
│  └─────────────────────┘   │
└─────────────────────────────┘
         ↓
    ┌────┴────┐
    ↓         ↓
┌────────┐ ┌────────┐
│ Human  │ │ Robot  │
│ Worker │ │ Worker │
└────────┘ └────────┘
    ✓         ✗
  All work   Can't eat/sleep!
             Must throw exception

✅ FOLLOWING ISP:
┌──────────┐  ┌──────────┐  ┌──────────┐
│Workable  │  │ Eatable  │  │Sleepable │
│ work()   │  │  eat()   │  │ sleep()  │
└──────────┘  └──────────┘  └──────────┘
     ↓             ↓             ↓
     └─────────────┴─────────────┘
              ↓           ↓
         ┌────────┐  ┌────────┐
         │ Human  │  │ Robot  │
         │ Worker │  │ Worker │
         └────────┘  └────────┘
         Implements  Implements
         all 3       only Workable
```

## Example Scenario

### Violation Example

```java
// Fat interface - forces all implementations to have all methods
interface Worker {
    void work();
    void eat();
    void sleep();
}

class HumanWorker implements Worker {
    public void work() {
        System.out.println("Human working...");
    }
    
    public void eat() {
        System.out.println("Human eating...");
    }
    
    public void sleep() {
        System.out.println("Human sleeping...");
    }
}

class RobotWorker implements Worker {
    public void work() {
        System.out.println("Robot working...");
    }
    
    // ISP Violation: Robot forced to implement methods it doesn't need
    public void eat() {
        throw new UnsupportedOperationException("Robots don't eat!");
    }
    
    public void sleep() {
        throw new UnsupportedOperationException("Robots don't sleep!");
    }
}
```

**Problems**:
- `RobotWorker` forced to implement `eat()` and `sleep()`
- Methods throw exceptions or have empty implementations
- Interface is too broad for all use cases
- Violates ISP by forcing unnecessary dependencies

### Following ISP

```java
// Segregated interfaces - each has a specific purpose
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

interface Sleepable {
    void sleep();
}

// Human implements all interfaces
class HumanWorker implements Workable, Eatable, Sleepable {
    public void work() {
        System.out.println("Human working...");
    }
    
    public void eat() {
        System.out.println("Human eating...");
    }
    
    public void sleep() {
        System.out.println("Human sleeping...");
    }
}

// Robot only implements what it needs
class RobotWorker implements Workable {
    public void work() {
        System.out.println("Robot working...");
    }
    // No need to implement eat() or sleep()
}
```

**Benefits**:
- Each interface has a single, clear purpose
- Classes only implement what they actually need
- No dummy implementations or exceptions
- Easy to add new worker types with different capabilities

## Real-World Analogy

Think of a smartphone app permissions:
- **Bad Design**: App requests ALL permissions (camera, contacts, location, storage) even if it only needs camera
- **Good Design**: App only requests the specific permissions it needs

ISP is like requesting only the permissions you actually use!

## Common ISP Violations

1. **God Interfaces**: One interface with too many methods
2. **Forced Implementation**: Classes implementing methods they don't need
3. **Empty Methods**: Methods with no implementation or throwing exceptions
4. **Tight Coupling**: Clients depending on methods they never call

## How to Apply ISP

1. **Identify Client Needs**: Understand what each client actually uses
2. **Split Large Interfaces**: Break down fat interfaces into smaller ones
3. **Role-Based Design**: Create interfaces based on roles/capabilities
4. **Compose Interfaces**: Use multiple small interfaces instead of one large one

## ISP and Other Principles

- **Works with SRP**: Each interface has one responsibility
- **Enables OCP**: Easy to extend with new interfaces
- **Supports LSP**: Proper substitution with focused contracts
- **Complements DIP**: Depend on small, focused abstractions

## Design Patterns Using ISP

1. **Adapter Pattern**: Adapts one interface to another specific need
2. **Facade Pattern**: Provides simplified interface for complex subsystem
3. **Strategy Pattern**: Each strategy implements specific interface
4. **Command Pattern**: Each command has focused interface

## When to Apply ISP

- When interface has many methods
- When clients use only subset of interface
- When adding methods forces changes to many classes
- When you see empty implementations or exceptions

## Benefits Summary

| Benefit | Description |
|---------|-------------|
| **Focused** | Each interface has clear purpose |
| **Flexible** | Easy to add new implementations |
| **Decoupled** | Clients depend only on what they use |
| **Maintainable** | Changes affect fewer classes |

## Quick Checklist

✅ **Good ISP Design**:
- Small, focused interfaces
- Clients implement only what they need
- No empty or exception-throwing methods
- Clear interface names describing purpose

❌ **ISP Violations**:
- Large interfaces with many methods
- Forced implementation of unused methods
- Empty implementations
- Clients depending on methods they don't call

## Summary

ISP is about **interface design**. Create small, focused interfaces that clients actually need. Don't force clients to depend on methods they don't use. Many small interfaces are better than one large interface.

**Remember**: "Keep interfaces small and focused!"
