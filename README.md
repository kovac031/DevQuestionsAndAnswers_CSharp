# Questions that every C# developer should answer:

## 1. what is an object in C#? What is a class?

Classes either contain methods/functions that are executed in the program/app or serve as structure templates for objects.

Objects are instances of the latter type of classes. Objects are entities, anything I want to store or process data about.

```C#
public class Person
{
   public string Name { get; set; }
   public datetime DOB { get; set; }
}

...
Person person = new Person(); // instantiate the Person object using "new"
person.Name = "Ivan";
person.DOB = new DateTime(1992, 3, 24);
```

## 2. list and describe four fundamental object oriented programming concepts.

Nesto tu ne stima, razlicti izvori objasnjavaju drugacije abstraction i encapsulation, i sad aj ti znaj

### Abstraction
The ability for objects to have parameters. The ability to instantiate objects from classes. 

Sources explain this as "hiding" code from the user, referring to only needing to instantiate the object and calling the relevant method and not needing to do anything with the underlying class that we instantiate into the object.

iz nekog drugog odgovora:
Abstraction: Interfaces are a way to achieve abstraction in your code, separating the definition of behavior from its implementation details. This promotes good coding practices and design principles like the Dependency Inversion Principle (DIP) and the Liskov Substitution Principle (LSP).


### Encapsulation
Bundling attributes of an object with methods/functions that operate on those attributes into single units = classes

```C#
public class Person
{
   public string Name { get; set; }
}
...
Person person = new Person();
person.Name = "Ivan";
Console.WriteLine(person.Name); // outputs Ivan
```
```C#
public class Person
{
   private string name;
   public void GetName()
   {
       Console.WriteLine(name); // outputs Ivan
   }
   public void SetName(string x)
   {
       name = x;
   }
}
...
Person person = new Person();
person.SetName("Ivan");
person.GetName();
```
### Inheritance

Sets up parent-child class relationship. Child class inherits/implements all parameters of the parent class.

Child classes inherit all the parameters of the parent class, but can have additional for-them particular parameters or methods. 

Person class can be parent to Employee class or Client class. Shape class can be parent to Circle or Square.

### Polymorphism

Allows objects of different classes to be treated as objects of a common base/parent class.

E.g. using the same method name. Circle and Square inherit the Draw method from Shape class.

```C#
public class Shape
{
    public void Draw()
    {
        Console.WriteLine("Drawing a shape.");
    }
}
public class Circle : Shape
{
    public void Draw()
    {
        Console.WriteLine("Drawing a circle.");
    }
}
public class Square : Shape
{
    public void Draw()
    {
        Console.WriteLine("Drawing a square.");
    }
}
...
Shape circle = new Circle();
Shape square = new Square();

circle.Draw(); // Outputs "Drawing a circle."
square.Draw(); // Outputs "Drawing a square."

// "method hiding"
```

## 3. what are constructors?
A special method with the purpose of initializing objects from different classes. Cannot have a return type, cannot be void.

Constructors have the same name as the class they belong to.

Used for injecting dependencies.

```C#
public class BoardGameController : Controller
    {
        public IBoardGameService BoardGameService { get; set; }
        public IReviewService ReviewService { get; set; }
         // constructor:
        public BoardGameController(IBoardGameService boardGameService, IReviewService reviewService)
        {
            BoardGameService = boardGameService; // initializes values for BoardGameService object
            ReviewService = reviewService;
        }
...
```

## 4. what is method overloading?
The ability to use the same method name for methods with different parameters.
```C#
[HttpGet]
public async Task<ActionResult> CreateBoardGameAsync()
{ ... }
[HttpPost]
public async Task<ActionResult> CreateBoardGameAsync(BoardGameView boardGameView, string sorting)
{ ... }
```

## 5. describe method overriding. What is the purpose of the virtual statement?
When calling a method makes it use the child class implementation instead of the parent class implementation.

Parent class method must be virtual or override.

```C#
public class Shape
{
    public virtual void Draw()
    {
        Console.WriteLine("Drawing a shape.");
    }
}
public class Circle : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a circle.");
    }
}
public class Square : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a square.");
    }
}
...
Shape circle = new Circle();
Shape square = new Square();

circle.Draw(); 
square.Draw(); 

// basically the same as method hiding, only keyword differences (override/virtual)
```

