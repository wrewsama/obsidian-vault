Tags:
- [[Python]]
- [[Concurrency]]
---
## what
- async-capable Python libraries to send http requests

## how
##### httpx
```python
async with httpx.AsyncClient() as client:
    response = await client.get("https://example.com")
    print(response.json())
```

##### aiohttp
```python
async with aiohttp.ClientSession() as session: 
    async with session.get("https://example.com") as response: 
        print(await response.text()) # note: no built-in json decoding
```

## differences

| HTTPX           | AIOHTTP              |
| --------------- | -------------------- |
| slower          | faster               |
| supports HTTP2  | no support for HTTP2 |
| sync-compatible | async-compatible     |


---
## References
- https://github.com/oxylabs/httpx-vs-requests-vs-aiohttp