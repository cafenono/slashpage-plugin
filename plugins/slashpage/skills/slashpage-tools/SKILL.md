---
name: slashpage-tools
description: Use Slashpage MCP tools to read or manage a connected Slashpage site, channels, posts, and comments. Use when the user asks about their Slashpage content, checks an MCP connection, or encounters an Unknown tool error while using Slashpage.
---

# Slashpage tools

## Discover and call tools

1. Discover the Slashpage tools available in the current client and inspect the relevant input schema. Use the client's tool search, tool inventory, or MCP tools/list facility. With code-mode tools, search the provided tool metadata for Slashpage and use the exact callable exposed there.
2. Copy the exact callable name from that inventory. Treat the server name, display namespace, raw MCP name, and code-mode helper as different identifiers; do not concatenate or translate them yourself.
3. For a connection check, retrieve the connected site's information first, then list its channels. Both are read-only. Summarize the returned results without exposing credentials or unrelated private content.
4. Use the tools and arguments actually provided by the client for subsequent tasks. Keep writes within the user's request; do not create or edit content just to verify connectivity.

## Recover from Unknown tool

- Treat `Unknown tool` as a tool lookup/routing failure, not evidence of an expired login.
- Refresh discovery and inspect the exact names at the layer being called before retrying. If code-mode helpers are available, use the helper returned by the live inventory rather than a display namespace.
- For example, a Codex installation exposed `slashpage_get-domain-info` in its `codex_apps` MCP inventory and `mcp__codex_apps__slashpage_get_domain_info` as a code-mode helper. Sending `slashpage.get-domain-info` to that MCP relay failed. These are examples from one client, not names to hard-code: use the names exposed in the current session. A direct connection may expose the raw server tool `get-domain-info` instead.
- Retry the read-only call once with the discovered callable. If the exact discovered name still fails, report a client routing problem and suggest refreshing the client's tool session. Do not repeatedly retry, reinstall, register a duplicate server, or change OAuth settings to repair a name lookup failure.
- Request sign-in only when the client or server actually reports missing authentication or an authentication failure. For permission failures, check access to the selected site.
