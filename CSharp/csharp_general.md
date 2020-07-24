- [1. C# Properties](#1-c-properties)
- [2. Lambda expression](#2-lambda-expression)
- [3. ?-Involved Operators](#3--involved-operators)
  - [3.1. Null Coalescing operator `??`](#31-null-coalescing-operator-)
  - [3.2. Null Coalescing Assignment Operator `??=`](#32-null-coalescing-assignment-operator-)
  - [3.3. Null Conditional Operators `?.` and `?[]`](#33-null-conditional-operators--and-)
- [4. LINQ](#4-linq)
  - [4.1. What is LINQ](#41-what-is-linq)
  - [4.2. Examples](#42-examples)
- [5. Constructor](#5-constructor)
  - [5.1. Chained Constructors](#51-chained-constructors)
  - [5.2. Avoid calling instance methods in constructors](#52-avoid-calling-instance-methods-in-constructors)
- [6. Interface vs Abstract Class](#6-interface-vs-abstract-class)
- [7. Floating point comparing](#7-floating-point-comparing)
- [8. Extension Methods](#8-extension-methods)
  - [8.1. KeyValuePairExtension](#81-keyvaluepairextension)
- [9. String Interpolation](#9-string-interpolation)
- [10. Class vs Struct](#10-class-vs-struct)
- [11. Array of Array vs Multidimensional Array](#11-array-of-array-vs-multidimensional-array)

# 1. C# Properties

Reference: https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/auto-implemented-properties

---

## What is it? <!-- omit in toc -->

To put it simple, it's just a (much much) shorter way to write getters setters, to illustrate, let me create a class with the requirement of:

Fields:

- Name (public get, private set)
- Age (public get, public set)

---

### 1.0.1. **If you're a Java person you would write this:**

```C#
public class Dragon
{
    private string name;
    private int age;

    public Dragon(string name, int age) {...}

    public string GetName()
    {
        return name;
    }

    public int GetAge()
    {
        return age;
    }

    public void SetAge(int age)
    {
        if(age < 0)
        {
            age = 0;
        }

        this.age = age;
    }
}
```

```C#
Dragon dragon = new Dragon("Deathbringer", 13);
string name = dragon.GetName();
int age = dragon.GetAge()
dragon.SetAge(14);
```

### 1.0.2. **With C# Properties we can save lines of code and use this instead**

```C#
public class Dragon
{
    public string Name { get; private set; }
    public int Age { get; set; }
}
```

```C#
Dragon dragon = new Dragon("Deathbringer", 13);
string name = dragon.Name;
int age = dragon.Age;
dragon.Age = 14; // Calls as if it's a public field
```

## Perks <!-- omit in toc -->

- Save lines of code and your time as you don't have to write getters setters
- And still provide OOP encapsulation!

---

# 2. Lambda expression

Only use it in obvious functions

```C#
public void ClearMiddleText() => middleScreenText.text = "";
```

# 3. ?-Involved Operators

## 3.1. Null Coalescing operator `??`

Reference: https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/null-coalescing-operator

---

### 3.1.1. **Usage**

```C#
someValue ?? backup
```

The `??` operator returns evaluates and return its left side `someValue` if the left side is not `null`. Otherwise it evaluates and returns the right side statement `backup` instead

---

### 3.1.2. **Example**

**Task** - We want to hatch `dragon.egg` and assign it to the variable `hatchling` but we're not sure if the `dragon` even has an egg yet. So we might have to let her lay an egg first.

```C#
Dragon dragon = new Dragon();

Egg dragonEgg;
if(dragon.egg != null)
{
    dragonEgg = dragon.egg;
}
else
{
    dragonEgg = dragon.LayEgg(); // Dragon.LayEgg() method returns Egg
}

this.hatchling = dragonEgg.hatch();
```

We could use ternary operator to reduce the length of this code:

```C#
Dragon dragon = new Dragon();
Egg dragonEgg;
dragonEgg = dragon.egg != null ? dragon.egg : dragon.LayEgg();
this.hatchling = dragonEgg.hatch();
```

But with Null-Coalesing operator we can even make this shorter!

```C#
Dragon dragon = new Dragon();
this.hatchling = (dragon.egg ?? dragon.LayEgg()).hatch();
```

---

## 3.2. Null Coalescing Assignment Operator `??=`

Reference: https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/null-coalescing-operator

---

### 3.2.1. **Usage**

```C#
someValue ??= backup
```

The `??=` operator **does not** do anything if its left side `someValue` is not `null` (already has some value). But if the left side is null, it will evaluate the right side `backup` and assign it to left side `someValue`.

So, could say the code snippet above is equivalent to

```C#
if(someValue == null)
{
    someValue = backup;
}
```

---

### 3.2.2. **Useful Scenario**

- When we have to reference to something that we could lost it reference, for example, if we call `Destroy(player)` and create a new one. And we need to reassign it.

---

### 3.2.3. **Example**

**Task** - Find a missing dragon

```C#
// TODO
```

## 3.3. Null Conditional Operators `?.` and `?[]`

Reference: https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-

---

### 3.3.1. **Usage**

```C#
someObject?.field;
```

- If `someObject` is **not** `null`, this will return the usual `someObject.field`
- Otherwise, if `someObject` is `null`, this will return `null` **Instead of raising NullReferenceException**.

```C#
int index = 3;
someArray?[index];
```

- If `someArray` is **not** `null`, this will return the usual `someArray[3]`
- Otherwise, if `someObject` is `null`, this will return `null` **Instead of raising NullReferenceException**.

### 3.3.2. **Why should we use it**

- **Really simple:** It gets rid of `NullReferenceException` (everyone knows how annoying can this exception be :c)

---

# 4. LINQ

Reference: https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/

Cheat Sheet: https://vslapp.files.wordpress.com/2011/11/linq-cheatsheet.pdf

---

## 4.1. What is LINQ

- Language-Integrated Query (LINQ) is the name for a set of technologies based on the integration of query capabilities directly into the C# language. [Reference](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/)
- To put it simple, it integrates a **query language** (like SQL) into our **C# language**.

---

### 4.1.1. **Tips:** You usually would want to use LINQ when you're gonna **foreach** with **if** in it, but beware of GC, **_don’t_** use it in Update()

---

## 4.2. Examples

Supposed we have a class `Dragon` for using with all LINQ examples here

```C#
class Dragon
{
    public string name;
    public int age;
}
```

---

### 4.2.1. **Filtering** - SQL `WHERE` statement

Supposed we have these dragons in the List `dragons` below

| name       | age |
| ---------- | :-: |
| Starflight |  7  |
| Kinkajou   |  3  |
| Glory      |  7  |
| Webs       | 23  |
| Scarlet    | 30  |

```C#
// We will choose dragons that either their name started with S, or their age is lower than 10
List<Dragon> dragons = { ... };
var foreachResults = new List<Dragon>();

// foreach approach
foreach(var dragon in dragons)
{
    if(dragon.name[0] == 'S' || dragon.age < 10)
    {
        foreachResults.append(dragon);
    }
}

// LINQ query approach
var queryResults =
    from dragon in dragons
    where dragon.name[0] == 'S' || dragon.age < 10
    select dragon;

// LINQ Lambda approach
var lambdaResults = dragons
    .Where(dragon => dragon.name[0] == 'S' || dragon.age < 10);

// *** Result : Starflight, Glory, Scarlet ***
```

### 4.2.2. **Ordering** - SQL `ORDERBY` statement

```C#
// We will order them by age, descending, then by their name, ascending
List<Dragon> dragons = { ... };
var foreachResults = new List<Dragon>();

// There is no foreach approach, as you should sort here with a custom comparer instead

// LINQ query approach
var queryResults =
    from dragon in dragons
    orderby dragon.age, dragon.name descending
    select dragon;

// LINQ Lambda approach
var lambdaResults = dragons
    .OrderBy(dragon => dragon.age)
    .ThenByDescending(dragon => dragon.name);

// *** Result : Kinkajou, Glory, Starflight, Webs, Scarlet ***
```

Notes: If we want to order by age descending, and name ascending, simply do this

```C#
// LINQ query approach
var queryResults =
    from dragon in dragons
    orderby dragon.age descending, dragon.name
    select dragon;

// LINQ Lambda approach
var lambdaResults = dragons
    .OrderByDescending(dragon => dragon.age)
    .ThenBy(dragon => dragon.name);

// *** Result : Kinkajou, Glory, Starflight, Webs, Scarlet ***
```

---

### 4.2.3. **Paging** - SQL `???` statement

// TODO

---

### 4.2.4. **First/Last/Single/Default/ElementAt** - SQL `???` statement

// TODO

---

# 5. Constructor

## 5.1. Chained Constructors

https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/using-constructors

## 5.2. Avoid calling instance methods in constructors

https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/constructor?redirectedfrom=MSDN

# 6. Interface vs Abstract Class

Try to use Interface if it can be adaptable. For example, use IEnumerable instead of List<T> so that we can use any class extended from more general type like Queue<T>

Another perk is that we can create mock of interface for testing

# 7. Floating point comparing

Even though the == comparison almost never really happens, we still should compare >= or <= in the code if it associates with the logic well, reason? To prevent confusing when working in team

```C#
public static bool ReadyToDisplayAds => durationSinceLastShowAds >= calculatedTimeToShowAds;
```

Technically operator > should suffice but it may confuse other programmers

# 8. Extension Methods

## 8.1. KeyValuePairExtension

```C#
public static class KeyValuePairExtension
{
    public static void Deconstruct<TKey, TValue>(
        this KeyValuePair<TKey,TValue> kvp,
        out TKey key,
        out TValue value)
    {
        key = kvp.Key;
        value = kvp.Value;
    }
}
```

# 9. String Interpolation

# 10. Class vs Struct

Struct Reference: https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/struct

# 11. Array of Array vs Multidimensional Array
