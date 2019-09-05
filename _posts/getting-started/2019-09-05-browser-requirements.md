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

## Firefox
Using Exokit Web in Firefox, you may get the error in console: `ReferenceError: SharedArrayBuffer is not defined`.

Per Mozilla [SharedArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer) is disabled in Firefox due to Spectre, but, re-enabled in Chrome since ~v67.

To fix:
Open the `about:config` page in Firefox and set the `javascript.options.shared_memory` value to `true`.
