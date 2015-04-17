---
layout: default
title: Advanced Usage
---

For many websites, TwoStage should just work right out of the box.  However there are a few scenarios that may require some extra effort in your server-side code.

### Placeholders for Dynamic Content
While dynamic content is loading, it will show as an empty space on the page.  You may want to add loading animations behind these elements.  You can do this as follows:

#### HTML

	<div class="dynamic-container">
		<img class="dynamic-loader" src="loader.gif" />
		<div data-dynamic-id="Some Dynamic Content">
			<!-- ...the dynamic content ... -->
		</div>
	</div>

#### CSS

	.dynamic-container {
		position: relative;
	}
	.dynamic-loader {
		position: absolute;
		left: 0;
		right: 0;
		margin: auto;
		/* Centering the image vertically is left as an exercise for the reader. */
	}
	.two-stage-finished .dynamic-loader {
		visibility: hidden;
	}

### Moving Dynamic Logic into Client Code
TwoStage becomes more effective as more of your content is static.  If you have logic on the server that modifies the UI based on cookies, User-Agent, etc. then you may want to consider moving this logic into client-side JavaScript.

For instance, a common scenario is to have an element that is only shown for logged-in users.  Rather than excluding this server-side for anonymous users, you can include it (hidden) for everybody, and show it for authenticated users in JavaScript.  (Authentication cookies should always be set HttpOnly, but you can use a client-visible "ShowLoggedInUI" cookie to make this work.)
	
### Optimizing the Dynamic Response
By default, TwoStage requests a full dynamic copy of the static HTML in the cache.  This means that it doesn't decrease overall server load; in fact it increases the download size required to load a page.

To optimize this, you can have your server code return a partial version of your HTML when the request originated from twoStage.js.  In fact, the overall structure of this HTML doesn't matter at all - twoStage.js simply finds elements with `data-dynamic-id` specified and copies them into the existing DOM.  So, you can omit all the static portions of the document.

To detect when the request originated from twoStage.js, you can modify the Nginx config to add a custom header (with `proxy_set_header`) for the `/_/` location (and check for this header in your server code).

### Authenticated Pages
If a page is only visible to authenticated users (i.e. it redirects to login for anonymous users), then it cannot be accelerated by TwoStage.

To work around this, you can implement your page so that anonymous users get a default version of the page with a logon prompt, rather than a redirect.

## Missing Features

TwoStage is a very new project and is still missing some functionality.  The following scenarios are not well-supported yet, and at this time they will require some extra effort in your application code.

### Version Mismatches
When you deploy a new version of your website, the old version will remain cached in Nginx and CDNs for a period of time (4 hours if you use the suggested Nginx config).  During this time, if there are new dynamic sections in the page, they will not be displayed.

Proper support for HTML versioning and cache invalidation is a high-priority feature that is coming soon.

### Server Redirects
TwoStage does not (yet) properly implement redirects.  This is a high-priority feature that is coming soon.

### Handling AJAX Errors
TwoStage does not (yet) properly handle error responses or failed connections on the AJAX call.  This is a high-priority feature that is coming soon.