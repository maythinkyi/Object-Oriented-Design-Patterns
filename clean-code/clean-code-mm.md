# Clean Code 
---

## 1. What is Clean Code?

Developer တစ်ယောက် code ရေးတဲ့ အခါမှာ Clean Code ဆိုတာ ဘာကိုဆိုလိုချင်တာလဲဆိုလို့ရှိရင် Clean Code ရဲ့ အဓိပ္ပာယ်က **Readable**, **Testable**, **Maintainable**, နဲ့ **Extendable** ဖြစ်ရပါမယ်။ 

ဒီလိုမျိုး Clean Code ကို Follow ခြင်းအားဖြင့် ဘာတွေ လုပ်နိုင်သလဲဆိုလို့ရှိရင်:
* **Unit Testing:** Unit test ရေးတဲ့အခါမှာ လွယ်လွယ်ကူကူ ရေးနိုင်ပါတယ်။
* **Enhancements:** နောက်ပိုင်း enhancement တွေအတွက်လည်း သက်ဆိုင်ရာ function ကို extendable လွယ်လွယ်ကူကူ လုပ်သွားလို့ရပါတယ်။

---

## 2. Boy Scout Rule

Clean Code နဲ့ ပတ်သက်လို့ရှိရင် **Boy Scout Rule** ဆိုတဲ့ Philosophy တစ်ခုရှိပါတယ်။

> **"Leave the campground cleaner than you found it."**

တကယ်လို့ မင်းထွက်သွားပြီးတဲ့အခါကျရင် မင်းနေခဲ့တဲ့ ရပ်ဝန်းပတ်ဝန်းကျင်ကို ပိုပြီးတော့ သန့်ရှင်းအောင်လို့ ပြုလုပ်ပေးခဲ့ရပါမယ်။ 

Programming philosophy အရ ပြောရမယ်ဆိုလို့ရှိရင် မိမိမရှိခင်တုန်းကတည်းက ရှိနေခဲ့ပြီးသား ကုဒ်တွေကို မိမိထွက်သွားတဲ့အခါ ပိုမို ကောင်းမွန်အောင် ပြုပြင်ခဲ့တာကို ပြောခြင်းဖြစ်ပါတယ်။

---

## 3. Have Empathy When Coding

Code ကို ရေးခဲ့တဲ့ developer က domain knowledge တစ်ခုပေါ်မှာ အခြေခံပြီးတော့ ဒီ code ကို business logic အနေနဲ့ ရေးထားတာ ဖြစ်နိုင်ပါတယ်။ ဒီ domain knowledge ရှိထားပြီးသား လူတစ်ယောက်က ဒီ code ကို လာဖတ်ပြီးတော့ business logic ကို နားလည်တာ သဘာဝကျပါတယ်။

သို့သော်လည်း တခြား domain knowledge မရှိ၊ context information မရှိသေးဘဲ ကုမ္ပဏီကို စရောက်လာတဲ့ developer အသစ်က ဒီ code ကို ဖတ်တဲ့အခါမှာ လုံလောက်တဲ့ domain knowledge / context information မရှိရင်တောင်မှ သူက ဒီ code ကို ကြည့်ပြီးတော့ **လွယ်လွယ်ကူကူ နားလည်သဘောပေါက်အောင်** ရေးသားပေးထားရပါမယ်။ နောက်လာတဲ့ လူအသစ်က ကိုယ်ရေးခဲ့တဲ့ code ကို ကြည့်ပြီးတော့ ရှုပ်ယှက်ခတ်တာမျိုး၊ ခက်ခက်ခဲခဲ စဉ်းစားရတာမျိုး၊ ရှင်းရှင်းလင်းလင်း မဖြစ်တာမျိုး မဖြစ်သင့်ပါ။

---

## 4. Meaningful Namings

Variable, Function, Class များအတွက် naming တွေ ပေးတဲ့အခါမှာ random ဖြစ်သော၊ unclear ဖြစ်စေသော၊ သဘောပေါက်ဖို့ ခက်စေသော naming တွေ မပေးသင့်ပါ။

* ❌ **ရှောင်ရန်:** `a`, `b`, `anything`, `doSomething()`, `doAnything()`
* ✅ **လိုက်နာရန်:** Code ကို ဖတ်မယ့်သူက variable, function, class naming တွေကို ကြည့်ပြီးတော့မှ ဒီ variable က ဘာကြောင့်ရှိရတာလဲ၊ ဒီ function က ဘာကို အလုပ်လုပ်တာလဲ၊ ဒီ class ရဲ့ intention က ဘာလဲဆိုတာကို ချက်ချင်း နားလည်သွားစေမည့် naming များ ပေးရပါမည်။

