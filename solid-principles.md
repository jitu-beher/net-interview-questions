The SOLID principles are a set of five design principles that help software developers create more maintainable, flexible, and scalable code. These principles are especially helpful when working with object-oriented programming languages like C#. Let's go over each principle with examples in C#:

### 1. **Single Responsibility Principle (SRP)**

**Principle**: A class should have only one reason to change, meaning it should have only one job or responsibility.

**Example**:

```csharp
// Violating SRP
public class Employee
{
    public string Name { get; set; }
    public decimal Salary { get; set; }

    public void CalculateSalary()
    {
        // Calculate salary logic
    }

    public void SaveToDatabase()
    {
        // Database save logic
    }
}

// Following SRP
public class Employee
{
    public string Name { get; set; }
    public decimal Salary { get; set; }
}

public class SalaryCalculator
{
    public void CalculateSalary(Employee employee)
    {
        // Salary calculation logic
    }
}

public class EmployeeDatabase
{
    public void SaveToDatabase(Employee employee)
    {
        // Database save logic
    }
}
```

Here, we split the responsibilities of calculating the salary and saving to the database into separate classes.

### 2. **Open/Closed Principle (OCP)**

**Principle**: A class should be open for extension, but closed for modification. This means you can add new functionality to a class without modifying its existing code.

**Example**:

```csharp
// Violating OCP
public class DiscountCalculator
{
    public decimal CalculateDiscount(string customerType, decimal amount)
    {
        if (customerType == "VIP")
            return amount * 0.1m;
        else if (customerType == "Regular")
            return amount * 0.05m;
        else
            return 0;
    }
}

// Following OCP using inheritance
public abstract class DiscountStrategy
{
    public abstract decimal CalculateDiscount(decimal amount);
}

public class VIPDiscountStrategy : DiscountStrategy
{
    public override decimal CalculateDiscount(decimal amount)
    {
        return amount * 0.1m;
    }
}

public class RegularDiscountStrategy : DiscountStrategy
{
    public override decimal CalculateDiscount(decimal amount)
    {
        return amount * 0.05m;
    }
}

public class DiscountCalculator
{
    private readonly DiscountStrategy _discountStrategy;

    public DiscountCalculator(DiscountStrategy discountStrategy)
    {
        _discountStrategy = discountStrategy;
    }

    public decimal CalculateDiscount(decimal amount)
    {
        return _discountStrategy.CalculateDiscount(amount);
    }
}
```

By using polymorphism and abstract classes, you can extend the discount calculation logic without changing the `DiscountCalculator` class.

### 3. **Liskov Substitution Principle (LSP)**

**Principle**: Objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program.

**Example**:

```csharp
// Violating LSP
public class Bird
{
    public virtual void Fly()
    {
        Console.WriteLine("Flying...");
    }
}

public class Ostrich : Bird
{
    public override void Fly()
    {
        throw new InvalidOperationException("Ostriches can't fly.");
    }
}

// Following LSP
public abstract class Bird
{
    public abstract void Move();
}

public class Sparrow : Bird
{
    public override void Move()
    {
        Console.WriteLine("Flying...");
    }
}

public class Ostrich : Bird
{
    public override void Move()
    {
        Console.WriteLine("Running...");
    }
}
```

By ensuring that subclasses follow the expected behavior of the superclass (i.e., replacing `Fly()` with a more general `Move()`), you prevent incorrect usage of objects.

### 4. **Interface Segregation Principle (ISP)**

**Principle**: A client should not be forced to depend on interfaces it doesn't use. This principle is about creating small, specific interfaces rather than large, general ones.

**Example**:

```csharp
// Violating ISP
public interface IWorker
{
    void Work();
    void Eat();
}

public class Worker : IWorker
{
    public void Work()
    {
        Console.WriteLine("Working...");
    }

    public void Eat()
    {
        Console.WriteLine("Eating...");
    }
}

public class Robot : IWorker
{
    public void Work()
    {
        Console.WriteLine("Working...");
    }

    public void Eat()
    {
        throw new NotImplementedException(); // Robots don't eat
    }
}

// Following ISP
public interface IWorkable
{
    void Work();
}

public interface IEatable
{
    void Eat();
}

public class Worker : IWorkable, IEatable
{
    public void Work()
    {
        Console.WriteLine("Working...");
    }

    public void Eat()
    {
        Console.WriteLine("Eating...");
    }
}

public class Robot : IWorkable
{
    public void Work()
    {
        Console.WriteLine("Working...");
    }
}
```

By splitting the interfaces, we prevent classes from being forced to implement methods they don't need (e.g., `Eat` for `Robot`).

### 5. **Dependency Inversion Principle (DIP)**

**Principle**: High-level modules should not depend on low-level modules. Both should depend on abstractions. Also, abstractions should not depend on details. Details should depend on abstractions.

**Example**:

```csharp
// Violating DIP
public class FileManager
{
    private Database _database;

    public FileManager()
    {
        _database = new Database();  // Direct dependency
    }

    public void SaveData()
    {
        _database.Save();
    }
}

public class Database
{
    public void Save()
    {
        Console.WriteLine("Saving data to the database.");
    }
}

// Following DIP using dependency injection
public interface IDataStorage
{
    void Save();
}

public class Database : IDataStorage
{
    public void Save()
    {
        Console.WriteLine("Saving data to the database.");
    }
}

public class FileManager
{
    private readonly IDataStorage _dataStorage;

    public FileManager(IDataStorage dataStorage)
    {
        _dataStorage = dataStorage;
    }

    public void SaveData()
    {
        _dataStorage.Save();
    }
}
```

In the second example, `FileManager` depends on the abstraction `IDataStorage`, not on the concrete class `Database`. This allows `FileManager` to work with any class that implements `IDataStorage`, making it more flexible.

---

These principles aim to create code that is more maintainable, testable, and flexible to changes in the future.
