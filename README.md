# Hiveround — MCP server for live venture-capital raises

[Hiveround](https://hiveround.com) is a marketplace for live startup
fundraises, built MCP-first. Founders publish a markdown pitch
(`pitch.md`); investors point any MCP-aware agent — Claude, Cursor, a
custom loop — at the marketplace, and the agent scouts raises, reads
pitches, requests introductions, and maintains a pipeline.

This repository hosts the public metadata for the hosted server. There
is nothing to install or run from here — the server is remote.

- **Endpoint:** `https://hiveround.com/api/mcp` (Streamable HTTP, JSON-RPC 2.0)
- **Documentation:** https://hiveround.com/mcp
- **Auth:** anonymous for read tools; `Bearer` API key (`hr_sk_…`) for
  write tools — create a key at https://hiveround.com/mcp

## Try it (no API key needed)

```bash
curl -X POST https://hiveround.com/api/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call",
       "params":{"name":"list_projects","arguments":{"limit":3}}}'
```

## Connect a client

**Claude Code**

```bash
claude mcp add --transport http hiveround https://hiveround.com/api/mcp
```

**Cursor / generic MCP client config**

```json
{
  "mcpServers": {
    "hiveround": {
      "type": "streamable-http",
      "url": "https://hiveround.com/api/mcp"
    }
  }
}
```

For write tools, add an `Authorization: Bearer hr_sk_…` header with a
key from https://hiveround.com/mcp.

## Tools (14)

| Tool | Auth | What it does |
|------|------|--------------|
| `list_projects` | anonymous | Newest open raises on the marketplace |
| `search_projects` | anonymous | Keyword search with stage and max-raise filters |
| `get_project` | anonymous | Full listing by slug, including the founder's pitch |
| `request_intro` | API key | Request an intro to a founder |
| `watch_project` | API key | Add a project to your pipeline |
| `update_watch` | API key | Move pipeline stage / append a note |
| `list_watches` | API key | Return your pipeline |
| `list_intros` | API key | List intro threads you're a party to |
| `read_intro_thread` | API key | Read messages in a thread |
| `send_intro_message` | API key | Follow up in an existing thread |
| `submit_commitment` | API key | Record an intent-to-fund (soft-circled / term sheet) |
| `acknowledge_commitment` | API key | Acknowledge a commitment; both sides acked → committed |
| `update_commitment_status` | API key | Withdraw, mark funded, etc. (role-gated) |
| `list_commitments` | API key | Commitments where you're investor or founder |

## Discovery surfaces

The same data feeds every surface an agent might arrive from:

- Server card: https://hiveround.com/.well-known/mcp/server-card.json
- Skills index: https://hiveround.com/.well-known/skills/index.json
- Claude Code skill (one command): `hermes skills install https://hiveround.com/hermes-skill`
- Plain-text site index: https://hiveround.com/llms.txt · https://hiveround.com/llms-full.txt

## Registry

Published to the official MCP Registry as
[`io.github.stevemilton/marketplace`](https://registry.modelcontextprotocol.io/v0.1/servers?search=io.github.stevemilton/marketplace)
— see [`server.json`](./server.json).

## Contact

hello@hiveround.com — or, fittingly, call `request_intro` on the
`hiveround` slug.
