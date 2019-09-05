---
title: xr-iframe API
description: How to create and manipulate reality tabs.
categories:
  - development-guide
type: Document
set: development-guide
set_order: 21
---

This page will cover how to create and manipulate reality tabs (`xr-iframe`).

## Top-level creation
```js
  // Import exokit-web
  import('https://web.exokit.org/src/index.js')
  .then(() => {
    // Create xr-iframe, define src attribute, and append/place it wherever you want as if it was a normal canvas
    const xrIframe = document.createElement('xr-iframe');
    xrIframe.src = 'child.html';
    document.body.appendChild(xrIframe);
  });
```

## xr-iframe manipulation

```js
  // Set xr-iframe position
  xrIframe.position = [1, 1, 1];
  // Set xr-iframe orientation
  xrIframe.orientation = new THREE.Quaternion().setFromEuler(new THREE.Euler(1, 1, 1, 'YXZ')).toArray();
  // Set xr-iframe scale
  xrIframe.scale = [1, 1, 1];
```

## Child xr-iframe creation

```js
  // Create WebGL renderer and define layers
  const renderer = new THREE.WebGLRenderer({
    antialias: true,
  });
  document.body.appendChild(renderer.domElement);
  const layers = [renderer.domElement];

...

  // Request WebXR session and assign session layers
  const session = await navigator.xr.requestSession({
    exclusive: true,
  });
  session.layers = layers;

...

  // Create xr-iframe, define src attribute, and push to layers
  xrIframe = document.createElement('xr-iframe');
  xrIframe.src = 'childschild.html';
  layers.push(xrIframe);
```
