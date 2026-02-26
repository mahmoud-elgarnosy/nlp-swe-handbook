The Liskov Substitution Principle (LSP) is often summarized as: *“If S is a subtype of T, then objects of type T may be replaced with objects of type S without altering any of the desirable properties of the program.”*

However, to truly understand LSP, we must look at it through the lens of **Design by Contract (DbC)**. A subclass isn't just a child of a parent; it is a partner in a contract that the parent signed with its clients.

Here is a technical breakdown of the four rules of LSP, framed by DbC.

---

### 1. The Core Concept: Design by Contract

Think of a class method as a legal contract:

* **Pre-conditions (Requirements):** What the caller *must* provide (e.g., "x must be positive").
* **Post-conditions (Guarantees):** What the method *promises* to return or do (e.g., "I will return a number > 10").

LSP dictates that a subclass is allowed to change the implementation, but it **must not break the contract** established by the parent.

---

### 2. The Four Rules of Inheritance

#### Rule 1: Covariance of Return Types (Output can be stricter)

* **Definition:** The return type of a method in a subclass must be the *same as* or a *subtype of* (narrower than) the return type in the parent class.
* **The Logic:** If the client expects the parent to return a `Fruit`, and the subclass returns an `Apple` (which is a `Fruit`), the client is happy. The subclass is promising *more* specific information, which is safe.
* **Violation:** If the subclass returns an `Object` (or something broader/unrelated like a `String`), the client code, which expects a `Fruit` to call `.peel()`, will crash.

#### Rule 2: Contravariance of Arguments (Input can be looser)

* **Definition:** The arguments of a method in a subclass must be the *same as* or a *supertype of* (broader than) the arguments in the parent class.
* **The Logic:** If the parent method accepts `Apple`, the subclass can choose to accept `Fruit` (any fruit, including apples). The client, holding an `Apple`, can still call the method safely.
* **Violation:** If the parent accepts `Fruit`, but the subclass restricts it to only `Apple`, a client passing a `Banana` (valid for the parent) will crash the subclass. This is strengthening the pre-condition (see below).

#### Rule 3: Pre-conditions (Cannot be Strengthened)

* **Definition:** A subclass cannot require *more* from the client than the parent did. It cannot add new constraints to the input.
* **The Contract:** If the parent says, "I accept any number," the subclass cannot say, "I only accept positive numbers."
* **Why it breaks:** The client code is written against the parent. It believes it can pass `-5`. If you substitute the subclass, and it rejects `-5`, the system breaks.

#### Rule 4: Post-conditions (Cannot be Weakened)

* **Definition:** A subclass cannot guarantee *less* than the parent did. It must fulfill all promises made by the parent.
* **The Contract:** If the parent promises, "I will save this to the database and return `True`," the subclass cannot say, "I might save it, and I'll return `void`."
* **Why it breaks:** The client code expects the database save to happen. If the subclass silently fails or does less work, the system is in an inconsistent state.

---

### 3. Code Example: Strengthening Pre-conditions

This is the most common and subtle violation. In this example, we have a `Bird` class. The contract is that birds can fly.

The `Ostrich` subclass violates LSP by adding a new pre-condition: "I can only fly if my height is < 0" (which effectively means never). This throws an exception the client doesn't expect.

#### ❌ Violation: The Ostrich Problem

```python
class Bird:
    def set_location(self, lat, long):
        self.latitude = lat
        self.longitude = long
        
    def fly(self, altitude):
        # The Contract (Pre-condition): Altitude can be any number > 0.
        self.altitude = altitude
        print(f"Flying at {altitude} feet.")

class Ostrich(Bird):
    def fly(self, altitude):
        # ❌ VIOLATION: Strengthening the Pre-condition.
        # The parent allowed any altitude. The child forbids it completely.
        raise NotImplementedError("Ostriches cannot fly!")

# Client Code
def migrate_bird(bird: Bird):
    # The client trusts the 'Bird' contract.
    # It assumes ANY bird can fly.
    bird.set_location(10, 20)
    
    try:
        bird.fly(1000) 
    except NotImplementedError:
        print("CRASH! The system failed because this bird broke the contract.")

# Usage
generic_bird = Bird()
migrate_bird(generic_bird)  # Works fine.

ostrich = Ostrich()
migrate_bird(ostrich)       # CRASHES!

```