---

## 5. Function Abstraction (Single Level of Abstraction)

Clean function တစ်ခုက **One Level of Abstraction** မှာပဲ operate လုပ်ရပါမည်။ 

ဥပမာအားဖြင့် `ProcessOrder` ဆိုတဲ့ function တစ်ခုရှိမယ်၊ သူက `Order` ဆိုတဲ့ parameter တစ်ခုကို လက်ခံမယ်ဆိုပါစို့။ အဲ့ဒီ `ProcessOrder` ရဲ့ implementation အထဲမှာ:
1. Order validation Logic
2. DB save order Logic
3. User notification Logic

အဆိုပါ low-level implementations တွေကို function တစ်ခုတည်းအောက်မှာ စုပြုံပြီး မရေးသင့်ပါ။ `validateOrder` ဆိုပြီး order validation implementation ကို abstraction တစ်ခုအနေနဲ့ ခွဲထုတ်ရပါမယ်။ DB save process ကိုလည်း `saveOrderToDB` အနေနဲ့ abstraction Provide ပေးရပါမယ်။ `notifyCustomer` အတွက်လည်း ထိုနည်းတူ ခွဲထုတ်ပေးရပါမည်။

### ❌ Clean မဖြစ်သည့် ပုံစံ (Mixed Levels of Abstraction)

```go
package main

import "fmt"

// Order ကို ကိုယ်စားပြုမည့် Struct
type Order struct {
	ID     string
	Amount float64
	Email  string
}

// ❌ Clean မဖြစ်ပါ - လုပ်ငန်းစဉ်အဆင့်ဆင့် (Abstraction Levels) ရောထွေးနေသည်
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

### ✅ Clean ဖြစ်သည့် ပုံစံ (Single Level of Abstraction)

```go
package main

import "fmt"

type Order struct {
	ID     string
	Amount float64
	Email  string
}

// ✅ Clean ဖြစ်သည် - ProcessOrder သည် High-level abstraction တစ်ခုတည်းကိုသာ လုပ်ဆောင်သည်
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

// --- Lower Abstraction Levels (သီးသန့်ခွဲထုတ်ထားသော Function များ) ---

func validateOrder(order Order) error {
	if order.ID == "" || order.Amount <= 0 {
		return fmt.Errorf("validation failed: invalid order details")
	}
	return nil
}

func saveOrderToDB(order Order) error {
	// Database query / saving implementation တွေ ရေးမည့်နေရာ
	fmt.Printf("[DB] Order %s saved successfully.\n", order.ID)
	return nil
}

func notifyCustomer(order Order) error {
	// Email / SMS ပို့မည့် implementation တွေ ရေးမည့်နေရာ
	fmt.Printf("[Notification] Email sent to %s for order %s.\n", order.Email, order.ID)
	return nil
}
```

---

## 6. Function Should Do Only One Thing (Single Responsibility)

Programming မှာ function တစ်ခုက အလုပ်တစ်ခုကိုပဲ လုပ်သင့်တယ်ဆိုပြီးတော့ ဖော်ပြထားတဲ့ **Single Responsibility Principle (SRP)** ဆိုတာရှိပါတယ်။ 

Function တစ်ခုမှာ validation logic, calculation logic, DB save logic များကို merge မလုပ်ထားရပါ။ Validation ဆိုရင် validation function သီးသန့်၊ Calculation နဲ့ DB save တာတွေအတွက်လည်း ၎င်းတို့နှင့် သက်ဆိုင်ရာ abstraction function သီးသန့် ရှိရပါမယ်။ ပြီးတော့မှ high-level abstraction function ကနေပဲ ဒီ function တွေကို ပြန်လည် ပေါင်းစပ် (compose) သုံးစွဲရပါမည်။

**အကျိုးကျေးဇူးများ:**
* Code များကို Reusable ဖြစ်စေသည်။
* Unit test ရေးသည့်အခါ Validation ကို သီးသန့် test လုပ်နိုင်သည်၊ Calculation ကို သီးသန့် test လုပ်နိုင်သည်၊ DB operation များကိုလည်း သီးသန့် test လုပ်နိုင်သွားမည် ဖြစ်သည်။

---

## 7. Dependency Inversion Principle (DIP)

Object-Oriented Design မှာ High-level module က Low-level module အပေါ်မှာ တိုက်ရိုက် coupling / dependency မရှိသင့်ပါ။ Abstraction (Interface) ပေါ်မှာသာ မှီခိုရပါမည်။

**ဥပမာ:** Car (High-level module) တစ်စီးမှာ `Car.Start()` ခေါ်လိုက်ရင် Engine (Low-level module) ရဲ့ `Engine.Start()` ကို လုပ်ဆောင်ပါတယ်။ တကယ်လို့ Engine က Diesel Engine သို့မဟုတ် Electric Motor သို့ ပြောင်းလဲသွားခဲ့ရင် Car Code ကို ပြန်လိုက်ပြင်ရသည့် အနေအထား မဖြစ်သင့်ပါ။ Dependency Inversion ကို လိုက်နာခြင်းအားဖြင့် Car ထဲသို့ Engine Interface မှတစ်ဆင့် dependency Inversion လုပ်ပေးလိုက်လျှင် တခြား မည်သည့် Engine ဘဲ ပြောင်းပြောင်း Car Code ကို ပြန်ပြင်စရာ မလိုတော့ပါ။

### ❌ Dependency Inversion မသုံးထားသည့် ပုံစံ (Tightly Coupled)

```go
package main

