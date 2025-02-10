.

Below is an example of how you can implement the State Design Pattern in Java for managing accounts with three states: `Open`, `Closed`, and `Suspended`.

### Step 1: Define the State Interface
The `AccountState` interface defines the methods that all concrete states must implement.

```java
public interface AccountState {
    void deposit(Account account, double amount);
    void withdraw(Account account, double amount);
    void suspend(Account account);
    void close(Account account);
    void activate(Account account);
}
```

### Step 2: Implement Concrete States
Implement the concrete states: `OpenState`, `ClosedState`, and `SuspendedState`.

```java
public class OpenState implements AccountState {
    @Override
    public void deposit(Account account, double amount) {
        account.setBalance(account.getBalance() + amount);
        System.out.println("Deposited: " + amount + ". New balance: " + account.getBalance());
    }

    @Override
    public void withdraw(Account account, double amount) {
        if (account.getBalance() >= amount) {
            account.setBalance(account.getBalance() - amount);
            System.out.println("Withdrew: " + amount + ". New balance: " + account.getBalance());
        } else {
            System.out.println("Insufficient funds.");
        }
    }

    @Override
    public void suspend(Account account) {
        account.setState(new SuspendedState());
        System.out.println("Account is now suspended.");
    }

    @Override
    public void close(Account account) {
        account.setState(new ClosedState());
        System.out.println("Account is now closed.");
    }

    @Override
    public void activate(Account account) {
        System.out.println("Account is already open.");
    }
}

public class SuspendedState implements AccountState {
    @Override
    public void deposit(Account account, double amount) {
        account.setBalance(account.getBalance() + amount);
        System.out.println("Deposited: " + amount + ". New balance: " + account.getBalance());
    }

    @Override
    public void withdraw(Account account, double amount) {
        System.out.println("Cannot withdraw from a suspended account.");
    }

    @Override
    public void suspend(Account account) {
        System.out.println("Account is already suspended.");
    }

    @Override
    public void close(Account account) {
        account.setState(new ClosedState());
        System.out.println("Account is now closed.");
    }

    @Override
    public void activate(Account account) {
        account.setState(new OpenState());
        System.out.println("Account is now open.");
    }
}

public class ClosedState implements AccountState {
    @Override
    public void deposit(Account account, double amount) {
        System.out.println("Cannot deposit into a closed account.");
    }

    @Override
    public void withdraw(Account account, double amount) {
        System.out.println("Cannot withdraw from a closed account.");
    }

    @Override
    public void suspend(Account account) {
        System.out.println("Cannot suspend a closed account.");
    }

    @Override
    public void close(Account account) {
        System.out.println("Account is already closed.");
    }

    @Override
    public void activate(Account account) {
        System.out.println("Cannot activate a closed account.");
    }
}
```

### Step 3: Define the Context Class
The `Account` class represents the context. It maintains a reference to the current state and delegates state-specific behavior to the current state object.

```java
public class Account {
    private AccountState state;
    private double balance;

    public Account() {
        this.state = new OpenState(); // Default state is Open
        this.balance = 0.0;
    }

    public void setState(AccountState state) {
        this.state = state;
    }

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {
        this.balance = balance;
    }

    public void deposit(double amount) {
        state.deposit(this, amount);
    }

    public void withdraw(double amount) {
        state.withdraw(this, amount);
    }

    public void suspend() {
        state.suspend(this);
    }

    public void close() {
        state.close(this);
    }

    public void activate() {
        state.activate(this);
    }
}
```

### Step 4: Test the Implementation
Finally, let's test the implementation with a simple `main` method.

```java
public class StatePatternDemo {
    public static void main(String[] args) {
        Account account = new Account();

        account.deposit(1000); // Deposited: 1000.0. New balance: 1000.0
        account.withdraw(500); // Withdrew: 500.0. New balance: 500.0

        account.suspend(); // Account is now suspended.
        account.withdraw(200); // Cannot withdraw from a suspended account.

        account.activate(); // Account is now open.
        account.withdraw(200); // Withdrew: 200.0. New balance: 300.0

        account.close(); // Account is now closed.
        account.deposit(100); // Cannot deposit into a closed account.
    }
}
```

### Output:
```
Deposited: 1000.0. New balance: 1000.0
Withdrew: 500.0. New balance: 500.0
Account is now suspended.
Cannot withdraw from a suspended account.
Account is now open.
Withdrew: 200.0. New balance: 300.0
Account is now closed.
Cannot deposit into a closed account.
```

### Explanation:
- The `Account` class delegates all state-specific behavior to the current `AccountState` object.
- Each state (`OpenState`, `SuspendedState`, `ClosedState`) implements the `AccountState` interface and defines behavior specific to that state.
- The state transitions are handled by the `Account` class, which updates its state based on the actions performed.

