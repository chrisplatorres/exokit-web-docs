---
title: XR Iframe Overview
description: Learn what XR Iframes' are from a high-level overview.
categories:
  - development-guide
type: Document
set: development-guide
set_order: 22
---

XR Iframes (`xr-iframes`) are like `iframes` but with extra spatial attributes like `position`, `scale`, `orientation`.

### iframe

On the 2d web, an `iframe` is basically a window to another webpage, within a webpage. So you could be on a webpage and on that webpage there's an `<iframe>` that points to another webpage.

Example:
```html
<html>
  <body>

  <iframe src="http://google.com" />

  </body>
</html>
```


### Position

<div style="width: 100%; height: 400px; overflow: hidden"> <img alt="Manipulating A-Blast and Solar System sites loaded together" target="_blank" src="https://user-images.githubusercontent.com/29695350/64731510-fc0ed180-d4a6-11e9-87d5-b76c36c51aea.gif"> </div>

Positioning an `iframe` on a 2d webpage only has 2 ways to move: up and down & left and right. In the 3d web, there is an additional `z` coordinate.

`xr-iframe` has an additional `position` attribute, which allows creators to set where the `xr-iframe` site is in virtual space.

It looks something like this:
`xrIframe.positon = [x, y, z]`

### Orientation

`xr-iframe` has an additional `orientation` attribute, which allows creators to set the rotational direction of the site in virtual space.

It looks something like this using threejs:
`xrIframe.orientation = new THREE.Quaternion().setFromEuler(new THREE.Euler(1, 1, 1, 'YXZ')).toArray()`



### Scale

Defining the size and scale of an `xr-iframe` is as simple as defining the `scale` attribute in three numbers for `x`, `y`, and `z`.

It looks something like this:
`xrIframe.scale = [x, y, z]`
