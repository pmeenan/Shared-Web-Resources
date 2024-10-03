# Shared-Web-Resources
Ideas for solving the sharing of web content across cache boundaries. 

# Privacy Risks

# Proposed solutions

## "public" web resources

Provide a way for pervasive public resources to use a shared cache partition. This involves an opt-in at request time to indicate that a resource would like to be considered as static and common but it is still up to the client to validate that (locally or centrally).

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

### .well-known compression dictionary

This is a variant of a ["public" web resource"](#public-web-resources) where it is limited to a single resource per-origin where the clients can use a centralized allow-list (bloom filter?) of vetted origins. It's mostly useful as a step towards allowing arbitrary resources from arbitrary origins while allowing for experimentation and validation.

In the broader case, it minimizes the risk of abuse of using multiple "public" resources that are really privately-keyed (based on URL).

* Allow each origin to advertise an origin-wide default compression dictionary at `/.well-known/compression-dictionary`. 
* Optionally advertise the existance of the dictionary in HTTP response headers (i.e. `Uses-Well-Known-Compression-Dictionary: true`).
* Clients (browsers) determine when/if to fetch the dictionary (same restrictions as public opt-in).
* Install the dictionary into an origin-keyed index (single-keyed)
* Fetches check for the single-keyed dictionary and advertise it using `Available-Dictionary` when a more-specific dictionary is not available.

## Web-wide compression dictionary

Create a compression dictionary based on the current usage of web technologies that are commonly used across cache boundaries (frameworks, libraries, common strings). The dictionary can be updated periodically and advertised using the existing `Available-Dictionary` support when a more-specific dictionary is not available.

* Fixes the library version problem where not all versions of popular libraries need to be included, just the latest (or most common strings shared across versions).
* Should it be limited to shared technologies or include strings that might be common in first-party responses?
* How would the dictionary be built and size determined.

Risks:
* Gives preference to currently popular technologies
* Potential Copyright considerations