## 6. list access modifiers in C#.
> public

... accessible from anywhere.

> private

... not visible from outside of the class.

> protected

... accesible from same class and any child classes

> private protected

... accessible from same assembly and child classes within that assembly

> internal

... accesible from the same assembly

> internal protected

... accessible from the same assembly and child classes in other assemblies


## 7. what is the difference between a Struct and a Class?
Struct is  value type and Class is a reference type.

```C#
public class Person
{
}
public struct Person // must have different name though
{
}
...
Person person2 = person1;
```

In a class, person1 and person1 reference the same object.

In a struct, person2 is a copy of person1, and we have two different objects.

## 8. describe boxing and unboxing in the context of C#.

Boxing converts a non-object data type to a on object, unboxing reverses it.

```C#
List<object> listOfObjects = new List<object>(); // object is a data type
int meaningOfLife = 42;

listOfObjects.Add(meaningOfLife); // Boxing

int retrievedValue = (int)listOfObjects[0]; // Unboxing
```

Not to be confused with mapping.

## 9. which design principles are described by the acronym SOLID? List and describe each of them.
- Single Responsibility Principle
> A class should have only one reason to change, meaning it should have a single responsibility. In other words, a class should do one thing and do it well.

https://dotnetcoretutorials.com/solid-in-c-single-responsibility-principle/

- Open/Closed Principle
> Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. E.g. change parameters in the actual helper method being called in the controller method, but don't introduce parameters in the controller method just so the method can work.

https://dotnetcoretutorials.com/solid-in-c-open-closed-principle/

- Liskov Substitution Principle * nije jasno
> Objects of a derived class should be able to replace objects of the base class without affecting the correctness of the program. In other words, if you have a class hierarchy, derived classes should be substitutable for their base classes without causing issues. (but, apparently this would cause errors anyway)

https://dotnetcoretutorials.com/solid-in-c-liskov-principle/

- Interface Segregation Principle * nije jasno
> Clients should not be forced to depend on interfaces they do not use. In essence, it encourages the creation of specific, smaller interfaces rather than large, monolithic ones.

https://dotnetcoretutorials.com/solid-in-c-interface-segregation-principle/

- Dependency Inversion Principle * kuzim ali ne znam objasnit
> High-level modules should not depend on low-level modules; both should depend on abstractions. Abstractions should not depend on details; details should depend on abstractions. This principle encourages the use of interfaces or abstract classes to define dependencies, promoting flexibility and decoupling.

> Dependeny injection is the implementation of dependency inversion principle.

https://dotnetcoretutorials.com/solid-in-c-dependency-inversion/


## 10. explain DRY acronym in the context of object oriented programming.
Don't Repeat Yourself = DRY

> When the same code or logic is needed in multiple places, it should be abstracted into a single, reusable component, such as a function, method, or class.

## 11. describe the difference between break and continue statement.

Break stops the loop when the condition is met.

Continue skips the iteration when the condition is met.
```C#
for (int i = 0; i < 5; i++)
{
    if (i == 2)
    {
        continue; 
    }
    Console.WriteLine(i); // 0,1,3,4 - skipped 2
}
```

## 12. what are the different approaches of passing parameters to a method?
Pass value type by value or reference, pass reference type by value or reference.

> Pass by value means passing a copy of the variable to the method.

> Pass by reference means passing access to the variable to the method.

> A variable of a reference type contains a reference to its data.

> A variable of a value type contains its data directly.

Passing value type, by value vs by reference:
```C#
int n = 5;
int m = 5;
```
```C#
SquareIt(n);  // passing the variable type by value
static void SquareIt(int x) // takes n
{
    x *= x; // x is 25
}
// n stays 5
```
```C#
SquareIt(ref m) // passing the variable type by reference
static void SquareIt(ref int x) // takes m
{
    x *= x; // x is 25
}
// m becomes 25
```

