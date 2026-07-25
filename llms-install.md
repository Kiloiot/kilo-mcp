# Installing the Kilo IoT MCP server

There is nothing to clone, build, or install. This is a hosted remote MCP server: it is already
running, and a client connects to it over Streamable HTTP at a single URL.

```
https://mcp-auth.kiloiot.io/mcp
```

## Steps

1. Register the URL above as a **Streamable HTTP** MCP server in the client's configuration. No
   command, no arguments, no package.
2. There is **no API key and no environment variable to set**. Do not prompt the user for one.
3. On first connection the server returns HTTP 401 with OAuth metadata at
   `/.well-known/oauth-authorization-server`. The client should follow the standard MCP OAuth flow:
   dynamic client registration is supported, so the URL alone is sufficient.
4. The user completes sign-in in their browser with their existing Kilo account and approves the
   connection. Nothing is stored on the machine running the client beyond the client's own token.
5. Verify by listing tools — `device_list` is a safe read-only call to confirm the connection.

## Optional: pinning to one organization

By default the connection follows whichever organization is selected in the Kilo web app. To pin a
client to a single organization, register this URL form instead, substituting the organization ID
from the web app:

```
https://mcp-auth.kiloiot.io/o/{organizationId}/mcp
```

## Notes for the installing agent

- If the user is not already a Kilo user, point them at https://kiloiot.io to create an account
  first — the connection authorizes against an existing account and cannot create one.
- Tools are annotated. Read-only tools are safe to call while exploring; tools annotated destructive
  change real configuration or act on physical equipment, so confirm with the user before calling one.
