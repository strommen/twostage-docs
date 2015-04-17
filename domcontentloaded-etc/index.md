---
layout: default
title: DOMContentLoaded, etc.
---

When using TwoStage, special care must be taken with regard to the DOMContentLoaded event (and onreadystatechanged in IE8, and all the various jQuery 'ready' events).

DOMContentLoaded will fire when the static version of the page (including all referenced stylesheets and scripts) has loaded.  However, the dynamic content will not have been populated into the document at this time.  So for instance, if you are binding to DOM events for elements, you will need to re-bind to these events after the dynamic content is populated.

To accomplish this, you can use `twoStage.onReady`.  This function accepts a query selector along with a callback to run for elements which satisfy that selector.  In DOMContentLoaded, the callback is run for all matching elements in the static version of the page.  Then, once the dynamic content has been populated, the callback is run for all _new_ matching elements.

This allows you to write DOM-based code naturally, without worrying about how everything changes when the dynamic content is populated.

### Example Usage

The following code (which assumes jQuery) binds to all `<a>` tags with a `submit` class, and when such links are clicked, submits the parent form.

	twoStage.onReady('a.submit', function () {
		for (var i = 0; i < this.length; i++) {
			var el = this[i];

			el.on('click', function() {
				var $parentForm = $(this).parents("form");
				$parentForm.submit();
			});
		}
	});

The callback passed to onReady will be called on DOMContentLoaded, and `this` will be an array of elements that match `a.submit`.  When the dynamic content is loaded, if any new elements match `a.submit` the callback will be called again for these new elements.