Passing reference, by value vs by reference:
```C#
int[] array = { 1, 2, 3 };
```
```C#
Change(array); // passing the reference type by value
static void Change(int[] pArray)
{
    pArray[0] = 42;  // This change affects the original element.
    pArray = new int[3] { -3, -2, -1 };   // This change is local.
}
// inside the method the first element becomes -3
// after execution, the first element is 42 instead of 1
```
```C#
Change(ref array); // passing the reference type by reference
static void Change(ref int[] pArray)
{
   // Both of the following changes will affect the original variables:
    pArray[0] = 42;  
    pArray = new int[3] { -3, -2, -1 };  
}
// inside the method the first element becomes -3
// after execution, the first element is -3 instead of 1
```
Referencing the original value always changes the original value.

> int is a value type, while int[] is a reference type, which explains the changes to the array and not the int despite the code seeming the same

https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/method-parameters

## 13. can multiple catch blocks be executed?
No, only the first in the sequence.

## 14. what are static properties and methods?
Static properties/methods are properties/methods of a class, rather than properties/methods tied to objects created from that class.

Used when we won't be creating objects BUT want the same properties or method to apply across instances of a class.

## 15. what is the purpose of using statement?
```C#
using System;
```
For including the System namespace, which has its own classes etc, so we don't have to specify the namespace each time:
```C#
System.Console.WriteLine(...);
```
Or to define a scope, resource management/cleanup:
```C#
using (SqlConnection connection = new SqlConnection(connectionString))
{
}
```

## 16. what is serialization?
Serialization converts complex objects, which may contain various data types and nested structures, into a stream of bytes or a textual format.
Used for e.g. passing objects across methods, storing session data.

```C#
HttpContext.Session.SetString("SelectedCharacters", JsonConvert.SerializeObject(_cardList)); // from my chinese characters app
```

## 17. what is an interface in C#?
An interface declares what its child class should have and should do.

https://www.youtube.com/watch?v=RuhGv81tpoU

## 18. What's the difference between an interface and abstract class?
A class can inherit from multiple interfaces, but only one base class. Abstract classes can serve as base class. Abstract classes cannot be instantiated. 

