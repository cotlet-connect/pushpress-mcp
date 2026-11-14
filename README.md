# How to connect PushPress to ChatGPT, Claude and any AI agent

**ChatGPT, Claude and other AI agents can work in your PushPress Core account.** Cotlet.dev connects your own signed-in PushPress staff account to an agent, workflow or script through hosted MCP and REST.

## What you can ask

> Which classes have open spots today, and who is on the waitlist?

> Check in the members on the 7:00 class roster.

> Show no-shows and late cancels for last week.

> Pause a member plan while they are away, after I approve it.

> Message the class roster, then cancel the class after I approve both steps.

## Connect in 2 minutes

1. Create an account at [Cotlet.dev](https://cotlet.dev/signup?app=pushpress) and enable PushPress for agents.
2. Sign in to PushPress Core with your staff login in Cotlet's Cloud Browser.
3. Add `https://cotlet.dev/mcp/v1` to your agent. Chat apps sign in with Cotlet.dev; editors and scripts can use `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings -> Apps -> add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No PushPress API key is needed.

### Cursor

Copy `.cursor/mcp.json` from this repository into your project. It uses `COTLET_API_KEY` from your environment.

## Run it locally (stdio)

This repository includes a small stdio MCP server for clients that only start local processes. It forwards calls to the hosted Cotlet.dev service. The hosted URL remains the recommended setup.

```json
{"mcpServers":{"pushpress":{"command":"uvx","args":["pushpress-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t pushpress-mcp .
docker run -i --rm -e COTLET_API_KEY pushpress-mcp
```

The local server exposes guide, status, open, read, click, type and close helpers. The hosted PushPress connection exposes 14 tools for classes, rosters, check-ins, members, plans, no-show reports, messages and cancellations.

## Command line

```bash
pip install pushpress-mcp
pushpress-mcp connect
pushpress-mcp status
pushpress-mcp guide
pushpress-mcp tools
pushpress-mcp serve
```

Keep `COTLET_API_KEY` in your credential store, never in a shared workflow export.

## PushPress tools through Cotlet.dev

The hosted connection works in your Core staff account and includes `list_classes`, `get_class_roster`, `book_class_reservation`, `check_in_member`, `remove_reservation`, `list_members`, `get_member`, `list_plan_templates`, `switch_member_plan`, `pause_member_plan`, `cancel_member_plan`, `list_no_shows_late_cancels`, `message_class_registrants` and `cancel_class`.

Six tools that change plans, send messages or cancel classes ask for your approval by default. Booking and check-in can be undone with `remove_reservation`. Use an admin Core login for plan changes and the No Show / Late Cancel report.

## Does PushPress have an API or MCP server?

PushPress publishes a Platform API and an SDK, but they do not provide the staff workflows above. Cotlet works in your own Core login and is independent from PushPress. It is **not affiliated with PushPress, Inc.**

## Privacy and access

Cotlet stores the signed-in Core profile encrypted and never stores your PushPress password. Your agent receives tool results, not the browser session cookie. Disable PushPress for agents, reset the browser or remove the staff login in Core to cut access.

## Find it on

- [PushPress on Cotlet.dev](https://cotlet.dev/integrations/pushpress-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=pushpress&utm_content=find)
- [Step-by-step guide (PDF) on Internet Archive](https://archive.org/details/cotlet-connect-pushpress-to-ai-agents-guide)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

More: https://cotlet.dev/integrations/pushpress-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=pushpress

Start free: https://cotlet.dev/signup?app=pushpress&utm_source=github&utm_medium=readme&utm_campaign=pushpress
