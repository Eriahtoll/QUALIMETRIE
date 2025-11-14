# Example 4 — Code Quality and Static Analysis Report

## BankAccount.java — Line 164

```
public int loadFromText(String text) {
    int accountsLoaded = 0;
    Bank accManager = new Bank();
    FileInputStream fis = null;
    Scanner fileScanner = null;
    try {
        while (fileScanner.hasNextLine()) {
            fis = new FileInputStream(text);
```


**Issue:**  
The variable `fis` represents an external resource (`FileInputStream`) that must be properly closed after use.  
If the stream is not closed:

- the access to the resource remains open,
- the file descriptor (or port) stays occupied,
- the resource becomes unavailable for future operations.

In more critical contexts—such as database connections—failing to close resources can eventually exhaust all available ports or connections, making the entire system unavailable. This is why it is essential to use proper resource management patterns such as *try-with-resources* or closing streams explicitly in a `finally` block.

---

## BankAccountApp.java — Lines 92, 149, 197

```
if (operation.equalsIgnoreCase("BALANCE")) {
System.out.println("Balance is: " + acc1.getBalance());
}

if (operation.equalsIgnoreCase("BALANCE")) {
System.out.println("Enter Account Number");
number = scan.nextInt();

if (!operation.equalsIgnoreCase("BALANCE") && !operation.equalsIgnoreCase("DEPOSIT")
&& !operation.equalsIgnoreCase("WITHDRAW") && !operation.equalsIgnoreCase("QUIT")
```

**Issue:**  
The string `"BALANCE"` is used multiple times directly in the code instead of being stored in a constant.

Using a constant is better because:

- It prevents typos: if the constant is misspelled, the IDE will immediately flag it as an error.
- It improves maintainability: if the keyword ever needs to change, it must be modified only once rather than searching through the whole file.
- It increases readability and reduces duplication.

Hard-coded strings used multiple times should always be extracted into well-named constants.

---

## BankAccountApp.java — Line 178

```
System.out.println("Account dosen't exist");
```

**Issue:**  
The code uses `System.out.println` instead of a logging framework.

Using loggers is preferable because:

- Loggers provide multiple log levels (INFO, WARNING, SEVERE, DEBUG, etc.).
- In production, only high-severity logs (e.g., SEVERE, FATAL) are printed.
- During debugging, we can enable more verbose logs (INFO, DEBUG) to help diagnose issues.
- With `System.out`, **all messages are printed unconditionally**, with no way to filter or categorize them.
- Logging frameworks also offer formatting, handlers, output rotation, and configuration options that `System.out` cannot provide.

I started correcting logger-related issues in `BankAccountApp`, reducing the number of issues from **107 to 79**.

---

## Class Metrics Overview

| Class           | LOC | WMC | CBO | LCOM | Total | Issues |
|-----------------|-----|-----|-----|------|-------|--------|
| Person          | 325 | 23  | 0   | 79   | 102   | 9      |
| BankAccountApp  | 491 | 2   | 3   | 1    | 6     | 46     |
| BankAccount     | 462 | 20  | 2   | 44   | 64    | 13     |
| Bank            | 413 | 14  | 3   | 0    | 17    | 19     |

---

## Interpretation of the Metrics

Higher **CBO** (Coupling Between Objects) seems correlated to a higher number of issues in this project—for example, `BankAccountApp` and `Bank` have higher coupling and also more reported issues.

However, this correlation does **not** hold for **WMC** (Weighted Methods per Class).  
For instance:

- `Person` has a high WMC but few issues.
- `BankAccountApp` has a very low WMC but many issues.

This suggests that the origin of the issues is **multifactorial**, and we cannot rely solely on CBO or WMC to predict the number of issues. Factors such as code duplication, improper resource handling, poor logging practices, or lack of constants also play a major role.

---


# Exercice 5 - GitHub Actions

See main.yml in .github/workflows/main.yml on GitHub