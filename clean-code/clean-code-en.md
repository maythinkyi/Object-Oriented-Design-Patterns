# Clean Code

---

## 1. What is Clean Code?

When a developer writes code, "Clean Code" means the codebase must be **Readable**, **Testable**, **Maintainable**, and **Extendable**.

By following Clean Code principles, you achieve the following benefits:
- **Unit Testing:** Unit tests become easy and straightforward to write.
- **Enhancements:** Future feature enhancements can easily extend existing functions without hassle.

---

## 2. Boy Scout Rule

In Clean Code practice, there is a well-known philosophy called the **Boy Scout Rule**:

> **"Leave the campground cleaner than you found it."**

When you leave a place, you should leave it cleaner than it was when you arrived.

In programming philosophy, this means whenever you touch or edit existing code written before your time, you should leave it better and cleaner than how you found it.

---

## 3. Have Empathy When Coding

The original author of a piece of code usually writes business logic based on their domain knowledge. Someone who already shares that domain knowledge will naturally find it easy to read and understand.

However, a new developer joining the team might lack that domain context and knowledge. Even without sufficient initial context, they should still be able to read and understand your code easily. Future developers should not feel confused, overwhelmed, or bogged down by overly complex logic when reading your code.

---

## 4. Meaningful Naming

When naming variables, functions, and classes, avoid random, unclear, or hard-to-understand names.

- ❌ **Avoid:** `a`, `b`, `anything`, `doSomething()`, `doAnything()`
- ✅ **Follow:** Anyone reading the code should immediately understand *why* a variable exists, *what* a function does, and *what* the intention of a class is just by looking at their names.

---

## 5. Function Abstraction (Single Level of Abstraction)

A clean function should operate at **One Level of Abstraction**.

For instance, consider a function named `ProcessOrder` that accepts an `Order` parameter. Inside its implementation, you might have:
1. Order validation logic
2. Database save order logic
3. User notification logic

These low-level implementation details should not be crammed together inside a single function. Instead, extract order validation into a `validateOrder` function to provide a separate abstraction layer. Provide another abstraction layer for database saving (`saveOrderToDB`), and do the same for notifications (`notifyCustomer`).

### ❌ Non-Clean Pattern (Mixed Levels of Abstraction)

```go
package main

import "fmt"

// Struct representing an Order
type Order struct {
  ID     string
  Amount float64
  Email  string
}

// ❌ Not Clean - Mixing multiple levels of abstraction
func ProcessOrder(order Order) error {
  // 1. Validation Logic (Low-level detail)
  if order.ID == "" || order.Amount <= 0 {
    return fmt.Errorf("invalid order details")
  }

  // 2. Database Logic (Low-level detail)
  fmt.Printf("Saving order %s to database...\n", order.ID)

  // 3. Notification Logic (Low-level detail)
  fmt.Printf("Sending email notification to %s...\n", order.Email)

  return nil
}
```

### ✅ Clean Pattern (Single Level of Abstraction)

```go
package main

import "fmt"

type Order struct {
  ID     string
  Amount float64
  Email  string
}

// ✅ Clean - ProcessOrder only operates at a single high-level abstraction
func ProcessOrder(order Order) error {
  if err := validateOrder(order); err != nil {
    return err
  }

  if err := saveOrderToDB(order); err != nil {
    return err
  }

  if err := notifyCustomer(order); err != nil {
    return err
  }

  return nil
}

// --- Lower Abstraction Levels (Extracted Functions) ---

func validateOrder(order Order) error {
  if order.ID == "" || order.Amount <= 0 {
    return fmt.Errorf("validation failed: invalid order details")
  }
  return nil
}

func saveOrderToDB(order Order) error {
  // Database query / saving implementation
  fmt.Printf("[DB] Order %s saved successfully.\n", order.ID)
  return nil
}

func notifyCustomer(order Order) error {
  // Email / SMS notification implementation
  fmt.Printf("[Notification] Email sent to %s for order %s.\n", order.Email, order.ID)
  return nil
}
```

---

## 6. Functions Should Do Only One Thing (Single Responsibility)

In software development, the **Single Responsibility Principle (SRP)** states that a function should do only one thing.

A single function should not combine validation logic, calculation logic, and database saving logic together. Validation logic belongs in its own validation function, while calculation and DB operations should also have their own single-level abstraction functions. Finally, a high-level abstraction function composes these single-level abstraction functions together.

**Key Benefits:**
- Makes code reusable.
- Enables isolated unit testing (e.g., testing validation, calculation, or DB operations independently).

---

## 7. Dependency Inversion Principle (DIP)

In Object-Oriented Design, high-level modules should not depend directly on low-level modules; both should depend on abstractions (interfaces).

