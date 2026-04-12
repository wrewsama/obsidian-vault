Tags:
- [[Docker]]
---
## images
- build image from Dockerfile in current working directory: `docker build -t <image_name> .`
- list images: `docker images`
- remove image: `docker rmi <image_name>`

## containers
- build and run container from a given image: `docker run --name <container_name> <image_name>`
    - use `-p <host_port>:<container_port>` to port-forward
- start/stop: `docker start|stop <container_name>`
- list containers: `docker ps --all`
- remove container (only if it's stopped): `docker rm <container_name>`
---
## References
- https://docs.docker.com/get-started/docker_cheatsheet.pdf