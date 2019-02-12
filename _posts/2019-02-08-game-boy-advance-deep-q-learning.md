---
layout: post
title: "(IN PROGRESS) Deep Q-Learning with the Game Boy Advance"
description: "Implementing deep Q-learning on the Bizhawk emulator for use with any GBA game."
categories: ["Machine Learning", "AI", "Software", "Video Games"]
location: "Ruston, LA"
thumbnail: images/gba_dqn/thumbnail.png
---


This is a project that I am currently working on! The goal is to create a bot that learns to play arbitrary Game Boy Advance games using Deep Q-Learning. So far, I have implemented just about everything required, except I'm running into a bug in BizHawk, the emulator I'm using. I'm using a socket connecting a Lua script running natively in the emulator to the master Python script which is actually handling the logic. The problem is, currently the socket dies for [unknown reasons](https://github.com/TASVideos/BizHawk/issues/1174) after about an hour of training. Once I have time after finals this quarter, I intend to circumvent this bug by essentially rebooting the emulator and Lua script if the tcp connection times out on the Python end and just resuming training from there. 

After I get this working, I plan on doing a full write-up that you'll be able to find here.

You can check out the current state of this project [here.](https://github.com/ajbradberry96/GBA-DQN) 