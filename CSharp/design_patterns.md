# Observer Pattern (Events)

Personally, I find that a class that manages all the events is really useful to control events that needs to be executed in multiple classes. For example, when a player jump, the movement script `PlayerMovement` should handle the position and speed in y-axis of the player, while the animation script `PlayerAnimation` should play the jumping animation.

## Without Observer Pattern:

```C#
public class PlayerAnimation : MonoBehaviour
{
    private Animator animator;

    ...

    private void PlayJumpAnimation()
    {
        animator.SetTrigger("Jump");
    }
}
```

If we want to play the animation, we might have to get the `PlayerAnimation` Component, or put the `Animator` Component inside `PlayerMovement` (which reduce the cohesion of the class). So if we get the `PlayerAnimation` and call the `PlayJumpAnimation` inside `PlayerMovement`, the code would look like this:

```C#
public class PlayerMovement : MonoBehaviour
{
    private Rigidbody2D rigidbody2D;

    ...

    private void Update()
    {
        if(Input.KeyDown(KeyCode.Space))
        {
            // Some code to make the player jump
            rigidbody2D.velocity = new Vector2D(rigidbody2D.velocity.x, 7.0f);

            // Get PlayerAnimation Component, 
            // Assuming both of the scripts are on the same gameObject
            PlayerAnimation playerAnimation = GetComponent<PlayerAnimation>();
            playerAnimation.PlayJumpAnimation();
        }
    }
}
```

Not only the code is longer, but it also decreases the cohesion of the code. Additionally, if we failed to `GetComponent<PlayerAnimation>()` this might also result in the confusing `NullReferenceException`. But with the Observer design pattern, this are all solved very easily, and the code is much cleaner too!

We just need to have one more class `EventPublisher` (I name it myself) to handle this stuff. And of course, if we need other scripts to handle something when the player jumps, this method is very highly scalable as well!

```C#
public class EventPublisher
{
    // Just function's signature
    // This should be the same type of the function you want to subscribe
    public delegate void OnPlayerJump();

    // And we have the real event here
    public static event OnPlayerJump PlayerJump;

    // Then we handle the trigger by using event?.Invoke();
    public static void TriggerPlayerJump()
    {
        PlayerJump?.Invoke();
    }
}
```
```C#
public class PlayerMovement : MonoBehaviour
{
    private Rigidbody2D rigidbody2D;

    ...

    // Extract Jump code into another method, for readability
    // This way we can subscribe with Jump() function as well
    private void Jump()
    {
        // Just like the old code
        rigidbody2D.velocity = new Vector2D(rigidbody2D.velocity.x, 7.0f);
    }

    private void Start()
    {
        // Then we need Jump() to subscribe to the event `PlayerJump`
        EventPublisher.PlayerJump += Jump;
    }

    private void OnDestroy()
    {
        // To prevent null referencing when the gameObject is somehow destroyed
        // Always unsubscribing in OnDestroy() method
        EventPublisher.PlayerJump -= Jump;
    }

    private void Update()
    {
        if(Input.KeyDown(KeyCode.Space))
        {
            // Here we trigger the event instead
            // This will call all the functions that subscribed to the `PlayerJump` event
            EventPublisher.TriggerPlayerJump();
        }
    }
}
```
```C#
public class PlayerAnimation : MonoBehaviour
{
    private Animator animator;

    ...

    // Subscribing and unsubscribing just like in PlayerMovement script
    private void Start()
    {
        EventPublisher.PlayerJump += PlayJumpAnimation;
    }

    private void OnDestroy()
    {
        EventPublisher.PlayerJump -= PlayJumpAnimation;
    }

    private void PlayJumpAnimation()
    {
        animator.SetTrigger("Jump");
    }
}
```

Now, every time `EventPublisher.TriggerPlayerJump` is called, all the functions that has subscribed to `EventPublisher.PlayerJump` will be executed. In this case, both `Jump` and `PlayJumpAnimation` subscribed, so both of them will be executed!



## event.Invoke()
## Unsubscribing

# Object Pooling

# Singleton

# Dependency Injection
## Zenject