---
title: Show the fly
sidebar:
  label: " - Show the Fly"
---

Now we have the spider moving it is going to need a target - the fly. We can get this working over a few steps, further breaking down the steps we have in our iteration plan. We can get something drawing quickly, then add the code to time its appearance.

## Drawing the fly

What will we need in the digital reality to help us create the fly?

- It will need a position: `flyX` and `flyY`
- It will need a constant size: lets stick with circles, so `FLY_RADIUS`

That should be enough to get something to appear. You should be able to plan this out yourself. Review the details on how we added the spider. This will be very similar. Pick a different color, so that you can tell the fly and spider apart. The one thing that will need some thought is how to position the fly.

We don't want the fly always being in the center of the screen. We need to randomise its location. To do this you can use the `Rnd` method from SplashKit. `Rnd(SCREEN_WIDTH)` will give you a random value from 0 to SCREEN_WIDTH - 1. You can use this to position the fly randomly.

## Timing Appearing

One of the game features was that the fly should not appear straight away, but appear after a short delay (lets say between 1 and 3 seconds). In order to hide the fly, we need to add an **if** statement around the drawing code so that we only draw the fly if it has appeared. We can track if the fly has appeared using a `boolean` variable. This can start false, and we can change it to true when sufficient time has passed.

:::tip[Boolean Variables]

Boolean variables are a great way to remember if something has or should happen. This feature can then exist as something within your digital reality.

In C# you use the `bool` type for these variables.

:::

To get this working we need to think a little about how we can work with time in these programs.

We cannot control time - computers are great, but we still don't have this capability. But we can observe the passing of time, much like other external events. The computer keeps track of the time, so we can use that to checking how long has elapsed.

SplashKit provides timer functionality that we can use to track time for now. In general, we can create one game timer, start this at the beginning of the game, and then use it to find out how many milliseconds have passed.

|**Method** | **Required Arguments** |**Description** |
|-----------|------------------------|----------------|
|`CreateTimer`| A name for the timer | Creates a timer. |
|`StartTimer`| The name of the timer to use | This records the current time, so that it can work out how much time has passed |
|`StopTimer`| The name of the timer to use | Resets the timer so that it is not recording time. |
|`TimerTicks`| The name of the timer to use | Returns you the time that has passed in milliseconds, since you called start on the timer |

We will need to code these steps somewhere within the event loop. They are not really about handing input, so we can create a new logical group for the steps related to **updating the game**. This can be the code for the fly appearing, escaping, and being eaten.

The general logic for this is shown below.

```
Steps:
  Open the window - use SCREEN_WIDTH and SCREEN_HEIGHT

  While not quit
    Handle Input
    Update the game
    Draw the game
    Process Events
```

### Putting this together

To make this work we will need:

- A game timer - we can create a constant to keep track of its name `GAME_TIMER`
- A variable to track how long needs to pass before we show the fly: `appearAtTime`
- A boolean to try if the fly has appeared: `flyAppeared`

These can be initialised as shown below. We can change the initialisation of flyX and flyY, as these values should now be set when the fly appears.

```
Constants:
  SCREEN_WIDTH = 800
  SCREEN_HEIGHT =  600
  
  SPIDER_RADIUS = 25
  SPIDER_SPEED = 3

  FLY_RADIUS = 15

  GAME_TIMER = "Game Timer"

Variables:
  spiderX (an int) = SCREEN_WIDTH / 2
  spiderY (an int) = SCREEN_HEIGHT / 2
  
  flyX (an integer) = 0
  flyY (an integer) = 0
  flyAppeared (a bool) = false
  appearAtTime (a long) = 1000 + Rnd(2000)
```

:::tip(The long type)

In this case you will need to use C#'s `long` data type for the time. This is an integer value with a larger range than `int`.

:::

Then we need to make use of these in our steps.

- We need to create and start the timer before the event loop. This way we are tracking time from the start of the game.
- In update game we will need to:
  - Check of the fly has not appeared, and the required time is elapsed. If it has, we then:
    - set `flyAppeared` to be true.
    - Assign `flyX` a random x value (`Rnd(SCREEN_WIDTH)`)
    - Assign `flyY` a random y value (`Rnd(SCREEN_HEIGHT)`)
- In draw game we can:
  - Only draw the fly if `flyAppeared` is true

```
Steps:
  Open the window - use SCREEN_WIDTH and SCREEN_HEIGHT

  Create the GAME_TIMER
  Start the GAME_TIMER
  
  While not quit
    Handle Input

    Update the game
      if not fly appeared, and Timer Ticks of the GAME_TIMER > appearAtTime
        Make the fly appear - set flyAppeared to true
        Give it a new position - set flyX/Y to a new random x/y value

    Draw the game
      Clear the screen white
      Fill a black circle using spiderX, spiderY, and SPIDER_RADIUS

      if fly appeared
        Fill a dark green circle using flyX, flyY, and FLY_RADIUS

      Refresh the screen to show it to the user

    Process Events
```

Code this up and test the game a few times. The fly should appear after a short period of time. The amount of time will differ between executions, based on the random value.

:::tip[Coding tips]

- In C#, not is the `!` character. So "not flyAppeared" is `!flyAppeared`.
- To create the game timer you can use `CreateTimer(GAME_TIMER)`
- Similarly, you can start the timer and get its ticks using `StartTimer(GAME_TIMER)` and `TimerTicks(GAME_TIMER)`
- If you have a boolean variable, you can use it directly in the if statement. For example, the following code will draw the fly when the value in the `flyAppared` code is true.

```csharp
if (flyAppeared)
{
  // Draw the fly
  // ...
}
```

You do **not** need to code it as `flyAppeared == true`. This would work, but is unnecessary.

:::