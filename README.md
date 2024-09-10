# Delegate Example: Improved Sorting Algorithm

This example demonstrates how to use delegates to create a more flexible and reusable sorting algorithm in C#. We'll start with a basic implementation and then refactor it to use delegates, allowing for more versatile sorting options.

## Initial Implementation

First, let's look at a basic implementation of bubble sort with separate methods for ascending and descending order:

```csharp
class SortingAlgorithms
{
    public static void BubbleSort(int[] array)
    {
        if (array is not null)
        {
            for (int i = 0; i < array.Length; i++)
            {
                for (int j = 0; j < array.Length - i - 1; j++)
                {
                    if (array[j] > array[j + 1])
                    {
                        SWAP(ref array[j], ref array[j + 1]);
                    }
                }
            }
        }
    }

    public static void BubbleSortDesc(int[] array)
    {
        if (array is not null)
        {
            for (int i = 0; i < array.Length; i++)
            {
                for (int j = 0; j < array.Length - i - 1; j++)
                {
                    if (array[j] < array[j + 1])
                    {
                        SWAP(ref array[j], ref array[j + 1]);
                    }
                }
            }
        }
    }

    private static void SWAP(ref int x, ref int y)
    {
        int temp = x;
        x = y;
        y = temp;
    }
}
```

Usage:
```csharp
int[] numbers = { 7, 2, 9, 6, 1, 4, 5, 3, 10, 8 };

SortingAlgorithms.BubbleSort(numbers);
foreach (int number in numbers)
    Console.WriteLine(number);

SortingAlgorithms.BubbleSortDesc(numbers);
foreach (int number in numbers)
    Console.WriteLine(number);
```

## Improved Implementation Using Delegates

Now, let's refactor this code to use delegates, making it more flexible and eliminating code repetition.

### Step 1: Define a Delegate

First, we define a delegate for the comparison function:

```csharp
public delegate bool CompareFuncDelegate(int x, int y);
```

### Step 2: Create Comparison Functions

Next, we create a class with static methods for different comparison operations:

```csharp
class CompareFunctions
{
    public static bool CompareGreaterThan(int x, int y)
    {
        return x > y;
    }  // sort Asc 

    public static bool CompareLessThan(int x, int y)
    {
        return x < y;
    } // sort Desc 
}
```

### Step 3: Refactor the Sorting Algorithm

Now, we can refactor our `BubbleSort` method to use the delegate:

```csharp
class SortingAlgorithms
{
    public static void BubbleSort(int[] array, CompareFuncDelegate compare)
    {
        if (array is not null && compare is not null)
        {
            for (int i = 0; i < array.Length; i++)
            {
                for (int j = 0; j < array.Length - i - 1; j++)
                {
                    if (compare(array[j], array[j + 1]))
                    {
                        SWAP(ref array[j], ref array[j + 1]);
                    }
                }
            }
        }
    }

    private static void SWAP(ref int x, ref int y)
    {
        int temp = x;
        x = y;
        y = temp;
    }
}
```

### Usage

Now we can use our improved sorting algorithm like this:

```csharp
int[] numbers = { 7, 2, 9, 6, 1, 4, 5, 3, 10, 8 };

// Sort in ascending order
SortingAlgorithms.BubbleSort(numbers, CompareFunctions.CompareGreaterThan);
Console.WriteLine("Sorted in ascending order:");
foreach (int number in numbers)
    Console.WriteLine(number);

// Sort in descending order
SortingAlgorithms.BubbleSort(numbers, CompareFunctions.CompareLessThan);
Console.WriteLine("Sorted in descending order:");
foreach (int number in numbers)
    Console.WriteLine(number);

// Alternative usage: store the delegate in a variable
CompareFuncDelegate compareFuncDelegate = CompareFunctions.CompareLessThan;
SortingAlgorithms.BubbleSort(numbers, compareFuncDelegate);
```

## Benefits of This Approach

1. **Flexibility**: The sorting method is now more flexible, allowing the user to define the sorting criteria.
2. **Reduced Code Duplication**: We no longer need separate methods for ascending and descending sorts.
3. **Extensibility**: New comparison functions can be easily added without modifying the sorting algorithm.
4. **Improved Readability**: The code is now more self-explanatory, with the comparison logic separated from the sorting logic.

