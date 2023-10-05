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
## 13. can multiple catch blocks be executed?
## 14. what are static properties and methods?
## 15. what is the purpose of using statement?
## 16. what is serialization?
## 17. what is an interface in C#?
## 18. What's the difference between an interface and abstract class?
## 19. can we specify the accessibility modifier for methods inside the interface?
## 20. describe the difference between value and reference types.
## 21. what are sealed classes?
## 22. what are partial classes?
## 23. What are the differences between System.String and System.Text.StringBuilder classes?
## 24. what are generics in the context of C#?
## 25. what are delegates in C#?
## 26. what are nullable types in C#?
## 27. what are indexers in C#?
## 28. what are C# attributes and how they can be used?
## 29. explain extension methods.
## 30. what is reflection in C#?
## 31. What is managed or unmanaged code in .NET?
## 32. what are namespaces?
## 33. describe arrays in C#.
## 34. what kinds of collections exist in C#?
## 35. what is LINQ?
## 36. describe the difference between IEnumerable and IQueryable.
## 37. describe the concept of lazy loading.
## 38. explain the concept of deferred execution.
## 39. what is the purpose of the C# yield statement?
## 40. explain lambda expressions.
## 41. what is type inference?
## 42. explain Func and Action delegates.
## 43. what is Entity Framework?
## 44. what are Context and Entity classes in EF?
## 45. explain asynchronous method calls with async/await pattern.
## 46. what is Dependency Injection. How and why is it used?
## 47. what types of Dependency Injection exist in C#?
## 48. what DI scopes are available with the container you used? Explain the scenarios for usage of
 each of these scopes.
## 49. what are software patterns?
## 50. explain several design patters (structural, creational, behavioral, ...). For example, singleton,
factory, adapter, repository, etc. Recommended resource for more info:
https://dotnettutorials.net/course/dot-net-design-patterns/
## 51. what is unit testing?
## 52. what is mocking? What is its purpose?

# Basic RDBMS and SQL questions:

## 1. What is a difference between a column and a field?
## 2. What is primary key?
## 3. What is natural vs surrogate primary key?
## 4. Can primary key be defined via multiple columns?
## 5. Describe the concept of database relation.
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
