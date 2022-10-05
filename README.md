# RefTagger
https://faithlife.com/products/reftagger

RefTagger allows you hover over a Bible verse into a Website such as Matt 1:1 and it will pull up the verse.  See the link above for an example. The script below goes at the end of the body of the html, it pulls RefTagger.js from the URL shown in the script.  

Here is the problem.  It doesn't work on WIX Blogs (wix.com).  It will work on a static page but not the blog page, or at least permanently work.  It shows the verse temporarily, then the hover link goes away.  After debugging the issue in the browser, I found the offending .js file, main.24eb524d.bundle.min.js.  At the point this file is loaded into the browser the hover link stops working.  When I block this file from loading in the browser, reftagger continues to work.

There seems to be overlapping variables between the WIX js file and RefTagger js file but not sure the best way to resolve since there are two different companies code.  If I know how to resolve it, I could host refTagger js file on jsdelivr.

Looking for input if anyone knows Javascript. 


```
<script>
	var refTagger = {
		settings: {
			bibleVersion: 'NKJV'
		}
	}; 

	(function(d, t) {
		var n=d.querySelector('[nonce]');
		refTagger.settings.nonce = n && (n.nonce||n.getAttribute('nonce'));
		var g = d.createElement(t), s = d.getElementsByTagName(t)[0];
		g.src = 'https://api.reftagger.com/v2/RefTagger.js';
		g.nonce = refTagger.settings.nonce;
		s.parentNode.insertBefore(g, s);
	}(document, 'script'));
</script>
```

The solution is to set the timeout on the RefTagger script so it will not run for 3 seconds after the initial page load.  This will allow the Wix javascript file to run first so that the RefTagger.js will overwrite the variables vs getting its functions/variables overwritten.  If your site loads slow, you many need to increase the timeout from 3 seconds to 4 or 5.  You will see a three second delay before RefTagger starts working.

In Wix, set this script to only run on the Post page for the blog.  The script w/o the SetTimeout works fine on static pages.

```
<script>
var refTagger = {
settings: {
bibleVersion: 'NKJV'
}
};

setTimeout(() => {
(function(d, t) {
var n=d.querySelector('[nonce]');
refTagger.settings.nonce = n && (n.nonce||n.getAttribute('nonce'));
var g = d.createElement(t), s = d.getElementsByTagName(t)[0];
g.src = 'https://api.reftagger.com/v2/RefTagger.js';
g.nonce = refTagger.settings.nonce;
s.parentNode.insertBefore(g, s);
}(document, 'script'));
}, "3000")
</script>
```
