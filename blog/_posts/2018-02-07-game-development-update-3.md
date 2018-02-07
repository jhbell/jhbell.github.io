---
layout: post
title:  "Game Development Update 3"
date:   2018-02-07 16:00:12 -500
category: blog
---
<img src="{{ site.url }}/assets/jeff-web.jpg" 
     alt="Jeff Bell" 
     style="width: 200px; padding-bottom: 25px" />  
We recently presented our work from the Global Game Jam. We put in a lot of
hours on the game for our first game review and it was great to see how
everything came together in our presentation to the class. Overall, it went
really well.  One of our mechanics involved using a rock and a catapult to
launch yourself up to the top of a building. We had never really done it
successfully, but it magically worked in our demo in front of the class.

# On Your Mark...

With the conclusion of the Global Game Jam, we are now moving into the next
phase of our game development: sprints! This week marks our first official
sprint in our product development cycle. A sprint is a development iteration
which picks a set of specific 'user stories' to complete. For example, a couple
of the stories we had in this sprint were:

> As a player, I want to have VR controls that work just as well as the desktop
> controls.

> As a player, I would like the head rotation of the player to be limited up
> and down so I can't flip upside down.

> As a player, I want a cohesive game map.

Notice that each of these user stories highlights a functionality or feature of
the game from the perspective of the user. These stories make up a small part
of the Agile Development method called Scrum. By focusing our efforts on
specific tasks each week, we will slowly add more and more features to the
game, testing along the way. Furthermore, we will end up with a working product
every week.

We are tracking our weekly Scrum efforts on [Trello][trello] and so far it has
been working out great. To learn more about Scrum and how it works,
[click here][scrum].

# The Game

Above, you saw some of the things on our agenda for the week. This week we
tried to tackle the major technical challenge of getting our game working with
Virtual Reality. More specifically, we had to get controls working with the
Oculus. Our other main goal was to create a final map design for the game that
we can use as we move forward. Here's what we came up with for the map:

<img src="{{ site.url }}/assets/game-map.jpg" 
     alt="The game map" 
     style="width: 800; margin: 0 auto; display: block;" />  

Our design attempts to take the linear progression of the game, getting 3
abilities one after the other, and lays it out over an area that can also be
used for sandbox play. The "mountain entrance" will be the entry point to
the final level of the game. We have decided that the sandbox/tutorial area
will be great as an MVP, so we will design the final challenges that will go
into the mountain later. For this week, we are still using a demo map that we
created during the game jam. Now that we have a design for the actual game's
map, we will likely get started building it in our next sprint.

# What's Next

Each week brings a new challenge and a new goal. We still have some fine tuning
on controls, but for the most part the functionality is there. The player can
pick up objects, rotate them, stick them together, and unstick them. As far as
mechanics go, there is mostly cleanup and adding some more VR support. There is
still a long way to go before we finish our game, but next week we should be
starting on a real level which will really start to show progress. Until then,
happy hacking!

[trello]: https://trello.com
[scrum]:  https://www.scrum.org/resources/what-is-scrum
