---
title: "js13k competition practice, part 3 of 12: item interactions, inventory and a win condition (day 30)"
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v=MtN08gejwFg'
youtube_embed: 'https://www.youtube.com/embed/MtN08gejwFg'
date: 2019-04-12 10:13:19
---

We're making solid progress so far and after today it really started to feel like "a game" in the sense that most of the component you would expect for a game are there in some minimal form.  This is also the furtherest I've ever really gotten into a game in which you interact with the environment; most games in my past have been psuedo-games where the computer part doesn't actually have knowledge of the rules of the game, it's more just a board in which you manipulate pieces (think like a chess game that doesn't know when you have won or lost).

It has become clear to me that the super-ad-hoc way this game has been developing has some serious flaws which we will address at some point before we get to the end.  Most notably when one thing needs to change, everything needs to change.  We have also hard baked in the idea of how rendering happens to most objects which is going to make it a huge task to switch should we decide to move to canvas rendering or something else other than manual DOM manipulation.

Picking up items was quite a bit of fun.  Moving them from their cell to our inventory was quite a challenge but seeing them disappear from the world was really cool.  Making the winning condition check for items in your inventory also really cool.  Time to start thinking more about how objects interact with other objects in the world.