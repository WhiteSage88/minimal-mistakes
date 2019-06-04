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
<h1>Abathur</h1>
<img src="/assets/images/aba.jpg" alt="Abathur">
Abathur's playstyle revolves around a passive gameplay. The character's body does not physically play a role in the game. 
Normally, a character would be in one of two/three lanes engaging the opposing team in fights. Abathur (Aba) does not do this. 
His physical body has low health and any physical engagement would get him killed. He relies on his main ability which is to place an extension of himself on a teammate to either deal damage or shield them. With this in mind, his main strength comes from being able to gain experience (an in game currency which makes the team stronger over time) with his frail body and this extension, effectivly being able to gain twice as much experience than his teammates. The downside is that he has to put himself in danger by being close to creep (small monsters that are automatically generated throughout the game and are sent down lanes) deaths to gain experience therefore exposing himself to the enemy team and possible death. He also releases a minion every 16 seconds which makes it possible for the enemy team to figure out his position.
<br>
This type of game play requires the player to look constantly to the bottom-right corner of the screen (the minimap) to always have knowledge of where his team is to aid them or if he is in possible danger of enemy players trying to find him. I hypothesized that about 50% of the time is spent looking at the minimap.
<br>
<h1>Brightwing</h1>
<img src="/assets/images/bw.jpg" alt="Brightwing">
Brightwing's playstyle is that she is an area of effect healer. Just being near a teammate, she is able to recuperate their health. Her skills require precision and prediction since in order for her to heal more she must cast a skill that has a travel time and must hit an enemy player in its center. Her unique ability is for her to fly to over to another player, where ever they are, and heal them for a percent of their health. This gives her a global presence since at any moement a 1v1 fight between players can turn to 2v1. Due to this, she must be aware of player positioning (bottom-right corner, minimap) and teammate's healths (top of screen, health bars). My hypothesis here is that there will be about a one-third split between looking at the minimap, middle of screen and the top of the screen throughout the game. 
<br>
<h1>Eye tracking</h1>
With my hypothesis in place, I went forward and used <b>OpenCV</b> to track eye positions during videos I recorded of myself playing either Abathur or Brightwing. This was done by using a publicly available Haas Cascade for facial and eye recognition. Then I performed a contour finding process again using <b>OpenCV</b> to find the pupil. There are several tutorials that present you with premade code but I had to change it to fit the dimensions of the video. The final threshold and contour finding image was this:
<br>
<img src="/assets/images/stacked.png" alt="Contour">
<br>A CSV file of X-position Eye, Y-position Eye, X-Position Pupil, Y-Position Pupil, Pupil Radius, and TimeStamp was created to store all the data created from the videos. 
<br>
<img src="/assets/images/face.jpeg" alt="Face Eye Tracked" width="400" height="400">
<br>
<h1>Machine Learning</h1>
As you can see from the image, a problem came up where my moustache was being recognized as an eye. Here I had to 1) Remove the possibility of false positives and 2) Classify positives into left and right eyes. Hard boundaries (eg. If the X-coordinate of the eye location was greater than 200, remove that data point) were an option but it did not account for possible head movements and therefore movements of the eye positions.
<br>
I then tried <b> K-Means clustering</b>, setting the algorithm to find only two clusters. 
<img src="/assets/images/incorr_kmeans.png" alt="Kmeans" width="400" height="400">
This was incorrect because now we have one eye being grouped together with the false positive. I could have changed the algorithm to three clusters but that agian caused problems where true positive clusters were being split. This is where I came upon <b>DBSCAN</b>. DBSCAN (Density-Based Spatial Clustering of Applications with Noise) is another method of catagorization of points based on density and proximity of other points. The parameters for this algorithm are minimum amount of points near it for it to count as a cluster and the maximum distance to other points. This algorithm does NOT require a predetermined or known amount of clusters in comparison to KMeans. 
<br> 
<img src="/assets/images/bw_kmeans.png" alt="DBSCAN" width="400" height="400">
<br>
As the image above shows, I was able to cluster the left eye, right eye, and false positives (mainly my lips). I removed the false positives and labeled the remaining points 0 or 1. 
<br>
<h1>Data Visualization</h1>
After categorizing the data between left and right eyes, I tried to make sense of the data by plotting it. These are the plots I came up with using data created by a Brightwing video. 
<img src="/assets/images/pupil_locations.png" alt="Both Pupils" width="400" height="400">
<img src="/assets/images/pupil_time.png" alt="Pupil Time" width="400" height="400">
<img src="/assets/images/x_movement.png" alt="X Movemnt" width="400" height="400">
<img src="/assets/images/y_movement.png" alt="Y Movement" width="400" height="400">
<br>
All this was interesting to see but it does not help me answer the question if I can predict what character I am playing based on my eye movements.
<br>
<h1>Neural Networks</h1>
This is where I turned to <b>Keras</b> to create a neural network that would help me predict the character I was playing. It would take the X and Y pupil locations and what character it was as input but also take in account it is a time series. This lead me to create a LSTM (Long-Short Term Memory) RNN (Recurring Neural Network). The neural network was compiled to return a prediction of whether the points were similar to Abathur data, Brightwing data or Valla data. I didn't touch upon Valla but its another character that I used as a third category for normal, center screen, gameplay.
<br> These were my results.
<img src="/assets/images/abathur.png" alt="Aba Predition" width="400" height="400">
<img src="/assets/images/brightwing.png" alt="Brightwing Prediction" width="400" height="400">
<img src="/assets/images/valla.png" alt="Valla Prediction" width="400" height="400">
<h1>NN Interpretation</h1>
