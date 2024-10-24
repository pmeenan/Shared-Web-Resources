# Web-wide compression dictionary

Create a compression dictionary based on the current usage of web technologies that are commonly used across cache boundaries (frameworks, libraries, common strings). The dictionary can be updated periodically and advertised using the existing `Available-Dictionary` support when a more-specific dictionary is not available.

* Fixes the library version problem where not all versions of popular libraries need to be included, just the latest (or most common strings shared across versions).
* Should it be limited to shared technologies or include strings that might be common in first-party responses?
* How would the dictionary be built and size determined.

Risks:
* Gives preference to currently popular technologies
* Potential Copyright considerations

## Creating the dictionary

Initial exploration will use the [HTTP Archive](https://httparchive.org/) which captures response bodies for text resources (HTML, script, css).

1. Javascript responses and script extracted from HTML responses will be parsed with [Esprima](https://esprima.org/).
1. The body of each function and each top-level comment block will be hashed.
1. The hash, resource URL and raw text of the function/comment will be stored for each separate hash (likely in BigQuery).
1. The blobs will be grouped by hash and the number of unique URLs that they were served from will be counted (to eliminate popular resources served from the same URL).
1. That hashes will be sorted by count of unique URLs.
1. Going through the hash blobs from most common to least until a target dictionary size is reached:
    1. Ignore blobs smaller than 10 bytes.
    1. Compress the blob using each previous blob as a dictionary with brotli. If the resulting size is less than half of the size using brotli alone then ignore the blob (too similar to one already in use).
    1. Add the blob to the aggregate dictionary.
