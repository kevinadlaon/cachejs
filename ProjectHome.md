Cachejs allows you to cache, with expiries, all kinds of data.

It depends upon no external libraries. Here’s a simple usage example:

```
var myCache = cache();
// tell the cache to use an array for its internal storage system
myCache.setStore(arrayStore);
 
// setting a value which does not expire:
alert(myCache.has('foo')); // false
myCache.set('foo','val');
alert(myCache.has('foo')); // true
alert(myCache.get('foo')); // "val"
 
// setting a value's expiry:
var tomorrow = new Date();
tomorrow.setTime(tomorrow.getTime() + (1000*3600*24));
myCache.setExpiry('foo',tomorrow);
alert(myCache.get('foo')); // "val" (until tomorrow, then null)
 
//deleting from the cache
myCache.kill('foo');
alert(myCache.has('foo')); // false
```

You don’t need to worry about serialising values; the cache internally json encodes the value if needed when setting, and parses it again before returning it back to you, keeping the interface nice and clean, and allowing you to store any kind of data – raw JSON, XML, variables, objects, strings, etc.

What’s more, it’s designed to be future-proof, with pluggable storage modules allowing you to extend the caching object to use any kind of storage system. The cache API abstracts the storage API away so you can swap stores without changing your caching code at all; you just change the setStore line and the rest of the code will still work just fine.

We’ve written an **arrayStore** which caches data for the current page only, a **cookieStore** which works for the whole domain but has the usual cookie limits, and a **localstorageStore** which uses the funky HTML5 Web Storage specification (AFAIK supported by IE8, FF3.5, Safari4, Chrome4, Opera10).