## Conclusion

By using delegates, we've created a more versatile and maintainable sorting algorithm. This approach allows for easy customization of the sorting behavior without modifying the core sorting logic, demonstrating the power and flexibility of delegates in C#.


# Advanced Delegate Example: Generic Sorting Algorithm

This example demonstrates how to create a generic sorting algorithm using delegates in C#. We'll extend our previous bubble sort example to work with any type and introduce more flexible delegate definitions.

## Generic Sorting Algorithm

First, let's create a generic version of our sorting algorithm:

```csharp
class SortingAlgorithms<T>
{
    public static void BubbleSort(T[] array, CompareFuncDelegate<T, T, bool> reference)
    {
        if (array is not null && reference is not null)
        {
            for (int i = 0; i < array.Length; i++)
            {
                for (int j = 0; j < array.Length - i - 1; j++)
                {
                    if (reference(array[j], array[j + 1]))
                    {
                        SWAP(ref array[j], ref array[j + 1]);
                    }
                }
            }
        }
    }

    private static void SWAP(ref T x, ref T y)
    {
        T temp = x;
        x = y;
        y = temp;
    }
}
```

## Generic Delegate Definition

We'll define a more flexible generic delegate that can work with different input and output types:

```csharp
public delegate TResult CompareFuncDelegate<in T1, in T2, out TResult>(T1 a, T2 b);
```

This delegate can now accept two parameters of potentially different types and return a result of any type.

## Comparison Functions

Let's create comparison functions for both integers and strings:

```csharp
class CompareFunctions
{
    public static bool SortAscending(string x, string y) { return x?.Length > y?.Length; }
    public static bool SortDescending(string x, string y) { return x?.Length < y?.Length; }
    public static bool SortAscending(int x, int y) { return x > y; }
    public static bool SortDescending(int x, int y) { return x < y; }
}
```

## Usage Examples

Now let's see how we can use our generic sorting algorithm:

### Sorting Integers

```csharp
int[] numbers = { 7, 2, 9, 6, 1, 4, 5, 3, 10, 8 };

CompareFuncDelegate<int, int, bool> intCompareDelegate = CompareFunctions.SortAscending;
SortingAlgorithms<int>.BubbleSort(numbers, intCompareDelegate);

Console.WriteLine("Sorted integers (ascending):");
foreach (int number in numbers)
    Console.WriteLine(number);
```

### Sorting Strings by Length

```csharp
string[] names = { "Ahmed", "Omar", "Ali", "Mahmoud", "MA" };

CompareFuncDelegate<string, string, bool> stringCompareDelegate = CompareFunctions.SortDescending;
SortingAlgorithms<string>.BubbleSort(names, stringCompareDelegate);

Console.WriteLine("Sorted strings by length (descending):");
foreach (string name in names)
    Console.WriteLine(name);
```

## Benefits of This Approach

1. **Type Safety**: The generic approach ensures type safety at compile-time.
2. **Flexibility**: The sorting algorithm can work with any type that can be compared.
3. **Customizable Comparison**: Users can define their own comparison logic for complex types.
4. **Reusability**: The same sorting method can be used for different data types without code duplication.

## Advanced Usage: Custom Object Sorting

You can even use this approach to sort custom objects. Here's an example:

```csharp
class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}

class Program
{
    static void Main(string[] args)
    {
        Person[] people = new Person[]
        {
            new Person { Name = "Alice", Age = 30 },
            new Person { Name = "Bob", Age = 25 },
            new Person { Name = "Charlie", Age = 35 }
        };

        CompareFuncDelegate<Person, Person, bool> personCompareDelegate = (p1, p2) => p1.Age > p2.Age;
        SortingAlgorithms<Person>.BubbleSort(people, personCompareDelegate);

        Console.WriteLine("Sorted people by age (descending):");
        foreach (var person in people)
            Console.WriteLine($"{person.Name}: {person.Age}");
    }
}
```

## Conclusion

This advanced example demonstrates the power and flexibility of generic delegates in C#. By making our sorting algorithm and delegate definitions generic, we've created a highly reusable and type-safe solution that can work with any data type. This approach showcases how delegates can be used to create flexible, maintainable, and extensible code in C#.


# Delegate Example: Flexible Number Filtering

