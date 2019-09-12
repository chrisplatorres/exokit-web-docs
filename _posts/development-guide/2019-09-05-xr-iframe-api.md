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

## Top-level index.html creation

Create the top-level `xr-scene`, which is the parent that holds all of the `xr-iframes`.
```js
  // Import exokit-web
  import('https://web.exokit.org/src/index.js');

  // Create top-level xr-scene, define src attribute, and append/place it wherever you want as if it were a normal canvas
  const xrScene = document.createElement('xr-scene');
  xrScene.src = 'app.html';
  document.body.appendChild(xrScene);
```

Create the top-level `xr-scene` XR entry button:

```js
const enterXrButton = document.getElementById('enter-xr-button');
enterXrButton.addEventListener('click', () => {
  xrScene.enterXr();
});
```

See the example [`index.html`](https://github.com/exokitxr/exokit-web/blob/master/index.html) in GitHub for a full example.


## Child app.html xr-iframe creation

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
