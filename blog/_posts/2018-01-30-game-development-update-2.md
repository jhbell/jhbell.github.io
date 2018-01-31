---
layout: post
title:  "Game Development Update 2"
date:   2018-01-23 13:17:49 -500
category: blog
---
<img src="{{ site.url }}/assets/jeff-web.jpg" 
     alt="Jeff Bell" 
     style="width: 200px; height: 200px; padding-bottom: 25px" />  
This week my team began work on the new game, The Monk and the Mountain
(working title).  This weekend was the Global Game Jam, so we worked together a
lot. Thanks to our efforts, we now have a working demo of the core mechanics of
our game which is very exciting.

# The Class

There was a lot of work to be done for the 3D Game Development Capstone this
past week. On Wednesday, all of the teams gave a pitch on their game concept
and idea. We felt pretty confident going into our pitch, but it ended up being
a little rocky.

On Friday we were required to demonstrate a paper prototype of our game. This
posed a fairly large initial challenge. With the core mechanic of our game
being telekinesis, we had to think of a way to simulate the fundamental part of
our game and prove that it is fun. We settled on a fun game involving
chopsticks and some 3D shapes.

Our paper prototype is played by attempting to stack the 3D shapes in a small
play area. This is to simulate the player's role in manipulating and sticking
together various objects in the game to achieve a goal. The goal in this case
is to build as much as you can in the play area without knocking anything out.
The catch is, you can only move the objects by using the chopsticks--one in
each hand.  Our prototype showed us that in addition to building and overcoming
a challenge, a lot of the fun of our game comes from the unpredictability of
the way the objects move. The fact that the pieces wobbled and were unstable
when you were moving and stacking made the game way more fun than if we had
really solid objects. This led us to keep some of the floppy physics we were
testing and makes our demo pretty fun to play with.

# The Game

We've solidified our game a little more this week. Gameplay involves picking up
and moving various objects around. The player can also enable a sort of
"sticky" power which allows the carried objects to stick to other objects it
touches. Lastly, the player can rotate objects it is carrying forwards,
backwards, left, and right (pitch and yaw). The player can even move carried
objects closer and farther away.

<img src="{{ site.url }}/assets/cube-rotation.png" 
     alt="Object Rotation" 
     style="width: 400px; height: 400px; margin: 0 auto; display: block;" />

We had to overcome an interesting challenge with the object rotation.  In
[Unity][unity], there are two main ways to rotate an object. The first is in
relation to the object's own axes. When we did rotation in this way, the user
would give input to make the object rotate down, but the object wouldn't
necessarily rotate in what you would expect "down" to be. This is because after
each rotation the object's axes changed in orientation. So you ended up getting
rotations that really made no sense whatsoever.

The second way to rotate an object in Unity is to rotate with relation to the
world's axes. This is simple enough to do in a script by adding `Space.world`
to the end of a `transform.Rotate()` call. This changes rotation to work
relative to the axes of the world. This rotation was much closer to what we
wanted when the user pressed the down button. The only issue, was that when the
player looked to the side, the "down" rotation would still be going the same
way. For example. If you looked left, pressing down would rotate the object
counter clockwise, because the axes of the world don't change when you look
around.

We had to figure out a way to rotate an object based on the angle of the first
person view. Nelson was able to help with the math and we were able to
implement this by taking the cross product of the `forward` orientation of the
player and the direction we want to rotate. In this case, `Vector3.down`.  Our
solution is essentially something like the following:

```csharp
if (<rotate-down>) {
    Vector3 forward = GameObject.Find("Head").transform.forward;
    Vector3 rotateDirection = Vector3.cross(Vector3.down, forward);
    object.transform.Rotate(direction * velocity, Space.World);
}
```

Now with this solution, whenever you press down the object rotates down towards
you. This works no matter what direction you are facing and it is absolutely
beautiful. I learned a lot about rotation working on this feature. Most
importantly that rotation occurs around the given axis, not in the direction of
the vector. For some reason my brain wanted rotation to happen from an edge or
point rather then the center of the object.

The work we did on solidifying the basic abilities in the game is going to be
extremely useful in future sprints. Now that we have a solid baseline of what
the character can do, the rest of the game will be putting the player into new
and interesting scenarios with these abilities. 

Next week I'll talk more about our demo from the Global Game Jam. I should have
much more to report once we show it to the rest of the class. Until then, happy
hacking!

[Unity]: http://www.unity.com