import "fmt"

// Low-level module (Diesel Engine တိုက်ရိုက်)
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

### ✅ Dependency Inversion သုံးထားသည့် ပုံစံ (Loosely Coupled)

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
	// Diesel Engine သုံးချင်လျှင်
	dieselCar := NewCar(DieselEngine{})
	dieselCar.StartCar()

	// Electric Motor သို့ ပြောင်းလဲသုံးချင်လျှင် Car Code ကို ပြင်စရာမလိုပါ
	electricCar := NewCar(ElectricMotor{})
	electricCar.StartCar()
}
```

---

## 8. Open/Closed Principle (OCP)

> **"Open for Extension, Closed for Modification"**

Software Entities တွေဖြစ်ကြသော Class, Module, Function များသည် Requirement အသစ်ထည့်ဖို့ လိုလာပါက နဂိုရှိနေပြီးသား Code များကို ပြုပြင်စရာ (Modify) မလိုဘဲ၊ Requirement အသစ်ကို Extend လုပ်ပြီး ထည့်သွင်းနိုင်သော ပုံစံမျိုး ဖြစ်ရပါမည်။

**ဥပမာ:** Payment Gateway ရဲ့ `ProcessPayment` function မှာ Payment Provider အသစ်များ (KPay, WavePay, AYAPay) ထပ်ထည့်ချင်ပါက ရှိပြီးသား `ProcessPayment` logic ကို လိုက်လံ ပြင်ဆင်စရာ မလိုရပါ။

### ❌ OCP ကို မလိုက်နာသည့် Design

```go
package main

import "fmt"

func ProcessPayment(method string, amount float64) {
	if method == "cash" {
		fmt.Println("Processing cash payment:", amount)
	} else if method == "card" {
		fmt.Println("Processing card payment:", amount)
	}
	// နောက်ထပ် KBZPay ထပ်ထည့်ချင်ရင် ရှိပြီးသား function ကို else-if ထပ်တိုးပြီး ပြန်ပြင်နေရမည်
}

func main() {
	ProcessPayment("cash", 100)
	ProcessPayment("card", 200)
}
```

### ✅ OCP ကို လိုက်နာသည့် Design

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

// Concrete Implementation 3 (Requirement အသစ် ထပ်တိုးခြင်း)
type KBZPayPayment struct{}

func (k KBZPayPayment) Pay(amount float64) {
	fmt.Println("Processing KBZPay:", amount)
}

// Existing function - Modify လုပ်စရာ မလိုပါ
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

Function တစ်ခုထဲသို့ parameter တွေ အများကြီး (ဥပမာ- `name`, `email`, `phone`, `address`, `occupation`) pass လုပ်ရပါက Argument Order Precedence ကို သတိထားရပါမည်။ Order မှားယွင်း pass မိပါက Logic များ လွဲမှားသွားနိုင်ပါသည်။

**ဖြေရှင်းနည်း:** Arguments တွေ အများကြီး သီးသန့် pass မည့်အစား **Parameter Object (Struct)** ကို အသုံးပြု၍ Object တစ်ခုတည်းအဖြစ် စုစည်းပြီး pass ပေးခြင်းက Clean Code အရ ပိုမိုကောင်းမွန်ပါသည်။

### ❌ Clean မဖြစ်သည့် ပုံစံ (Too Many Arguments)

```go
package main

