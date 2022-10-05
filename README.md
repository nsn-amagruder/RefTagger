# RefTagger
https://faithlife.com/products/reftagger

RefTagger allows you hover over a Bible verse into a Website such as Matt 1:1 and it will pull up the verse.  See the link above for an example. The script below goes at the end of the body of the html, it pulls refTagger.js from the URL shown in the script and that it is.  

Here is the problem.  It doesn't work on WIX Blogs (wix.com).  It will work on a static page but not the blog page, or at least permently work.  It shows the verse temporarily, then the hover link goes away.  After debugging the issue in the browser, I found the offending .js file, main.24eb524d.bundle.min.js.  When I block this file from loading in the browser, reftagger continues to work.

There seems to be overlapping variables between the WIX js file and RefTagger js file but not sure the best way to resolve since there are two different companies code.  If I know how to resolve it, I could host refTagger js file on jsdelivr.

Looking for input if anyone knows Javascript.

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
