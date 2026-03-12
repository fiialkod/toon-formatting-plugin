# toon-formatting-plugin

Claude Code plugin that automatically compresses structured JSON data using TOON format, saving 30-60% tokens on tabular data like API responses, database results, and lists.

## Prerequisites

Requires the [toon MCP server](https://www.npmjs.com/package/@fiialkod/toon-mcp-server). Add it to your Claude Code MCP settings:

```json
{
  "mcpServers": {
    "toon": {
      "command": "npx",
      "args": ["@fiialkod/toon-mcp-server"]
    }
  }
}
```

## Installation

```bash
claude plugin add fiialkod/toon-formatting-plugin
```

Or add this repo as a custom marketplace source:

```bash
claude marketplace add https://github.com/fiialkod/toon-formatting-plugin
```

## What it does

When invoked, the `toon-formatting` skill tells Claude to:

- Compress large JSON tool results through `toon_format_response` before reasoning
- Encode structured output with `toon_encode` when presenting data
- Automatically pick the most token-efficient format (TOON vs JSON compact)

## License

MIT
