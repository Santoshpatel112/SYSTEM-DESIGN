# Liskov Substitution Principle (LSP)

## Definition

**"Objects of a superclass should be replaceable with objects of a subclass without breaking the application."**

The Liskov Substitution Principle states that if class B is a subtype of class A, then objects of type A should be replaceable with objects of type B without altering the correctness of the program. In simpler terms: derived classes must be substitutable for their base classes.

## Key Concepts

- **Behavioral Compatibility**: Subtypes must behave like their parent types
- **Contract Preservation**: Subclasses must honor the contract of the parent class
- **No Unexpected Exceptions**: Subclasses shouldn't throw exceptions that the parent doesn't throw
- **Proper Inheritance**: Use inheritance only when there's a true "is-a" relationship

## Why LSP Matters

1. **Reliability**: Code works correctly with any subtype
2. **Polymorphism**: True polymorphic behavior without surprises
3. **Maintainability**: Changes to subtypes don't break existing code
4. **Design Quality**: Ensures proper use of inheritance

## Example Explanation

### Violation (LSPViolated.java)

In the violated example, `FixedTermAccount` implements `Account` but **breaks the contract**:

```java
interface Account {
    void deposit(double amount);
    void withdraw(double amount);  // Contract: all accounts can withdraw
}

class SavingAccount implements Account {
    public void deposit(double amount) { ... }
    public void withdraw(double amount) { ... }  // Works fine
}

class FixedTermAccount implements Account {
    public void deposit(double amount) { ... }
    public void withdraw(double amount) {
        throw new UnsupportedOperationException("Withdrawal not allowed!");
    }
}
```

**Problems**:
- `FixedTermAccount` claims to be an `Account` but can't withdraw
- Throws exception for `withdraw()` method, breaking the contract
- Client code expects all `Account` objects to support withdrawal
- Violates LSP because `FixedTermAccount` cannot substitute `Account`
- Forces client to use try-catch or type checking

```java
class BankClient {
    public void processTransactions() {
        for (Account acc : accounts) {
            acc.deposit(1000);
            acc.withdraw(500);  // BOOM! Exception for FixedTermAccount
        }
    }
}
```

### Following LSP (LSPFollowed.java)

The corrected version uses **proper interface segregation**:

```java
// Base interface: only deposit capability
interface DepositOnlyAccount {
    void deposit(double amount);
}

// Extended interface: adds withdrawal capability
interface WithdrawableAccount extends DepositOnlyAccount {
    void withdraw(double amount);
}

// Accounts that support withdrawal
class SavingAccount implements WithdrawableAccount {
    public void deposit(double amount) { ... }
    public void withdraw(double amount) { ... }
}

class CurrentAccount implements WithdrawableAccount {
    public void deposit(double amount) { ... }
    public void withdraw(double amount) { ... }
}

// Account that only supports deposits
class FixedTermAccount implements DepositOnlyAccount {
    public void deposit(double amount) { ... }
    // No withdraw method - doesn't promise what it can't deliver
}
```

**Benefits**:
- Each account type implements only what it can actually do
- No broken promises or unexpected exceptions
- Client code handles different account types appropriately
- `FixedTermAccount` is substitutable for `DepositOnlyAccount`
- `SavingAccount` is substitutable for `WithdrawableAccount`

```java
class BankClient {
    public void processTransactions() {
        // Handle withdrawable accounts
        for (WithdrawableAccount acc : withdrawableAccounts) {
            acc.deposit(1000);
            acc.withdraw(500);  // Safe - all support withdrawal
        }
        
        // Handle deposit-only accounts
        for (DepositOnlyAccount acc : depositOnlyAccounts) {
            acc.deposit(5000);  // Safe - all support deposits
        }
    }
}
```

## Real-World Analogy

Think of vehicles:
- **Bad Design**: Claiming a bicycle is a "Vehicle" with a `refuel()` method
  - Bicycles can't refuel, so they'd throw an exception
  - Violates LSP

- **Good Design**: 
  - `Vehicle` interface with `move()` method
  - `FuelVehicle` extends `Vehicle` and adds `refuel()` method
  - Bicycle implements `Vehicle` (can move)
  - Car implements `FuelVehicle` (can move and refuel)

## LSP Rules

1. **Preconditions cannot be strengthened**: Subclass can't require more than parent
2. **Postconditions cannot be weakened**: Subclass must deliver at least what parent promises
3. **Invariants must be preserved**: Conditions that are always true in parent must remain true in subclass
4. **No new exceptions**: Subclass shouldn't throw exceptions parent doesn't throw

## Common LSP Violations

1. **Empty or Exception-Throwing Methods**: Implementing interface methods that throw exceptions
2. **Strengthening Preconditions**: Requiring more strict inputs than parent
3. **Weakening Postconditions**: Returning less than what parent promises
4. **Type Checking**: Using `instanceof` to check types before calling methods

## How to Fix LSP Violations

1. **Interface Segregation**: Split large interfaces into smaller, focused ones
2. **Composition over Inheritance**: Use composition when "is-a" relationship doesn't truly exist
3. **Abstract Base Classes**: Create proper hierarchies with shared behavior
4. **Design by Contract**: Clearly define what each interface promises

## When to Apply LSP

- When designing class hierarchies
- When using inheritance or implementing interfaces
- When you find yourself checking types before calling methods
- When subclasses throw unexpected exceptions

## Summary

LSP is about **honest contracts**. If a class claims to be a certain type, it must fully behave like that type. Don't promise what you can't deliver. Use proper interface design to ensure subtypes can truly substitute their parent types without surprises.
