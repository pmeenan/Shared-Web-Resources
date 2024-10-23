# .well-known compression dictionary

This is a variant of a ["public" web resource"](public-resources.md) where it is limited to a single resource per-origin where the clients can use a centralized allow-list (bloom filter?) of vetted origins. It's mostly useful as a step towards allowing arbitrary resources from arbitrary origins while allowing for experimentation and validation.

In the broader case, it minimizes the risk of abuse of using multiple "public" resources that are really privately-keyed (based on URL).

* Allow each origin to advertise an origin-wide default compression dictionary at `/.well-known/compression-dictionary`. 
* Optionally advertise the existance of the dictionary in HTTP response headers (i.e. `Uses-Well-Known-Compression-Dictionary: true`).
* Clients (browsers) determine when/if to fetch the dictionary (same restrictions as public opt-in).
* Install the dictionary into an origin-keyed index (single-keyed)
* Fetches check for the single-keyed dictionary and advertise it using `Available-Dictionary` when a more-specific dictionary is not available.
