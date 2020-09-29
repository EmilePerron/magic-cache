

# MagicCache: a simple PHP caching library
MagicCache is a simple caching library that wraps Symfony's FilesystemAdapter in order to give you the simplest caching experience possible.

If you have a small project that requires caching, or have a large project with very simple caching needs, this library is for you.

The highlight of this library are the `Cache::magicGet()` and `Cache::magicSet()` method, which allow you to save or retrieve data from the cache without having to define unique cache keys yourself. More details on that below.

## Getting started
To get started, simply require the package via Composer:

```
composer require emileperron/magic-cache
```

Once that's done, you can start using the library in your project. Here's a very brief overview of the library's usage and functionalities:

```php
<?php

use Emileperron\MagicCache\Cache;

// Saving data to cache
// Definition: set($key, $value, $expiresAfter = 0)
Cache::set('my_cache_key', 'my value');
Cache::set('my_cache_key_2', 'my second value', 3600);
Cache::set('my_cache_key_3', ['obj' => 'value']);

// Getting data from cache
// Definition: get($key, $default = null)
Cache::get('my_cache_key');
Cache::get('my_non_existing_key', 'default/fallback value');

// Manually removing data from cache
// Definition: remove($key)
Cache::remove('my_cache_key_3');

// Pruning the cache (deletes all expired cache entries)
// Definition: prune()
Cache::prune();

// This library defines two magic methods: magicGet() and magicSet()
// These methods automatically generate a cache key based on the calling function's class, name and parameters.
// Here's an example:
function getRecordFromDatabase($id)
{
	if ($cachedValue = Cache::magicGet()) {
		return $cachedValue;
	}

	$value = fetchValueFromDatabase($id); // replace this with your own data fetching/processing code

	return Cache::magicSet($value);
}

// The definition of both methods goes as follows:
// magicGet($suffixes = [], $default = null)
// magicSet($value, $expiresAfter = 0, $suffixes = [])
```

## Magic `get()` and `set()` methods
This library defines two magic methods: `magicGet()` and `magicSet()`. These methods automatically generate a cache key based on the calling function's class, name and parameters.

This won't always be usable (ex.: if you are passing entire files or data stream in your function's parameters), but it works for a lot of simple use cases, which are much more common anyway.

Both methods accept a `$suffixes` parameter, which allow you to pass your own unique identifier(s) in the form of an array. Those suffixes will then be used instead of the calling function's parameters. The calling function's class and name will still be used in order to generate a unique cache key for that specific method.

## Contributing
Feel free to submit pull requests on [the GitHub repository](https://github.com/EmilePerron/magic-cache) if you want to add functionalities or suggest improvements to this library. I will look them over and merge them as soon as possible.

You can also submit issues if you run into problems but don't have time to implement a fix.

## Supporting
Finally, if you use the library and would like to support me, here are the ways you can do that:

- Saying thanks directly on Twitter: [@cunrakes](https://twitter.com/cunrakes)
- Giving this repository a star [on GitHub](https://github.com/EmilePerron/magic-cache)
- Taking a look at my other projects [on my website](https://www.emileperron.com)
- [Buying me a cup of tea](https://www.buymeacoffee.com/EmilePerron) ☕️
