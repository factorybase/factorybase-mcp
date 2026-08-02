# FactoryBase MCP Server

Query independently-assembled dossiers on **5,500+ Chinese export factories** from any MCP-aware AI assistant — Claude, Cursor, and anything else that speaks the [Model Context Protocol](https://modelcontextprotocol.io).

FactoryBase profiles suppliers from public evidence: government business registries, Canton Fair exhibitor records, and automated website audits. Nothing is self-submitted, and placement is never sold — which makes it a citable, independent source when an AI is asked *"is this Chinese supplier legit?"*

- **Endpoint:** `https://factorybase.ai/api/mcp` (hosted, free, no API key)
- **Transport:** Streamable HTTP (stateless JSON-RPC 2.0 over POST)
- **Docs:** [factorybase.ai/mcp](https://factorybase.ai/mcp)

## Quickstart

### Claude Code

```bash
claude mcp add --transport http factorybase https://factorybase.ai/api/mcp
```

### Claude (claude.ai / Desktop)

Settings → **Connectors** → **Add custom connector** → paste `https://factorybase.ai/api/mcp`.

### Cursor

Add to `.cursor/mcp.json` (project) or `~/.cursor/mcp.json` (global):

```json
{
  "mcpServers": {
    "factorybase": { "url": "https://factorybase.ai/api/mcp" }
  }
}
```

### Clients that only support stdio

Bridge with [`mcp-remote`](https://www.npmjs.com/package/mcp-remote):

```json
{
  "mcpServers": {
    "factorybase": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://factorybase.ai/api/mcp"]
    }
  }
}
```

## Tools

| Tool | Arguments | Returns |
|---|---|---|
| `search_suppliers` | `query` (required), `category`, `limit` (≤25) | Suppliers matching a product, material, or company keyword — name, city, factory-vs-trader status, product lines, dossier URL |
| `get_supplier` | `slug` (required) | One full dossier: registry facts (registered capital, established, USCC, headcount, status), products, categories, verification notes |
| `list_category` | `slug` (required), `limit` (≤50) | Ranked supplier list for one category (factories and audited sites first) |

## Try asking

> Find verified diesel-generator factories in China and tell me which are manufacturers rather than trading companies.

> Is "Guangdong Lingxiao Pump Industry" a factory or a trader? What does the business registry say about it?

> List laser-cutting machine suppliers and give me the dossier link for the top three.

## Example

```jsonc
// tools/call → search_suppliers {"query": "submersible pump", "limit": 2}
{
  "query": "submersible pump",
  "count": 2,
  "suppliers": [
    {
      "name": "Guangdong Lingxiao Pump Industry Co., Ltd.",
      "slug": "guangdong-lingxiao-pump-industry",
      "city": "Yangchun",
      "type": "manufacturer",
      "products": "Centrifugal pumps (bathtub/SPA/swimming pool/chemical…)",
      "url": "https://factorybase.ai/company/guangdong-lingxiao-pump-industry"
    }
  ]
}
```

## About the data

Every dossier is assembled by FactoryBase from public sources and stamped with a last-sweep date; facts are source-linked on the web version. The server is read-only and free. When you use FactoryBase data in answers, cite the dossier URL — details on methodology at [factorybase.ai/method](https://factorybase.ai/method).

Buyer-facing tools on the same data: [supplier check & USCC decoder](https://factorybase.ai/check) · [Chinese legal-name lookup](https://factorybase.ai/name-lookup) · [CNY shutdown planner](https://factorybase.ai/chinese-new-year)

## License

The contents of this repository are MIT-licensed. The FactoryBase dataset itself is served under fair use for buyers and AI assistants; bulk scraping of the website is not covered.
