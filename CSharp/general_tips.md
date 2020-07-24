# 2. Magic Numbers are illegal

## Q: What are magic numbers?

## A: Numbers that appears from nowhere in the code

```C#
...

if(x <= 0.6f) {...}
//      ^ like this 0.6f here

...
```

## What's the problem? It looks pretty fine!

- **Low readability**, other people won't know what does the number mean
- May leads into **inconsistency** throughout the code as **typos** could happen
- It's possible that you have to edit numbers so many places
- Not just for numbers to be honest, but applies to any data type (like string)

## Instead, just do this really!

```C#
const float thresholdToDestroy = 0.6f;

...

if(x <= thresholdToDestroy){...}
```

---
