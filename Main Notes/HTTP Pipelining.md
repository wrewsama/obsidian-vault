Tags:
- [[Computer Networking]]
---
## what
- HTTP/1.1 feature for sending multiple requests without waiting for corresponding response
- i.e. can go `send(req1), send(req2), recv(resp1), recv(resp2)` instead of `send(req1), recv(resp1), send(req2), recv(resp2)`

## benefits
- improves performance of multiple requests by "batching" the waiting time instead of waiting after each request

## issues
- **NOT SUPPORTED BY MOST BROWSERS**
- responses must still be in order, hence if a slow request can affect all those after it - **Head of Line Blocking**

---
## References
- https://www.ituonline.com/tech-definitions/what-is-http-pipelining/
- https://brianbondy.com/blog/119/what-you-should-know-about-http-pipelining