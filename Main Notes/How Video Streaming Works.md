Tags:
- [[Computer Networking]]
---
**Server**
1. Divides video file into chunks (few s long)
2. each chunk is stored and encoded @ different bitrates
3. A manifest file containing URLs for different chunks is created
4. segments are stored on a server (usually in a CDN)

**Client**
1. Client requests for manifest file from the server
2. Based on the available bandwidth, use the manifest to request for the chunk with the highest sustainable bitrate
3. The CDN's DNS server gives the client the IP address of the closest CDN server
4. Client gets the chunk and buffers it for playback

---
## References
- 