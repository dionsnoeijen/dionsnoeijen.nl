---
layout: post
title:  "Rethinking my approach"
date:   2020-02-17 08:00:00 +0200
categories: concept
comments: true
author: "Dion"
---

<div class="larger">
So, in my <a href="https://dionsnoeijen.nl/ui/dev/2020/02/15/leaving-projects-behind.html">last post</a> I describe how my first attempt failed. But I'm not done yet.
</div>

I have taken some time to rethink what I'm doing. First of all, there needs be be a bigger plan. I mean, when I zoom out a little, and think about the concept itself. How is this thing even going to work at all?
 Ok, the idea is to have a node based system to build backend web applications without having to write code. I have written about this idea extensively in my first posts some years ago. However, as I find out while exploring the idea, I conclude that working that way wil lead to spaghetti, almost literal spaghetti if you look at the final design of the concept.

 ![Completed flow](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/everything.PNG)

 At this point, the solution to this problem is still quite vague. In fact, I'm exploring the solution right now. Maybe this should have been the first thing to do, but I don't like it when such a thing keeps me from building. So, also in this case. I already have a brand new, working ui engine that facilitates a lot of the basic requirements. It's much better than the last one. But for clarity, I will not talk about it to much yet. If I would write about my projects the way I work, it would be really hard to follow. Let me start at the beginning as if I'm a structured person.

## First things first

I need to outline the big steps that are required to take.

1. The concept
2. The engine
3. The backend
4. The business
5. The hosting

Those steps break it down in the largest possible chunks I can think of.

### The concept

Really, al I had in mind before starting the development on the new engine were some vague ideas. I need grouping, to prevent spaghetti. And I need a clear starting point of the flow. Which is, in basis a route. But of course. There is much more to it. This is where I am now. I will start showing the conceptual flows of the application to come to a workable starting point.

### The engine

A lot of work has been done regarding the reworking of the engine. And a lot of the basic functionality stands already. But, after the concept is finished, a lot of work still needs to be done. The choices of software have been way different. For example, this is a React with Redux application. Canvas is involved, but in a very different manner. I will show all about it soon.

### The backend

See, this is another huge thing that needs to be tackled. I can have a massive and beautiful ui. But a backend is required as well. What else is going to run and interpret all the connections and configuration? I may already have a big chunk of this backend in my open source project SexyField. Much more on this later as well.

### The business

Before I can even start thinking about how to host this thing, I have to know how to bring this project to market. Will it be a saas platform, or rather a licence based system. Maybe open source even? I really don't know yet. I hope things will become more clear later on.

### The hosting

Say it will be a saas platform, how to host? And if not, which is most likely going to be the case, how will I make sure it's widely supported. Will I have to make an installer, remote updates? There's a lot to decide on this side of the project.

## Mvp

This brings me to another point. This thing has the potency to explode right in my face when it comes to the amount of work. Honestly I don't even know if it's reasonable to keep at it by myself. But as for now, there really is no other option. I will have to make sure not to implement every little feature I can think of. In fact, I should not even try to work towards a real working mvp at first. The first thing I will go for, is a small promo movie, tho show what it should do. I may fake some things, just for the movie. But that's fine. That way I will finally be able to really show what it's supposed to do!

Thanks for reading this far! The next post will really go into detail about the concept!

{% include concept-footnote.html %}
