Tags:
- [[Computer Networking]]
---
Remote Procedure Calls

**Advantages over HTTP**
* Better performance due to binary data format
* Consistency across platforms by defining a IDL
* (possible) streaming support

**Process**
1. client constructs req using generated code from IDL
2. client calls method on the Thrift/proto stub, passing the req object
3. stub serialises the req data (to binary) and sends it (usually via HTTP)

---
## References
- 