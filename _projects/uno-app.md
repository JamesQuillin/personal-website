---
layout: default
title: Online Uno Card Game
description: Built with Socket.IO, this app is a virtual Uno deck that's easy to use on any device. I built it to play with my family while on Zoom.
tags: HTML CSS JavsScript Socket.IO NodeJS Express DigitalOcean Linux VPS
---

# Online Uno Card Game

![text](/assets/images/projects/uno-app/uno-app-screenshot.png)

This application that lets you play Uno with anyone else on any internet-connected device just by going to the website. It is built with NodeJS and Socket.io and deployed on a Linux VPS. You can try it out live at its website [here](http://143.198.106.140).

To play, all you need to do is decide on a group name and then each player can choose whatever username they want from there and that's it.

The reason I wanted to make this in the first place was because I have always had fun at family gatherings playing card games and board games like Uno, but with the pandemic going on we haven't seen eachother nearly as much, and so making this would allow any of us to play with anyone else, especially over something like Zoom or even a phone call.

## Design

The key design feature of the application is actually a little unique. The game is not designed as a game, but instead is modeled after a real-world deck of Uno cards. There are three main actions: click/tap the deck to draw a card, click/tap the pile to take the top card back, and click/tap a card in your hand to play it.

Because of this three things happen:

- I can implement the game in a much cleaner and functional fashion with fewer edge cases by not checking or enforcing any game logic

- the user interface can be kept extremely simple, needing only to support those actions I do implement

- extending the application to other games is also much simpler

For the first point, take the idea of drawing a card. I don't need to define whether or not a player can have a certain number of cards in their hand. I don't need to know how many of what types of cards are allowed in a deck. And the only edge case I need to keep in mind is that the deck can't be empty. 

The whole process becomes: user draws a card -> server receives the event -> game removes a card from the deck and puts the card in player's hand -> game sends updates all players local gamestates with the new central game state.

And each step in that is very isolated, testable, and only handles one small part of the overall functionality of the application.

In addition to the technical benefits of not having to implement or enforce any game logic or rules, there is also a fun factor to it. Just like the games you play at the kitchen table growing up, your little cousin can try to draw all the cards out of turn, until his device is taken away, and players can also try to cheat or sneak cards if they really want to. Then you get the same arguments of "put that back" or "I saw you" or "you can't do that, let's get out the rule book!" or even just going for house rules. Designing the application this way allows for a similar experience to a real board game or card game, and that can be very fun.

On the second point, keeping the user interface simple is also an advantage. When I consider the target audience for the application (my family) I really really need to make sure that it is as simple as possible, when there are many people in my family who are less technically inclined. By doing this, the only thing I have to explain is that you need to type the same group name as everyone else, and you only have to click on the cards for the game to work.

Finally, by modeling it as a real-world deck of cards, extending to a different game is as simple as creating different assets and modifying a couple of simple event dispatchers for the new assets. It certainly woulnd't be too hard to find an SVG set of regular playing cards, or to draw your own board for a board game with some basic tools, and modeling the state of most card and board games can be done with only a handful of properties. So that's another big advantage for modeling the game this way.

## Extending the Application

If anyone wanted to clone and modify this to play a different game, the key thing to do would be to update the assets. 

In this game, the cards are actually short strings of SVG stored in a single object the first time the game loads. Then, anytime a card needs to be drawn, the local game asks for the corresponding SVG string from the object and can insert that into a template to be rendered. 

This drastically reduces the amount of data to send back and forth, making it easier on the server and on slow connections, and it makes scaling the cards work well on all sizes of devices. So instead of sending an image for each new card back, it can send a new game state object with simple enums like: 

```js
groupName: "super fun group",
  players: [
    {
      id: 2jk23fkjskdjf23kj23fkjsdkf,
      username: "alice",
      hand: ["r3", "b4", "g8"]
    },
    ...
  ],
  deck: ["w3", "v4", "b1", ...],
  pile: ["y3", "y4", "g4", ...]
```

If you wanted to change it to a deck of normal cards, then, you would want to update the enums and the corresponding SVG strings as a starting point.

Beyond that you would want to modify any Socket.io code as well. For this game there are simply five events the server listens for: `join group`, `draw card`, `undo card`, `play card`, and `disconnect`. 

`join group` is fired when a user clicks "join game" on the home page after filling out their simple form for a username and group name for that session, and `disconnect` fires when the tab is closed.

The other three are fired when the three elements on the screen are clicked or tapped (the hand, the deck, and the pile), so modifying these are as simple as updating the UI to have an interaction that fires an event and making sure the server is listening for that event and updates the game state appropriately.

One other thing that I might add in the future would be additional logic for sessions. Currently if the tab/window is closed then the player is removed from the game and their cards shuffled into the deck automatically. This can be a problem on some mobile devices where swapping out of the browser (for instance to check Facebook) can cause the disconnect to fire, but overall it hasn't been enough of a problem for me and my family when playing. If it happens we just draw a new hand and keep going.

It could be implemented really simply by saving their username and group name locally with something like `LocalStorage` and handling the disconnect logic for their hand differently. It could also be implemented more robustly with some of the Socket.io session methods, but for the purposes of this application, I'm mostly going for a "stateless" sort of "join and play" experience that is wiped clean when everyone leaves, just the same as when a real game is tidied up and put back into the box for later.

## Downloading the Code

If you would like to explore this more, then check out the code on [GitHub](https://github.com). The README has instructions for installing and running the app locally. 

If you want to try deploying it then you can do the same thing I did by following [this post](/articles/simple-website-deployment) I wrote, describing how to deploy an app to a Linux VPS.