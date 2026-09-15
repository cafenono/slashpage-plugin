# Slashpage MCP Plugin

**English** | [한국어](README.kr.md)

Connect Claude Code and Codex to the remote MCP server for [Slashpage](https://slashpage.com).

- MCP server: `https://mcp.slashpage.com/`
- Authentication: OAuth sign-in with your Slashpage account
- Marketplace / plugin name: `slashpage` / `slashpage`
- Version: `0.1.2`

The plugin installs the connection settings and a `slashpage-tools` skill for discovering and calling the tools exposed by your client. Sign in with your own account and authorize the site you want to connect. You do not need to put API keys or tokens in any files.

## Install in Claude Code

Run these commands in a Claude Code conversation:

```text
/plugin marketplace add cafenono/slashpage-plugin
/plugin install slashpage@slashpage
```

Restart Claude Code, then open `/mcp`, select the Slashpage server, and sign in. The plugin server may appear as `plugin:slashpage:slashpage`. In your browser, select the site to connect and approve access.

## Install in Codex

Run these commands in your terminal:

```bash
codex plugin marketplace add cafenono/slashpage-plugin
codex plugin add slashpage@slashpage
```

If prompted during installation, sign in. Start a new Codex session and check the connection status in `/mcp`. If authentication is still required, complete it through the plugin's connection or sign-in screen.

These commands use the syntax supported by Codex CLI `0.137.0`. If your CLI does not have the `plugin` subcommand, update to a version that supports it. Include the marketplace name when running `codex plugin add`, as in `slashpage@slashpage`.

You can use `codex mcp login slashpage` when the CLI can find an MCP configuration named `slashpage`. Check `codex mcp list` first. If the plugin server is not listed, use the plugin's authentication screen. Authentication for an existing manually registered `slashpage` server does not establish the plugin's authentication status.

## Connect without the plugin

If you prefer to register the server directly, use the commands below. We recommend choosing either the plugin or manual configuration to avoid registering the same server twice.

### Claude Code

Register the server in your terminal, then sign in through `/mcp` in Claude Code.

```bash
claude mcp add --transport http --scope user slashpage https://mcp.slashpage.com/
```

### Codex

```bash
codex mcp add slashpage --url https://mcp.slashpage.com/
codex mcp login slashpage
```

## Verify the connection and troubleshoot

- After signing in, ask the assistant to retrieve information from your connected site. Installing the plugin alone does not verify authentication or tool calls.
- If your site is missing or you receive a permission error, check the account you signed in with and its management permissions for that site.
- If you canceled authentication or your token has expired, sign in again through the client's connection screen.
- Use `https://mcp.slashpage.com/` as the server URL. Do not append `/mcp`.
- When reporting an issue, include your client version and the error message. Do not include access tokens or private site content.

### Unknown tool errors

If you see `Unknown tool: slashpage.get-domain-info`, ask the assistant to use the bundled `slashpage-tools` skill, rediscover the available tools, and call the exact name exposed by the client. Tool display names and callable names can differ. This error alone does not mean your login has expired.

Version `0.1.2` adds guidance for this discovery and recovery flow; it does not patch the client's internal tool routing. If the exact discovered tool still fails, refresh the client's tool session and report the client version and error.

To update an existing Codex installation, run:

```bash
codex plugin marketplace upgrade slashpage
codex plugin add slashpage@slashpage
```

Then start a new session so the updated skill is available.

## Verification status

JSON, plugin manifests, and skill metadata are validated. On September 15, 2026, site information and channel listing succeeded through Codex using an existing authorized account and the exact tools from its live inventory. The dotted tool name reproduced the reported lookup error. A fresh-user installation and OAuth flow, Claude Code runtime calls, and automatic skill selection have not been fully verified.

During a server check on September 15, 2026, OAuth metadata was accessible, but the unauthenticated response had a renamed authentication header and a duplicate `/` in its metadata URL. The effect on authentication in each client still needs verification.

## Repository structure

```text
.agents/plugins/marketplace.json         # Codex marketplace
.claude-plugin/marketplace.json          # Claude Code marketplace
plugins/slashpage/
  .codex-plugin/plugin.json              # Codex plugin
  .claude-plugin/plugin.json             # Claude Code plugin
  .mcp.json                             # Shared MCP connection settings
  skills/slashpage-tools/SKILL.md        # Tool discovery and error recovery
```

Both marketplaces point to the same plugin directory. The `source.path` in `.agents/plugins/marketplace.json` is relative to the repository root. This package uses the `.codex-plugin` and `.claude-plugin` compatibility formats.

### Maintenance

With Claude Code installed, run:

```bash
claude plugin validate --strict .
claude plugin validate --strict ./plugins/slashpage
```

Keep the names and versions in both plugin manifests in sync, and make sure the package includes the MCP configuration file. Verify releases in both clients using a separate test user environment without existing MCP settings or tokens.

## References

- [Claude Code marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)
- [Claude Code MCP](https://code.claude.com/docs/en/mcp)
- [OpenAI plugin packaging](https://developers.openai.com/plugins/build/plugins)
- [Codex MCP](https://developers.openai.com/codex/mcp)
