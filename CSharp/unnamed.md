# Coding Convention and Style

## Auto Property
- Easy to use
- Provide OOP encapsulation without much hassle like in Java

### Example
  
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

## Magic Numbers are illegal

### Q: What are magic numbers?
### A: A number that appears from nowhere in the code
```
if(x <= 0.6f) {...}
```

### What's the problem? It looks pretty fine!
- Low readability, other people won't know what does the number mean
- May leads into inconsistency throughout the code as typos could happen
- It's possible that you have to edit numbers so many places
- Not just for numbers to be honest, but applies to any data type

### Instead, just do this really!

```
const float thresholdToDestroy = 0.6f;

...

if(x <= thresholdToDestroy){...}
```

## Null Coalescing Operators

Use Linq, but beware of GC, don’t use it in Update()
	
arr[arr.Count - 1];
arr.Last();

	Lambda expression (only use it in obvious functions)

public void ClearMiddleText() => middleScreenText.text = "";

Animator
	
StringToHash()

		To identify animator parameters, always use Animator.StringToHash() method and make it as a constant somewhere the reasons are that it’s more efficient and prevent errors from typo

private static readonly param1 = Animator.StringToHash(“param1”);
…
animator.SetTrigger(param1);


Abstraction
	
Try to use Interface if it can be adaptable. For example, use IEnumerable instead of List<T> so that we can use any class extended from more general type like Queue<T>

Another perk is that we can create mock of interface for testing

Time
	Separate the usage of time, unscaled time, and fixed time
time : for game logic
Unscaled time: for time that has to concern outside game logic as well
Fixed time: physics

Floating point comparing
	
	Even though the == comparison almost never really happens, we still should compare >= or <= in the code if it associates with the logic well, reason? To prevent confusing when working in team

public static bool ReadyToDisplayAds => durationSinceLastShowAds >= calculatedTimeToShowAds;

Technically operator > should suffice but it may confuse other programmers

# Optimization
## Condition evaluation
`if` always evaluate from left to right, so always try to put lighter weight condition on the left side first as there may be a chance of not having to check the condition on the rights at all



# Attribute
## UnityEngine Attributes
### Range
[Range(0.0f, 1.0f)]
private float volume = 1;

### SerializeField

Note: Use default value to silence CS0649 warning

## UnityEditor Attributes
// TODO

# Out parameters

## TryParse

## TryGetValue
if(soundCache.TryGetValue(name, out Sound sound))
{
sound.Source.Play();
}
else
{
Debug.LogAssertion("[ERROR] AudioManager.Play: soundCache missed");
}

## Discards (out _)

# Extension Methods
## KeyValuePairExtension
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

# String Interpolation

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

# Class vs Struct

Testing
	All test should be meaningful by itself and should not test more than one scenario or case, and it should not need to have the programmer to try to trace the error stack
	If the error occurred, the programmer should immediately know what kind of error has happened
	
