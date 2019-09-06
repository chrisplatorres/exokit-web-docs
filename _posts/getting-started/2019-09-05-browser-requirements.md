---
date: 2018-01-01
title: Browser Requirements
description: Guide on browser requirements for Exokit Web.
redirect_from:
  - /documentation/browser-requirements/
categories:
  - getting-started
type: Document
set: getting-started
set_order: 2
---

This page covers any browsers requirements needed before running Exokit Web.

# WebXR Compatiblity
The [WebXR Device API](https://github.com/immersive-web/webxr/blob/master/explainer.md) provides access to input and output capabilities commonly associated with Virtual Reality (VR) and Augmented Reality (AR) devices. It allows you develop and host VR and AR experiences on the web. Browsers must have WebXR compatibility to enter XR mode on Exokit Web.

# Chrome
To run Exokit Web with Chrome, you must run Chrome 66 and later, and enable the `chrome://flags/#webxr` flag. (The URL must be entered manually.)

# Firefox
Using Exokit Web in Firefox, you may get the error in console: `ReferenceError: SharedArrayBuffer is not defined`.

Per Mozilla [SharedArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer) is disabled in Firefox due to Spectre, but, re-enabled in Chrome since ~v67.

To fix:
Open the `about:config` page in Firefox and set the `javascript.options.shared_memory` value to `true`.