**Example:** Calling `Car.Start()` on a `Car` (high-level module) triggers `Engine.Start()` on an `Engine` (low-level module). If the engine changes from a Diesel Engine to an Electric Motor, the `Car` code should not need modification. By following Dependency Inversion, we inject the `Engine` interface into the `Car`. This ensures that no matter how the engine implementations change, the `Car` code remains untouched.

### ❌ Without Dependency Inversion (Tightly Coupled)

```go
package main

import "fmt"

// Low-level module (Direct dependency on Diesel Engine)
type DieselEngine struct{}

func (d DieselEngine) Start() {
  fmt.Println("Diesel engine started...")
}

// High-level module
type Car struct {
  engine DieselEngine // ❌ Tightly coupled to DieselEngine
}

func NewCar() Car {
  return Car{engine: DieselEngine{}}
}

func (c Car) StartCar() {
  c.engine.Start()
}
```

### ✅ With Dependency Inversion (Loosely Coupled)

```go
package main

import "fmt"

// --- 1. Abstraction (Interface) ---
type Engine interface {
  Start()
}

// --- 2. Low-level Modules (Concrete Implementations) ---

type DieselEngine struct{}

func (d DieselEngine) Start() {
  fmt.Println("Vroom! Diesel engine started.")
}

type GasolineEngine struct{}

func (g GasolineEngine) Start() {
  fmt.Println("Vroom! Gasoline engine started.")
}

type ElectricMotor struct{}

func (e ElectricMotor) Start() {
  fmt.Println("Silent! Electric motor started.")
}

// --- 3. High-level Module ---
type Car struct {
  engine Engine // ✅ Depends on Abstraction (Interface)
}

// Constructor - Dependency Inversion
func NewCar(e Engine) Car {
  return Car{engine: e}
}

func (c Car) StartCar() {
  c.engine.Start()
}

func main() {
  // Using a Diesel Engine
  dieselCar := NewCar(DieselEngine{})
  dieselCar.StartCar()

  // Switching to an Electric Motor requires zero changes to the Car struct
  electricCar := NewCar(ElectricMotor{})
  electricCar.StartCar()
}
```

---

## 8. Open/Closed Principle (OCP)

> **"Open for Extension, Closed for Modification"**

Software entities (classes, modules, functions, etc.) should be open for extension when new requirements arise, but closed for modification regarding existing source code.

**Example:** In a Payment Gateway's `ProcessPayment` function, adding new payment providers (e.g., KPay, WavePay, AYAPay) should not require modifying the existing `ProcessPayment` code logic.

### ❌ Violating OCP

```go
package main

import "fmt"

func ProcessPayment(method string, amount float64) {
  if method == "cash" {
    fmt.Println("Processing cash payment:", amount)
  } else if method == "card" {
    fmt.Println("Processing card payment:", amount)
  }
  // Adding KBZPay requires modifying this existing function with another else-if block
}

func main() {
  ProcessPayment("cash", 100)
  ProcessPayment("card", 200)
}
```

### ✅ Adhering to OCP

```go
package main

import "fmt"

// Abstraction
type PaymentProcessor interface {
  Pay(amount float64)
}

// Concrete Implementation 1
type CashPayment struct{}

func (c CashPayment) Pay(amount float64) {
  fmt.Println("Processing cash payment:", amount)
}

// Concrete Implementation 2
type CardPayment struct{}

func (c CardPayment) Pay(amount float64) {
  fmt.Println("Processing card payment:", amount)
}

// Concrete Implementation 3 (Extending with a new requirement)
type KBZPayPayment struct{}

func (k KBZPayPayment) Pay(amount float64) {
  fmt.Println("Processing KBZPay:", amount)
}

// Existing function - No modification required
func ProcessPayment(payment PaymentProcessor, amount float64) {
  payment.Pay(amount)
}

func main() {
  ProcessPayment(CashPayment{}, 100)
  ProcessPayment(CardPayment{}, 200)
  ProcessPayment(KBZPayPayment{}, 300)
}
```

---

## 9. Handling Functions with Too Many Arguments

When passing multiple arguments (e.g., `name`, `email`, `phone`, `address`, `occupation`) to a single function, parameter order precedence becomes a major risk. Passing arguments out of order can introduce subtle runtime bugs.

**Solution:** Instead of passing many separate arguments, encapsulate them into a **Parameter Object (Struct)** and pass that single object to the function.

### ❌ Non-Clean Pattern (Too Many Arguments)

```go
package main

import "fmt"

// ❌ Not Clean - Too many parameters; easy to break due to incorrect parameter order
func CreateUser(name string, email string, phone string, address string, occupation string) {
  fmt.Printf("Creating user: %s, Email: %s\n", name, email)
}

func main() {
  // Callers must be extremely careful with parameter ordering
  CreateUser("Aung Aung", "aung@gmail.com", "09123456789", "Yangon", "Software Engineer")
}
```

