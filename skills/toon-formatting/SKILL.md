---
name: toon-formatting
description: Use when receiving structured JSON data from tool results (API responses, database queries, email lists, calendar events) or when presenting structured data to the user. Triggers on any tool output containing arrays of objects or tabular data.
---

# TOON Formatting

## Overview

Automatically compress structured JSON data using the most token-efficient format. The server compares LEAN, TOON, and JSON compact, picking the best one automatically. LEAN typically saves ~49% tokens vs JSON for tabular data.

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
| `mcp__toon__lean_format_response` | Auto-pick best format (LEAN vs TOON vs JSON compact). Use this by default. |
| `mcp__toon__lean_encode` | Force LEAN encoding. Use when presenting data to user. |
| `mcp__toon__lean_decode` | Convert LEAN back to JSON. Use when user needs raw JSON. |
| `mcp__toon__lean_stats` | Compare token usage across formats. Use when user asks about efficiency. |

## Core Pattern

1. Receive structured JSON from a tool call
2. Extract relevant fields (drop noise like historyId, sizeEstimate, etc.)
3. Pass cleaned data through `lean_format_response` with a descriptive `label`
4. Use the formatted output internally for reasoning; present a human-friendly table to the user

```
Tool result (raw JSON) → Extract fields → lean_format_response → Reason over output → Present as markdown table
```

## Common Mistakes

- **Encoding the raw response verbatim** — Strip irrelevant fields first to maximize savings
- **Forgetting the label** — Always pass a `label` to `lean_format_response` for clarity
- **Using for tiny payloads** — Under ~5 small items, JSON compact may be smaller; `lean_format_response` handles this automatically
