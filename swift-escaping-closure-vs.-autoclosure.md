# Swift Escaping Closure Vs. Autoclosure

In Swift, an **escaping closure** is a closure that is called **after** the function it was passed to has returned. This means the closure **escapes** the function’s scope and can be stored or executed later.

## Why Use Escaping Closures?

Escaping closures are commonly used in **asynchronous operations**, such as:

* **Completion handlers** in network requests
* **Delayed execution** of code
* **Storing closures** for later use

### How to Define an Escaping Closure

By default, Swift assumes closures are **non-escaping**, meaning they must be executed within the function. To explicitly allow a closure to escape, you use the `@escaping` keyword:

```swift
func fetchData(completion: @escaping () -> Void) {
    DispatchQueue.global().async {
        // Simulating network request
        sleep(2)
        DispatchQueue.main.async {
            completion() // Closure is executed later
        }
    }
}
```

### Example of Escaping vs. Non-Escaping

```swift
class Example {
    var storedClosure: (() -> Void)?

    func nonEscapingClosureExample(closure: () -> Void) {
        closure() // Must be executed inside the function
    }

    func escapingClosureExample(closure: @escaping () -> Void) {
        storedClosure = closure // Can be stored and executed later
    }
}
```

### Key Takeaways

* **Non-escaping closures** must be executed within the function.
* **Escaping closures** can be stored and executed later.
* **Use `@escaping`** when passing a closure that will be used asynchronously or stored.

You can find more details in the [Swift documentation](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/closures/) or check out discussions on [Stack Overflow](https://stackoverflow.com/questions/39504180/escaping-closures-in-swift). Let me know if you need further clarification! 🚀

In Swift, an **autoclosure** is a special type of closure that automatically wraps an expression inside a closure. This allows you to pass an expression as an argument without explicitly writing `{}` to create a closure.

## Why Use `@autoclosure`?

* **Delays execution** until it's actually needed.
* **Improves readability** by allowing expressions instead of full closures.
* **Used in short-circuiting operations** like `assert()` and logical operators (`&&`, `||`).

### Example Without `@autoclosure`

```swift
func logMessage(_ message: () -> String) {
    print("Log: \(message())")
}

logMessage({ "This is a log message" }) // Explicit closure syntax
```

### Example With `@autoclosure`

```swift
func logMessage(_ message: @autoclosure () -> String) {
    print("Log: \(message())")
}

logMessage("This is a log message") // No need for `{}`!
```

Here, `"This is a log message"` is automatically wrapped into a closure.

### Practical Use Case: Short-Circuiting

Swift uses `@autoclosure` in logical operations to **avoid unnecessary computation**:

```swift
func checkCondition(_ condition: @autoclosure () -> Bool) {
    if condition() {
        print("Condition met!")
    }
}

checkCondition(2 > 1) // No need for `{}` around `2 > 1`
```

### Key Takeaways

* **`@autoclosure`** automatically wraps expressions into closures.
* **Improves readability** by removing explicit `{}`.
* **Used in assertions and logical operations** to delay execution.

You can explore more details in the [Swift documentation](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/closures/) or check out practical examples on [Hacking with Swift](https://www.hackingwithswift.com/example-code/language/what-is-the-autoclosure-attribute) and [Stack Overflow](https://stackoverflow.com/questions/24102617/how-to-use-swift-autoclosure). Let me know if you need further clarification! 🚀

## **Autoclosure vs. Escaping Closure in Swift**

Both `@autoclosure` and `@escaping` modify how closures behave in Swift, but they serve **different purposes**.

***

### **1. Autoclosure (`@autoclosure`)**

* **Purpose:** Automatically wraps an expression inside a closure.
* **Execution:** Delays evaluation until the closure is called.
* **Use Case:** Short-circuiting operations (`&&`, `||`), assertions (`assert()`), and improving readability.

**Example**

```swift
func checkCondition(_ condition: @autoclosure () -> Bool) {
    if condition() {
        print("Condition met!")
    }
}

checkCondition(2 > 1) // No need for `{}` around `2 > 1`
```

Here, `2 > 1` is automatically wrapped into a closure.

***

### **2. Escaping Closure (`@escaping`)**

* **Purpose:** Allows a closure to be stored and executed **after** the function returns.
* **Execution:** The closure **escapes** the function’s scope.
* **Use Case:** Asynchronous operations (e.g., network requests, completion handlers).

**Example**

```swift
func fetchData(completion: @escaping () -> Void) {
    DispatchQueue.global().async {
        sleep(2) // Simulating network delay
        DispatchQueue.main.async {
            completion() // Executed later
        }
    }
}
```

Here, `completion` is stored and executed **after** `fetchData()` returns.

***

### **Key Differences**

| Feature       | `@autoclosure`                  | `@escaping`                                     |
| ------------- | ------------------------------- | ----------------------------------------------- |
| **Purpose**   | Wraps expressions into closures | Allows closure to be stored for later execution |
| **Execution** | Delayed until needed            | Executed after function returns                 |
| **Use Case**  | Short-circuiting, assertions    | Asynchronous operations, completion handlers    |
| **Syntax**    | `@autoclosure () -> Type`       | `@escaping () -> Type`                          |

For more details, check out the [Swift documentation](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/closures/) or discussions on [Stack Overflow](https://stackoverflow.com/questions/45658608/what-is-the-difference-and-purpose-of-auto-and-escaping-closure-in-swift). Let me know if you need further clarification! 🚀
