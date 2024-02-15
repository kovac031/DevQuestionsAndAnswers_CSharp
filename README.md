# Questions that every C# developer should answer:

## 1. What is an object in C#? What is a class?

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

## 2. List and describe four fundamental object oriented programming concepts.
"APIE" acronym.

This about the four main aspects that describe what OOP is about. It's about explaining what is going on with OOP in the code, that's all.

### Abstraction
In OOP, objects don't exist until they are instantiated (using "new"), but the "blueprint" for them is already made in the form of classes (e.g. model class). Those classes already have all the parameters and methods which will apply to the future object, and in this way those objects are "abstracted".

### Encapsulation
Hiding complexity. If you don't want for some methods to be accessed directly, you "encapsulate" them within a method that is meant to be accessed directly.

Instead having everything public...
```C#
public class Person
{
   public string Name = "";
   public string Age = "";

   public void RegisterPerson()
   {
      // code
   }
}
public bool Validate() [...]
public bool SomethingElse() [...]
```
... and calling methods individually ...
```C#
// in Program
Person person = new Person
person.Name = "Ivan";
person.Age = "31";

person.Validate();
person.SomethingElse();
person.RegisterPerson();
```
... we encapsulate methods, that we want to execute anyway, within the main method ...
```C#
public class Person
{
   public string Name = "";
   public string Age = "";

   public void RegisterPerson()
   {
      // code
      Validate();
      SomethingElse();
   }
}
private bool Validate() [...]
private bool SomethingElse() [...]
```
... and just call the main method. 
```C#
// in Program
Person person = new Person
person.Name = "Ivan";
person.Age = "31";

person.RegisterPerson();
```
There, complexity hidden.

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

## 3. What are constructors?
A special method with the purpose of initializing objects from different classes. Cannot have a return type, cannot be void.

Constructors have the same name as the class they belong to.

Used for e.g. injecting dependencies, singleton design pattern.

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

## 4. What is method overloading?
The ability to use the same method name for methods with different parameters.
```C#
[HttpGet]
public async Task<ActionResult> CreateBoardGameAsync()
{ ... }
[HttpPost]
public async Task<ActionResult> CreateBoardGameAsync(BoardGameView boardGameView, string sorting)
{ ... }
```

## 5. Describe method overriding. What is the purpose of the virtual statement?
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

## 6. List access modifiers in C#.
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


## 7. What is the difference between a Struct and a Class?
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

In a class, person1 and person2 reference the same object.

In a struct, person2 is a copy of person1, and we have two different objects.

## 8. Describe boxing and unboxing in the context of C#.

Boxing converts a non-object data type to an object, unboxing reverses it.

```C#
List<object> listOfObjects = new List<object>(); // object is a data type
int meaningOfLife = 42;

listOfObjects.Add(meaningOfLife); // Boxing

int retrievedValue = (int)listOfObjects[0]; // Unboxing
```

Not to be confused with mapping.