### ✅ Refactoring for LSP

To fix this, we must recognize that `Ostrich` is *not* a subtype of a `FlyingBird`. We separate the hierarchy.

```python
class Bird:
    # Common bird behavior (eating, sleeping)
    pass

class FlyingBird(Bird):
    def fly(self, altitude):
        print(f"Flying at {altitude} feet.")

class FlightlessBird(Bird):
    pass # No fly method at all

class Eagle(FlyingBird):
    def fly(self, altitude):
        # Satisfies the contract
        print(f"Eagle soaring at {altitude}")

class Ostrich(FlightlessBird):
    pass 

# Client Code
def migrate_flying_bird(bird: FlyingBird):
    bird.fly(1000)

eagle = Eagle()
migrate_flying_bird(eagle) # Works

ostrich = Ostrich()
# migrate_flying_bird(ostrich) # Static type checker error! Code won't even compile/run.
# This prevents the runtime crash.

```
The Liskov Substitution Principle (LSP) is the "correctness" check for inheritance. While the Open/Closed Principle encourages you to extend code using polymorphism, LSP tells you **when** that extension is valid.

The principle, introduced by Barbara Liskov in 1987, is often summarized as:

> **"Subtypes must be substitutable for their base types."**

However, to truly understand *why* a subclass breaks code, we must look through the lens of **Design by Contract**. This framework views a class as having a "contract" with its client: "If you give me X (Pre-condition), I promise to do Y (Post-condition)."

Here is the technical breakdown of the four rules that enforce this contract.

---

### 1. The Variance Rules (Data Flow)

These rules govern the *types* of data entering and leaving the methods.

#### A. Covariance of Return Types (Output)

* **The Rule:** The return type of a subclass method must be the **same as** or **more specific than** (a subtype of) the return type of the base class.
* **The Logic:** If the client expects the base class to return a `Fruit`, and the subclass returns an `Apple` (which *is-a* `Fruit`), the client is happy. The client can still peel it, eat it, or juice it. However, if the subclass returns an `Object` (which might be a Car), the client code breaks.
* **Formal:** $Return(Sub) \preceq Return(Base)$
#### B. Contravariance of Arguments (Input)

* **The Rule:** The arguments of a subclass method must be the **same as** or **more general than** (a supertype of) the arguments of the base class.
* **The Logic:** This is counter-intuitive. If the base class method `feed(Apple a)` accepts an `Apple`, the subclass can override it to `feed(Fruit f)`. Why? Because if the client passes an `Apple` (as the base contract allows), the subclass (which can handle *any* `Fruit`) can certainly handle that `Apple`.
* **The Violation:** If the subclass narrows the input to `feed(RedDelicious a)`, it violates LSP. The client might pass a `GrannySmith` (valid for the base), but the subclass will reject it.
* **Formal:** $Input(Base) \preceq Input(Sub)$

---

### 2. The Condition Rules (Logic Flow)

These rules govern the logical *state* required for execution.

#### C. Pre-conditions (The "Requirement")

* **The Rule:** A subclass cannot strengthen pre-conditions. It cannot require *more* of the client than the base class did.
* **The Logic:** Imagine the Base Class says, "I accept any number greater than 0." The Client writes code knowing this rule. If the Subclass says, "I only accept numbers greater than 100," it has **strengthened** the requirement. When the client passes `50` (valid for the base), the subclass crashes.
* **Formal:** $Pre(Base) \implies Pre(Sub)$

#### D. Post-conditions (The "Guarantee")

* **The Rule:** A subclass cannot weaken post-conditions. It cannot guarantee *less* than the base class did.
* **The Logic:** If the Base Class promises, "I will always save the data to the database," the Subclass cannot override this with "I might save it, or I might just cache it." The client code relies on that data being in the DB. If the subclass does less, the system enters an invalid state.
* **Formal:** $Post(Sub) \implies Post(Base)$

---

### 3. Code Example: Pre-condition Strengthening

The most common LSP violation involves a subclass throwing an exception for an input that the base class would have accepted.

#### Scenario: A Banking System

We have a standard `BankAccount` that allows withdrawal as long as you have the funds. We then create a `FreezingAccount` (perhaps for fraud detection) that adds a new rule: you cannot withdraw if the account is "frozen."

#### ❌ The Violation

