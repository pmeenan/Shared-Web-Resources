# "public" web resources

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

