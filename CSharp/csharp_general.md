- [1. Properties](#1-properties)
  - [What is it?](#what-is-it)
  - [Perks](#perks)
  - [Example](#example)
- [2. Magic Numbers are illegal](#2-magic-numbers-are-illegal)
  - [2.1. Q: What are magic numbers?](#21-q-what-are-magic-numbers)
  - [2.2. A: A number that appears from nowhere in the code](#22-a-a-number-that-appears-from-nowhere-in-the-code)
  - [2.3. What's the problem? It looks pretty fine!](#23-whats-the-problem-it-looks-pretty-fine)
  - [2.4. Instead, just do this really!](#24-instead-just-do-this-really)
- [3. ?? and ?. operator](#3--and--operator)
  - [3.1 ?? (Null Coalescing operator)](#31--null-coalescing-operator)
- [4. Linq](#4-linq)
- [5. Lambda expression](#5-lambda-expression)
- [6. Constructor](#6-constructor)
  - [6.1. Chained Constructors](#61-chained-constructors)
  - [6.2. Avoid calling instance methods in constructors](#62-avoid-calling-instance-methods-in-constructors)
- [7. Abstraction](#7-abstraction)
- [8. Floating point comparing](#8-floating-point-comparing)
- [9. Extension Methods](#9-extension-methods)
  - [9.1. KeyValuePairExtension](#91-keyvaluepairextension)
- [10. String Interpolation](#10-string-interpolation)
- [11. Class vs Struct](#11-class-vs-struct)
- [12. Array of Array vs Multidimensional Array](#12-array-of-array-vs-multidimensional-array)

# 1. Properties

Reference: https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/auto-implemented-properties

## What is it?

To put it simple, it's just a (much much) shorter way to write getters setters, to illustrate, let me create a class with the requirement of:

Fields:

- Name (public get, private set)
- Age (public get, public set)

If you're a Java person you would write this:

```
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

And we call getters setters by doing

```
Dragon dragon = new Dragon("Deathbringer", 13);
string name = dragon.GetName();
int age = dragon.GetAge()
dragon.SetAge(14);
```

With C# Properties we can save lines of code and use this instead

```
public class Dragon
{
    public string name
}
```

## Perks

- Save lines of code and your time as you don't have to write getters setters
- And still provide OOP encapsulation

## Example

```
public class Player
{
    public int Health { get; private set; }

    // Changing Health is only allowed inside Player due to private set
    public void TakeDamage(int damage)
    {
        this.Health -= damage;
        if(this.Health < 0)
        {
            this.Health = 0;
        }
    }
}
```

```
public class SomeOtherClass
{
    [SerializeField]
    private Player player;

    private Text healthText

    ...

    private void UpdateHealthText()
    {
        // Simply call Health as if it's a getter method
        this.healthText.text = player.Health.ToString();
    }

    private void SetHealthToZero()
    {
        // This will result in compile error as Health property is private set
        player.Health = 0;
    }
}
```

---

# 2. Magic Numbers are illegal

## 2.1. Q: What are magic numbers?

## 2.2. A: A number that appears from nowhere in the code

```
if(x <= 0.6f) {...}
//      ^ like this 0.6f here
```

## 2.3. What's the problem? It looks pretty fine!

- Low readability, other people won't know what does the number mean
- May leads into inconsistency throughout the code as typos could happen
- It's possible that you have to edit numbers so many places
- Not just for numbers to be honest, but applies to any data type

## 2.4. Instead, just do this really!

```
const float thresholdToDestroy = 0.6f;

...

if(x <= thresholdToDestroy){...}
```

---

# 3. ?? and ?. operator

## 3.1 ?? (Null Coalescing operator)

https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/null-coalescing-operator

Supposed we have

```
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

```
Dragon dragon = new Dragon();
Egg dragonEgg;
dragonEgg = dragon.egg != null ? dragon.egg : dragon.LayEgg();
this.hatchling = dragonEgg.hatch();
```

But with Null-Coalesing operator we can even make this shorter!

```
Dragon dragon = new Dragon();
this.hatchling = (dragon.egg ?? dragon.LayEgg()).hatch();
```

---

# 4. Linq

Use Linq, but beware of GC, don’t use it in Update()

```
arr[arr.Count - 1];
arr.Last();
```

---

# 5. Lambda expression

only use it in obvious functions

```
public void ClearMiddleText() => middleScreenText.text = "";
```

# 6. Constructor

## 6.1. Chained Constructors

https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/using-constructors

## 6.2. Avoid calling instance methods in constructors

https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/constructor?redirectedfrom=MSDN

# 7. Abstraction

Try to use Interface if it can be adaptable. For example, use IEnumerable instead of List<T> so that we can use any class extended from more general type like Queue<T>

Another perk is that we can create mock of interface for testing

# 8. Floating point comparing

    Even though the == comparison almost never really happens, we still should compare >= or <= in the code if it associates with the logic well, reason? To prevent confusing when working in team

public static bool ReadyToDisplayAds => durationSinceLastShowAds >= calculatedTimeToShowAds;

Technically operator > should suffice but it may confuse other programmers

# 9. Extension Methods

## 9.1. KeyValuePairExtension

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

# 10. String Interpolation

foreach (var (stateName, soundName) in stateToSound)
{
if (stateInfo.shortNameHash == stateName)
{
AudioManager.Instance.Play(soundName);
}
}

private Dictionary<int, Sound.SoundName> stateToSound = new Dictionary<int, Sound.SoundName>()
{
{ AnimatorStateName.HitState, Sound.SoundName.Hit },
{ AnimatorStateName.BurnedState, Sound.SoundName.Fire },
{ AnimatorStateName.FlattenState, Sound.SoundName.Hit }
};

# 11. Class vs Struct

Struct Reference: https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/struct

# 12. Array of Array vs Multidimensional Array