```python
class BankAccount:
    def __init__(self, balance):
        self.balance = balance

    def withdraw(self, amount):
        # Pre-condition: Amount must be <= balance
        if amount > self.balance:
            raise ValueError("Insufficient funds")
        self.balance -= amount
        print(f"Withdrew ${amount}. Remaining: ${self.balance}")

class FrozenAccount(BankAccount):
    def __init__(self, balance):
        super().__init__(balance)
        self.is_frozen = True

    def withdraw(self, amount):
        # ❌ VIOLATION: Strengthening the Pre-condition.
        # The Base Class contract says: "If amount <= balance, I will succeed."
        # This Subclass says: "If amount <= balance AND NOT frozen, I will succeed."
        if self.is_frozen:
            raise RuntimeError("Account is frozen! Cannot withdraw.")
        
        super().withdraw(amount)

# --- The Client Code ---
def process_withdrawal(account: BankAccount, amount: float):
    # The client trusts the Base Class contract.
    # "If I check the balance, I should be safe to withdraw."
    if account.balance >= amount:
        try:
            account.withdraw(amount)
        except ValueError:
            # The client expects ValueError (defined in Base),
            # but does NOT expect RuntimeError.
            print("Client handled insufficient funds.")

# Usage
standard_acc = BankAccount(100)
process_withdrawal(standard_acc, 50) # Works fine.

risky_acc = FrozenAccount(100)
# CRASH! The client receives a RuntimeError it was not designed to catch.
# The 'risky_acc' is NOT substitutable for 'standard_acc'.
process_withdrawal(risky_acc, 50) 

```

#### ✅ The Fix (Refactoring)

To fix this, the "frozen" concept must be part of the base contract, or the hierarchy is wrong.

1. **Change the Hierarchy:** `FrozenAccount` should perhaps not inherit `withdraw` if it cannot support it.
2. **Change the Contract:** Update `BankAccount` to include a method `can_withdraw()` that checks both balance and state. The client must call `can_withdraw()` instead of just checking `balance`.

```python
class BankAccount:
    # ... init ...
    
    def can_withdraw(self, amount):
        # Base rule: do we have money?
        return self.balance >= amount

    def withdraw(self, amount):
        if not self.can_withdraw(amount):
             raise ValueError("Cannot withdraw")
        # ... logic ...

class FrozenAccount(BankAccount):
    # ... init ...

    def can_withdraw(self, amount):
        # Subclass adds its own rule implies the Base rule is respected
        # strictly as part of the check, not a hidden crash.
        return super().can_withdraw(amount) and not self.is_frozen

```

Now, the client calls `can_withdraw()`. Whether the object is a `BankAccount` or `FrozenAccount`, the method returns a boolean, and the client logic holds true.

---

Here are 4 specific examples of Liskov Substitution Principle (LSP) violations, one for each of the core rules.

These examples are written in Python. Since Python is dynamically typed, these violations often result in runtime errors (`AttributeError`, `TypeError`) rather than compile-time errors, making them particularly dangerous in large systems.

---

### 1. Violation of Covariance (Return Types)

**The Rule:** A subclass must return the same type or a *more specific* type (subtype) than the base class.
**The Violation:** The subclass returns a *more general* type (supertype) or an unrelated type.

**Scenario:** We have a system for vehicle factories. The client expects a full `Car` object with an engine, but the subclass only returns a generic `Vehicle` shell or a string ID.

```python
class Vehicle:
    def start_engine(self):
        print("Vroom!")

class Car(Vehicle):
    def open_trunk(self):
        print("Trunk opened.")

# Base Class
class CarFactory:
    def create(self) -> Car:
        return Car()

# ❌ VIOLATION: Subclass returns a wider/different type
class GenericFactory(CarFactory):
    def create(self) -> Vehicle:
        # Returns a generic Vehicle, NOT a Car.
        return Vehicle()

# --- The Client Code ---
def get_ready(factory: CarFactory):
    vehicle = factory.create()
    vehicle.start_engine()
    # CRASH: 'Vehicle' object has no attribute 'open_trunk'
    # The client expects a Car (from CarFactory contract), but got a Vehicle.
    vehicle.open_trunk() 

```

---

### 2. Violation of Contravariance (Method Arguments)

**The Rule:** A subclass must accept the same argument types or *more general* types (supertypes) than the base class.
**The Violation:** The subclass requires a *more specific* argument type (subtype). This restricts what the client can pass in.

