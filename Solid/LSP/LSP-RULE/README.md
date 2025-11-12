# LSP Rules & Constraints

## 📖 Overview

This folder contains detailed examples of **Liskov Substitution Principle Rules** that must be followed for proper inheritance.

## 📂 Folder Structure

### 1. Signature-Rule
Rules related to method signatures in inheritance:
- `ReturnTypeRule.java` - Return type covariance rules
- `MethodArgumentRule.java` - Parameter contravariance rules
- `ExceptionRule.java` - Exception handling rules

### 2. MethodRule
Rules related to method behavior:
- `PreConditions.java` - Preconditions cannot be strengthened
- `Postcondition.java` - Postconditions cannot be weakened

### 3. Property-Rule
Rules related to object properties:
- `ClassInvariants.java` - Class invariants must be preserved
- `HistoryConstraint.java` - History constraints must be maintained

## 🎯 LSP Rules Summary

### Signature Rules
1. **Return Type**: Subclass method can return more specific type (covariance)
2. **Parameters**: Subclass method can accept more general parameters (contravariance)
3. **Exceptions**: Subclass cannot throw new checked exceptions

### Method Rules
1. **Preconditions**: Cannot be strengthened in subclass
2. **Postconditions**: Cannot be weakened in subclass

### Property Rules
1. **Invariants**: Must be preserved in subclass
2. **History**: Immutable properties must remain immutable

## 🚀 Quick Start

```bash
# Navigate to specific rule folder
cd Signature-Rule
javac ReturnTypeRule.java
java ReturnTypeRule

cd ../MethodRule
javac PreConditions.java
java PreConditions

cd ../Property-Rule
javac ClassInvariants.java
java ClassInvariants
```

## 📚 Learn More

These examples demonstrate the formal rules that ensure proper substitutability in object-oriented design.

---

[← Back to LSP](../)
