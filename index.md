---
layout: default
---

TwoStage is a way to make dynamic web pages load **extremely fast**.

It works by serving a static snapshot of your HTML from a low-latency server such as a CDN or caching reverse proxy.  The dynamic portions of the page (which are typically a very small amount of the content) are hidden at first, then populated with dynamic data in the background via AJAX.

Thus the page is loaded in two separate stages for static and dynamic content &mdash; hence the name.

TwoStage is __compatible with any server back-end__ that generates HTML, and usually requires only a few minor code changes.

[Read more about how TwoStage works](/how-it-works/)

## How do I use TwoStage?

There are a few steps required to implement TwoStage:

1. Include twoStage.js in your page &mdash; ideally as early in the `<head>` as possible. (Get the script from [the Github repository](https://github.com/strommen/twoStage/))
1. Add an nginx reverse proxy in front of your server, with the suggested configuration from [the Github repository](https://github.com/strommen/twoStage/).  If you're already using nginx, you can merge this into your existing configuration.
1. Mark dynamic sections of your HTML with a `data-dynamic-id` attribute.
  * The value for this should be unique within an individual page.
  * These sections will have `visibility: hidden;` while the dynamic content is loading.
1. If necessary, adapt your JavaScript to the new DOM loading behavior.  See [DOMContentLoaded, etc.](/domcontentloaded-etc/) for details on this.
  
**Your dynamic pages are now public-cacheable**, and will be served lightning-fast from your nginx reverse proxy.  If you want, you can serve through a CDN for even more speed.

ProTip: Since TwoStage eliminates server processing for the initial HTML document, it works especially well with a preloader like [InstantClick](http://instantclick.io/) or `<link rel="prefetch"...`.

[More tips for advanced usage](/advanced-usage/)

## What's Next?
This project is in its very beginning stages.
If you're interested at all, I _highly_ recommend you sign up for the mailing list, or add a watch to the Github repository.

<!-- Begin MailChimp Signup Form -->
<div id="mc_embed_signup">
<form action="//fasterweb.us8.list-manage.com/subscribe/post?u=ff88bc7630de02baa7f84dc85&amp;id=46e3e958e2" method="post" id="mc-embedded-subscribe-form" name="mc-embedded-subscribe-form" class="validate" target="_blank" novalidate>
    <div id="mc_embed_signup_scroll">
	<h2>Keep me up-to-date with TwoStage</h2>
<div class="mc-field-group">
	<label for="mce-EMAIL">Email Address </label>
	<input type="email" value="" name="EMAIL" class="required email" id="mce-EMAIL">
</div>
	<div id="mce-responses" class="clear">
		<div class="response" id="mce-error-response" style="display:none"></div>
		<div class="response" id="mce-success-response" style="display:none"></div>
	</div>    <!-- real people should not fill this in and expect good things - do not remove this or risk form bot signups-->
    <div style="position: absolute; left: -5000px;"><input type="text" name="b_ff88bc7630de02baa7f84dc85_46e3e958e2" tabindex="-1" value=""></div>
    <div class="clear"><input type="submit" value="Subscribe" name="subscribe" id="mc-embedded-subscribe" class="button"></div>
    </div>
</form>
</div>
<script type='text/javascript' src='//s3.amazonaws.com/downloads.mailchimp.com/js/mc-validate.js'></script><script type='text/javascript'>(function($) {window.fnames = new Array(); window.ftypes = new Array();fnames[0]='EMAIL';ftypes[0]='email';fnames[1]='FNAME';ftypes[1]='text';fnames[2]='LNAME';ftypes[2]='text';}(jQuery));var $mcj = jQuery.noConflict(true);</script>
<!--End mc_embed_signup-->

<iframe id="github-watch" src="https://ghbtns.com/github-btn.html?user=strommen&repo=twoStage&type=watch&size=large&v=2" frameborder="0" scrolling="0" width="160px" height="30px"></iframe>


The following features will soon be supported:

* Support for server redirects.
* Support for versioning, so that the cached HTML gets refreshed when the dynamic AJAX request returns a different version.
* Proper handling for 400/500 responses (and other errors) on the AJAX request.


## Request for Feedback
If you have any thoughts about this project &mdash; and especially you're interested in using it or contributing, I'd love to chat.
Hit me up at [joe@joestrommen.com](mailto:joe@joestrommen.com) or [@strommen](https://twitter.com/strommen).
