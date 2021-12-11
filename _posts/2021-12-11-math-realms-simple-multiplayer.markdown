---
layout: post
title:  "A simple multiplayer system with firebase"
date:   2021-12-11 00:00:00 -0400
categories: mathrealms
---


### Preface
*TL;DR I began using Firebase for a variety of reasons involving life events and private server instability.*

Like many other developers in the world, I have discovered the utility and benefits of cloud-based "serverless" development. For a while, I shied away from these services, fearing that using them would make me less of a developer.

For me, if something is going to happen, a need must exist. I've always been fortunate enough to own a personal server that I use for most of my projects. I've never *needed* any kind of serverless platform because all I'd have to do is use the existing resources already available to me. Ultimately, using this personal server caused many of my projects to fail, but that is an article for another day.

The point is, as of late, my private server has been acting up a bit. As of writing this, I'm still at a loss as to what the problem is. For a while, many of the web services that I host would just randomly stop working with no apparent cause. I'd check logs and run tests but nothing I did could point me towards any resolution to the problem. Every few days I'd get calls and texts informing me that something I was serving was unreachable. Then, after fiddling around with the server a bit to try and solve the problem, everything would just start working again, no thanks to my efforts of course. Frustrated, I began to just blame Verizon (my ISP) and call it a day. Still, the problems occurred, but life must go on.

Enter, **Math Realms**.

Before I go further into what Math Realms is, I should talk about some personal changes I've recently made. Like many college students, I came into school believing that I wanted to do one thing, only to later realize that what I was doing just wasn't a good fit. For a variety of reasons that I won't go into in this article, I recently decided that I am going to become a Game Studies major. Also, due to my current financial predicament (ie. being a broke college student), I decided to take up a Job at Norfolk Public Schools as a Math Tutor. Without going into too much detail, my job is to help kids who struggle in math... learn math.

Maybe you can see where this is going. Due to my recent change in both employment status and major I've been very excited about all things related to game design and secondary level mathematics. In the wake of COVID-19, a lot of middle school students these days just don't have a solid grasp of the fundamental concepts. Most of the students have not been in a traditional classroom format since the fourth or fifth grade. This, in my opinion, has left students who may never have had strong study skills, an inherent liking to math as a subject, or a strong base in math, in the dust.

There already exist many online options for students to take advantage of to learn math concepts. But, given my current situation, why not take this opportunity for learning and portfolio development?

Years ago there was a game called [Club Penguin](https://en.wikipedia.org/wiki/Club_Penguin). I was addicted to it. Like a proper addict, you'd see me online every day for hours, waddling around Club Penguin island discovering the hidden [Easter eggs](https://en.wikipedia.org/wiki/Easter_egg_(media)) and playing the minigames so that I could earn enough coins to furnish my virtual igloo mansion.

Of course, with the advent of [Adobe Flash Player's End of Life](https://www.adobe.com/products/flashplayer/end-of-life.html) and a loss of interest in the game, primarily due to mismanagement by Disney (in my opinion). The cord was cut and the game was shut down.

My idea is to recreate a world similar to that of Club Penguin. A multiplayer universe where students can show off their math skills to their friends through virtual status symbols. I hypothesize that students will want to progress in this world. If a student sees their peers' virtual homes with a cool new item in them or maybe a player has a new outfit on, they too will want that virtual item. Furthermore, I feel that by introducing some level of creative freedom into the game, students will begin to express themselves and have fun. Of course, all of this progression will come at a cost to the player. The fee? You must complete games designed around various secondary-level math concepts.

I am aware that this idea is not a revolutionary one. Using rewards systems to incentivize users to learn or complete difficult (or undesirable) tasks is about as basic as it gets. Our entire society is based on this concept. However, time and time again I see math games that fail to provide adequate rewards to their users. Sure, these games may be more fun or interesting than straight-up worksheets, but in the end, if given the option between a math game and a game like Roblox or Minecraft, what game do you think the student will choose?

Math Realms is a bit of an experiment. I want to see if I can use the psychological aspect of games to benefit the user. Can I get students to crave math?

### A Basic Multiplayer System

<blockquote class="imgur-embed-pub" lang="en" data-id="3Ib194Y" data-context="false" ><a href="//imgur.com/3Ib194Y"></a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

Firebase is somewhat limiting. To be honest, it's a bit frustrating at times but generally speaking, working around the limitations actually made this project quite fun. 

For those who are unaware, Firebase is a "serverless" platform for running web applications. It has built-in user authentication, two types of databases, web hosting, and a variety of other useful features built in to help make developing web-based apps *painless*.

For Math Realms to become a reality, I need a **stable** and generally low-cost solution which operates relatively seamlessly. Luckily, Firebase's SDK comes built-in with a whole set of features that help out with replication and it has a free tier!

This makes Firebase a very attractive option for multiplayer games. But when your budget, is nonexistent, some limitations begin to crop up.

First off, I needed a way to decrease the number of requests that occur between the clients and the server (Firebase Realtime Database). This ultimately decided the type of player movement system I was going to use, which in this case is "point and click". 

If I wanted to use some form of non-deterministic movement system, I'd ultimately have to send an update every couple of frames at the very least to keep the replicated player movement somewhat smooth. Unfortunately, this can eat up a lot of bandwidth that I do not have.

With a deterministic movement system like point and click, you only need to send one update to the database (the new location). Then you can trigger the character's movement on all of the clients once and let the clients figure out where the character should be in between.

I plan on using the [EasyStar Pathfinding Library](https://easystarjs.com/) to help move characters around when I begin to introduce more complex world maps that include restricted portions. But right now I have a simple move to function that just accelerates the characters game object to the new coordinates when Firebase fires a value changed event.

Originally, I was using Firebase's Firestore which also has built-in functionality that updates the client when a certain set of data changes (replication). But, as it turns out, (as of publication) Firestore does not have any way to distinguish if a client is actively connected to the backend or not. Sure, you can hack your way around this limitation by using Cloud Functions to essentially link Realtime Database and Firestore but this just needlessly increases the complexity of the project and it forces you to provide credit card info. So, no way, José.

As a side note, it's interesting to me that when I search Google for "Realtime Database" I get very few useful results. However, when I search for Firestore the results I am getting are generally on point and exactly what I need.

Anyway, once I determined my control scheme and the database that I'd be using, I needed a way to differentiate between players. Obviously, the solution here was to use Firebase's authentication system.

I must say, it was quite nice to not have to worry about storing password hashes. But what was even nicer was that I could very easily determine when a user was logged in and when they weren't without much fuss. In the past, this has always been one of the last things I do as I find it annoying to get everything set up and working properly.

It isn't often when I find a tool that truly makes a difference in my ability to complete a task. Most of the time I find that the latest and greatest tool actually slows me down and just makes projects needlessly complicated. I always feel like I have to relearn how to do things and find the quarks in each system before I can get anything done with them.

I think I've generally covered what I wanted to cover in today's article. I'll post another update regarding the game's progress soon.