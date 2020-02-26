---
layout: post
title:  "Css and JavaScript based grid view"
excerpt: "Making an infinite grid based on css and made dynamic with JavaScript"
date:   2020-02-26 14:14:14 +0200
categories: development css javascript
comments: true
author: "Dion"
---

Let me share some code that may be of use to somebody. For my application I need a grid. Not the kind for responsive websites but like a grid intended for visual guidelines. Much like a notebook.

<img src="https://i.ebayimg.com/images/i/400910923911-0-1/s-l1000.jpg" alt="Writing block" width="300">

I have some requirements that I want to support.
- Zoom
- Panning
- It must be infinite. So no max height or width.
- No background images

So, I came up with this.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="js,result" data-user="octopus11" data-slug-hash="yLNMzwg" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Infinite grid">
  <span>See the Pen <a href="https://codepen.io/octopus11/pen/yLNMzwg">
  Infinite grid</a> by Dion Snoeijen (<a href="https://codepen.io/octopus11">@octopus11</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

It's best viewed from within codepen because the embed widget is ignoring the scrollwheel and mouse events. Or at least, it works wonky like this.

You can use the scrollwheel, or whatever that you are using to scroll to zoom in and out. And if you hold down space, you can click and drag the grid in any direction.

The solution is relatively simple. It's using a css gradient to draw the grid. Then we update the css on events and then we set the gradient css on the grid element.

So, first let's create the html and css.

```html
<div class="grid">
</div>
```

We really don't need anything fancy, let's just focus on the grid and give it minimal styling.

```css
body, html {
  padding: 0;
}

.grid {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
}

.panning {
  cursor: grab;
}
```

I start with initializing some variables in JavaScript. Their use will be clear soon.

```js
const grid = document.querySelector('.grid');
const translate = {
  scale: 1,
  translateX: 0,
  translateY: 0
};
let panning = 0;
const pinnedMousePosition = { x: 0, y: 0 };
const pinnedGridPosition = { x: 0, y: 0 };
```

Now I will introduce a `doGrid()` method so we can initialize the grid system.

```js
const doGrid = () => {};
doGrid();
```

Right below that the method is called. The doGrid method will contain all event listeners.

####  Events for panning

`keydown` is needed to register if the spacebar is being pressed. With that info we can update `panning` with +1.

`keyup` will cancel the spacebar press to set `panning` back with 1.

`mousedown` will store all current parameters and will add +1 to panning.

`mouseup` will take 1 of `panning`.

`mousemove` to handle the changes made by `panning`.

#### Event for zooming

`scrollwheel` It's for registering a zoom in / out action.

```js
  grid.addEventListener('wheel', event => {
    translate.scale += (event.deltaY * (translate.scale / 5000));
    if (translate.scale > 3) {
      translate.scale = 3; 
    }
    if (translate.scale < 0.4) {
      translate.scale = 0.4;
    }
    update();
  });

  document.addEventListener('keydown', event => {
    if (event.key === ' ' && !panning) {
      event.preventDefault();
      grid.classList.add('panning'); 
      panning++;
    }
  });

  document.addEventListener('mousedown', event => {
    pinnedGridPosition.x = translate.translateX;
    pinnedGridPosition.y = translate.translateY;
    pinnedMousePosition.x = event.clientX;
    pinnedMousePosition.y = event.clientY;
    panning++;
  });

  document.addEventListener('mouseup', () => {
    panning--;
  });

  document.addEventListener('keyup', event => {
    panning--;
    grid.classList.remove('panning');
  }); 

  grid.addEventListener('mousemove', event => {
    if (panning === 2) {
      const diffX = (event.clientX - pinnedMousePosition.x) / translate.scale;
      const diffY = (event.clientY - pinnedMousePosition.y) / translate.scale;
      translate.translateX = pinnedGridPosition.x + diffX;
      translate.translateY = pinnedGridPosition.y + diffY;
      update();
    }
  });
```

It's all about updating the `translate` object. Once updated, we need to call the method that's doing all the real work. The `update` method which is also part of `doGrid`.

```js
  const update = () => {
    let transform = '';
    Object.keys(translate).forEach(property => {
      transform += property + '(' + translate[property] + (property !== 'scale' ? 'px' : '') + ') ';
    });
    grid.style.transform = transform.trim();

    const vars = {
      a: 9 * translate.scale,
      b: 10 * translate.scale,
      c: -.5 * translate.scale,
      d: translate.scale,
      e: 99.5 * translate.scale,
      f: 100 * translate.scale
    };
    const backgroundPosition = grid.getBoundingClientRect();
    const colors = { a: 57, b: 75, c: 80 };

    const background = `
    repeating-linear-gradient(
      0deg,
      transparent 0,
      transparent ${vars.a}px,
      rgb(${colors.b}, ${colors.b}, ${colors.b}) ${vars.a}px,
      rgb(${colors.b}, ${colors.b}, ${colors.b}) ${vars.b}px
    ) 
    ${backgroundPosition.x}px ${backgroundPosition.y}px / ${vars.f}px ${vars.f}px repeat,
    repeating-linear-gradient(
      90deg,
      transparent 0,
      transparent ${vars.a}px,
      rgb(${colors.b}, ${colors.b}, ${colors.b}) ${vars.a}px,
      rgb(${colors.b}, ${colors.b}, ${colors.b}) ${vars.b}px
    ) 
    ${backgroundPosition.x}px ${backgroundPosition.y}px / ${vars.f}px ${vars.f}px repeat,
      repeating-linear-gradient(
      0deg,
      rgb(${colors.c}, ${colors.c}, ${colors.c}) ${vars.c}px,
      rgb(${colors.c}, ${colors.c}, ${colors.c}) ${vars.d}px,
      transparent ${vars.d}px,
      transparent ${vars.e}px
    ) 
    ${backgroundPosition.x}px ${backgroundPosition.y}px /  ${vars.f}px ${vars.f}px repeat,
    repeating-linear-gradient(
      90deg,
      rgb(${colors.c}, ${colors.c}, ${colors.c}) ${vars.c}px,
      rgb(${colors.c}, ${colors.c}, ${colors.c}) ${vars.d}px,
      transparent ${vars.d}px,
      transparent ${vars.e}px
    ) 
    ${backgroundPosition.x}px ${backgroundPosition.y}px /  ${vars.f}px ${vars.f}px repeat,
    rgb(${colors.a}, ${colors.a}, ${colors.a});
`;
    grid.setAttribute('style', 'background: ' + background);
  }
```

As you can see the update method is used to generate the background css attribute. It needs to be set to the element by using setAttribute. `grid.style.background = background;` does not work and I honestly don't know why to be honest ;).

Please check the full example on [codepen](https://codepen.io/octopus11/pen/yLNMzwg "codepen"){:target="_blank"}.
