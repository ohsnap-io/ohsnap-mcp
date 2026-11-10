# OhSnap MCP server

Website uptime monitoring for AI agents. Ask Claude, Cursor or VS Code whether
a site is down, start monitoring it, read its outage history, or send a test
alert, all through [OhSnap](https://ohsnap.io).

This is a hosted server. There is nothing to install or run: point your MCP
client at the endpoint below.

- **Endpoint:** `https://mcp.ohsnap.io/mcp` (Streamable HTTP)
- **Auth:** optional header `Authorization: Bearer ohsnap_mcp_...` (or `X-API-Key: ohsnap_mcp_...`)
- **Get a key:** [app.ohsnap.io/connect/mcp](https://app.ohsnap.io/connect/mcp) (free account, no credit card)
- **Docs:** [ohsnap.io/mcp](https://ohsnap.io/mcp/)

`check_website` works without a key, so you can try it before signing up.

## Tools

| Tool | Needs key | What it does |
|------|-----------|--------------|
| `check_website` | No | Checks right now whether a site is up: status code, response time, and the reason it fails (DNS, SSL, timeout, HTTP error, Cloudflare edge error). |
| `whoami` | Yes | Shows the connected account, its plan and its limits. |
| `list_monitors` | Yes | Lists monitored sites and which are down at this moment. |
| `add_monitor` | Yes | Starts monitoring a site and can set up email alerts in the same call. |
| `get_incidents` | Yes | Returns a site's outage history; on Pro each outage includes an AI diagnosis. |
| `send_test_alert` | Yes | Sends a labelled test alert through every linked channel (email, SMS, WhatsApp, Slack, Telegram). |

Keys follow the same plan limits as the dashboard. Free covers 5 sites checked
every 5 minutes, with email and SMS alerts. Pro is $6.99/month for up to 100
sites checked every 30 seconds. Agency is $14.99/month for any number of sites.

## Setup

**Claude Code**

```bash
claude mcp add --transport http ohsnap https://mcp.ohsnap.io/mcp --header "Authorization: Bearer ohsnap_mcp_YOUR_KEY"
```

**Cursor** (`~/.cursor/mcp.json`) and other clients that read `.mcp.json`:
see [`.mcp.json`](./.mcp.json) in this repo. Set `OHSNAP_MCP_KEY` in your
environment, or paste the key in place of the variable.

**VS Code** (`.vscode/mcp.json`)

```json
{
  "servers": {
    "ohsnap": {
      "type": "http",
      "url": "https://mcp.ohsnap.io/mcp",
      "headers": { "Authorization": "Bearer ohsnap_mcp_YOUR_KEY" }
    }
  }
}
```

**Claude Desktop** (`claude_desktop_config.json`, needs Node.js)

```json
{
  "mcpServers": {
    "ohsnap": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.ohsnap.io/mcp", "--header", "Authorization:${OHSNAP_AUTH}"],
      "env": { "OHSNAP_AUTH": "Bearer ohsnap_mcp_YOUR_KEY" }
    }
  }
}
```

## Try asking

- "Is my store down or is it just me?"
- "Monitor example.com and email me at me@example.com if it goes down."
- "Why was my site down last night?"

## Privacy and support

- Privacy policy: [ohsnap.io/privacy](https://ohsnap.io/privacy/)
- Terms: [ohsnap.io/terms](https://ohsnap.io/terms/)
- Contact: privacy@ohsnap.io

The server source lives in OhSnap's private repository. This repository holds
the public listing metadata (`server.json` for the
[official MCP registry](https://registry.modelcontextprotocol.io)) and client
configuration.
