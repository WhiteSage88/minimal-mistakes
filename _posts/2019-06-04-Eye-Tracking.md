---
layout: posts
title: "Eye Tracking and Classification"
tags: OpenCV Python Keras Matplotlib
---

# Classification of Eye Movement Patterns in Heroes of the Storm

I play video games. A lot of different kinds. RPGs, shooters, ARPGs and MOBAs. The acronyms don't really matter but just that
their play styles are all different. One game in particular caught my attention, Heroes of the Storm (HotS) which is a MOBA 
(Multiplayer Online Battle Arena) where two teams of five take control of a particular character and battle against each other 
on differing maps. One unique thing that sets HotS apart from other MOBAs is the gameplay style certain charcters have. Three 
in particular come to mind as far as where I am looking on the screen. Abathur and Brightwing. I was noticing 
as I was playing that I would look at certain locations of the screen at different frequencies depending on which character I was playing. 
<br>
<img src="/assets/images/locations.png" alt="Locations">
The top circle points out the health bars of each team mate. This is more important when it comes to Brightwing since they are 
a healing type character with global presence. 
<br>
The middle circle is where the character is normally posiitoned. Its where I hypothesized that was where I maintained my view since that is where most of the action occured.
<br>
The bottom right is the minimap. It shows all teammate positions, all <b>visible</b> enemy team member positions, map objectives and warning pings from other players. I hypothesized that I would look at this location a lot during the course of the game.
<br>
I then asked myself "How often do I look at these locations and can I predict based off of eye movements which character I am 
playing?". 
<br>
<br>
<h1>Abathur</h1>
<img src="/assets/images/aba.jpg" alt="Abathur">
<br>
Abathur's playstyle revolves around a passive gameplay. The character's body does not physically play a role in the game. 
Normally, a character would be in one of two/three lanes engaging the opposing team in fights. Abathur (Aba) does not do this. 
His physical body has low health and any physical engagement would get him killed. He relies on his main ability which is to place an extension of himself on a teammate to either deal damage or shield them. With this in mind, his main strength comes from being able to gain experience (an in game currency which makes the team stronger over time) with his frail body and this extension, effectivly being able to gain twice as much experience than his teammates. The downside is that he has to put himself in danger by being close to creep (small monsters that are automatically generated throughout the game and are sent down lanes) deaths to gain experience therefore exposing himself to the enemy team and possible death. He also releases a minion every 16 seconds which makes it possible for the enemy team to figure out his position.
<br>
This type of game play requires the player to look constantly to the bottom-right corner of the screen (the minimap) to always have knowledge of where his team is to aid them or if he is in possible danger of enemy players trying to find him. I hypothesized that about 50% of the time is spent looking at the minimap.
<br>
<br>
<h1>Brightwing</h1>
<img src="/assets/images/bw.jpg" alt="Brightwing">
<br>
Brightwing's playstyle is that she is an area of effect healer. Just being near a teammate, she is able to recuperate their health. Her skills require precision and prediction since in order for her to heal more she must cast a skill that has a travel time and must hit an enemy player in its center. Her unique ability is for her to fly to over to another player, where ever they are, and heal them for a percent of their health. This gives her a global presence since at any moement a 1v1 fight between players can turn to 2v1. Due to this, she must be aware of player positioning (bottom-right corner, minimap) and teammate's healths (top of screen, health bars). My hypothesis here is that there will be about a one-third split between looking at the minimap, middle of screen and the top of the screen throughout the game. 
<br>
<br>
<h1>Eye tracking</h1>
<br>
With my hypothesis in place, I went forward and used <b>OpenCV</b> to track eye positions during videos I recorded of myself playing either Abathur or Brightwing. This was done by using a publicly available Haas Cascade for facial and eye recognition. Then I performed a contour finding process again using <b>OpenCV</b> to find the pupil and created a csv file of X-position Eye, Y-position Eye, X-Position Pupil, Y-Position Pupil, Pupil Radius, and TimeStamp.  

