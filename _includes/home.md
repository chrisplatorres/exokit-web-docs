# Exokit Web, the web metaverse engine


*Exokit Web* is a meta-engine for composing multiple WebXR apps in web page. It works in all web browsers, VR and AR headsets, and has no dependencies.


Exokit Web can load sites made with any WebXR engine -- THREE.js, A-Frame, Babylon.js, JanusWeb, Unity.

[Try the demo](https://web.exokit.org/), or check out the [on Github](https://github.com/exokitxr/exokit-web).

# Why?

At Exokit we're part of a mastermind group -- Metaverse Makers (M3). We meet on a regular basis to discuss how we can collectively build a practical metaverse with current technology.

We use Hubs for our meetings and it's great to be able to share videos and screenshots of our projects, face-to-face.

![](https://i.imgur.com/qe1YVnn.gif)
<sub><sup>*Avaer presenting in Hubs at M3 on "The Metaverse Technology in your Web Browser".*</sup></sub>

So we asked ourselves -- why are we sharing videos when we could simply load the real app in the world?

Exokit Web makes that possible.

![](https://i.imgur.com/lZvxp1P.gif)
<sub><sup>*Mozilla Hubs avatar seamlessly painting inside of a Cryptovoxels parcel.*</sup></sub>

The best part is we didn't have to bug Mozilla to add this feature -- it works at the compsitor level!

And we don't have to ask anyone to change their app to make it compatible -- that's handled by WebXR!

# How does it work?

The core technology we created for Exokit Web is XR IFrames.

`<xr-iframe>` is a regular HTML Element:

```html
<xr-iframe src="https://aframe.io/a-painter" position="2 1.6 -1"></xr-iframe>
```

When you add an `xr-iframe` to your WebXR session, it will be automagically blended into your scene. You can also control `xr-iframe` programmatically:

```js
xrIframe = document.createElement('xr-iframe');
xrIframe.src = 'https://aframe.io/a-painter';
xrIframe.position = [2, 1.6, -1];
session.layers.push(xrIframe);
```

To get started with what's possible, see the [README.md on Github](https://github.com/exokitxr/exokit-web).

So what else are we doing with XR IFrames?

# A 3D web browser

We've had a lot of people ask for a browser built on top of Exokit technology.

The good news is: we've been working on this for a while!

![](https://i.imgur.com/w3HcjBM.gif)
<sub><sup>*Opening and managing `xr-iframe` can be as simple as point, click, and drag.*</sup></sub>

*Exokit Browser* is coming soon to a browser near you, and it's built on top of Exokit Web. It's a regular web app that works as an immersive browser:

- URL autocomplete
- Search integration
- Persistent home space
- Virtual keyboard
- XR IFrame placment
- Voice controls
- Multiplayer parties with WebRTC

Of course, Exokit Browser supports the same headsets, platforms, and WebXR sites as Exokit Web.

Keep an eye on [our socials](https://twitter.com/exokitxr) for Exokit Browser news in the coming weeks. (psst: [Sign up for early access here](https://mailchi.mp/ee614096d73a/exokitweb)).

## Exokit Interlink

The web is all about connecting. Connecting with the apps we use -- but more importantly with each other.

Exokit Interlink is our experiment in taking the Exokit Browser and making it into a web universe.

![](https://i.imgur.com/QPTHKic.gif)
<sub><sup>*Moon Rider, Mozilla Hubs, nature, and a solar system overhead. Each one is a seperate site simultaneously loaded in a shared space.*</sup></sub>

Every location in Exokit Interlink is a url you can share. When your friends join -- from VR, AR, or desktop, you see each other, and all of the same apps.

Exokit Interlink is perfect for sharing on Twitter.

Aditionally, we're exploring some crazy ideas that start to make sense in a multiplayer 3D browser:

- Blockchain integrations
- 3D model, images, audio, and video tags
- Deploy WebXR from Github
- AI bots
- Web Payments API
- Mixer + Twitch inegration

# The tech

As you can imagine, Exokit Web squeeze some serious juice from the web platform.

## WebXR

The main thing that makes Exokit Web possible is the WebXR API.

WebXR defines a contract between the browser and applicaton that looks like this:

```
Headset -> WebXR Pose -> Web site -> WebGL frame -> Headset
```

WebXR sites transform poses to frames. That is, they are functions that you call with a pose to get a frame!

This opened the door for a meta-engine like Exokit Web -- the engine computes the pose, calls the site to get frames, and then blends everything together.

![](https://i.imgur.com/DYWN7A6.gif)
<sub><sup>*Solar system and Exokit tutorial sites loaded through Exokit Web on Magic Leap One's Helio browser.*</sup></sub>

Exoit Web works not only with a headset, but also with keyboard-and-mouse, as well as forming a basis for headless WebXR bots :eyes: (shh).

## Virtual GL

Another key part of Exokit Web is its WebGL context virtualization.

In order to blend multiple sites for displaying into a single scene, the engine needs to juggle the WebGL resources (framebuffers, textures, etc.) used by the sites.

To make this work, we "virtualized" the GL contexts in XR Iframes and intelligently mapped them to contexts attached to the headset. This was relatively straightforward to do in Javascript.

<div style="width: 100%; height: 550px; overflow: hidden">
<img alt="Supercraft and Moonrider" target="_blank" src="https://i.imgur.com/PteCXjG.gif"/>
</div>
<sub><sup><i>Metaverse pro-tip: Load Moon Rider to play background music while you build creations with Supercraft.</i></sup></sub>

## Javascript modules

We wanted Exokit Web to be loadable into any app with no dependencies or build step.

```html
<script type=module src="https://web.exokit.org/ew.js"></script>
```

We also wanted it to be super simple to update the engine -- there should be no build or bundler step required.

This is made possible with Javascript Modules, which allow us to write modular code that is resolved at runtime by the brwoser.

```js
import {HTMLElement} = from './DOM.js';
```

The end result is that using Exokit Web is one line of code, and deploying an update to Exokit Web is a single `git push` to Github Pages. Everyone wins!

## Service Workers

The biggest piece of infrastructure we had to build for Exokit Web turned out to be the Service Worker.

The reason for this was twofold:

- The browser security model
- Instability in the WebVR/WebXR apis

Browser security is based around origins -- essentially domain names. This means that by default one origin (like `https://web.exokit.org/` can access its own resources like `https://web.exokit.org/logo.svg`, but not other origin's resources like `https://aframe.io/a-painter`). This presents a problem when you want to load `https://aframe.io/a-painter` as a world in `https://web.exokit.org/`, as Exokit Web does 😐.

We worked around this this with a service worker routing requests to a proxy server, which adds approrpriate CORS headers. This means that the whole web can be made to work in Exokit Web!

Another problem that we ran into is that there have been several versions of WebVR and WebXR, and each site author coded against whatever their browser works with.

API polyfills help -- and we use them in Exokit Web -- but only to an extent.

For example, there are engines that assume that a headset has exactly two equally sized eyes to render. That might seem reasonable until you try to do a single-"eye" rendering on a desktop screen.

Another common hiccup is VR auto-entry: many sites require the user to click a 2D button to enter the experience, but that is either inconvenient or outright impossible in an XR IFrame environment like Exokit Web.

Our service worker implementation handles these situations by dynamically rewriting the code to patch over known issues in libraries.

# What's next!

[*Exokit Web* is on Github](https://github.com/exokitxr/exokit-web). We're excited to see the hacks you come up with!

*Exokit Browser* and *Exokit Interlink*, our secret projects built on Exokit Web, are entering beta 😉. [Click here to add your email to the waitlist](https://mailchi.mp/ee614096d73a/exokitweb). We'll be sharing more on these in the coming weeks.

Finally, if this stuff excites you as much as it excites us, [follow us on Twitter](https://twitter.com/exokitxr), hang out with us on [the Exokit Discord](https://discord.gg/UgZDFZW), or better yet: join us live at the next [Metaverse Makers Mastermind](https://discord.gg/UgZDFZW) ([schedule](https://github.com/m3-org/schedule)).
