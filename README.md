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