This example demonstrates how to use delegates to create a flexible number filtering function in C#. We'll start with specific functions for finding odd and even numbers, then refactor them into a single, more versatile function using delegates.

## Initial Implementation

First, let's look at the initial implementation with separate methods for finding odd and even numbers:

```csharp
public class NumberOperations
{
    public static List<int> FindOddNumbers(List<int> numbers)
    {
        List<int> result = new List<int>();
        if (numbers is not null)
        {
            for (int i = 0; i < numbers.Count; i++)
            {
                if (numbers[i] % 2 == 1)
                    result.Add(numbers[i]);
            }
        }
        return result;
    }

    public static List<int> FindEvenNumbers(List<int> numbers)
    {
        List<int> result = new List<int>();
        if (numbers is not null)
        {
            for (int i = 0; i < numbers.Count; i++)
            {
                if (numbers[i] % 2 == 0)
                    result.Add(numbers[i]);
            }
        }
        return result;
    }
}
```

Usage:
```csharp
List<int> numbers = Enumerable.Range(1, 100).ToList(); // Generate numbers from 1 to 100

List<int> oddNumbers = NumberOperations.FindOddNumbers(numbers);
Console.WriteLine("Odd numbers:");
foreach (int oddNumber in oddNumbers)
    Console.Write($"{oddNumber} ");

List<int> evenNumbers = NumberOperations.FindEvenNumbers(numbers);
Console.WriteLine("\nEven numbers:");
foreach (int evenNumber in evenNumbers)
    Console.Write($"{evenNumber} ");
```

## Improved Implementation Using Delegates

Now, let's refactor this code to use delegates, making it more flexible and eliminating code repetition.

### Step 1: Define a Delegate

First, we define a delegate for the condition function:

```csharp
public delegate bool ConditionFuncDelegate(int number);
```

### Step 2: Create Condition Functions

Next, we create a class with static methods for different conditions:

```csharp
class ConditionFunctions
{
    public static bool CheckOdd(int number) { return number % 2 == 1; }
    public static bool CheckEven(int number) { return number % 2 == 0; }
}
```

### Step 3: Refactor the Filtering Method

Now, we can refactor our `FindNumbers` method to use the delegate:

```csharp
public class NumberOperations
{
    public static List<int> FindNumbers(List<int> numbers, ConditionFuncDelegate condition)
    {
        List<int> result = new List<int>();
        if (numbers is not null && condition is not null)
        {
            for (int i = 0; i < numbers.Count; i++)
            {
                if (condition(numbers[i]))
                    result.Add(numbers[i]);
            }
        }
        return result;
    }
}
```

### Usage

Now we can use our improved filtering method like this:

```csharp
List<int> numbers = Enumerable.Range(1, 100).ToList(); // Generate numbers from 1 to 100

List<int> oddNumbers = NumberOperations.FindNumbers(numbers, ConditionFunctions.CheckOdd);
Console.WriteLine("Odd numbers:");
foreach (int oddNumber in oddNumbers)
    Console.Write($"{oddNumber} ");

List<int> evenNumbers = NumberOperations.FindNumbers(numbers, ConditionFunctions.CheckEven);
Console.WriteLine("\nEven numbers:");
foreach (int evenNumber in evenNumbers)
    Console.Write($"{evenNumber} ");

// We can also use lambda expressions for more specific conditions
List<int> multiplesOfThree = NumberOperations.FindNumbers(numbers, n => n % 3 == 0);
Console.WriteLine("\nMultiples of three:");
foreach (int number in multiplesOfThree)
    Console.Write($"{number} ");
```

## Benefits of This Approach

1. **Flexibility**: The filtering method is now more flexible, allowing the user to define any condition.
2. **Reduced Code Duplication**: We no longer need separate methods for different conditions.
3. **Extensibility**: New conditions can be easily added without modifying the filtering method.
4. **Improved Readability**: The code is now more self-explanatory, with the condition logic separated from the filtering logic.
5. **Inline Conditions**: We can use lambda expressions for one-off conditions without defining a new method.

## Conclusion

By using delegates, we've created a more versatile and maintainable number filtering method. This approach allows for easy customization of the filtering behavior without modifying the core logic, demonstrating the power and flexibility of delegates in C#. It also shows how we can simplify our codebase by replacing multiple specific methods with a single, more general method that can be customized at runtime.