import "fmt"

// ❌ Clean မဖြစ်ပါ - Parameter တွေ အများကြီးပါဝင်ပြီး Order မှားရင် Bug တက်နိုင်သည်
func CreateUser(name string, email string, phone string, address string, occupation string) {
	fmt.Printf("Creating user: %s, Email: %s\n", name, email)
}

func main() {
	// Function ခေါ်သုံးသည့်အခါ Order မှန်ဖို့ အလွန်အမင်း သတိထားရသည်
	CreateUser("Aung Aung", "aung@gmail.com", "09123456789", "Yangon", "Software Engineer")
}
```

### ✅ Clean ဖြစ်သည့် ပုံစံ (Parameter Object / Struct)

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

// ✅ Clean ဖြစ်သည် - Parameter Object ကိုသာ လက်ခံသည်
func CreateUser(user UserDetails) {
	fmt.Printf("Creating user: %s, Email: %s, Occupation: %s\n", user.Name, user.Email, user.Occupation)
}

func main() {
	// Struct Initializer သုံးထားသဖြင့် Order မှားမည့် ပြဿနာမရှိပါ
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

* **Command:** Database / System state ကို ပြောင်းလဲသည့် **Set/Write** operations များ။
* **Query:** Database / System ထံမှ Data ကို ဆွဲထုတ်သည့် **Get/Read** operations များ။

Function တစ်ခုထဲမှာ Set Operations နဲ့ Get Operations တွေရဲ့ Implementation တွေကို စုပြုံ မရေးသင့်ပါ။ Command/Set operation ကို သီးသန့် function၊ Query/Get operation ကို သီးသန့် function ခွဲထုတ်ပြီး High-level abstraction function ကနေမှ ပြန်လည် ယူသုံးရပါမည်။

### ❌ CQS မလိုက်နာထားသည့် ပုံစံ

```go
package main

import "fmt"

type User struct {
	ID    string
	Email string
}

// ❌ CQS မလိုက်နာပါ - Query (ဖတ်ခြင်း) ရော Command (ပြင်ခြင်း) ပါ တစ်နေရာတည်းမှာ ရောနေသည်
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

### ✅ CQS လိုက်နာထားသည့် ပုံစံ

```go
package main

import "fmt"

type User struct {
	ID    string
	Email string
}

// 1. Query Operation (Data ဖတ်ရန်သာ၊ State မပြောင်းပါ)
func GetUserByID(id string) (User, error) {
	fmt.Printf("[QUERY] Fetching user %s from database...\n", id)
	return User{ID: id, Email: "old@gmail.com"}, nil
}

// 2. Command Operation (State ကို ပြောင်းလဲရန်သာ)
func UpdateUserEmailInDB(id string, newEmail string) error {
	fmt.Printf("[COMMAND] Updating user %s email to %s in DB...\n", id, newEmail)
	return nil
}

// 3. High-Level Abstraction Function (Command နှင့် Query ကို ပေါင်းစပ်ပေးသည်)
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

* **Structured Programming:** `if-else`, `for`, `switch` စသည့် **Control Flow Structures** များကို ပံ့ပိုးပေးထားသော Programming ပုံစံဖြစ်သည်။ Code များ၏ စီးဆင်းမှုကို စနစ်တကျ ထိန်းချုပ်နိုင်သည်။
* **Unstructured Programming:** Control Flow ကို တိုက်ရိုက် မပံ့ပိုးဘဲ Assembly ကဲ့သို့ ဘာသာစကားများတွင် `goto` သို့မဟုတ် `jump` များကို အသုံးပြု၍ Condition အလိုက် သက်ဆိုင်ရာ နေရာများသို့ ခုန်ကူး (Jump) ခိုင်းသော ပုံစံဖြစ်သည်။

### ❌ Unstructured Programming ပုံစံ (Using Jumps/goto)

```go
package main

import "fmt"

// ❌ Unstructured Programming (goto / jump များကို အသုံးပြုထားသည်)
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

### ✅ Structured Programming ပုံစံ

```go
package main

import "fmt"

// ✅ Structured Programming (if-else control flow များကို အသုံးပြုထားသည်)
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