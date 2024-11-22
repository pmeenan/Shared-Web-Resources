# "public" web resources

Provide a way for pervasive public resources to use a shared cache partition. This involves an opt-in at request time to indicate that a resource would like to be considered as static and common but it is still up to the client to validate that (locally or centrally).

Required:

* Create a new `public` HTML attribute (and fetch property)
    * Request fails if response if not `cache-control = public, immutable` with a non-zero expiration
    * Implies `crossorigin=anonymous`
    * When active, use a single-keyed cache for public resources
* Require [Subresource Integrity](countermeasures.md#subresource-integrity-sri). This protects against abusing the payload as a tracking vector (see [Unique Payload](privacy-risks.md#unique-payload)).


Optionally:

* Only activate cache entry after N fetches with the same response hash (where N may include a random factor). This reduces the risk of seeding a unique ID with a random fetch, particularly in the dictionary case where the hash of the resource will be used without having to know the original dictionary URL.
* Ignore `public` if request is same-site as the document. This puts the request back into a partitioned cache so that first-party requests are cached separately from requests in a third-party context.
* Restrict it to specific resource types (maybe initially only compression dictionaries initiated by a link tag).
* Restrict it to an allowlist of trusted origins where resources are widely used in a 3rd-party context.
* Randomly ignore `public` attribute for some percent of fetches.
* Fetch from trusted proxy (privacy-preserving proxy?).