While interfaces serve as "contracts", abstract classes serve as a common space to share code implementation across child classes (it's DRY). 

Unlike with interfaces, child classes do not have to implement everything from the abstract class.

## 19. can we specify the accessibility modifier for methods inside the interface?
Interface methods are always public. We do not set the access modifiers inside interfaces.

## 20. describe the difference between value and reference types.
A variable of a reference type contains a reference to its data.
A variable of a value type contains its data directly.

See answer for #12.

## 21. what are sealed classes?
Classes which are finalized, will not be extended further or modified and cannot be inherited, i.e. serve as base class.

Has to do with inheritance. "Either design for inheritance, or else prohibit it"

## 22. what are partial classes?
Allows dividing a class into parts. E.g. "public class Person" can have multiple "public partial class Person" (same name) that effectively behave the same, but help with code readibality.

## 23. What are the differences between System.String and System.Text.StringBuilder classes?
System.String objects are immutable, meaning they can't be changed once created. Changing is actually creating a new object.
System.Text objects are mutable, you append/insert/remove/replace/clear the same string you created, not create new ones.

```C#
using System;

public class Program
{
    public static void Main()
    {
        string immutableString = "Hello ";
        immutableString += "world!"; // Creates a new string for concatenation

        Console.WriteLine(immutableString); // Hello world!
    }
}
```
```C#
using System.Text;

public class Program
{
    public static void Main()
    {
        StringBuilder mutableStringBuilder = new StringBuilder("Hello ");
        mutableStringBuilder.Append("world!"); // Modifies the same object

        Console.WriteLine(mutableStringBuilder.ToString()); // Hello world!
    }
}
```

## 24. what are generics in the context of C#?
Instead of using overloading and having same-name methods that accept different parameters when we want the method to accept a different data type, we use a generic T data type instead.

```C#
...
int[] intArray = { 1,2,3 };
double[] doubleArray = { 1.0, 2.0, 3.0 };
string[] stringArray = { "1", "2", "3" };

displayElements(intArray);
displayElements(doubleArray);
displayElements(stringArray);
}
public static void displayElements<T>(T[] array) // T can be any word
{
   foreach (T item in array)
   {
      Console.Write(item + "\n");
   }
}
```

## 25. what are delegates in C#?
Allows to pass methods as parameters in other methods, and to instantiate methods (using new) like we'd do with objects from classes.

```C#
public delegate int MyDelegate(int a, int b);

public class Program
{
    public static int PerformOperation(int x, int y, MyDelegate operation) // passing MyDelegate as parameter
    {
        return operation(x, y);
    }
    public static int Add(int a, int b)
    {
        return a + b;
    }
    public static int Subtract(int a, int b)
    {
        return a - b;
    }
    public static void Main()
    {
        PerformOperation(5, 3, Add);        // int, int, MyDelegate
        PerformOperation(8, 2, Subtract);  
    }
}
```
```C#
MyDelegate operation = new MyDelegate();
MyDelegate addMethod = operation.Add;
MyDelegate subtractMethod = operation.Subtract;
```

## 26. what are nullable types in C#?
Data type which can hold null value ... e.g. int?, bool?

## 27. what are indexers in C#?
Allows for indexing of object lists or arrays.

"this" is a keyword

```C#
public class CardCollection
    {
        private List<CardDTO> cards = new List<CardDTO>(); // already have a CardDTO model class (from my Chinese project)

        public CardDTO this[int index] // Define a custom collection class for CardDTO objects
        {
            get { return cards[index]; }
            set { cards[index] = value; }
        }        
        public void Add(CardDTO card) // Add a CardDTO object to the collection.
        {
            cards.Add(card);
        }
    }
public class Program
    {
      public static void Main()
        {
            CardCollection collection = new CardCollection();

            collection.Add(new CardDTO { Simplified = "你好", Traditional = "你好", Pinyin = "Nǐ hǎo" }); // indexes are assigned to entries as they are added
            collection.Add(new CardDTO { Simplified = "谢谢", Traditional = "謝謝", Pinyin = "Xièxiè" });

            Console.WriteLine(collection[0].Simplified); // 你好
            Console.WriteLine(collection[1].Simplified); // 谢谢
        }
    }
```

## 28. what are C# attributes and how they can be used?
Attributes specify metadata or custom behavior that will apply to classes or methods.

```C#
[HttpGet]
[HttpPost]
...
```
```C#
class MyAttribute : Attribute // similar to controller naming convention
{
}
[My] // because of name MyAttribute 
class SomeClass
{
   [My]
   public int MyProperty { get; set; }
   [My]
   public void MyMethod()
   {
   }
}
```

## 29. explain extension methods.
Static methods within static classes where we implement functionality that would otherwise clutter or be repeated (counter DRY) in main methods. Easier and cleaner to just call a helper method from outside.

## 30. what is reflection in C#?
Reflection is the ability of a code to access the metadata of the assembly during runtime. We're retrieving some specific info we need.

for example:
```C#
Type myType = Type.GetType("System.String");
```
```C#
PropertyInfo propertyInfo = typeof(MyClass).GetProperty("MyProperty");
```
```C#
MethodInfo methodInfo = typeof(MyClass).GetMethod("MyMethod");
```
```C#
Type[] types = Assembly.GetExecutingAssembly().GetTypes();
```

## 31. What is managed or unmanaged code in .NET?
Managed code is executed by CLR (Common Language Runtime), while unmanaged code falls outside of that scope and is executed by the OS.

All code I wrote or encountered within the .NET environment was managed code, while potentially some added libraires may have contained unmanaged code.

## 32. what are namespaces?
Namespaces act as containers or folders for your code, prevent naming conflicts by providing a unique scope for each group of types and can be nested within other namespaces, creating a hierarchical structure. This allows for even finer-grained organization of your code.

```C#
using System; // using is a relevant keyword, System is the used namespace here
using Model;
using DAL;
...
```

## 33. describe arrays in C#.
A data structure which allows for storing and manipulating a collection of indexed elements of the same data type.

## 34. what kinds of collections exist in C#?
> Arrays

> BitArray

> Lists

> ArrayList

> LinkedList

> Dictionaries

> Queues

> Stacks

> HashSet

> SortedSet

> Hashtable

> Concurrent Collections

> Immutable Collections

## 35. what is LINQ?
LINQ (Language Integrated Query) is a powerful feature in C# and .NET that enables you to query and manipulate collections of data in a more concise, readable, and expressive manner. LINQ provides a unified query syntax to work with various data sources, such as arrays, collections, databases, XML, and more.

LINQ is often described as a query language because it allows you to express queries in a way that resembles SQL (Structured Query Language). You can filter, project, group, join, and order data using a familiar syntax.
```C#
...
if (!string.IsNullOrEmpty(searchBy)) // trazi po imenu i prezimenu
            {
                filteredList = listDTO.Where(x =>
                    x.FirstName.Contains(searchBy) ||
                    x.LastName.Contains(searchBy))
                    .ToList();
            }
...
List<StudentDTO> filteredDTO = filteredList.Skip((pageNumber - 1) * pageSize)
                                                        .Take(pageSize)
                                                        .ToList();
...
```

## 36. describe the difference between IEnumerable and IQueryable.
Both are for handling collections, but IEnumerable executes immediately and stores a collection in memory, which is performance heavy, while IQueryable builds a query and does not execute until .ToList

```C#
IEnumerable<Customer> collection = db.GetCustomersAsEnumerable(); // gets all customers

var goodCustomers = collection.Where(c => c.Revenue > 2500); // narrows it to some amount of goodCustomers
```
```C#
IQueryable<Customer> query = db.GetCustomersAsQueryable(); // gets no customer

goodCustomers = query.Where(c => c.Revenue > 2500);) // again, no customers

var goodCustomersList = goodCustomers.ToList(); // gets some amount of customers based on criteria
```

## 37. describe the concept of lazy loading.
The approach in designing your code so that required data is loaded as necessary instead of in advance. E.g. not loading an entire list first and then narrowing it down.

There is also the Lazy<T> "wrapper" which makes it so that a method is not executed in normal order but only when the variable (of an instanced class) is first used elsewhere in code. (I guess something like async, did not encounter this yet).

## 38. explain the concept of deferred execution.
Execution of code later, like with .ToList or .First with IQueryable (see #36)

## 39. what is the purpose of the C# yield statement?
Using yield return with loops we return results one by one on each iteration, as opposed to waiting for the entire sequence to complete first and then see results.

https://www.youtube.com/watch?v=uv74SZ5MX5Q

## 40. explain lambda expressions.
=> is the lambda operator, reads as "goes to" or "by". Lambda expression is way of writing concise code.

```C#
void MyMathMethod()
{
    int Add(int a, int b) => a + b;
    int result = Add(3, 4); // 3 + 4 = 7
}
```
```C#
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };
List<int> evenNumbers = numbers.Where(x => x % 2 == 0);

```
```C#
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };
numbers.RemoveAll(x => x % 2 == 0); // Removes even numbers
```

## 41. what is type inference?
The ability to not have to specifically declare each data type, it gets infered with var.

## 42. explain Func and Action delegates.
Delegate types. Func is a generic delegate. Action delegate does not return a value (void type).

```C#
static void Main(string[] args)
{
   Func<double,double> square = Square; // takes double as input, outputs double, square variable calls Square method
}
static double Square(double nmb) => Math.Pow(nmb, 2);
```
```C#
static void Main(string[] args)
{
   Action<double> square = Square;
   square(4);
}
static void Square(double nmb) => Console.WriteLine(Math.Pow(nmb, 2));
```

## 43. what is Entity Framework?
Entity Framework (EF) is an Object-Relational Mapping (ORM) framework developed by Microsoft.

Object-Relational Mapping (ORM) is a programming technique and framework that allows developers to work with databases using object-oriented concepts. It bridges the gap between the relational databases, which store data in tables, and object-oriented programming languages, which use objects to represent data and behavior.

## 44. what are Context and Entity classes in EF?
The Context class, derived from DbContext in EF, represents the database session and is responsible for the interaction between your application and the database.

Entity classes contain properties that map to the columns in the database table.

```C#
// Entity class representing a "Product" table
public class Product
{
    public int ProductId { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}

// Context class representing the database session
public class MyDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }
}
```

## 45. explain asynchronous method calls with async/await pattern.
Asynchronous refers to a style of executing tasks where a program doesn't need to wait for one task to complete before moving on to the next one.

```C#
async Task<int> CalculateAsync()
{
    int result = await SomeAsyncOperation();
    return result;
}
```

## 46. what is Dependency Injection. How and why is it used?
Passing the classes that your class depends on as interfaces via the constructor rather than having your class create those dependencies.

```C#
public class OrderService
{
    private readonly IDatabase _database;

    public OrderService(IDatabase database)
    {
        _database = database;
    }
    public void PlaceOrder(Order order)
    {
        _database.Save(order);
    }
}
```
> If we need to change database related code, we'll do it in IDatabase and not in PlaceOrder in OrderService.

## 47. what types of Dependency Injection exist in C#?
Constructor injection:
```C#
public class OrderService
{
    private readonly IDatabase _database;

    public OrderService(IDatabase database)
    {
        _database = database;
    }
}
```

Property injection:
```C#
public class OrderService
{
    public IDatabase Database { get; set; }

    public void PlaceOrder(Order order)
    {
         ...
        Database.Save(order);
    }
}
```

Method injection:
```C#
public class OrderService
{
    public void ProcessOrder(IDatabase database, Order order)
    {
        database.Save(order);
    }
}
```

## 48. what DI scopes are available with the container you used? Explain the scenarios for usage of each of these scopes.
DI containers I used:
- Autofac in .NET EF 4.7.2 [(1)](https://github.com/kovac031/PlayPalMini-MVC/blob/main/PlayPalMini/PlayPalMini.MVC/App_Start/DIContainer.cs), [(2)](https://github.com/kovac031/PlayPalMini-WebAPI/blob/main/PlayPalMini/PlayPalMini.WebAPI/App_Start/DependencyInjectionConfig.cs)
- ASP.NET Core's built-in DI container [(1)](https://github.com/kovac031/AutoMapper-Core/blob/main/ProjectMVC/MVC/Program.cs), [(2)](https://github.com/kovac031/AutoMapper-Core/blob/main/ProjectWebAPI/WebAPI/Program.cs)

Transient scope - instances are created each time they are called.
> used when I don't want data to persist during app usage

Scoped - instances are created once per http request.
> used when I want some data to persist, session data and such

Singleton scope - instances are created the first time they are requested and persist while the app is running.
> used for things that need to persist and stay the same, some settings or preferences

```C#
builder.Services.AddScoped<IService, StudentService>();
builder.Services.AddScoped<IRepository, StudentRepository>();
```
```C#
builder.Services.AddTransient<IService, StudentService>();
builder.Services.AddTransient<IRepository, StudentRepository>();
```
```C#
builder.Services.AddSingleton<IService, StudentService>();
builder.Services.AddSingleton<IRepository, StudentRepository>();
```

## 49. what are software patterns?
Design patterns are typical solutions to commonly occurring problems in software design. They are like pre-made blueprints that you can customize to solve a recurring design problem in your code.

## 50. explain several design patters (structural, creational, behavioral, ...). For example, singleton, factory, adapter, repository, etc. 
> Recommended resource for more info: https://dotnettutorials.net/course/dot-net-design-patterns/

Creational design patterns:
> deals with Object Creation and Initialization

- Singleton
  
Instead of creating new objects when we call a class (instantiate with "new"), we instead set it up so that the same object is used (we do ClassName.MethodName). [video](https://www.youtube.com/watch?v=YGGg9ecy0K4&list=PL6n9fhu94yhUbctIoxoVTrklN3LMwTCmd&index=2)

If multiple threads, it may violate singleton because additional instances may be created on different threads, [solution](https://www.youtube.com/watch?v=QWrcOmLWi_Q&list=PL6n9fhu94yhUbctIoxoVTrklN3LMwTCmd&index=4).

- Factory Method

A factory is an object which can create other objects. 

Factory method design pattern is about creating objects without exposing the creation logic to the client. Subclasses choose which class to instantiate.

> E.g. client is selecting some product for whatever reason ([factory is making boats or trucks](https://refactoring.guru/design-patterns/factory-method)) - instead of having all logic be inside a switch loop or similar and have it duplicated for trucks and boats, we place the logic in a different class, a single "factory", and simply call a method that accept the client's choice as parameter.

- Abstract Factory
Like Factory method pattern, but instead of creating a single object, it creates multiple objects.

- Builder
- Prototype
- Fluent Interface



Structural Design Pattern:
> used to Manage the Structure of Classes and Interfaces and the Relationship Between the Classes and Interfaces

- Adapter
- Facade
- Decorator
- Composite
- Proxy
- Flyweight
- Bridge

Behavioral Design Patterns:
> deal with the Communication Between Classes and Objects

- Chain of Responsibility
- Command
- Observer
- Iterator
- State
- TemplateMethod
- Visitor
- Strategy
- Mediator
- Memento
- Interpreter

Architectural Patterns:
> deal with the overall architecture and organization of an application

- Repository
- Clean
- MVC


## 51. what is unit testing?
Testing individual units of code, such as methods/functions, to see if they produce the expected result in isolation.

In contrast, there is integration testing which tests how multiple components work together.

## 52. what is mocking? What is its purpose?
Mocking is a technique used in unit testing. It involves creating fake or "mock" objects to simulate the behavior of real components or dependencies. 

The purpose of mocking is to isolate the code being tested by replacing its dependencies with these mock objects. This helps in testing the code in isolation and ensures that the tests focus solely on the unit of code under examination. (E.g. when testing controller methods, we don't want to ACTUALLY call the methods in the service layer, so we mock that)

# Basic RDBMS and SQL questions:

## 1. What is a difference between a column and a field?
A column is a collection of cells alligned vertically in a table. 

A field is an element in which one piece of information is stored.

## 2. What is primary key?
The primary key in SQL is a single, or a group of fields or columns that can uniquely identify a row in a table.

## 3. What is natural vs surrogate primary key?
Natural primary keys are unique identifiers that we already would have included in our table, like OIB, driver's card licence number.

Surrogate primary keys are those we include in addition to data we'd normally collect, like Guid or Int (if we know they wont repeat), just so we have a unique identifier for each row.

## 4. Can primary key be defined via multiple columns?
Yes.

From my [practie repository](https://github.com/kovac031/SQL-practice/blob/main/createOtherTable-ForeignKey-InsertIntoFancy.sql)
```SQL
CREATE TABLE TheOrdersTheProducts (
OrderId uniqueidentifier not null,
ProductId uniqueidentifier not null,
CONSTRAINT FK_TheOrdersTheProducts_TheOrders_OrderId FOREIGN KEY (OrderId) REFERENCES TheOrders(Id),
CONSTRAINT FK_TheOrdersTheProducts_TheProducts_ProductId FOREIGN KEY (ProductId) REFERENCES TheProducts(Id),
PRIMARY KEY (OrderId, ProductId)
);
```

## 5. Describe the concept of database relation.
Databse relation refers to how tables are related to each other, specifying the associations between records in different tables. 

One-to-One: 
> Each record in one table is associated with exactly one record in another table, and vice versa.

One-to-Many: 
>Each record in one table can be associated with one or more records in another table, but each record in the second table is associated with only one record in the first table.

Many-to-Many: 
>Each record in one table can be associated with multiple records in another table, and vice versa, creating a many-to-many relationship. This type of relationship is often implemented using a junction table.


## 6. What is a foreign key?
## 7. Can foreign key be nullable if primary key is not?
## 8. What is a purpose of a JOIN keyword? What types of JOINs do you know?
## 9. What is a temp table / table variable and when would you use it?
## 10. What is an index and what is its purpose?
## 11. What is a check constraint?
## 12. Explain the difference between a stored procedure and a function.
## 13. What is a query plan?
## 14. What is parametrized query?
## 15. What is sql transaction?

# Basic Web development topics:

## 1. explain the concept of MVC.
## 2. name several different types of results returned by action methods.
## 3. What is the difference between ViewResult and ActionResult?
## 4. what are filters in MVC? Name some of them.
## 5. how routing works in MVC? What types of routing exist?
## 6. what are the differences between TempData, ViewData and ViewBag?
## 7. explain the difference between View and Partial View.
## 8. what is ViewModel in MVC?
## 9. what are HTML helpers?
## 10. what are layouts and what is their purpose?
## 11. what is Razor? How are code blocks defined in Razor?
## 12. how is validation implemented in ASP.NET MVC?
## 13. name and explain different web application architectures. Good resource:
https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/commonweb-
application-architectures
