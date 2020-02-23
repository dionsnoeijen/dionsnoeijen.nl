---
layout: post
title:  "Leaving projects behind"
date:   2020-02-15 12:12:12 +0200
categories: ui dev
comments: true
author: "Dion"
---

<div class="larger">
I don't write here often, as testified by the lack of posts on my developers blog. But I never stopped looking for the user interface of my dreams. The interface that should be able to relieve us developers of the monotonous tasks we all face all to often. But it should not just work for developers. How many of us have an idea that involves tech that they would love to try, but just don't have the skills, time or people around to make it into a reality. Mind you, this is not about a lack of intelligence or determination. Coding is just hard to master. If it's not your day job, it may be too much. What I'm after, is taking this steep learning curve out of the equation. Actually, I'm after taking coding out of the equation as a whole. But only the code, not the power. This is not a small task, it probably proves to be way harder than I expected but at some point, it will work.
</div>

In the mean time, I haven't been sitting still. In fact, over a year ago I made a pretty big application that facilitates a lot of what the user interface must be able to do as I outlined some years ago. It was a massive endeavour. I spent a big part of a year working on the thing, every free moment I had. Let me show you some of it before I explain why this failed miserably.

<video width="100%" controls>
    <source src="https://dionsnoeijen.s3-eu-west-1.amazonaws.com/dionsnoeijen/continuing/octopus-1.mp4" type="video/mp4">
</video>

I know, it looks kind of crappy, but it was still in the middle of delopment.

During development, some things became clear to me and the conclusion that followed was. This is nog going to work. Not so much the system itself, but the way I was programming it. My mistake revolves around a single choice I made.

## Canvas element

Somehow, I always thought it to be obvious this thing had to be made with the html canvas element. My reasoning was that the canvas element would allow me to make anything I want. Especially regarding some ideas I had that weren't even planned for the first version. But most of all, I was under the impression that the connections between the elements, which are bezier curves, would be way to hard to draw otherwise. But also zooming and panning seemed to be pointing into the direction of canvas too. All things considered, that is the route I took. It's all based on the canvas element. This is probably the main reason why I eventually stopped working on this attempt.

The thing is, with the canvas element, all of the arguments I had for using it are true. But man, it's a lot of work to implement. I mean, every single detail you take for granted with normal html elements are not available. And I'm not just talking about the obvious lack of events. There are much more things to consider when taking the canvas route.

Let me sum up some of the things that were taking up a lot of extra time.

- State management
- Event handling
- Performance

Let me elaborate a little on those issues.

### State management

Managing a state is always a thing to consider carefully of course. At the beginning of this project I made the choice not to use a framework like react, because I figured it wouldn't make a lot of sense in combination with using canvas. But I also did not use a separate state management system. This on itself has been fine, I just made a very simple implementation of a state manager that does the trick. However, using canvas does bring in extra work in another way because drawing the elements is something very different from the abstract state of the elements.

What I did was make a clear separation between the state of an element, it's config if you will, and the drawing of an element. This, theoretically also allowed me to implement different renderers. I mand two renderers, one svg renderer, just for experimentation. And of course a canvas renderer. The renderer contains draw instructions for the canvas context. So if the renderer queries the state and it encounters a rectangle, it will call it's rectangle draw component and voilla, rectangle. So, to be clear. For a rectangle I would have a state component describing it's properties. And I would have a renderer component that can interpret the properties from the state and convert it to something drawn on the canvas.

### Event handling

Obviously, you have no event handling for elements you draw on the canvas. For those that do not know, the canvas element in 2d context really is just an image with a pretty low level graphics api. So drawing something on the canvas comes down to some simple instructions aling the lines of:

```js
context.fillStyle = 'red';
context.beginPath();
context.rect(100, 100, 200, 200);
context.fill();
context.closePath();
```

Doing such a thing inside of an animation callback loop will simply draw a region of pixels red sixty times per second. Your browser does not care about that region at all.

This means you have to implement all of that yourself. What that comes down to is comparing positions of the mouse click and drawn elements. Including their z index.

So, did click on position `{ x: 120, : y:150 }` fall within this element that is in the state positioned at: `{ x: 100, y: 100 }` with a width of `{ width: 200, height: 200 }`? Yes? Dispatch an event.

This comes with a lot of interesting gotchas when you have overlapping elements for example.

### Performance

The html canvas can be pretty fast under some circumstances. But I did learn a couple of things I didn't really expect. Of course, drawing a lot of elements has an impact on the performance. But that got an issue pretty quickly. A single node element was quickly an accumulation of many rectangles, circles, lines and text. Having a hundred of those nodes on the canvas without optimizing is definitely something the processor is going to notice. I took measure to make the performance as good as possible. But not just performance that you would notice in the sense of jitter when you are dragging things around or zooming. I also wanted to keep overall processor activity down as much as possible.

Several measurements were taken to make this into a reality. First thing was to stop the animation loop when nothing is happening. In many cases using a canvas you will just have the animation callback running for the whole time the canvas is visible. But this was heating up my laptop significantly. In fact, the fans on my laptop became somewhat of a benchmark for me. I didn't want to hear my laptop while running the app in my browser at any time. This was a pretty easy fix, just stop updating when there's no user interaction.

The other thing was to make sure all that is rendered is what is in the viewport. Actually, I already had that before I was optimizing performance. This will save you a huge amount of draw calls of course.

The last thing was something I was trying to prevent, but eventually had to implement anyway. When panning or moving, draw less. So instead of showing all details of the nodes, I just show a simple rectangle on the place where the nodes are. This reduced the amount of items that needed to be drawn during performance heavy operations with about 99%.

In the end, those measurements were working really well.

## Why stop?

So, all in all, would this have worked? I'm sure it would! But still, I didn't want to continue due to all the extra work that kept coming up making things you usually take for granted work. It took me some time to accept the fact that I wasted a lot of time on this thing. But sure, I learned a lot as well! And not just about taming a canvas based web application.

<img src="https://dionsnoeijen.s3-eu-west-1.amazonaws.com/dionsnoeijen/continuing/bloodborne.png" title="Bloodborne" alt="Bloodborne" />

You see. Gaming may be something that is easily regarded as being benign, or even an infantile waste of time. And to be honest, a lot of times it is. However, there are games that have actually thought me something in a way no great self-help book could have done.

Anyone that made it to the end of a 'souls' game may know what I'm talking about. I see this loss as a boss I didn't yet defeat. But persistence will lead to the win. Just like all bosses in Bloodborne eventually went down, this ui will come into existence. In fact, I'm working hard on my next attempt. In the following posts on this blog I will attempt to describe the journey. Maybe I will see you along the journey sometime! Thank you for reading!

{% include concept-footnote.html %}
