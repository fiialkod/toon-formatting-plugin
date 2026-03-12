---
name: toon-formatting
description: Use when receiving structured JSON data from tool results (API responses, database queries, email lists, calendar events) or when presenting structured data to the user. Triggers on any tool output containing arrays of objects or tabular data.
---

# TOON Formatting

## Overview

Automatically compress structured JSON data using TOON format for token efficiency. TOON typically saves 30-60% tokens vs JSON for tabular data (arrays of uniform objects).

## Prerequisites

Requires the `toon` MCP server. Add to your Claude Code MCP settings.

## When to Use

- Tool results return arrays of objects (emails, events, records, API responses)
- Presenting structured data to the user
- Any large JSON payload before reasoning over it

**Don't use for:**
- Small payloads (< 5 items with few fields)
- Deeply nested non-tabular configs
- Data you need to modify and pass back as JSON

## Quick Reference

| Tool | Purpose |
|------|---------|
| `mcp__toon__toon_format_response` | Auto-pick best format (TOON vs JSON compact). Use this by default. |
| `mcp__toon__toon_encode` | Force TOON encoding. Use when presenting data to user. |
| `mcp__toon__toon_decode` | Convert TOON back to JSON. Use when user needs raw JSON. |
| `mcp__toon__toon_stats` | Compare token usage across formats. Use when user asks about efficiency. |

## Core Pattern

1. Receive structured JSON from a tool call
2. Extract relevant fields (drop noise like historyId, sizeEstimate, etc.)
3. Pass cleaned data through `toon_format_response` with a descriptive `label`
4. Use the TOON output internally for reasoning; present a human-friendly table to the user

```
Tool result (raw JSON) → Extract fields → toon_format_response → Reason over TOON → Present as markdown table
```

## Common Mistakes

- **Encoding the raw response verbatim** — Strip irrelevant fields first to maximize savings
- **Forgetting the label** — Always pass a `label` to `toon_format_response` for clarity
- **Using toon for tiny payloads** — Under ~5 small items, JSON compact may be smaller; `toon_format_response` handles this automatically
