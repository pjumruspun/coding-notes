# Unity Basics

## Entity Component vs OOP

## MonoBehaviours

# Optimization

## Condition evaluation

`if` always evaluate from left to right, so always try to put lighter weight condition on the left side first as there may be a chance of not having to check the condition on the rights at all

# Animator

## StringToHash()

To identify animator parameters, always use Animator.StringToHash() method and make it as a constant somewhere the reasons are that it’s more efficient and prevent errors from typo

```
private static readonly param1 = Animator.StringToHash(“param1”);
...
animator.SetTrigger(param1);
```

```
foreach (var (stateName, soundName) in stateToSound)
{
	if (stateInfo.shortNameHash == stateName)
	{
		AudioManager.Instance.Play(soundName);
	}
}
```

```
private Dictionary<int, Sound.SoundName> stateToSound = new Dictionary<int, Sound.SoundName>()
{
	[AnimatorStateName.HitState] = Sound.SoundName.Hit,
	[AnimatorStateName.BurnedState] = Sound.SoundName.Fire,
	[AnimatorStateName.FlattenState] = Sound.SoundName.Hit,
};
```

# Time

Separate the usage of time, unscaled time, and fixed time

time : for game logic
Unscaled time: for time that has to concern outside game logic as well
Fixed time: physics

# Attribute

## UnityEngine Attributes

### Range

[Range(0.0f, 1.0f)]
private float volume = 1;

### SerializeField

Note: Use default value to silence CS0649 warning
Tips: Use with lambda expression property

## UnityEditor Attributes

// TODO

# Out parameters

## TryParse

## TryGetValue

```
if(soundCache.TryGetValue(name, out Sound sound))
{
	sound.Source.Play();
}
else
{
	Debug.LogAssertion("[ERROR] AudioManager.Play: soundCache missed");
}
```

## Discards (out \_)
