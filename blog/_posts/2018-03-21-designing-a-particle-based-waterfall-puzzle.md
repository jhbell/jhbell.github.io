---
layout: post
title:  "Designing a Particle-Based Waterfall Puzzle"
date:   2018-03-21 01:39:23 -500
category: blog
---
<img src="{{ site.url }}/assets/jeff-web.jpg" 
     alt="Jeff Bell" 
     style="width: 200px; padding-bottom: 25px" />  
One of the puzzles in our game revolves around a particle-based waterfall. In
this post, I will explain this puzzle, how it works and how it was implemented
in Unity. 

This post took a little longer than I would have liked to complete. Two weeks
ago, we had spring break and this past week was a lot of catch-up as well as a
few project deadlines, so I have been pressed for time. I should be back on
track for the rest of the semester now.

# The Puzzle

The waterfall puzzle in our game is fairly simple in design. There is a
waterfall that is blocked by some stones. In order to complete the puzzle
you have to move the stones out of the way allowing the waterfall to flow.

<img src="{{ site.url }}/assets/anautho-waterfall.gif"
     alt="The waterfall"
     style="width: 600px; margin: 0 auto; display: block;" />

There were two ways I thought to approach this puzzle when building it. The
first idea I had was to have some sort of particle effect that shows the
waterfall is blocked. Then, when the boulders are moved out of the way, an
event triggers that will change the particle effect to a complete waterfall.

The problem with this approach is that there is no sense of flow of the water.
While you are moving the stones, it would be really hard to tell that you are
actually accomplishing anything. Similarly, once the stones are moved out of
the way, an immediate change between the particle effect would be jarring
visually.

The other approach, which I have implemented in the game, would be to use the
particles themselves to collide with the blockage, and then find a way to
trigger an event when enough particles are flowing through the waterfall.
This requires playing around a lot more with the particles, but makes the
puzzle work more realistically.

# Playing With Particles

There are two main things we need to play with in order to make this second
idea work. The first thing we need to do is enable particle collision. This
is easy enough. Just enable particle collision and allow them to collide with
everything.

<img src="{{ site.url }}/assets/particle-collision.png"
     alt="Unity particle collision menu"
     style="width: 300px; margin: 0 auto; display: block;" />

The second thing we need to make this work is some sort of object to detect the
particles. As you would likely expect, we need to create a trigger. I made a
simple box collider and checked `isTrigger`. I moved it to the base of the
waterfall, where the "water" will flow out once the pathway is cleared.

Although our particles will collide, they will not currently cause a trigger
event with our new trigger. This is another simple menu item within the
particle system. We enable the Triggers menu and add our waterfall detection
trigger to the field.

<img src="{{ site.url }}/assets/particle-trigger.png"
     alt="Unity particle collision menu"
     style="width: 300px; margin: 0 auto; display: block;" />

Now we have a way of detecting particles, and to finish up we just need to
decide what to do with that information. We know we will have more particles
flowing through the waterfall when the blockage is cleared, so it would be nice
to be able to measure how many particles are flowing.

We have a trigger, and we have particles that will trigger an event when they
collide with the trigger. A solution would be to measure the number of
particles that are inside the trigger, and when that reaches some threshold
trigger the puzzle completion. An important thing to notice in the above photo
is that we are ignoring all collisions except for `Inside` which is set to
Callback our function.

The last thing for us to do is write the `OnParticleTrigger()` function and add
the script to our waterfall particle system. This function looks something like
the following:

```C#
// Count the number of particles in the trigger, and if greater than some threshold, complete the waterfall
void OnParticleTrigger () {
    if (!Events.WaterfallComplete) {
        Inside = new List<ParticleSystem.Particle>();
        int numInside = WaterfallParticles.GetTriggerParticles(ParticleSystemTriggerEventType.Inside, Inside);
        if (ShowDebug) {
            Debug.Log("Particles in trigger: " + numInside);
        }
        if (numInside >= TriggerThreshold) {
            Debug.Log("WaterfallTrigger.cs: Completing waterfall!");
            Events.CompleteWaterfall();
        }
        WaterfallParticles.SetTriggerParticles(ParticleSystemTriggerEventType.Inside, Inside);
    }
}
```

There you have it! Upon running the game, we can now move the boulders out
of the way, the particles will proceed down their path just like a waterfall,
and when enough particles fall inside the trigger, the puzzle can be completed.

This puzzle is still a work in progress, but this basic functionality will
definitely stay in the game. We are nearing the phase of the game where much
of our time goes into testing and tweaking what we have created. Everything
beyond what we have now is polish and new items to make the game more
feature-rich and immersive. I hope to make more technical posts about the game
in the future. Until then, happy hacking!
