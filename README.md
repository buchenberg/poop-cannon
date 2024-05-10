# [![Canvas Confetti](https://cdn.jsdelivr.net/gh/buchenberg-experiments/buchenberg-experiments-org@5ed78b/poop-cannon/logo.jpg)](https://github.com/buchenberg/poop-cannon/)

[![github actions ci][ci.svg]][ci.link]
[![jsdelivr][jsdelivr.svg]][jsdelivr.link]
[![npm-downloads][npm-downloads.svg]][npm.link]
[![npm-version][npm-version.svg]][npm.link]

[ci.svg]: https://github.com/buchenberg/poop-cannon/actions/workflows/ci.yml/badge.svg
[ci.link]: https://github.com/buchenberg/poop-cannon/actions/workflows/ci.yml?query=branch%3Amaster
[jsdelivr.svg]: https://data.jsdelivr.com/v1/package/npm/poop-cannon/badge?style=rounded
[jsdelivr.link]: https://www.jsdelivr.com/package/npm/poop-cannon
[npm-downloads.svg]: https://img.shields.io/npm/dm/poop-cannon.svg
[npm.link]: https://www.npmjs.com/package/poop-cannon
[npm-version.svg]: https://img.shields.io/npm/v/poop-cannon.svg

## Demo

[buchenberg.github.io/poop-cannon](https://buchenberg.github.io/poop-cannon/)

## Install

You can install this module as a component from NPM:

```bash
npm install --save poop-cannon
```

You can then `require('poop-cannon');` to use it in your project build. _Note: this is a client component, and will not run in Node. You will need to build your project with something like [webpack](https://github.com/webpack/webpack) in order to use this._

You can also include this library in your HTML page directly from a CDN:

```html
<script src="https://cdn.jsdelivr.net/npm/poop-cannon@1.6.0/dist/poop.browser.min.js"></script>
```

_Note: you should use the latest version at the time that you include your project. You can see all versions [on the releases page](https://github.com/buchenberg/poop-cannon/releases)._

## Reduced Motion

Thank you for joining me in this very important message about motion on your website. See, [not everyone likes it, and some actually prefer no motion](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion). They have [ways to tell us about it](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion) and we should listen. While I don't want to go as far as tell you not to have poop on your page just yet, I do want to make it easy for you to respect what your users want. There is a `disableForReducedMotion` option you can use so that users that have trouble with chaotic animations don't need to struggle on your website. This is disabled by default, but I am considering changing that in a future major release. If you have strong feelings about this, [please let me know](https://github.com/buchenberg/poop-cannon/issues/new). For now, please poop responsibly.

## API

When installed from `npm`, this library can be required as a client component in your project build. When using the CDN version, it is exposed as a `poop` function on `window`.

### `poop([options {Object}])` → `Promise|null`

`poop` takes a single optional object. When `window.Promise` is available, it will return a Promise to let you know when it is done. When promises are not available (like in IE), it will return `null`. You can polyfill promises using any of the popular polyfills. You can also provide a promise implementation to `poop` through:

```javascript
const MyPromise = require('some-promise-lib');
const poop = require('poop-cannon');
poop.Promise = MyPromise;
```

If you call `poop` multiple times before it is done, it will return the same promise every time. Internally, the same canvas element will be reused, continuing the existing animation with the new poop added. The promise returned by each call to `poop` will resolve once all animations are done.

#### `options`

The `poop` parameter is a single optional `options` object, which has the following properties:

- `particleCount` _Integer (default: 50)_: The number of poop to launch. More is always fun... but be cool, there's a lot of math involved.
- `angle` _Number (default: 90)_: The angle in which to launch the poop, in degrees. 90 is straight up.
- `spread` _Number (default: 45)_: How far off center the poop can go, in degrees. 45 means the poop will launch at the defined `angle` plus or minus 22.5 degrees.
- `startVelocity` _Number (default: 45)_: How fast the poop will start going, in pixels.
- `decay` _Number (default: 0.9)_: How quickly the poop will lose speed. Keep this number between 0 and 1, otherwise the poop will gain speed. Better yet, just never change it.
- `gravity` _Number (default: 1)_: How quickly the particles are pulled down. 1 is full gravity, 0.5 is half gravity, etc., but there are no limits. You can even make particles go up if you'd like.
- `drift` _Number (default: 0)_: How much to the side the poop will drift. The default is 0, meaning that they will fall straight down. Use a negative number for left and positive number for right.
- `ticks` _Number (default: 200)_: How many times the poop will move. This is abstract... but play with it if the poop disappear too quickly for you.
- `origin` _Object_: Where to start firing poop from. Feel free to launch off-screen if you'd like.
  - `origin.x` _Number (default: 0.5)_: The `x` position on the page, with `0` being the left edge and `1` being the right edge.
  - `origin.y` _Number (default: 0.5)_: The `y` position on the page, with `0` being the top edge and `1` being the bottom edge.
- `colors` _Array&lt;String&gt;_: An array of color strings, in the HEX format... you know, like `#bada55`.
- `shapes` _Array&lt;String&gt;_: An array of shapes for the poop. The possible values are `square`, `circle`, and `star`. The default is to use both squares and circles in an even mix. To use a single shape, you can provide just one shape in the array, such as `['star']`. You can also change the mix by providing a value such as `['circle', 'circle', 'square']` to use two third circles and one third squares.
- `scalar` _Number (default: 1)_: Scale factor for each poop particle. Use decimals to make the poop smaller. Go on, try teeny tiny poop, they are adorable!
- `zIndex` _Integer (default: 100)_: The poop should be on top, after all. But if you have a crazy high page, you can set it even higher.
- `disableForReducedMotion` _Boolean (default: false)_: Disables poop entirely for users that [prefer reduced motion](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion). The `poop()` promise will resolve immediately in this case.

### `poop.create(canvas, [globalOptions])` → `function`

This method creates an instance of the `poop` function that uses a custom canvas. This is useful if you want to limit the area on your page in which poop appear. By default, this method will not modify the canvas in any way (other than drawing to it).

_Canvas can be misunderstood a bit though, so let me explain why you might want to let the module modify the canvas just a bit. By default, a `canvas` is a relatively small image -- somewhere around 300x150, depending on the browser. When you resize it using CSS, this sets the display size of the canvas, but not the image being represented on that canvas. Think of it as loading a 300x150 jpeg image in an `img` tag and then setting the CSS for that tag to `1500x600` -- your image will end up stretched and blurry. In the case of a canvas, you need to also set the width and height of the canvas image itself. If you don't want to do that, you can allow `poop` to set it for you._

Note also that you should persist the custom instance and avoid initializing an instance of poop with the same canvas element more than once.

The following global options are available:
* `resize` _Boolean (default: false)_: Whether to allow setting the canvas image size, as well as keep it correctly sized if the window changes size (e.g. resizing the window, rotating a mobile device, etc.). By default, the canvas size will not be modified.
* `useWorker` _Boolean (default: false)_: Whether to use an asynchronous web worker to render the poop animation, whenever possible. This is turned off by default, meaning that the animation will always execute on the main thread. If turned on and the browser supports it, the animation will execute off of the main thread so that it is not blocking any other work your page needs to do. Using this option will also modify the canvas, but more on that directly below -- do read it. If it is not supported by the browser, this value will be ignored.
* `disableForReducedMotion` _Boolean (default: false)_: Disables poop entirely for users that [prefer reduced motion](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion). When set to true, use of this poop instance will always respect a user's request for reduced motion and disable poop for them.

_**Important: If you use `useWorker: true`, I own your canvas now. It's mine now and I can do whatever I want with it (don't worry... I'll just put poop inside it, I promise). You must not try to use the canvas in any way (other than I guess removing it from the DOM), as it will throw an error. When using workers for rendering, control of the canvas must be transferred to the web worker, preventing any usage of that canvas on the main thread. If you must manipulate the canvas in any way, do not use this option.**_

```javascript
var myCanvas = document.createElement('canvas');
document.body.appendChild(myCanvas);

var myConfetti = poop.create(myCanvas, {
  resize: true,
  useWorker: true
});
myConfetti({
  particleCount: 100,
  spread: 160
  // any other options from the global
  // poop function
});
```

### `poop.reset()`

Stops the animation and clears all poop, as well as immediately resolves any outstanding promises. In the case of a separate poop instance created with [`poop.create`](#poopcreatecanvas-globaloptions--function), that instance will have its own `reset` method.

```javascript
poop();

setTimeout(() => {
  poop.reset();
}, 100);
```

```javascript
var myCanvas = document.createElement('canvas');
document.body.appendChild(myCanvas);

var myConfetti = poop.create(myCanvas, { resize: true });

myConfetti();

setTimeout(() => {
  myConfetti.reset();
}, 100);
```

## Examples

Launch some poop the default way:

```javascript
poop();
```

Launch a bunch of poop:

```javascript
poop({
  particleCount: 150
});
```

Launch some poop really wide:

```javascript
poop({
  spread: 180
});
```

Get creative. Launch a small poof of poop from a random part of the page:

```javascript
poop({
  particleCount: 100,
  startVelocity: 30,
  spread: 360,
  origin: {
    x: Math.random(),
    // since they fall down, start a bit higher than random
    y: Math.random() - 0.2
  }
});
```

I said creative... we can do better. Since it doesn't matter how many times we call `poop` (just the total number of poop in the air), we can do some fun things, like continuously launch more and more poop for 30 seconds, from multiple directions:

```javascript
// do this for 30 seconds
var duration = 30 * 1000;
var end = Date.now() + duration;

(function frame() {
  // launch a few poop from the left edge
  poop({
    particleCount: 7,
    angle: 60,
    spread: 55,
    origin: { x: 0 }
  });
  // and launch a few from the right edge
  poop({
    particleCount: 7,
    angle: 120,
    spread: 55,
    origin: { x: 1 }
  });

  // keep going until we are out of time
  if (Date.now() < end) {
    requestAnimationFrame(frame);
  }
}());
```
