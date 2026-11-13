# Sabre Marketplace for Agent Plugins

A plugin marketplace from Sabre that connects AI agents and coding assistants to Sabre travel services.

The marketplace works with Cursor today. Its plugins are built on open standards, mainly the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/), so you can use them from any MCP-capable client or agent framework without being tied to one tool.

## Available plugins

| Plugin | Description | Version |
| --- | --- | --- |
| [Sabre Travel Connector](plugins/sabre-travel-connector/) | Shop flights and hotels side by side, see what's included and restricted, then book and manage the whole trip in one place. | 1.0.0 |

See each plugin's README for setup, credentials and the list of tools.

## Getting started

### Cursor

1. Add this repository as a plugin marketplace in Cursor. Team admins can add it once for the whole team.
2. Install a plugin from **Settings → Plugins**.
3. Enter the credentials the plugin asks for, then complete the sign-in.

### Other MCP clients

Each plugin includes a standard `mcp.json` that points to a hosted MCP server over HTTP. To use a plugin from another client (for example VS Code, Claude Code, or your own agent), add that server to the client's MCP configuration and follow the plugin's README for authentication.

## Open standards

- **Model Context Protocol:** the plugins expose their features as MCP servers, so tool names and schemas come from the server, not from a client-specific integration.
- **OAuth 2.0 / OpenID Connect:** users sign in with their own account, and tool calls run with that user's permissions. Credentials are entered at install time and are never stored in the repository.
- **Plain JSON manifests:** the marketplace and plugin metadata are simple JSON files that are easy to read, check, and adapt to other plugin systems.

## Repository layout

```
.
├── .cursor-plugin/
│   └── marketplace.json        # Marketplace manifest (lists all plugins)
└── plugins/
    └── <plugin-name>/
        ├── .cursor-plugin/
        │   └── plugin.json     # Plugin metadata, variables, MCP reference
        ├── mcp.json            # Standard MCP server configuration
        ├── assets/             # Logo and other assets
        ├── LICENSE.md          # Plugin license
        └── README.md           # Plugin documentation
```

## Access and credentials

Some plugins need credentials that Sabre issues during partner onboarding. Contact Sabre to start that process. Keep client secrets private and never commit them to a repository.

## Contributing

To add a plugin:

1. Create `plugins/<plugin-name>/` following the layout above.
2. Add a `README.md` covering prerequisites, installation, and the tools the plugin provides.
3. Register the plugin in `.cursor-plugin/marketplace.json`.

## License

[MIT](LICENSE.md)
