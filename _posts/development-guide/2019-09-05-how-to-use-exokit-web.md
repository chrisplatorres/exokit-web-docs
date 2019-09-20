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

# The Basics

## index.html  
This is the most simple code to get started. This loads the Solar System WebXR demo and A-Painter. It also sets up an "Enter XR" button to be able to enter the site in immersive mode.

```html  
<html>
<body>
  <input type="button" value="Enter XR" onclick="document.getElementById('xr-engine').enterXr();">
  <script type="module" src="https://web.exokit.org/ew.js"></script>
  <xr-engine>
    <xr-site>
      <xr-iframe src="https://exokitxr.github.io/webxr-samples/xr-presentation.html"></xr-iframe>
      <xr-iframe src="https://aframe.io/a-painter/"></xr-iframe>
    </xr-site>
  </xr-engine>
</body>
</html>
```


## sw.js
The service worker loaded by the browser points to use Exokit Web's `sw.js`.

```js
importScripts('https://web.exokit.org/sw.js');
```

# Cross-origin setup

To access web origins Exokit Web can use a proxy. This requires setting an API key.

*note: you do not need an API key to [access the proxy on localhost via dev.exokit.org](https://github.com/exokitxr/exokit-web/blob/master/index.js#L11-L17).*
## 1. Get an API key

API keys are used to whitelist your domain on the proxy, and are free. Get one in our [Discord](https://discord.gg/zgYEJgS).

## 2. Add API key

Add your API key to your Exokit Web import:

```html
<script type=module src="https://web.exokit.org/ew.js?key=YOUR_API_KEY_HERE"></script>
```
