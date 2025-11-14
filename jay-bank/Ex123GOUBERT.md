# Ex 1 – Class Metrics

## Person
- **LOC (Lines of Code):** 325
- **NOM (Number of Methods):** 21
- **WMC (Weighted Methods per Class):** 23
- **CBO (Coupling Between Objects):** 0
- **LCOM (Lack of Cohesion of Methods):** 79
- **Short Responsibility:** Represent the object with all info on the person holding an account.

## BankAccountApp
- **LOC (Lines of Code):** 491
- **NOM (Number of Methods):** 2
- **WMC (Weighted Methods per Class):** 2
- **CBO (Coupling Between Objects):** 3
- **LCOM (Lack of Cohesion of Methods):** 1
- **Short Responsibility:** Interface for the user.

## BankAccount
- **CC (Cyclomatic Complexity):** 21.7
- **LOC (Lines of Code):** 462
- **NOM (Number of Methods):** 18
- **WMC (Weighted Methods per Class):** 20
- **CBO (Coupling Between Objects):** 2
- **LCOM (Lack of Cohesion of Methods):** 44
- **Short Responsibility:** Represent the object with all info on an account held by a person.

## Bank
- **LOC (Lines of Code):** 413
- **NOM (Number of Methods):** 12
- **WMC (Weighted Methods per Class):** 14
- **CBO (Coupling Between Objects):** 3
- **LCOM (Lack of Cohesion of Methods):** 0
- **Short Responsibility:** Controller with all the logic of the program.

I find that is difficult to relly the LOC and the NOM to the responsability of a program  because who can do very important thing with less lines and have a lot of lines just to specify attributes, getters and setters. 



---

# Ex 2 – Method Analysis

## Method: `withdrawMoney`
```java
public boolean withdrawMoney(double withdrawAmount) { // CC = 5
    // Decision point
    if (withdrawAmount >= 0 && balance >= withdrawAmount && withdrawAmount < withdrawLimit
            && withdrawAmount + amountWithdrawn <= withdrawLimit) {
        balance = balance - withdrawAmount;
        success = true;
        amountWithdrawn += withdrawAmount;
    // Decision point
    } else {
        success = false;
    }
    return success;
}
```

## Refactoring Proposal for `withdrawMoney`

To reduce the complexity of the `withdrawMoney` method, I would extract the long conditional inside the `if` statement into a separate helper method. This helper could be named `isWithdrawalAllowed(double amount)` and would return a boolean indicating whether the withdrawal satisfies all conditions. By doing this, the main method becomes shorter, easier to read, and isolates the rules that check limits and balances. This also improves maintainability, since any future changes to withdrawal rules can be made directly in the helper method. Overall, this approach reduces cyclomatic complexity by moving one decision point out of the main method.

**Bonus:**  
- Implement the helper method and simplify the original `withdrawMoney` method.  
- Re-run CK Metrics on the refactored file to verify that the complexity of the method decreased.  

---

## Ex 3 - Class Metrics Analysis

| Class           | LOC | WMC | CBO | LCOM | Total |
|-----------------|-----|-----|-----|------|-------|
| Person          | 325 | 23  | 0   | 79   | 102   |
| BankAccountApp  | 491 | 2   | 3   | 1    | 6     |
| BankAccount     | 462 | 20  | 2   | 44   | 64    |
| Bank            | 413 | 14  | 3   | 0    | 17    |

- **Highest WMC:** Person  
- **Highest CBO:** BankAccountApp / Bank (tie)  
- **Most concerning for maintenance:** Person, due to high WMC and very low cohesion (high LCOM: poor cohesion, methods are not well related, making it harder to understand and maintain.), making it harder to understand and maintain.
