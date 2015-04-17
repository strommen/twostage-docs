---
layout: default
title: How it Works
---

Here is a more detailed summary of how TwoStage works.  For further detail, you may want to browse through the JavaScript and nginx config on [Github](https://github.com/strommen/twoStage).

### Step 1 - Browser Navigation Request

When the browser requests a page, the request is handled by an nginx reverse proxy.  If this page has already been cached, it is simply returned.  

If the proxy has not yet cached this page, it anonymizes the request by changing the source IP address and removing the `Cookie` and `User-Agent` headers.  (This ensures that no private information is ever cached, since no identifying information is sent to the origin server.)

### Step 2 - Browser NavigationResponse

When the server returns the response, it goes back through the nginx proxy.  The proxy removes cookies and sets the `Cache-Control` header.  It also caches the response body.

In the browser, twoStage.js dynamically creates a CSS rule to set `visibility: hidden !important` for all elements with a `data-dynamic-id` attribute.  This prevents stale data from being displayed.

### Step 3 - AJAX Request

When twoStage.js is run, it re-requests the current page with an underscore prepended to the URL path (e.g. `http://local.twostage.io/how-it-works/` would request `http://local.twostage.io/_/how-it-works/`).

The nginx proxy removes the underscore from the URL and forwards the request onward.  This time, all headers are left intact so the origin server can populate personalized data into the response.

### Step 4 - AJAX Response

When twoStage.js receives the AJAX response, it creates a DOM fragment and searches for elements with a `data-dynamic-id` attribute.  Each such dynamic element replaces the corresponding DOM element in the static page (matched up by attribute value).

If the AJAX response is received before the DOMContentLoaded event, everything described above is deferred until after DOMContentLoaded.