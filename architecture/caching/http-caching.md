# HTTP Caching

Caching is a great example of the ubiquitous time-space tradeoff in programming. You can save time by using space to store results.

In the case of websites, the browser can save a copy of images, stylesheets, javascript or the entire page. The next time the user needs that resource (such as a script or logo that appears on every page), the browser doesn’t have to download it again.

## Stale Cache

Images, javascript etc can become stale when a newer version exists on the server.

## Strategies
### Last Modified

One fix is for the server to tell the browser what version of the file it is sending. A server can return a Last-modified date along with the file (let’s call it logo.png), like this:

    Last-modified: Fri, 16 Mar 2007 04:00:25 GMT File Contents (could be an image, HTML, CSS, Javascript...)

Now the browser knows that the file it got (logo.png) was created on Mar 16 2007. The next time the browser needs logo.png, it can do a special check with the server:

1. Browser: Hey, give me logo.png, but only if it’s been modified since Mar 16, 2007.
2. Server: (Checking the modification date)
3. Server: Hey, you’re in luck! It was not modified since that date. You have the latest version.
4. Browser: Great! I’ll show the user the cached version.

Sending the short “Not Modified” message is a lot faster than needing to download the file again, especially for giant javascript or image files.

### ETag

Comparing versions with the modification time generally works, but could lead to problems. What if the server’s clock was originally wrong and then got fixed? What if daylight savings time comes early and the server isn’t updated? The caches could be inaccurate.

ETags to the rescue. An ETag is a unique identifier given to every file. It’s like a hash or fingerprint: every file gets a unique fingerprint, and if you change the file (even by one byte), the fingerprint changes as well.

Instead of sending back the modification time, the server can send back the ETag (fingerprint):

    ETag: ead145f File Contents (could be an image, HTML, CSS, Javascript...)

The ETag can be any string which uniquely identifies the file. The next time the browser needs logo.png, it can have a conversation like this:

1. Browser: Can I get logo.png, if nothing matches tag “ead145f”?
2. Server: (Checking fingerprint on logo.png)
3. Server: You’re in luck! The version here is “ead145f”. It was not modified.
4. Browser: Score! I’ll show the user my cached version.

### Expires

Caching a file and checking with the server is nice, except for one thing: we are still checking with the server. It’s like analyzing your milk every time you make cereal to see whether it’s safe to drink. Sure, it’s better than buying a new gallon each time, but it’s not exactly wonderful.

And how do we handle this milk situation? With an expiration date!

If we know when the milk (logo.png) expires, we keep using it until that date (and maybe a few days longer, if you’re a college student). As soon as it goes expires, we contact the server for a fresh copy, with a new expiration date. The header looks like this:

    Expires: Tue, 20 Mar 2007 04:00:25 GMT File Contents (could be an image, HTML, CSS, Javascript...)

In the meantime, we avoid even talking to the server if we’re in the expiration period:

There isn’t a conversation here; the browser has a monologue.

1. Browser: Self, is it before the expiration date of Mar 20, 2007? (Assume it is).
2. Browser: Verily, I will show the user the cached version.

### Max-Age

Expires is great, but it has to be computed for every date. The max-age header lets us say “This file expires 1 week from today”, which is simpler than setting an explicit date.

Max-Age is measured in seconds. Here’s a few quick second conversions:

* 1 day in seconds = 86400
* 1 week in seconds = 604800
* 1 month in seconds = 2629000
* 1 year in seconds = 31536000 (effectively infinite on internet time)

### Public & Private

Sometimes a server needs to control when certain resources are cached.

* ```Cache-control: public``` means the cached version can be saved by proxies and other intermediate servers, where everyone can see it.
* ```Cache-control: private``` means the file is different for different users (such as their personal homepage). The user’s private browser can cache it, but not public proxies.
* ```Cache-control: no-cache``` means the file should not be cached. This is useful for things like search results where the URL appears the same but the content may change.

* http://betterexplained.com/articles/how-to-optimize-your-site-with-http-caching/
* https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching
