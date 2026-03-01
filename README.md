# Web Search MCP Server

A Model Context Protocol (MCP) server that enables free web searching using Google search results, with no API keys required.

## Features

- Search the web using Google search results
- No API keys or authentication required
- Returns structured results with titles, URLs, and descriptions
- Configurable number of results per search

## Installation

1. Clone or download this repository
2. Install dependencies:

```bash
npm install
```

3. Build the server:

```bash
npm run build
```

4. Add the server to your MCP configuration:

For VSCode (Claude Dev Extension):

```json
{
  "mcpServers": {
    "web-search": {
      "command": "node",
      "args": ["/path/to/web-search/build/index.js"]
    }
  }
}
```

For Claude Desktop:

```json
{
  "mcpServers": {
    "web-search": {
      "command": "node",
      "args": ["/path/to/web-search/build/index.js"]
    }
  }
}
```

## Telemetry / Analytics (Optional)

You can enable analytics reporting to a third-party telemetry API to track MCP server usage events (such as search requests). This is disabled by default.

To enable telemetry, set the following environment variables before starting the server:

- `TELEMETRY_API_URL` – The endpoint to POST analytics events to (required to enable telemetry)
- `TELEMETRY_API_KEY` – (Optional) Bearer token for authentication with your analytics API

Example (Unix shell):

```bash
export TELEMETRY_API_URL="https://your-analytics.example.com/track"
export TELEMETRY_API_KEY="your_api_key_here" # optional
node build/index.js
```

Each time the search tool is used, an event is sent with the query and limit (no user-identifying information is sent by default).

---

## Usage

The server provides a single tool named `search` that accepts the following parameters:

```typescript
{
  "query": string,    // The search query
  "limit": number     // Optional: Number of results to return (default: 5, max: 10)
}
```

Example usage:

```typescript
use_mcp_tool({
  server_name: "web-search",
  tool_name: "search",
  arguments: {
    query: "your search query",
    limit: 3, // optional
  },
});
```

Example response:

```json
[
  {
    "title": "Example Search Result",
    "url": "https://example.com",
    "description": "Description of the search result..."
  }
]
```

## Limitations

Since this tool uses web scraping of Google search results, there are some important limitations to be aware of:

1. **Rate Limiting**: Google may temporarily block requests if too many searches are performed in a short time. To avoid this:
   - Keep searches to a reasonable frequency
   - Use the limit parameter judiciously
   - Consider implementing delays between searches if needed

2. **Result Accuracy**:
   - The tool relies on Google's HTML structure, which may change
   - Some results might be missing descriptions or other metadata
   - Complex search operators may not work as expected

3. **Legal Considerations**:
   - This tool is intended for personal use
   - Respect Google's terms of service
   - Consider implementing appropriate rate limiting for your use case

## Contributing

Feel free to submit issues and enhancement requests!
# testMCP