## 9. Which design principles are described by the acronym SOLID? List and describe each of them.
[video](https://www.youtube.com/watch?v=kF7rQmSRlq0)

### - Single Responsibility Principle
> A class should have only one reason to change, meaning it should have a single responsibility. In other words, a class should do one thing and do it well.

https://dotnetcoretutorials.com/solid-in-c-single-responsibility-principle/

### - Open/Closed Principle
> Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.

> E.g. change parameters in the actual helper method being called in the controller method, but don't introduce parameters in the controller method just so the method can work.

https://dotnetcoretutorials.com/solid-in-c-open-closed-principle/

### - Liskov Substitution Principle 
> Objects of a derived class should be able to replace objects of the base class without affecting the correctness of the program. In other words, if you have a class hierarchy, derived classes should be substitutable for their base classes without causing issues.

> When using interfaces, child classes can't break Liskov anyway, but when overriding it is possible, so it should not be done that way. 

https://dotnetcoretutorials.com/solid-in-c-liskov-principle/

### - Interface Segregation Principle
> Clients should not be forced to depend on interfaces they do not use. In essence, it encourages the creation of specific, smaller interfaces rather than large, monolithic ones.

> E.g. if I am doing CRUD and have StudentService class with CRUD methods which inherits from IService, and if I also have a ClassService which also inherits from IService, ClassService will be obliged to implement all the methods suited for StudentService simply because it inherits them from IService. Therefore, IService is bad and should be broken into separate, use-specific interfaces.

https://dotnetcoretutorials.com/solid-in-c-interface-segregation-principle/

### - Dependency Inversion Principle 
> High-level modules should not depend on low-level modules; both should depend on abstractions. Abstractions should not depend on details; details should depend on abstractions. This principle encourages the use of interfaces or abstract classes to define dependencies, promoting flexibility and decoupling.

> Dependeny injection is the implementation of dependency inversion principle.

https://dotnetcoretutorials.com/solid-in-c-dependency-inversion/

[from this video explanation](https://www.youtube.com/watch?v=8M7pLjacCPI)
```C#
public class DataAccessLayer // higher level class
{
   public void AddCustomer(string name)
   {
      FileLogger logger = new FileLogger(); // lower level class; the higher level class depends on this lower level class
      logger.Log("Customer added: " + name);
   }
}
```
```C#
public class DataAccessLayer // higher level class
{
   private ILogger logger;

   public DataAccessLayer(ILogger logger) // constructor
   {
      this.logger = logger
   }

   public void AddCustomer(string name)
   {
      // no more lower level class here; FileLogger is implemented elsewhere and inherits from ILogger, which we inject
      logger.Log("Customer added: " + name);
   }
}
```
The interface is that abstraction mentioned in the definition.


## 10. Explain DRY acronym in the context of object oriented programming.
Don't Repeat Yourself = DRY

> When the same code or logic is needed in multiple places, it should be abstracted into a single, reusable component, such as a function, method, or class.

## 11. Describe the difference between break and continue statement.

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

## 12. What are the different approaches of passing parameters to a method?
Pass value type by value or reference, pass reference type by value or reference.

> Pass by value means passing a copy of the variable to the method.

> Pass by reference means passing access to the variable to the method.

> A variable of a value type contains its data directly.

> A variable of a reference type contains a reference to its data.

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

## 13. Can multiple catch blocks be executed?
No, only the first in the sequence.

## 14. What are static properties and methods? 
Static properties/methods are properties/methods of a class, rather than properties/methods tied to objects created from that class.

> static method:
```C#
public class Calculator
{
	public static int Add(int x, int y) // static method
	{
		int result = x + y;
		return result;
	}
}
```
```C#
static void Main(string[] args)
{
	int number = Calculator.Add(5,10); // calling the static method without instantiating an object
	Console.WriteLine(number);
}
```
> static property:
```C#
static void Main(string[] args)
{
	Car car1 = new Car("Mustang");
	Car car2 = new Car("Corvette");
	Console.WriteLine(car1.numberOfCars); // prints 1 in first row
	Console.WriteLine(car2.numberOfCars); // prints 1 in second row
}
class Car
{
	public int numberOfCars; // NO STATIC property, two objects are instantiated above with "new"

	public Car(string model)
	{
		this.model = model;
		numberOfCars++;
	}
}
```
```C#
static void Main(string[] args)
{
	Car car1 = new Car("Mustang");
	Car car2 = new Car("Corvette");

	Console.WriteLine(Car.numberOfCars); // only prints 2	
}
class Car
{
	public static int numberOfCars; // STATIC property, belongs to the Car class and not the car1 or car2 objects

	public Car(string model)
	{
		this.model = model;
		numberOfCars++;
	}
}
```
Used when we won't be creating objects.

## 15. What is the purpose of using statement?
```C#
using System;
```
For including the System namespace, which has its own classes etc, so we don't have to specify the namespace each time:
```C#
System.Console.WriteLine(...);
```
This also let's me define an alias for the namespace, such as "using BLORB = System;"
Also, I can use static classes with using, such as "using static System.Math;" ... so then I don't have to write Math.Abs();, I just write Abs();

Or to define a scope, resource management/cleanup:
```C#
using (SqlConnection connection = new SqlConnection(connectionString))
{
}
```
The connection/stream is closed automatically and anything in the memory is disposed of by the garbage collector (it implements the IDisposable interface).
Alternatively there is connection.Dispose();

## 16. What is serialization?
Serialization converts complex objects, which may contain various data types and nested structures, into a stream of bytes or a textual format.
Used for e.g. passing objects across methods, storing session data.

```C#
HttpContext.Session.SetString("SelectedCharacters", JsonConvert.SerializeObject(_cardList)); // from my chinese characters app
```

## 17. What is an interface in C#?
An interface declares what its child class should have and should do.

https://www.youtube.com/watch?v=RuhGv81tpoU

## 18. What's the difference between an interface and abstract class?
A class can inherit from multiple interfaces, but only one base class. Abstract classes can serve as base class. Abstract classes cannot be instantiated. 

While interfaces serve as "contracts", abstract classes serve as a common space to share code implementation across child classes (it's DRY). 

Unlike with interfaces, child classes do not have to implement everything from the abstract class.

## 19. Can we specify the accessibility modifier for methods inside the interface?
Interface methods are always public. We do not set the access modifiers inside interfaces.

## 20. Describe the difference between value and reference types.
A variable of a reference type contains a reference to its data.
A variable of a value type contains its data directly.

See answer for #12.

## 21. What are sealed classes? ***
Classes which are finalized, will not be extended further or modified and cannot be inherited, i.e. serve as base class.

Has to do with inheritance. "Either design for inheritance, or else prohibit it"

## 22. What are partial classes?
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

## 24. What are generics in the context of C#?
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

## 25. What are delegates in C#? **
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
[better example from my repo](https://github.com/kovac031/FuncAndActionDelegatesIntro)

## 26. What are nullable types in C#?
Data type which can hold null value ... e.g. int?, bool?

## 27. What are indexers in C#?
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

## 28. What are C# attributes and how they can be used?
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

## 29. Explain extension methods.
Static methods within static classes where we implement functionality that want to ADD to an existing class or method, and it is done by simply calling this new method from within the method or class we want to extend functionality for.

> method has "this" keyword in its parameter

[video](https://www.youtube.com/watch?v=iI9sfsMIZE8)

## 30. What is reflection in C#? *
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

## 31. What is managed or unmanaged code in .NET? *
Managed code is executed by CLR (Common Language Runtime), while unmanaged code falls outside of that scope and is executed by the OS.

All code I wrote or encountered within the .NET environment was managed code, while potentially some added libraires may have contained unmanaged code.

## 32. What are namespaces?
Namespaces act as containers or folders for your code, prevent naming conflicts by providing a unique scope for each group of types and can be nested within other namespaces, creating a hierarchical structure. This allows for even finer-grained organization of your code.

```C#
using System; // using is a relevant keyword, System is the used namespace here
using Model;
using DAL;
...
```

## 33. Describe arrays in C#.
A data structure which allows for storing and manipulating a collection of indexed elements of the same data type.

## 34. What kinds of collections exist in C#? **
Arrays
> indexed, fixed size that needs to be declared in advance, no dynamic resizing
```C#
int[] integerArray = new int[] { 1, 2, 3, 4, 5 };
```
> int[], bool[], string[] etc

BitArray
> indexed and fixed size, for storing boolean values
```C#
BitArray bitArray = new BitArray(new bool[] { true, false, true, false });
```

Lists
> dynamic arrays, no fix size, add and remove elemenets as you want = dynamic resizing

ArrayList
> ...

LinkedList
> ...

Dictionaries
> used to associate keys with values, providing efficient lookup based on keys
```C#
Dictionary<string, int> ageDictionary = new Dictionary<string, int>(); // Creating a dictionary

ageDictionary["Alice"] = 25; // Adding key-value pairs
ageDictionary["Bob"] = 30;
ageDictionary["Charlie"] = 22;

int bobAge = ageDictionary["Bob"]; // Retrieving values // Returns 30 
bool hasAlice = ageDictionary.ContainsKey("Alice"); // Checking if a key exists // Returns true
```

Enum
> used to define a set of named integral constants, where each constant represents a unique symbolic name for a value
```C#
enum DaysOfWeek
{
    Monday = 1,
    Tuesday = 2,
    Wednesday = 3,
    // ... 
}
DaysOfWeek today = DaysOfWeek.Wednesday; // Using an enum value // today will be Wed
int numericValue = (int)today; // Enum to integer conversion // numericValue = 3
```

Queues
> FIFO, "enqueue" adds an element to the end of the queue, "dequeue" removes and returns the element from the front of the queue
```C#
Queue<string> taskQueue = new Queue<string>();
taskQueue.Enqueue("Task1");
taskQueue.Enqueue("Task2");
string nextTask = taskQueue.Dequeue(); // nextTask is Task1, taskQueue only has Task2 now
```

Stacks
> LIFO, "push" adds an element to the top of the stack, "pop" removes and returns the element from the top of the stack
```C#
Stack<string> callStack = new Stack<string>();
callStack.Push("Function1");
callStack.Push("Function2");
string currentFunction = callStack.Pop(); // // currentFunction: "Function2", callStack: ["Function1"]
```

HashSet
> for storing a collection of unique elements with no duplicates, add/remove/contains
```C#
HashSet<int> uniqueNumbers = new HashSet<int>();
uniqueNumbers.Add(5);
uniqueNumbers.Add(10);
bool containsTen = uniqueNumbers.Contains(10); // Returns true
```

Hashtable
> similar to dictionaires and enum
```C#
Hashtable employeeSalaries = new Hashtable();
employeeSalaries.Add("Alice", 50000);
employeeSalaries.Add("Bob", 60000);

if (employeeSalaries.ContainsKey("Alice"))
{
    int aliceSalary = (int)employeeSalaries["Alice"];
    Console.WriteLine($"Alice's salary is: {aliceSalary}");
}
```

SortedSet
> stores elements in sorted order, eliminating duplicates (ignores adding them)
```C#
SortedSet<int> sortedSet = new SortedSet<int>();
sortedSet.Add(3);
sortedSet.Add(1);
sortedSet.Add(2);

int min = sortedSet.Min; // Retrieves the minimum element (1)
int max = sortedSet.Max; // Retrieves the maximum element (3)
bool contains = sortedSet.Contains(3); // Returns true
```

Concurrent Collections
> for concurrent programming scenarios where multiple threads can access and modify the collection concurrently, part of System.Collections.Concurrent namespace, provide thread-safe operations without the need for explicit locking (e.g., lock keyword)

> ConcurrentQueue, ConcurrentStack, ConcurrentBag, ConcurrentDictionary, BlockingCollection
```C#
ConcurrentDictionary<string, int> concurrentDict = new ConcurrentDictionary<string, int>();
concurrentDict.TryAdd("Alice", 30);
int aliceAge = concurrentDict.GetOrAdd("Alice", 25); // Returns 30 (existing value)
```

Immutable Collections
>  state cannot be modified after creation, part of System.Collections.Immutable namespace, trying to change the collection creates new instances
```C#
static readonly ImmutableList<int> numberList = new List<int> { 1, 2, 3 }.ToImmutableList

static void Main(string[] args)
{
	numberList[0] = 5; // red underline, can't be changed

	numberList.Add(12);
	numberList.Remove(3); // ImmutableList allows for .Add and .Remove
	// to disallow, change ImmutableList<> type to IReadOnlyList<>
}
```

## 35. What is LINQ?
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

## 36. Describe the difference between IEnumerable and IQueryable.
The distinction is relevant in the context of making database calls.

IEnumerable executes immediately and stores a collection in memory, which is performance heavy, while IQueryable builds a query and does not execute until .ToList

```C#
IEnumerable<Customer> collection = db.GetCustomersAsEnumerable(); // gets all customers

var goodCustomers = collection.Where(c => c.Revenue > 2500); // narrows it to some amount of goodCustomers
```
```C#
IQueryable<Customer> query = db.GetCustomersAsQueryable(); // gets no customer

goodCustomers = query.Where(c => c.Revenue > 2500);) // again, no customers

var goodCustomersList = goodCustomers.ToList(); // gets some amount of customers based on criteria
```

Because IEnumerable uses deferred execution, it can cause issues in specific situations - such as when you need to iterate and filter through a collection. Unlike a list, it will not have the entire collection stored in memory, but rather it will have to fetch it anew for each operation on an item (note to self, make a project to showcase the difference).

## 37. Describe the concept of lazy loading. *
The approach in designing your code so that required data is loaded as necessary instead of in advance. E.g. not loading an entire list first and then narrowing it down.

There is also the Lazy<T> "wrapper" which makes it so that a method is not executed in normal order but only when the variable (of an instanced class) is first used elsewhere in code. (I guess something like async, did not encounter this yet).

## 38. Explain the concept of deferred execution.
Execution of code later, like with .ToList or .First with IQueryable (see #36)

## 39. What is the purpose of the C# yield statement?
Using yield return with loops we return results one by one on each iteration, as opposed to waiting for the entire sequence to complete first and then see results.

[video explaining yield](https://www.youtube.com/watch?v=uv74SZ5MX5Q)

## 40. Explain lambda expressions.
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

## 41. What is type inference?
The ability to not have to specifically declare each data type, it gets infered with var.

## 42. Explain Func and Action delegates. *
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
[better example from my repo](https://github.com/kovac031/FuncAndActionDelegatesIntro)

## 43. What is Entity Framework?
Entity Framework (EF) is an Object-Relational Mapping (ORM) framework developed by Microsoft.

Object-Relational Mapping (ORM) is a programming technique and framework that allows developers to work with databases using object-oriented concepts. It bridges the gap between the relational databases, which store data in tables, and object-oriented programming languages, which use objects to represent data and behavior.

## 44. What are Context and Entity classes in EF?
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

## 45. Explain asynchronous method calls with async/await pattern.
In asynchronous execution, every Task (keyword) creates a new thread that executes code in parallel. 

Synchronous execution runs line by line, method by method, it does not start executing new tasks before the previous one is complete.
> Three runners starting the race at or nearly the same time (async) VS one runner starting, finishing, then the second one running, and so on ...

The "await" keyword is a required part of the async syntax and cannot be ommited. It indicates end of thread utilization.
```C#
async Task<int> CalculateAsync()
{
    int result = await SomeAsyncOperation();
    return result;
}
```
If no await keyword inside the method, the method will run synchronously despite the async Task declaration, which may therefore be removed.

## 46. What is Dependency Injection. How and why is it used?
Passing the classes that your class depends on as interfaces via the constructor rather than having your class create those dependencies.

```C#
public class OrderService
{
    private readonly IDatabase _database;

    public OrderService(IDatabase database) // constructor
    {
        _database = database;
    }
    public void PlaceOrder(Order order)
    {
        _database.Save(order); // if no DI, we'd have to instantiate an object from a class, creating a dependency and breaking SOLID
    }
}
```
> If we need to change database related code, we'll do it in IDatabase and not in PlaceOrder in OrderService.

## 47. What types of Dependency Injection exist in C#? **
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

## 48. What DI scopes are available with the container you used? Explain the scenarios for usage of each of these scopes. *
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

## 49. What are software patterns?
Design patterns are typical solutions to commonly occurring problems in software design. They are like pre-made blueprints that you can customize to solve a recurring design problem in your code.

## 50. Explain several design patters (structural, creational, behavioral, ...). For example, singleton, factory, adapter, repository, etc. ***
> Recommended resource for more info: https://dotnettutorials.net/course/dot-net-design-patterns/

### Creational design patterns:
> deals with Object Creation and Initialization

### - Singleton  
Instead of creating new objects when we call a class (instantiate with "new"), we instead set it up so that the same object is used (we do ClassName.MethodName). [video](https://www.youtube.com/watch?v=r6Y0SmbufmU)

```C#
public class MyExample
{
    private static MyExample myExample;

    private MyExample() { } // private constructor

    public static MyExample MyInstanceMethod()
    {
        if (myExample == null)
        {
            myExample = new MyExample();
        }
        return myExample;
    }
...
```
If multiple threads, it may violate singleton because additional instances may be created on different threads, [solution](https://www.youtube.com/watch?v=QWrcOmLWi_Q&list=PL6n9fhu94yhUbctIoxoVTrklN3LMwTCmd&index=4).

### - Factory Method
A factory is an object which can create other objects. 

Factory method design pattern is about creating objects without exposing the creation logic to the client. Subclasses choose which class to instantiate.

> E.g. client is selecting some product for whatever reason ([factory is making boats or trucks](https://refactoring.guru/design-patterns/factory-method)) - instead of having all logic be inside a switch loop or similar and have it duplicated for trucks and boats, we place the logic in a different class, a single "factory", and simply call a method that accept the client's choice as parameter.

Very similar to my polymorphism example:

```C#
public abstract class Shape
{
    public abstract void Draw();
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
public class ShapeFactory // Factory class to create shapes
{
    public Shape CreateShape(string shapeType)
    {
        if (shapeType.Equals("Circle", StringComparison.OrdinalIgnoreCase))
        {
            return new Circle();
        }
        else if (shapeType.Equals("Square", StringComparison.OrdinalIgnoreCase))
        {
            return new Square();
        }
        else
        {
            throw new ArgumentException("Invalid shape type.");
        }
    }
}
class Program
{
    static void Main()
    {
        ShapeFactory factory = new ShapeFactory();

        Shape circle = factory.CreateShape("Circle");
        Shape square = factory.CreateShape("Square");

        circle.Draw(); // Outputs "Drawing a circle."
        square.Draw(); // Outputs "Drawing a square."
    }
}
```
The point of this pattern is that there is a base abstract class or interface (Shape) which has a method (Draw) that is intended to be common to all products (Circle and Square), but those products then have their own implementation of that common method. I use Factory when I want to reuse a method.

### - Abstract Factory
Unlike the Factory pattern, the Abstract Factory pattern does not have a base class and common method as the focus, that's not the point of it. This one is about creating "families of related objects". 

It defines a set of abstract factory methods (CreateCircle and CreateSquare) for creating different product families (PrettyShapes and UglyShapes). Concrete factories (PrettyShapeFactory and UglyShapeFactory) implement these methods to create their product families (PrettyShapes and UglyShapes).

```C#
public abstract class Circle // Abstract Product A
{
    public abstract void Draw();
}
public class PrettyCircle : Circle // Concrete Product A1
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a pretty circle.");
    }
}
public class UglyCircle : Circle // Concrete Product A2
{
    public override void Draw()
    {
        Console.WriteLine("Drawing an ugly circle.");
    }
}

public abstract class Square // Abstract Product B
{
    public abstract void Draw();
}
public class PrettySquare : Square // Concrete Product B1
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a pretty square.");
    }
}
public class UglySquare : Square // Concrete Product B2
{
    public override void Draw()
    {
        Console.WriteLine("Drawing an ugly square.");
    }
}

public abstract class ShapeFactory // Abstract Factory
{
    public abstract Circle CreateCircle();
    public abstract Square CreateSquare();
}
public class PrettyShapeFactory : ShapeFactory // Concrete Factory for PrettyShapes
{
    public override Circle CreateCircle()
    {
        return new PrettyCircle();
    }
    public override Square CreateSquare()
    {
        return new PrettySquare();
    }
}
public class UglyShapeFactory : ShapeFactory // Concrete Factory for UglyShapes
{
    public override Circle CreateCircle()
    {
        return new UglyCircle();
    }
    public override Square CreateSquare()
    {
        return new UglySquare();
    }
}

class Program
{
    static void Main()
    {
        // Create PrettyShapes - product family
        ShapeFactory prettyFactory = new PrettyShapeFactory();
        Circle prettyCircle = prettyFactory.CreateCircle();
        Square prettySquare = prettyFactory.CreateSquare();

        prettyCircle.Draw(); // Outputs "Drawing a pretty circle."
        prettySquare.Draw(); // Outputs "Drawing a pretty square."

        // Create UglyShapes - product family
        ShapeFactory uglyFactory = new UglyShapeFactory();
        Circle uglyCircle = uglyFactory.CreateCircle();
        Square uglySquare = uglyFactory.CreateSquare();

        uglyCircle.Draw(); // Outputs "Drawing an ugly circle."
        uglySquare.Draw(); // Outputs "Drawing an ugly square."
    }
}
```
E.g. for real world usage, Windows vs MacOS UI both need buttons and checkboxes and other objects but will need different implementation.

### - Builder
### - Prototype
### - Fluent Interface

...

### Structural Design Patterns:
> used to Manage the Structure of Classes and Interfaces and the Relationship Between the Classes and Interfaces

### - Adapter
We have two classes with different behaviors, each inherit from their respective interfaces. We make an "adapted" class in which we inject the incompatible class via dependency injection AND which inherits from an interface we want methods from. BUT, we then change the contents of those methods (to behave like in the original class) while keeping the names and parameters so that the interface doesn't complain. 

That way we masked (wrapped, adapted) our incompatible class/code to work with whatever needs the new setup and can't work with the original class, WITHOUT (!) changing or losing the original class, because some other code may still use it.

[E.g. here we want to use Adaptee, but for whatever reason we can't. So we mask it using a new Adapter class, to trick the code into accepting it.](https://refactoring.guru/design-patterns/adapter/csharp/example)

[video explnation and example](https://www.youtube.com/watch?v=BvV84tzWLm0)

### - Facade
We encapsulate (for lack of better terms) different classes in a new facade class, where we have those different classes injected with a constructor. This creates a dependency and allows for methods of the facade class to call the outside methods, like how calling a repository layer method from service layer would work ( _repository.MethodName(params) ) ...

[video explanation and example](https://www.youtube.com/watch?v=z46yENs1RYw)

### - Decorator
### - Composite
### - Proxy
### - Flyweight
### - Bridge

...

### Behavioral Design Patterns:
> deal with the Communication Between Classes and Objects

### - Chain of Responsibility
Scenario where when something needs to be executed and fails or otherwise doesn't pass a check, it passes the responsibility to some other class, and so on, down a chain until the end.

> Similar concept to IF -> ELSE IF loops and other condition based loop logic.

[video explanation and example](https://www.youtube.com/watch?v=d_z_l7cD4rg)

### - Command
### - Observer
### - Iterator
### - State
### - TemplateMethod
### - Visitor

### - Strategy
It's about polymorphism.

```C#
// set up interface and classes which inherit from this interface, as well as a context class
interface IStrategy
{
    void Execute();
}
class SomeClass : IStrategy
{
    public void Execute()
    {
        Console.WriteLine("SomeClass execution");
    }
}
class AnotherCLass : IStrategy
{
    public void Execute()
    {
        Console.WriteLine("AnotherClass execution");
    }
}
class Context // this class is important as its ExecuteStrategy() method will be called later
{
    private IStrategy _strategy;

    public Context(IStrategy strategy)
    {
        _strategy = strategy;
    }
    public void ExecuteStrategy()
    {
        _strategy.Execute();
    }
}
// Here is where we actually use the Strategy pattern
class Client
{
    static void Main()
    {
        IStrategy strategy1 = new SomeClass(); // instantiate the interface object as the child class object
        Context context1 = new Context(strategy1); // instantiate the Context object and pass the IStrategy object (here SomeClass) as parameter
        context1.ExecuteStrategy(); 	// in the Context class, enters the ExecuteStrategy, which realizes its parameter is SomeClass
					// goes into SomeClass and does its Execute()
					// does Console.WriteLine("SomeClass execution");
        IStrategy strategy2 = new AnotherClass();
        Context context2 = new Context(strategy2);
        context2.ExecuteStrategy(); 	// does Console.WriteLine("AnotherClass execution");
    }
}
```
[video explanation and example](https://www.youtube.com/watch?v=qX0iZs_3VyU)

### - Mediator
### - Memento
### - Interpreter

...

### Architectural Patterns:
> deal with the overall architecture and organization of an application

### - Repository

When we use a repository interface that implementes methods (e.g. CRUD), but have several different repository classes that inherit from that interface while also operating differently (one connecting to one database, another to some other), we'd be injecting the repository interface to the controller or service or whatever layer using dependency injection constructor. But this injection does not specify which repository class should be used, so we also have to specify this in the DI container/config file.

Like here, "use this class with this interface":
```C#
builder.RegisterType<BoardGameService>().As<IBoardGameService>();
```
```C#
builder.Services.AddScoped<IService, StudentService>();
```
[video](https://www.youtube.com/watch?v=qJmEI2LtXIY)

### - Clean
### - MVC


## 51. What is unit testing?
Testing individual units of code, such as methods/functions, to see if they produce the expected result in isolation.

In contrast, there is integration testing which tests how multiple components work together.

## 52. What is mocking? What is its purpose?
Mocking is a technique used in unit testing. It involves creating fake or "mock" objects to simulate the behavior of real components or dependencies. 

The purpose of mocking is to isolate the code being tested by replacing its dependencies with these mock objects. This helps in testing the code in isolation and ensures that the tests focus solely on the unit of code under examination. (E.g. when testing controller methods, we don't want to ACTUALLY call the methods in the service layer, so we mock that)

...

# Basic RDBMS and SQL questions:

## 1. What is the difference between a column and a field?
A column is a collection of cells alligned vertically in a table. 

A field is an element in which one piece of information is stored.

## 2. What is a primary key?
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
FK creates a relationship between tables by referencing a primary key in another table.

## 7. Can foreign key be nullable if primary key is not?
No.

## 8. What is a purpose of a JOIN keyword? What types of JOINs do you know?
The JOIN keyword in SQL is used to combine rows from two or more tables based on a related column between them. Its primary purpose is to retrieve data from multiple tables in a single query by matching records based on a specified condition.

The INNER JOIN keyword selects records that have matching values in both tables.
```SQL
SELECT ProductID, ProductName, CategoryName
FROM Products
INNER JOIN Categories ON Products.CategoryID = Categories.CategoryID;
```
<img src="https://github.com/kovac031/TermsAndDefinitionsAndExamples/blob/main/pics/img_inner_join.png">

The LEFT JOIN keyword returns all records from the left table (table1), and the matching records from the right table (table2). The result is 0 records from the right side, if there is no match.
```SQL
SELECT column_name(s)
FROM table1
LEFT JOIN table2
ON table1.column_name = table2.column_name;
```
<img src="https://github.com/kovac031/TermsAndDefinitionsAndExamples/blob/main/pics/img_left_join.png">

The RIGHT JOIN keyword returns all records from the right table (table2), and the matching records from the left table (table1). The result is 0 records from the left side, if there is no match.
```SQL
SELECT column_name(s)
FROM table1
RIGHT JOIN table2
ON table1.column_name = table2.column_name;
```
<img src="https://github.com/kovac031/TermsAndDefinitionsAndExamples/blob/main/pics/img_right_join.png">

The FULL OUTER JOIN keyword returns all records when there is a match in left (table1) or right (table2) table records.
```SQL
SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2
ON table1.column_name = table2.column_name
WHERE condition; 
```
<img src="https://github.com/kovac031/TermsAndDefinitionsAndExamples/blob/main/pics/img_full_outer_join.png">

## 9. What is a temp table / table variable and when would you use it?
[Temporary tables](https://www.c-sharpcorner.com/UploadFile/rohatash/temporary-tables-in-sql-server-2012/) are created for temporary uses (doing some calculations to produce data that we don't want to permanently store), is dropped after session is over.
```SQL
create table #table_name
(
column_name varchar(20),
column_no int
)
-- some stuff
DROP TABLE #table_name -- it's a temporary table, not a variable, it can be located as a file
Print 'Deleted table'
```

[Table variable](https://www.c-sharpcorner.com/UploadFile/75a48f/table-variable-in-sql-server/) is used similarly, except it is a variable stored in memory instead of the database, and does not exist outside of the query it is supposed to execute.
```SQL
Declare @TempTable TABLE(
id int,
Name varchar(20)
)
-- some stuff
Select * from @TempTable -- must be executed along with the part above or it does not see the variable
```

## 10. What is an index and what is its purpose?
Index is a data structure which references values found in table columns. They are just used to speed up searches/queries. [video](https://www.youtube.com/watch?v=fsG1XaZEa78)

```SQL
CREATE INDEX idx_lastname
ON Persons (LastName)
```
https://www.w3schools.com/sql/sql_ref_index.asp

## 11. What is a check constraint?
The CHECK constraint is used to limit the value range that can be placed in a column.

```SQL
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int CHECK (Age>=18)
); 
```
https://www.w3schools.com/sql/sql_check.asp

## 12. Explain the difference between a stored procedure and a function.
Both are about preparing queries for future use, and so that they can be reused.

A stored procedure can contain various queries and is versatile in how it can be used.
```SQL
CREATE PROCEDURE SelectAllCustomers
AS
SELECT * FROM Customers
GO;
```
```SQL
EXEC SelectAllCustomers;
```
https://www.w3schools.com/sql/sql_stored_procedures.asp

Functions are used to return a value, their purpose is just to do calculations.
```SQL
CREATE FUNCTION CalculateTotalPrice (@UnitPrice DECIMAL(10, 2), @Quantity INT)
RETURNS DECIMAL(10, 2)
AS
BEGIN
    DECLARE @TotalPrice DECIMAL(10, 2)
    SET @TotalPrice = @UnitPrice * @Quantity
    RETURN @TotalPrice
END
```
```SQL
SELECT ProductName, UnitPrice, Quantity, dbo.CalculateTotalPrice(UnitPrice, Quantity) AS TotalPrice
FROM Products;
```

## 13. What is a query plan?
A query plan, also known as an execution plan, is a detailed description of how a database management system (DBMS) intends to execute a specific SQL query.

Query plans are critical for optimizing the performance of database queries.

https://www.youtube.com/watch?v=VcA92fe1Erw

## 14. What is parametrized query?
Parameterized query, also known as a prepared statement, is a type of SQL query in which placeholders are used for input values, and those values are provided separately.
```SQL
SELECT * FROM Customers WHERE CustomerName = ? AND Country = ?
```
> CustomerName and Country will be assigned a value elsewhere in code

```SQL
SELECT * FROM Orders WHERE OrderDate >= @StartDate AND OrderDate <= @EndDate
...
DECLARE @StartDate DATE = '2023-01-01'
DECLARE @EndDate DATE = '2023-12-31'
```

## 15. What is sql transaction?
An SQL transaction is a logical unit of work that groups one or more SQL statements into a single, indivisible, and atomic operation. The primary purpose of transactions is to ensure the consistency, integrity, and reliability of a database when multiple operations need to be executed together as a single, coherent task.

```SQL
BEGIN TRANSACTION; -- Start a new transaction

-- Execute SQL statements within the transaction
UPDATE Account SET Balance = Balance - 100 WHERE AccountNumber = '12345';
INSERT INTO TransactionLog (AccountNumber, Amount) VALUES ('12345', -100);

-- Commit the transaction
COMMIT;

-- Or, if there's a problem, roll back the transaction
-- ROLLBACK;
```

...

# Basic Web development topics:

## 1. Explain the concept of MVC.
MVC stands for Model-View-Controller, a design pattern commonly used in web development.

Model:
> This is where your data and business logic reside. It represents your application's data and how it should be processed.

View: 
> This is the part that the user sees and interacts with. It's responsible for presenting data to the user in a user-friendly way.

Controller: 
> This acts as an intermediary between the Model and View. It receives user input, processes it, interacts with the Model to get the required data, and then updates the View to show the results to the user

## 2. Name several different types of results returned by action methods.
View, PartialView, Json, Redirect, RedirectToRoute, Content, File, Empty

```C#
public IActionResult Index()
{
    return View(); // Renders the "Index" view.
}
```
```C#
public IActionResult GetPartialView()
{
    return PartialView("_PartialView"); // Renders the "_PartialView" partial view.
}
```
```C#
public IActionResult GetJsonData()
{
    var data = new { Name = "John", Age = 30 };
    return Json(data);
}
```
```C#
public IActionResult RedirectExample()
{
    return Redirect("https://www.example.com");
}
```
```C#
public IActionResult RedirectToCustomRoute()
{
    return RedirectToRoute("CustomRouteName");
}
```
```C#
public IActionResult GetText()
{
    return Content("This is plain text content.", "text/plain");
}
```
```C#
public IActionResult DownloadFile()
{
    byte[] fileBytes = GetFileBytes(); // Get your file data here.
    string fileName = "example.pdf";
    return File(fileBytes, "application/pdf", fileName);
}
```
```C#
public IActionResult NoContent()
{
    return new EmptyResult(); // Represents no content returned.
}
```

## 3. What is the difference between ViewResult and ActionResult?
ViewResult is a specific type of ActionResult used when you want to return a view, while ActionResult is a more general type that can represent a variety of result types.

## 4. What are filters in MVC? Name some of them.
Filters are specific types of attributes that are used to perform pre-processing and post-processing logic around action methods.

Authorization filters like [Authorize], Action filters, Result filters and Exception filters like [HandleError].

https://www.tutorialsteacher.com/mvc/filters-in-asp.net-mvc

Custom filters can be made.

## 5. How routing works in MVC? What types of routing exist?
Routing is a pattern matching system. Routing maps an incoming request (from the browser) to particular resources (controller & action method).

Routing parses the request in route configuration. Then, it matches the request in the route table to ensure which controller and which action will be processed.

Conventional routing (in RouteConfig.cs)
```C#
routes.MapRoute(
    name: "Default",
    url: "{controller}/{action}/{id}",
    defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }
);
```
Attribute routing
```C#
[Route("products/show/{id}")]
public ActionResult Show(int id)
{
    // Action logic
}
```

## 6. What are the differences between TempData, ViewData and ViewBag?
They handle temporary data.

TempData for persisting data from action to action. ViewData from controller to view.

ViewBag is a collection of ViewData and uses dynamic keyword.

```C#
public class MyController : Controller
{
    public IActionResult Index()
    {
        ViewBag.MessageInViewBag = "Hello from ViewBag!";
        ViewData["MessageInViewData"] = "Hello from ViewData!";
        TempData["MessageInTempData"] = "Hello from TempData";
        return View();
    }
}
```
```HTML+Razor
<body>   
    <p>@ViewBag.MessageInViewBag</p>
    <p>@ViewData["MessageInViewData"]</p>
    <p>@TempData["MessageInTempData"]</p>
</body>
```

## 7. Explain the difference between View and Partial View.
A View represents a complete web page or a significant portion of it. It is typically used to render the main content of a page.

A Partial View is designed to represent a smaller, reusable component of a page. It is a way to break down complex pages into smaller, manageable sections.
 
## 8. What is ViewModel in MVC?
ViewModel is the model class containing all the data required by a specific View.

## 9. What are HTML helpers?
HTML Helpers are a set of methods in ASP.NET MVC that provide a way to generate HTML markup in views using C# or VB.NET code. They make it easier to create HTML elements and controls in your views.
```HTML+Razor
@Html.ActionLink("Click Me", "About", "Home") // uses html helper
<a href="@Url.Action("About", "Home")">Click Me</a> // no html helper
```

## 10. What are layouts and what is their purpose?
Layouts in ASP.NET MVC are a feature that allows you to define a common structure for your web application's pages. They serve as a template or a master page that provides the overall layout and structure that is consistent across multiple views or pages.
```HTML+Razor
<!DOCTYPE html>
<html>
<head>
    <title>@ViewBag.Title</title>
    <!-- Common CSS and JavaScript references go here -->
</head>
<body>
    <header>
        <!-- Header content goes here -->
    </header>
    <nav>
        <!-- Navigation menu goes here -->
    </nav>
    <div class="content">
        @RenderBody() <!-- Placeholder for view-specific content -->
    </div>
    <footer>
        <!-- Footer content goes here -->
    </footer>
</body>
</html>
```
```HTML+Razor
@{
    Layout = "_Layout"; // Specifies the layout to use for this view
}
<!-- Content specific to this view -->
<h1>Welcome to our website</h1>
<p>This is the home page content.</p>
```

## 11. What is Razor? How are code blocks defined in Razor?
Razor is a markup syntax used in ASP.NET MVC (and later in ASP.NET Core) to embed server-side code within HTML markup. It provides a clean and concise way to create dynamic web pages by mixing C# or VB.NET code with HTML.

To define a codeblock use @{ ... } syntax
```HTML+Razor
@{
    var message = "Hello, Razor!";
}
```

## 12. How is validation implemented in ASP.NET MVC?
ASP.NET MVC includes built-in attribute classes in the System.ComponentModel.DataAnnotations namespace. We used attributes to validate data.

```C#
public class Person
{
    [Required]
    [StringLength(50)]
    public string Name { get; set; }
} // e.g. sign up form will demand a Name input, which will be limited to 50 characters max
```

In addition we can use model validation checks with ModelState.IsValid
```C#
public ActionResult Create(Person person)
{
    if (ModelState.IsValid)
    {
        // Data is valid, proceed.
    }
    else
    {
        // something else
    }
}
```

## 13. Name and explain different web application architectures. Good resource:
https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/commonweb-application-architectures

### Monolithic
> All the components, such as the user interface, business logic, and data access, are tightly integrated into a single codebase (single layer, just one project/class library).

For example "All-in-one" applications

### N-tier / layered
> Divides the application into components, layers, where each layer has serves a unique purpose. Layers are built on top of one another.

E.g. user interface layer (MVC app controllers layer), business logic layer (service), database communication layer (repository, DAL) etc. ...

### Clean and Onion
> Layers the application into core and external components. Dependencies flow from the center outwards.

The domain is the innermost part, containing entities, then the appliation layer, containing all the business logic. The external layers, such as the UI layer (containing controllers) and the Data layer (communication with database) both have dependencies on the Application layer.

Difference between Clean and Onion is in the Application layer (Clean makes classes based around use case (e.g. Create/Edit/Get etc), Onion makes classes based around entities (Users, Reviews, BoardGames...).

### Microservices
> All functional parts or features of the application are separated into independent codebases and can be run independently

In terms of Netflix, playing a video could be handled as one microservice, the recommendations page as another, user management a third (each are their own project).

...

# Extras

## Getters and setters and auto-implemented properties

https://www.youtube.com/watch?v=8FmE_-QXg3Y

https://www.youtube.com/watch?v=Z70AwnYYJOc

## Aliases in C#

object:  System.Object

string:  System.String

bool:    System.Boolean

byte:    System.Byte

sbyte:   System.SByte

short:   System.Int16

ushort:  System.UInt16

int:     System.Int32

uint:    System.UInt32

long:    System.Int64

ulong:   System.UInt64

float:   System.Single

double:  System.Double

decimal: System.Decimal

char:    System.Char

> A method called ReadInt32 is unambiguous, whereas a method called ReadInt requires interpretation. The caller could be using a language that defines an int alias for Int16, for example. The .NET framework designers have followed this pattern, good examples being in the BitConverter, BinaryReader and Convert classes.

## List VS IEnumerable

IEnumerable can be more memory-efficient, as it processes elements one at a time without requiring the entire collection to be loaded into memory.

Lazy Evaluation, deferred execution and generates elements on-demand.

> for e.g. streaming a large set of data

List does Eager evaluation - loads the entire collection to memory, meaning it will have it at the ready when you need to do something with the collection. Faster.



