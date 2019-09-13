---
title: How to Use Exokit Web
description: How to use the Exokit Web library in your project.
categories:
  - development-guide
type: Document
set: development-guide
set_order: 21
---

This page will cover how to create and manipulate XR iframes (`xr-iframe`).

# Top-level index.html creation

Create the top-level `xr-scene`, which is the parent that holds all of the `xr-iframes`.
```js
  // Import exokit-web
  import('https://web.exokit.org/ew.js');

  // Create top-level xr-scene, define src attribute, and append/place it wherever you want as if it were a normal canvas
  const xrScene = document.createElement('xr-scene');
  xrScene.src = 'app.html';
  document.body.appendChild(xrScene);
```
*Note: you can also import as a script tag:*
```html
<script type=module src="https://web.exokit.org/ew.js"></script>
```

Create the top-level `xr-scene` XR entry button:

```js
const enterXrButton = document.getElementById('enter-xr-button');
enterXrButton.addEventListener('click', () => {
  xrScene.enterXr();
});
```

In the *top-level directory of your app*, create a file named `sw.js` with these contents:

```js
importScripts('https://web.exokit.org/sw.js');
```

Finally, make sure you are serving your app over `https://` (or `localhost`), which is [required for Service Workers](https://developers.google.com/web/fundamentals/primers/service-workers/#you_need_https).

If your app is limited to your own site (same origin), you're done!

If your app accesses other sites (i.e. [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)), the above will not work. If you wish to access web origins, you will need to [request an API key](#cross-origin-setup).

See the example [`index.html`](https://github.com/exokitxr/exokit-web/blob/master/index.html) in GitHub for a full example.


# Child app.html xr-iframe creation

```js
// Create WebXR session
session = await navigator.xr.requestSession('immersive-vr');

// Add Mozilla Hub
fooFrame = document.createElement('xr-iframe');
fooFrame.src = 'https://hubs.mozilla.com/VxGmqjU/fuchsia-winding-room?vr_entry_type=vr_now';
session.layers.push(fooFrame);

// Add A-Painter too
barFrame = document.createElement('xr-iframe');
barFrame.src = 'https://aframe.io/a-painter';
session.layers.push(barFrame);

// Set the xr-iframe position, from any array
barFrame.position = [1, 1, 1];
// Set the xr-iframe orientation
barFrame.orientation = new THREE.Quaternion().setFromEuler(new THREE.Euler(1, 1, 1, 'YXZ')).toArray();
// Set the xr-iframe scale
barFrame.scale = [1, 1, 1];

```

See the example [`app.html`](https://github.com/exokitxr/exokit-web/blob/master/app.html) in GitHub for a full example.

# Cross-origin setup

To access web origins Exokit Web can use a proxy. This requires setting an API key.

## 1. Get an API key

API keys are used to whitelist your domain on the proxy, and are free. Get one in our [Discord](https://discord.gg/zgYEJgS).

## 2. Add API key

Add your API key to your Exokit Web import:

```html
<script type=module src="https://web.exokit.org/ew.js?key=YOUR_API_KEY_HERE"></script>
```