**Scenario:** An event handling system. The base handler can handle any `Event`. The subclass is "picky" and only handles `ClickEvent`.

```python
class Event:
    pass

class ClickEvent(Event):
    self.x = 10
    self.y = 20

# Base Class
class EventHandler:
    def handle(self, event: Event):
        print("Handling event...")

# ❌ VIOLATION: Subclass restricts input to only ClickEvent
class ClickHandler(EventHandler):
    def handle(self, event: ClickEvent):
        # This crashes if passed a generic Event because it expects .x and .y
        print(f"Clicked at {event.x}, {event.y}")

# --- The Client Code ---
def dispatch(handler: EventHandler, event: Event):
    # The client thinks: "This is an EventHandler, so it can handle ANY Event."
    handler.handle(event)

# Usage
general_event = Event()
# CRASH: ClickHandler tries to access .x on a generic Event object.
dispatch(ClickHandler(), general_event)

```

---

### 3. Violation of Pre-condition Strengthening

**The Rule:** A subclass cannot impose stricter rules on input than the base class.
**The Violation:** The subclass adds validation checks that the base class did not have.

**Scenario:** A `PaymentProcessor`. The base class accepts any positive amount. The subclass (for MicroTransactions) rejects amounts over $50.

```python
# Base Class
class PaymentProcessor:
    def pay(self, amount):
        # Base Pre-condition: Amount must be positive
        if amount <= 0:
            raise ValueError("Amount must be positive")
        print(f"Paid ${amount}")

# ❌ VIOLATION: Subclass adds a NEW restriction
class MicroPaymentProcessor(PaymentProcessor):
    def pay(self, amount):
        # Subclass Pre-condition: Amount must be <= 50 (Stricter!)
        if amount > 50:
            raise ValueError("Micro payments cannot exceed $50")
        
        super().pay(amount)

# --- The Client Code ---
def process_salary(processor: PaymentProcessor):
    # Client logic relies on the Base Contract: "Positive numbers represent valid payments."
    # The client has no idea about the $50 limit.
    processor.pay(1000) 

# Usage
# CRASH: The client tries to pay $1000, which is valid for PaymentProcessor
# but fails for MicroPaymentProcessor.
process_salary(MicroPaymentProcessor())

```

---

### 4. Violation of Post-condition Weakening

**The Rule:** A subclass must fulfill all guarantees (effects) of the base class.
**The Violation:** The subclass fails to perform a task the base class promised to do.

**Scenario:** A `DataManager` that promises to save data to a database *and* close the connection. The subclass decides to leave the connection open for performance (Connection Pooling), but fails to inform the client or handle the cleanup elsewhere.

```python
# Base Class
class DataManager:
    def save_data(self, data):
        print("Data saved to disk.")
        self._close_connection() # Post-condition: Connection is always closed.

    def _close_connection(self):
        print("Connection closed.")

# ❌ VIOLATION: Subclass removes a guarantee
class PooledDataManager(DataManager):
    def save_data(self, data):
        print("Data saved to cache.")
        # We purposely DO NOT close the connection to reuse it.
        # But the client expects the connection to be closed!

# --- The Client Code ---
def backup_system(manager: DataManager):
    manager.save_data("User backup")
    # Client assumes connection is closed and it's safe to delete the lock file.
    delete_lock_file() 

# Usage
# CRASH: The file system is corrupted because PooledDataManager left
# the file handle open, violating the post-condition of the base class.
backup_system(PooledDataManager())

```

### Summary of Violations

| Rule | The Base Class Says... | The Bad Subclass Says... | Why it Breaks |
| --- | --- | --- | --- |
| **Covariance** | "I return a Fruit." | "I return a PlasticModel." | Client tries to eat the PlasticModel. |
| **Contravariance** | "I accept any Fruit." | "I only accept Apples." | Client passes a Banana; Subclass chokes. |
| **Pre-conditions** | "Bring cash." | "Bring cash AND wear a tie." | Client brings cash, gets kicked out for no tie. |
| **Post-conditions** | "I will fix the car." | "I will look at the car." | Client drives away in a broken car. |

---

**Would you like to move on to the Interface Segregation Principle (ISP), or discuss how Python's `abc` module can help enforce some of these rules?**