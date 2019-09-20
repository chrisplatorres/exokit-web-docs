---
title: How to Use Exokit Web
description: How to use the Exokit Web library in your project.
categories:
  - development-guide
type: Document
set: development-guide
set_order: 21
---

This page will cover how to create a standalone page with XR iframes (`xr-iframe`). You can view the full [boilerplate code on GitHub](https://github.com/exokitxr/exokit-web/tree/master/boilerplate).

# Exokit Web v2

Exokit Web v2 is an update to exokit-web, simplifying the code needed to get started.

![](https://i.imgur.com/i2nFvgQ.png)

## Elements

### <xr-engine>

`<xr-engine>` is the instance of the engine running in the browser. It acts as a `<canvas>` for the WebXR in your scene.

**5 different ways to make:**

<b>1.</b> Declare it in the HTML raw: `<xr-engine>code</xr-engine>`
<br>
<b>2.</b> Declare it in the HTML src'd: `<xr-engine src=app.html></xr-engine>`
<br>
<b>3.</b> Declare it as a template: `<template is=xr-engine-template>code</template>`
<br>
<b>4.</b> `document.createElement('xr-engine');`
<br>
<b>5.</b> `xrEngine = new XREngine(); document.body.appendChild(xrEngine);`


```html
const xrEngine = new XREngine();
xrEngine.innerHTML = '<xr-site></xr-site>';
document.body.appendChild(xrEngine);
xrEngine.enterXr();
```

Everything in `xr-engine` runs in an isolated `<iframe>` context, so the HTML inside cannot see the top `window`.

That also means if you want to inline things like `<scipt>` tags in your `<xr-engine>` declaration, you need to use the `<template is=xr-engine-template>` variant which uses the standard [HTML Template Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/template). This tells the browser to _not_ immediately run your code as it's parsed, which is the default behavior.

### <xr-site>

This lives inside `<xr-engine>` and defines your XR site. It handles things like setting up the WebXR session and can also control the virtual camera.

```html
<xr-site camera-position="0 1.6 3">
  <xr-iframe src="webxr-site.html"></xr-iframe>
</xr-site>
<script>
const xrSite = document.querySelector('xr-site');
xrSite.cameraPosition = [1, 1, 1];
const session = await xrSite.requestSession(); // get the WebXR session that xr-site auto-created
xrSite.layers.push(canvas); // add a locally-rendered WebGL canvas (such as from THREE.js) as an additional layer
</script>
```

`xr-site` is optional; it handles the work of setting up a WebXR session, but you can make the WebXR session yourself and attach `<xr-iframe>` to it manually. This might be necessary if you want to integrate Exokit Web with another Engine, like A-Frame or Babylon.

### <xr-iframe>

This lives in `<xr-site>` and represents a layer of reality rendered in the world. When you attach it to `xr-site`, it will automatically display in the WebXR session.

`xr-iframe` can be offset in the world with HTML attributes.

```html
<xr-iframe src="webxr-site.html" position="0 0 0"></xr-iframe>
```

## Recursion

Because `<xr-engine>` and `<xr-site>` are WebXR sites, you can put them inside `<xr-iframe>`. It works like you'd expect: [recursively](https://docs.exokit.org/development-guide/how-to-use-exokit-web/#Recursion).

# Cross-origin setup

To access web origins Exokit Web can use a proxy. This requires setting an API key.

## 1. Get an API key

API keys are used to whitelist your domain on the proxy, and are free. Get one in our [Discord](https://discord.gg/zgYEJgS).

## 2. Add API key

Add your API key to your Exokit Web import:

```html
<script type=module src="https://web.exokit.org/ew.js?key=YOUR_API_KEY_HERE"></script>
```
