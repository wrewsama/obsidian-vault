Tags:
- [[Computer Networking]]
---
Remote Procedure Calls

**Advantages over HTTP**
* Better performance due to binary data format
* Consistency across platforms by defining a IDL (Interface Definition Language, e.g. Thrift, protobuf)
* (possible) streaming support

**Process**
1. client constructs client stub using IDL during development
2. client calls method on the Thrift/proto stub, passing the request object
3. stub marshals the request data (to binary) and sends it (usually via HTTP)
4. server stub (also generated from the IDL during development) unmarshals the data and calls the server code (the handler procedure for this method, defined by the developer)
5. response gets marshalled, sent, and unmarshalled in a similar fashion

---
## References
- https://en.wikipedia.org/wiki/Remote_procedure_call