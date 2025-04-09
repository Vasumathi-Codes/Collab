# Delegates and Events in C# - Explained with Example

## 🔹 What is a Delegate in C#?
A **delegate** is like a *function pointer*. It defines the signature of a method and allows you to store references to methods in variables.

> ✅ It lets you pass methods around like data.

### Syntax:
```csharp
public delegate void MyDelegate(string message);
```
Any method with `void` return type and one `string` parameter can be assigned to `MyDelegate`.

---

## 🔹 What is an Event in C#?
An **event** is a special kind of delegate used to provide notifications. It allows a class to notify other classes or components when something happens.

> ✅ Events are based on delegates but provide controlled access — only the declaring class can invoke the event.

---

## ✅ Delegate + Event Flow

### Steps:
1. **Declare a delegate** — defines method signature.
2. **Declare an event** — based on that delegate.
3. **Subscribe** methods to the event.
4. **Invoke** the event — all subscribed methods get called.

---

## 🧩 Example Code: Build Notification System

### Delegate Declaration
```csharp
public delegate void BuildStatusHandler(string message);
```

### Event Declaration in a Class
```csharp
public event BuildStatusHandler? BuildCompleted;
public event BuildStatusHandler? BuildFailed;
```

### Triggering Events
```csharp
public void BuildSuccess(string message) {
    BuildCompleted?.Invoke(message);
}

public void BuildFailure(string message) {
    BuildFailed?.Invoke(message);
}
```

### Subscriber Classes
```csharp
class Logger {
    public void Log(string message) {
        Console.WriteLine("[LOG] :" + message);
    }
}

class Notifier {
    public void Notify(string message) {
        Console.WriteLine("[NOTIFICATION] :" + message);
    }
}

class RetryEngine {
    public void Retry(string message) {
        Console.WriteLine("[RETRY ENGINE] Retrying the build due to: " + message);
    }
}
```

### Subscribing to Events
```csharp
buildStatus.BuildCompleted += log.Log;
buildStatus.BuildCompleted += notifier.Notify;

buildStatus.BuildFailed += log.Log;
buildStatus.BuildFailed += notifier.Notify;
buildStatus.BuildFailed += retry.Retry;
```

### Program Execution
```csharp
int counter = 0;
int retryLimit = 5;

while (counter < retryLimit) {
    Console.WriteLine($"\nAttempt #{counter + 1} at building...");
    if (counter < 2) {
        buildStatus.BuildFailure($"Build failed at attempt {counter + 1}. Issue still unresolved.");
        Console.WriteLine("Waiting for 2 seconds before retrying...");
        Thread.Sleep(2000);
    } else {
        buildStatus.BuildSuccess($"Build completed successfully at attempt {counter + 1}.");
        break;
    }
    counter++;
}
```

---

## 🧠 Why Use This Pattern?

- **Delegates** allow flexible method referencing.
- **Events** provide structured and secure notifications.
- Encourages **loose coupling** — core logic doesn't need to know what subscribers do.

---

## ✅ Summary
| Concept    | Purpose                             |
|------------|--------------------------------------|
| Delegate   | Holds reference to method(s)         |
| Event      | Fires when something important happens |
| Subscriber| Method attached to an event           |
| Invocation | Event being triggered (called)        |

Use delegates + events to build modular, maintainable, and reactive applications.

