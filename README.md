# Kilo IoT Platform MCP Server

[![smithery badge](https://smithery.ai/badge/kiloiot/kilo-iot-platform)](https://smithery.ai/servers/kiloiot/kilo-iot-platform)

Connect an AI client to a live IoT deployment and let it work the real thing: read devices, provision
hardware, deploy automation rules, work the alarm queue, and send commands to physical equipment.

The server is hosted by [Kilo IoT](https://kiloiot.io) — there is nothing to install and no API key to
mint. You sign in with your usual Kilo account in the browser, and the connection carries your own
permissions.

```
https://mcp-auth.kiloiot.io/mcp
```

This repository holds the public metadata for that server: its registry manifest, its icon, and the
setup instructions below. The service itself is closed-source.

## What is the Model Context Protocol?

[MCP](https://modelcontextprotocol.io) is an open standard that lets an AI client discover and call
tools on a remote server. Because it is a standard rather than a per-vendor integration, any MCP
client can connect — Claude Code, Claude Desktop, ChatGPT, Codex, Cursor and others — over Streamable
HTTP.

## What can an AI agent do with the Kilo MCP server?

| Area | What the connected client can do |
|---|---|
| **Devices** | List and inspect devices, read telemetry and diagnostics, provision LoRaWAN devices, MQTT devices and GPS trackers, manage device profiles |
| **Commands** | List the commands a device supports, send one, and check how it completed — this is where the assistant acts on physical equipment |
| **Connectors** | Review connectors, create connections for a device to report through |
| **Rules** | Review rules, generate and deploy automation, simulate logic against real sensor values before it reaches production, stop a running rule, read execution history |
| **Alarms** | List and resolve alarms, summarize activity for a shift or a site, manage alarm rules and notification channels, send a test notification |
| **Dashboards** | List dashboards and query the data behind any widget |
| **Sensors** | Inspect and change sensor mappings, manage sensor templates |
| **Organization** | Read organization details, list teams, invite users, assign roles |

Every tool is annotated, so a client knows which ones only read and which ones change something.
Read-only tools run without interrupting you; anything destructive asks first.

## How do I connect Claude Code to Kilo?

```bash
claude mcp add --transport http kilo https://mcp-auth.kiloiot.io/mcp
```

Then run `/mcp` inside Claude Code, pick `kilo`, and authorize in the browser window that opens. Run
`/mcp` again to confirm it reports **connected**.

## How do I connect Claude Desktop, ChatGPT, Cursor or Codex?

Clients with a connector dialog — Claude Desktop, ChatGPT, Cursor — take the URL directly: open
**Settings → Connectors** (or Integrations / MCP servers), choose **Add custom connector**, paste
`https://mcp-auth.kiloiot.io/mcp`, and authorize in the browser.

Clients configured from a file or a terminal, such as Codex, register the same URL as a **Streamable
HTTP** server. Your client's own documentation says where its MCP settings live; nothing about this
endpoint is client-specific.

## Which organization does the connection see?

By default, whichever organization is currently selected in the Kilo web app. Switch organizations
there and reconnect the client to follow it.

To pin a client to one organization regardless of what is selected in the web app:

```
https://mcp-auth.kiloiot.io/o/{organizationId}/mcp
```

The organization ID comes from the web app. Pinning is worth doing for a workstation that must always
operate against one production organization — and it cannot be moved by a stray click in the
organization switcher. Requests for an organization you are not a member of are refused.

## Is it safe to give an AI agent access to my IoT devices?

- **You sign in, not a service account.** Authorization happens in your browser against your normal
  Kilo account. No key is generated, copied or stored for the connection.
- **Your permissions are the ceiling.** The client can only do what your account can do. If you cannot
  deploy a rule or invite a user, neither can it.
- **Organization boundaries hold**, on the default endpoint and the pinned one alike.
- **Actions keep their normal records.** Rule changes and executions appear in rule history, device
  commands in command execution history, and access changes in the Audit Trail.

Treat an authorized client like a signed-in session: it belongs on machines you control.

## MCP server vs REST API — which should I use?

The [Public REST API](https://docs.kiloiot.io/kilo-iot-server/api/) is for programs you write: a sync
job, a reporting pipeline, a bridge to another system. It authenticates with a scoped API key that
runs unattended. MCP is for an AI client acting on your behalf, authorized by your own sign-in. If you
are writing code, use REST. If you are working with an assistant, use MCP.

## Links

- Platform: [kiloiot.io](https://kiloiot.io) · [Kilo IoT Platform](https://kiloiot.io/iot-server/)
- Documentation: [docs.kiloiot.io — MCP Server](https://docs.kiloiot.io/kilo-iot-server/api/mcp-server)
- Support: [kiloiot.io/contact](https://kiloiot.io/contact/)
- Privacy policy: [kiloiot.io/privacy-policy](https://kiloiot.io/privacy-policy/) · Terms: [kiloiot.io/terms-of-service](https://kiloiot.io/terms-of-service/)

Start free at [kiloiot.io](https://kiloiot.io).
