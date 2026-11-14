# Sabre Mosaic Travel Connector

Cursor plugin that connects agents to the Sabre Mosaic Travel Connector through its remote [Model Context Protocol](https://modelcontextprotocol.io/) server.

Search hotels and flights, compare room rates and fare terms, build a trip plan and check it out, all as the signed-in traveler.

## Before you start

The plugin needs a **Client ID** and a **Client Secret** before it can connect. Sabre issues them during onboarding, so contact Sabre to start the process. Until you have both, the plugin cannot sign you in.

Keep the Client Secret private: do not share it or commit it to a repository.

## Install

1. Complete onboarding and have your Client ID and Client Secret at hand.
2. Install **Sabre Mosaic Travel Connector** from the Cursor Plugins settings (or run `/add-plugin sabre-mosaic-travel-connector`) once the plugin is available in your marketplace.
3. Set **Sabre Mosaic Travel Connector Client ID** and **Sabre Mosaic Travel Connector Client Secret** when prompted, then complete the Sabre sign-in prompt.

On a team marketplace, a Cursor admin can set the credentials once for everyone in **Dashboard → Plugins → Configure**; each member still completes their own sign-in.

## MCP

```json
{
  "mcpServers": {
    "sabre-mosaic-travel-connector": {
      "type": "http",
      "url": "https://mcp.partners.sabre.com/mcp",
      "auth": {
        "CLIENT_ID": "${CLIENT_ID}",
        "CLIENT_SECRET": "${CLIENT_SECRET}",
        "scopes": ["openid", "email", "profile", "offline_access"]
      }
    }
  }
}
```

`${CLIENT_ID}` and `${CLIENT_SECRET}` are filled in from the credentials you enter in Cursor; they are not stored in the plugin. Each user signs in with their own account, and tool calls run with that traveler's permissions.

## What agents can do

| Category | Tools |
| --- | --- |
| Hotels | `search_hotels`, `get_hotel_details`, `get_hotel_rates`, `add_hotel_to_trip_plan` |
| Flights | `search_flights`, `get_flight_details`, `add_flight_to_trip_plan` |
| Checkout | `checkout`, `get_checkout_status` |
| Help | `get_help` |

The hosted server is the source of truth for tool names and schemas. Results are plain text; there is no widget in Cursor.

## Notes

- A trip plan holds one flight and one hotel. Adding a hotel or flight returns a trip plan ID that can be passed to the next add, and `checkout` takes one plan's ID. Adding to a trip plan is not a booking.
- `checkout` books and pays for the trip. Agents should start it only on the user's explicit request.
- During payment the server may ask for card details to be typed into the chat. If you do not want card data to pass through the model, turn off `checkout` for this server in Cursor's MCP settings.
- Prices, availability and fare terms come only from the tools and can change between steps.
