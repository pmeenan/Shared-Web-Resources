# .well-known compression dictionary

This is a variant of a ["public" web resource"](public-resources.md) where it is limited to a single resource per-origin, limited to [compression dictionaries](https://datatracker.ietf.org/doc/draft-ietf-httpbis-compression-dictionary/) fetched using a link tag/header. It's mostly useful as a step towards allowing arbitrary resources from arbitrary origins while allowing for experimentation and validation.

If the following conditions are met then the resource uses a cache and dictionary store keyed on only the origin for the request:

* The URL is `/.well-known/compression-dictionary`.
* The fetch is initiated using a `compression-dictionary` link relation.
* The link relation uses [Subresource Integrity](countermeasures.md#subresource-integrity-sri). This protects against abusing the dictionary payload or hash as a tracking vector (see [Unique Payload](privacy-risks.md#unique-payload)).
* The resource URL and top-level document URL are NOT [same site](https://html.spec.whatwg.org/#same-site). This explicitly separates the caches for first-party and third-party use cases.
* The link relation uses [crossorigin=anonymous](https://html.spec.whatwg.org/#attr-crossorigin-anonymous-keyword).

The dictionary matching at fetch time will also be updated to check the shared cache key for requests where the resource URL and top-level document URL are not `same-site`.

## Risks

There is still the potential for abuse using [fingerprinting](privacy-risks.md#fingerprinting) with using dictionaries from multiple unique origins (`1.uid.example.com`, `2.uid.example.com`, ...). This can be mitigated in several ways:

1. Limit the number of fetches for .well-known compression dictionaries on a given top-level document by eTLD (i.e. only allow 2 dictionaries per eTLD on a given page).
1. Use an allow-list of trusted origins.
