# Serpapi
Serpapi
node_modules
.env
.DS_Store


# SerpAPI MCP Server for Perplexity

This is a small remote MCP server that wraps SerpAPI so you can register it as a custom connector in Perplexity Computer.

## Why this approach
Perplexity supports custom remote connectors using an MCP server URL, with authentication such as API key or open auth. SerpAPI itself is not a built-in native Perplexity toggle, so the usual pattern is to expose SerpAPI through an MCP-compatible server.

## Files
- `server.js` â€” Express-based remote MCP server
- `.env.example` â€” required environment variables
- `package.json` â€” Node package manifest

## Local run
```bash
npm install
export SERPAPI_KEY='your_serpapi_key'
export MCP_AUTH_TOKEN='choose-a-random-secret'
npm start
```

Health check:
```bash
curl http://localhost:3000/health
```

## Deploy
Deploy to Railway, Render, Fly.io, Cloud Run, or any HTTPS host that can run Node.js.

Required env vars:
- `SERPAPI_KEY` â€” your SerpAPI key
- `MCP_AUTH_TOKEN` â€” shared bearer token for your MCP endpoint
- `PORT` â€” optional, platform usually injects this

## Perplexity setup
In Perplexity Computer, add a custom remote connector and use your deployed MCP URL, for example:
- MCP URL: `https://your-app.example.com/mcp`
- Auth header: `Authorization: Bearer YOUR_MCP_AUTH_TOKEN`

## Example request
```bash
curl -X POST http://localhost:3000/mcp \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer your-secret' \
  -d '{
    "jsonrpc":"2.0",
    "id":1,
    "method":"tools/call",
    "params":{
      "name":"serp_search",
      "arguments":{"q":"best react admin templates","num":3}
    }
  }'
```

## Notes
- Do not hardcode secrets in source control.
- Rotate both the SerpAPI key and MCP auth token if they were ever pasted into chat, logs, or screenshots.
- This example returns top organic Google results from SerpAPI.