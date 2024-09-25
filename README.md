# Shared-Web-Resources
Ideas for solving the sharing of web content across cache boundaries. 

# Privacy Risks

# Proposed solutions

## "public" web resources

Provide a way for pervasive public resources to use a shared cache partition.

Required:

* Create a new `public` HTML attribute (and fetch property)
    * Request fails if response if not `cache-control = public, immutable` with a non-zero expiration
    * Implies `crossorigin=anonymous`
* When active, use a single-keyed cache for public resources

Optionally:

* Randomly ignore `public` attribute for some percent of fetches
* Fetch from trusted proxy (privacy-preserving proxy?)
* Only activate cache entry after N fetches with the same response hash (where N may include a random factor)
* Ignore `public` if request is same-site as the document

## Web-wide compression dictionary

Create a compression dictionary based on the current usage of web technologies that are commonly used across cache boundaries (frameworks, libraries, common strings). The dictionary can be updated periodically and advertised using the existing `Available-Dictionary` support when a more-specific dictionary is not available.

Risks:
* Gives preference to currently popular technologies
* Potential Copyright considerations

## .well-known compression dictionary

* Allow each origin to advertise an origin-wide default compression dictionary at `/.well-known/compression-dictionary`. 
* Advertise the existance of the dictionary in HTTP response headers (i.e. `Uses-Well-Known-Compression-Dictionary: true`).
* Clients (browsers) determine when/if to fetch the dictionary (same restrictions as public opt-in).
* Install the dictionary into an origin-keyed index (single-keyed)
* Fetches check for the single-keyed dictionary and advertise it using `Available-Dictionary` when a more-specific dictionary is not available.