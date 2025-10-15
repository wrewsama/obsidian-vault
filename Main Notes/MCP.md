Tags:
- [[AI ML]]
---
## What is it
- Model Context Protocol
- Standardised protocol for LLMs to access external data sources and tools
- Built on JSON-RPC2.0

## Architecture
- Host app: The LLM system (e.g. Claude Code or Cursor)
- MCP client: connects host app to MCP servers, marshals and unmarshals to/from MCP protocol
- MCP server: exposes functionality to the MCP client

## Process
- LLM receives prompt and decides if it needs external info
- LLM selects tool/resource to request from MCP server
- client translates request using the MCP protocol and sends it to the MCP server
- MCP server handles request and returns a response
- client translates response back to natural language
- LLM integrates the natural language response into its context
- LLM responds to the user

---
## References
- https://www.descope.com/learn/post/mcp