### ✅ Clean Pattern (Parameter Object / Struct)

```go
package main

import "fmt"

// --- Parameter Object (Struct) ---
type UserDetails struct {
  Name       string
  Email      string
  Phone      string
  Address    string
  Occupation string
}

// ✅ Clean - Accepts a single Parameter Object
func CreateUser(user UserDetails) {
  fmt.Printf("Creating user: %s, Email: %s, Occupation: %s\n", user.Name, user.Email, user.Occupation)
}

func main() {
  // Using struct field initialization eliminates order precedence errors
  newUser := UserDetails{
    Name:       "Aung Aung",
    Email:      "aung@gmail.com",
    Phone:      "09123456789",
    Address:    "Yangon",
    Occupation: "Software Engineer",
  }

  CreateUser(newUser)
}
```

---

## 10. Command Query Separation (CQS)

- **Command:** **Set/Write** operations that mutate system or database state.
- **Query:** **Get/Read** operations that fetch data without modifying state.

A single function should not mix set and get operation details together. Command operations and Query operations should be extracted into separate functions. A higher-level abstraction function can then compose and execute them as needed.

### ❌ Violating CQS

```go
package main

import "fmt"

type User struct {
  ID    string
  Email string
}

// ❌ Violates CQS - Query (reading) and Command (writing/updating) are mixed in one function
func ProcessAndSaveUser(id string, newEmail string) (User, error) {
  // Query part (Get data)
  fmt.Printf("Fetching user %s from DB...\n", id)
  user := User{ID: id, Email: "old@gmail.com"}

  // Command part (Set/Update data)
  fmt.Printf("Updating user %s email in DB to %s...\n", id, newEmail)
  user.Email = newEmail

  return user, nil
}
```

### ✅ Adhering to CQS

```go
package main

import "fmt"

type User struct {
  ID    string
  Email string
}

// 1. Query Operation (Reads data only; does not mutate state)
func GetUserByID(id string) (User, error) {
  fmt.Printf("[QUERY] Fetching user %s from database...\n", id)
  return User{ID: id, Email: "old@gmail.com"}, nil
}

// 2. Command Operation (Mutates state only)
func UpdateUserEmailInDB(id string, newEmail string) error {
  fmt.Printf("[COMMAND] Updating user %s email to %s in DB...\n", id, newEmail)
  return nil
}

// 3. High-Level Abstraction Function (Composes Query and Command operations)
func ProcessUserEmailChange(userID string, newEmail string) error {
  user, err := GetUserByID(userID)
  if err != nil {
    return fmt.Errorf("user not found: %v", err)
  }

  if user.Email == newEmail {
    return fmt.Errorf("new email is the same as old email")
  }

  if err := UpdateUserEmailInDB(userID, newEmail); err != nil {
    return fmt.Errorf("failed to update email: %v", err)
  }

  fmt.Println("User email process completed successfully.")
  return nil
}

func main() {
  err := ProcessUserEmailChange("U123", "new_email@gmail.com")
  if err != nil {
    fmt.Println("Error:", err)
  }
}
```

---

## 11. Structured vs. Unstructured Programming

- **Structured Programming:** A paradigm that relies on explicit **Control Flow Structures** such as `if-else`, `for`, and `switch`. Program execution flows predictably and systematically.
- **Unstructured Programming:** A paradigm lacking high-level control flows, commonly found in low-level languages like Assembly, relying on explicit `goto` or `jump` statements to jump between lines of execution based on conditions.

### ❌ Unstructured Programming Pattern (Using Jumps / `goto`)

```go
package main

import "fmt"

// ❌ Unstructured Programming (uses goto/jumps)
func CheckAgeUnstructured(age int) {
  if age > 18 {
    goto AdultLabel
  } else {
    goto MinorLabel
  }

AdultLabel:
  fmt.Println("Allowed: You are an adult.")
  goto EndLabel

MinorLabel:
  fmt.Println("Denied: You are a minor.")
  goto EndLabel

EndLabel:
  fmt.Println("Process completed (Unstructured).")
}

func main() {
  CheckAgeUnstructured(20)
}
```

### ✅ Structured Programming Pattern

```go
package main

import "fmt"

// ✅ Structured Programming (uses structured if-else control flow)
func CheckAgeStructured(age int) {
  if age > 18 {
    fmt.Println("Allowed: You are an adult.")
  } else {
    fmt.Println("Denied: You are a minor.")
  }

  fmt.Println("Process completed (Structured).")
}

func main() {
  CheckAgeStructured(20)
}
```