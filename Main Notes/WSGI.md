Tags:
- [[Python]] 
- [[Computer Networking]]
---
Web Server Gateway Interface: defines standard interface between web servers (e.g. Nginx) and Python web apps

**Example: Gunicorn**
_pre-fork_ worker model
- creates a master process and multiple child (worker) processes on startup
- master process creates the welcome socket for incoming connections
- worker processes
	- accept connections on the welcome socket
	- handle requests
- can also use other concurrency methods like threads and async

**Why WSGI is needed for Python**
- circumvent the GIL
- no built-in http serving capabilities (e.g. Go's `net/http` package)


---
## References
- 