Tags:
- [[Computer Networking]]
---
Cross Origin Resource Sharing

Lets website on 1 domain access resources from a website on another domain.

- Browsers add origin header whenever they make a request
- If the request goes to a different url, it's a Cross Origin Request
- Server adds the Access-Control-Allow-Origin header to the response

- defines who can request it
- can be a wildcard *

- The values of the req's Origin and the res's ACAO headers must match

preflight:

- for PUT, DELETE, and other methods without a standard header
- client will send preflight req with the origin header
- server responds with ACAO and Access-Control-Allow-Methods

- ACAM: allowed methods e.g. PUT, DELETE

---
## References
- 