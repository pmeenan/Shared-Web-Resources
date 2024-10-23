# Web-wide compression dictionary

Create a compression dictionary based on the current usage of web technologies that are commonly used across cache boundaries (frameworks, libraries, common strings). The dictionary can be updated periodically and advertised using the existing `Available-Dictionary` support when a more-specific dictionary is not available.

* Fixes the library version problem where not all versions of popular libraries need to be included, just the latest (or most common strings shared across versions).
* Should it be limited to shared technologies or include strings that might be common in first-party responses?
* How would the dictionary be built and size determined.

Risks:
* Gives preference to currently popular technologies
* Potential Copyright considerations
