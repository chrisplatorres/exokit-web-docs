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

Create the top-level `xr-scene`, which is the parent that holds all of the `xr-iframes`.
```js
  // Import exokit-web
  import('https://web.exokit.org/src/index.js')
  .then(() => {
    // Create xr-scene, define src attribute, and append/place it wherever you want as if it was a normal canvas
    const xrScene = document.createElement('xr-scene');
    xrScene.src = 'child.html';
    document.body.appendChild(xrScene);
  });
```

Top-level XR entry button:

```js
const enterXrButton = document.getElementById('enter-xr-button');
enterXrButton.addEventListener('click', () => {
  xrScene.enterXr();
});
```

## xr-iframe manipulation

Manipulate the `xr-iframe` attributes: `position`, `orientation` and `scale`:

```js
  // Set xr-iframe position, from any array
  xrIframe.position = [1, 1, 1];
  // Set xr-iframe orientation
  xrIframe.orientation = new THREE.Quaternion().setFromEuler(new THREE.Euler(1, 1, 1, 'YXZ')).toArray();
  // Set xr-iframe scale
  xrIframe.scale = [1, 1, 1];
```

## Child xr-iframe creation

Create a WebGL renderer and define layers:

```js
  const renderer = new THREE.WebGLRenderer({
    antialias: true,
  });
  document.body.appendChild(renderer.domElement);
  const layers = [renderer.domElement];
```

Enter XR:

```js
const _enterXr = async () => {
  const session = await navigator.xr.requestSession({
    exclusive: true,
  });
  session.layers = layers;

  session.requestAnimationFrame((timestamp, frame) => {
    renderer.vr.setSession(session, {
      frameOfReferenceType: 'stage',
    });

    const pose = frame.getViewerPose();
    const viewport = session.baseLayer.getViewport(pose.views[0]);
    const height = viewport.height;
    const fullWidth = (() => {
      let result = 0;
      for (let i = 0; i < pose.views.length; i++) {
        result += session.baseLayer.getViewport(pose.views[i]).width;
      }
      return result;
    })();
    renderer.setSize(fullWidth, height);
    renderer.setPixelRatio(1);

    renderer.setAnimationLoop(null);

    renderer.vr.enabled = true;
    renderer.vr.setSession(session);
    renderer.vr.setAnimationLoop(animate);
  });
};
```

Create an `xr-iframe`, define the `src` attribute, and push to `layers`:

```js
  xrIframe = document.createElement('xr-iframe');
  xrIframe.src = 'childschild.html';
  layers.push(xrIframe);
```