This design allows for easy addition of new states and ensures that the behavior of the `Account` class changes dynamically based on its current state.The State Design Pattern is a behavioral design pattern that allows an object to change its behavior when its internal state changes. This pattern is useful when an object needs to change its behavior based on its state, and you want to avoid large conditional statements.

Below is an example of how you can implement the State Design Pattern in Java for managing accounts with three states: `Open`, `Closed`, and `Suspended`.

### Step 1: Define the State Interface
The `AccountState` interface defines the methods that all concrete states must implement.

```java
public interface AccountState {
    void deposit(Account account, double amount);
    void withdraw(Account account, double amount);
    void suspend(Account account);
    void close(Account account);
    void activate(Account account);
}
```

### Step 2: Implement Concrete States
Implement the concrete states: `OpenState`, `ClosedState`, and `SuspendedState`.

```java
public class OpenState implements AccountState {
    @Override
    public void deposit(Account account, double amount) {
        account.setBalance(account.getBalance() + amount);
        System.out.println("Deposited: " + amount + ". New balance: " + account.getBalance());
    }

    @Override
    public void withdraw(Account account, double amount) {
        if (account.getBalance() >= amount) {
            account.setBalance(account.getBalance() - amount);
            System.out.println("Withdrew: " + amount + ". New balance: " + account.getBalance());
        } else {
            System.out.println("Insufficient funds.");
        }
    }

    @Override
    public void suspend(Account account) {
        account.setState(new SuspendedState());
        System.out.println("Account is now suspended.");
    }

    @Override
    public void close(Account account) {
        account.setState(new ClosedState());
        System.out.println("Account is now closed.");
    }

    @Override
    public void activate(Account account) {
        System.out.println("Account is already open.");
    }
}

public class SuspendedState implements AccountState {
    @Override
    public void deposit(Account account, double amount) {
        account.setBalance(account.getBalance() + amount);
        System.out.println("Deposited: " + amount + ". New balance: " + account.getBalance());
    }

    @Override
    public void withdraw(Account account, double amount) {
        System.out.println("Cannot withdraw from a suspended account.");
    }

    @Override
    public void suspend(Account account) {
        System.out.println("Account is already suspended.");
    }

    @Override
    public void close(Account account) {
        account.setState(new ClosedState());
        System.out.println("Account is now closed.");
    }

    @Override
    public void activate(Account account) {
        account.setState(new OpenState());
        System.out.println("Account is now open.");
    }
}

public class ClosedState implements AccountState {
    @Override
    public void deposit(Account account, double amount) {
        System.out.println("Cannot deposit into a closed account.");
    }

    @Override
    public void withdraw(Account account, double amount) {
        System.out.println("Cannot withdraw from a closed account.");
    }

    @Override
    public void suspend(Account account) {
        System.out.println("Cannot suspend a closed account.");
    }

    @Override
    public void close(Account account) {
        System.out.println("Account is already closed.");
    }

    @Override
    public void activate(Account account) {
        System.out.println("Cannot activate a closed account.");
    }
}
```

### Step 3: Define the Context Class
The `Account` class represents the context. It maintains a reference to the current state and delegates state-specific behavior to the current state object.

```java
public class Account {
    private AccountState state;
    private double balance;

    public Account() {
        this.state = new OpenState(); // Default state is Open
        this.balance = 0.0;
    }

    public void setState(AccountState state) {
        this.state = state;
    }

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {
        this.balance = balance;
    }

    public void deposit(double amount) {
        state.deposit(this, amount);
    }

    public void withdraw(double amount) {
        state.withdraw(this, amount);
    }

    public void suspend() {
        state.suspend(this);
    }

    public void close() {
        state.close(this);
    }

    public void activate() {
        state.activate(this);
    }
}
```

### Step 4: Test the Implementation
Finally, let's test the implementation with a simple `main` method.

```java
public class StatePatternDemo {
    public static void main(String[] args) {
        Account account = new Account();

        account.deposit(1000); // Deposited: 1000.0. New balance: 1000.0
        account.withdraw(500); // Withdrew: 500.0. New balance: 500.0

        account.suspend(); // Account is now suspended.
        account.withdraw(200); // Cannot withdraw from a suspended account.

        account.activate(); // Account is now open.
        account.withdraw(200); // Withdrew: 200.0. New balance: 300.0

        account.close(); // Account is now closed.
        account.deposit(100); // Cannot deposit into a closed account.
    }
}
```

### Output:
```
Deposited: 1000.0. New balance: 1000.0
Withdrew: 500.0. New balance: 500.0
Account is now suspended.
Cannot withdraw from a suspended account.
Account is now open.
Withdrew: 200.0. New balance: 300.0
Account is now closed.
Cannot deposit into a closed account.
```

#
