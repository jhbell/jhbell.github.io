---
layout: post
title:  "Game Development Update 4"
date:   2018-02-15 01:36:12 -500
category: blog
---
<img src="{{ site.url }}/assets/jeff-web.jpg" 
     alt="Jeff Bell" 
     style="width: 200px; padding-bottom: 25px" />  
This week development took a very different direction. Now that the core
mechanics of the game have been developed, we shifted away from doing a lot
of programming. My major focus this week was (attempting) to learn how to use
Wwise.

# None the Wwiser

Quick background on our group: we have 4 programmers and 2 artists. For most
of the groups in this class there are also group members specializing in audio
and design. Our group has a lot of interdisciplinary members to cover those two
fields.

Long story short, I'm filling the role of "the audio guy" for our group in
addition to doing programming. I'm learning that only a very small portion of
this will involve me creating the sounds. The majority will involve me actually
putting the sounds in the game and creating interesting ways to play them. The
supposed industry standard for sound design in video games is a tool called
[Wwise][wwise]. From the Audiokinect website:

  > Wwise is the most advanced, feature-rich interactive audio solution for
  > games. Whether you're an indie or a multi-million dollar production, Wwise
  > will work for you. The Wwise audio solution has made its mark in the gaming
  > industry and is now facilitating the advancement of interactive audio
  > across multiple sectors.

I agree that the features that Wwise has are very impressive. There are some
really awesome features that allow you to randomize sounds, create emitters,
and have interactive music for your game. The capabilities are supposed to be
far greater than what you can do with the built-in audio system that Unity has.
I've learned, however, that the learning curve is very steep, and the Unity
integration is not the friendliest.

Before I go on about this, I want to say that there are a couple of limitations
which made this more frustrating the past week. The first is that [Unity
Collaborate][collab] is still a really new tool. The second is that we are
frequently working remotely, and across multiple platforms.

# Looping Some Music

So my first objective was fairly simple: play a music loop I had previously
made on repeat as long as the game is running. Sounds simple enough, right?
I was quickly confronted by a very complicated process to perform this task.
Let's start by going through the average Wwise workflow:

Wwise uses different *containers* which contain your audio files. Then, for
your container you add an *event*. This event triggers something for your
audio. These events can be to play, stop, or seek to a part of an audio file.
Then, you generate a *soundbank*. The soundbank holds all of your files, 
containers, and events and produces a nice package to be used in the game.

Initially, I didn't really know what a container was. I also didn't see the
need for it. I mean, all you had to do was loop an audio file so I shouldn't
need to put it in anything. Boy, was I wrong. My first attempt involved adding
a new music track. I quickly found that these didn't have the ability to loop,
but after some googling I found out that if I make the track a sound effect I
should be able to loop the sound effect. So I try it. I select Loop, change
the radio button to "Infinite", and make an event to "Play" the track. Seems
good to me so I export my soundbanks, add the soundbank to the
GameObject in Unity and then click play. The sound plays...

Then it stops after one loop.

Okay... well, that's weird. I definitely checked the Loop box, should keep
playing, but it doesn't.

So I keep trying various things, sometimes getting errors, other times the
track just plays once still.

After a lot of effort searching and trial and error, I finally figure something
out. I made a "Music Playlist Container", added the song to the playlist,
turned on continuous, infinite, looping, created an event to play the playlist,
added the event and the playlist to my soundbank, exported the soundbanks,
reloaded in unity, added the event to a game object, added a listener to
my player, and then...

*I get an error.*

More searching.

Aha! Turns out all I needed to do this time was add the soundbank to the object
that I want to play the playlist. (For this I was just using an empty
GameObject.) So drag the soundbank onto the GameObject in Unity and then...

*Finally* the music plays, and, after holding my breath, the track loops. 

# Collaborating with Wwise

I understand you may just be thinking that I really didn't know how this software
works and that it's not fair for me to criticize it so heavily when *I* am the
one who didn't understand how to use it. To be honest, you're probably right.
I will say, however, that there were a few things that were confusing about my
experience.

1. Seeing an error felt like the greatest thing ever. When the track would play
just once, there was no indication of why and I had no idea why it wouldn't
loop. I still don't know why that original loop setting on the sound effect
didn't work. Maybe I have to have a container? Still not sure.

2. The Wwise editor is pretty clunky. The program runs like a windows program
in an emulator (I have a mac). I didn't have any issues with functionality, but
there were some really strange things with the way the views work in Wwise. For
example, to generate the soundbanks, you have to have a specific panel open
that doesn't open when you click or even double click on the soundbank in the
project hierarchy. Instead, you have to press `F7` to change views and finally
see the `Generate` button.

Overall, when it works, it does work. It looks like once I learn how to use the
software better, we can do some really cool things with it. As I mentioned
earlier, however, the learning curve is very steep and I'm going to have to
invest quite a bit of time into learning how to take full advantage of Wwise.

There are still some major issues that come with collaborating with Wwise. As
far as I can tell, the Unity integration for Wwise is not bad, it just has
a long way to go when it comes to Unity Collaborate. (Unity Collaborate has
a long way to go as well, but that's for a different time.) We are still
working on the best solution, but the problem is that Collaborate does not
Synchronize the Wwise project itself. This should be okay. The soundbanks get
synchronized via the Assets folder, and everyone using Collaborate has the
Wwise integration. According to everything I've read online so far, as long as
everyone has they soundbanks, they should be able to play the game with sound.

This sadly isn't the case. After some testing today, it looks like everyone
needs the entire Wwise project in addition to the soundbanks. Our plan is to
try to add the Wwise project to the Assets folder so that Collaborate will
synchronize it. The only issue with this is that having the entire project
there bloats the build of our game. We could keep it in Collaborate and remove
when building, or just pass zips around whenever the audio is updated, but 
those are both very gacky solutions. Since Collaborate is still fairly
new, I can understand why there isn't much documentation around doing these
things. It would sure be helpful if there was more out there, though.

I'm hoping that we can figure out a system to manage the Wwise project files
(and Wwise itself) soon. All the trouble I had the past week has burned me out
on wanting to touch any audio so I'll probably take a few days away from it.
Thankfully, this next sprint focuses much more on gameplay than aesthetics, so
I get to go back to programming for a while. For sprint 3 we will be making a
working "first playable" level of the game. We have some basics for the level
from this sprint, but now we get to refine it into the actual beginning of our
game. Any great successes or failures I'll be sure to talk about next week but
until then, happy hacking!

[wwise]:  https://www.audiokinetic.com/products/wwise/
[collab]: https://unity3d.com/unity/features/collaborate
