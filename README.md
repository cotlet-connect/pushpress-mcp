# PushPress API integration and MCP server: connect PushPress to ChatGPT, Claude and any AI agent

PushPress API integration and MCP server, unofficial, by Cotlet. Your AI works in your own signed-in PushPress account: members, plans, check-ins, classes and bookings, appointments, invoices and reports, on the same screens your login opens. No developer and no PushPress API key needed. Cotlet is independent and is not affiliated with PushPress.

**Does PushPress have an API or an MCP server?** PushPress has a Platform API: staff create a key in Core under Settings, Security & Access (checked 23 Nov 2026), and its SDK, marked early alpha, can run as a local MCP server for developers. API keys are switched off in Grow. Its Zapier app has triggers for subscriptions, check-ins and people and two actions, Create a Person and Find Person. PushPress has no hosted MCP server and no ChatGPT or Claude app; its own AI Assistant works inside Core on the Pro and Max plans.

## What gym and CrossFit box owners ask their AI to do in PushPress

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Members who stopped coming | "List members who have not booked in 14 days and draft a check-in message for each. Send nothing yet." | Weekly |
| Pending cancellations | "Who has a pending cancellation this month? Draft a save message for each." | Weekly |
| No-shows to charge | "List Friday's 6 am no-shows and late cancels with the fee each one owes, so I can charge them." | After each class day |
| Failed invoices | "List this month's failed invoices and draft a message to each member to update their card." | Weekly |
| Coach pay | "Work out each coach's pay for the period from the classes and appointments they ran." | Every pay period |
| Leads follow-up without Grow | "List this week's new leads and draft a follow-up for each." | Daily |
| Tomorrow's class reminder | "Draft a text to everyone booked into tomorrow's 6 am class." | Daily |
| Plans about to expire | "List members on non-recurring plans that expire in the next 14 days." | Weekly |
| Reports for your bookkeeper | "Export last month's revenue, retail sales and sales tax for my bookkeeper." | Monthly |
| Check-ins in a spreadsheet | "Put each month's check-ins per member in my Google Sheet." | Your own endpoint, called from Google Sheets |
| Leaving PushPress | "Export my members, active plans and payment history before I switch software." | Once |

Your AI asks you before it messages a member, books or changes anything. Charging cards stays in your hands: Cotlet refuses payments.

## Your own PushPress API, built with Cotlet

PushPress's API and SDK are for developers, and its Zapier app only creates and finds people. With Cotlet you build your own: your AI does the job once in your Cloud Browser, you save it as an endpoint, and you call it from Google Sheets, QuickBooks, HubSpot, ActiveCampaign, Zapier, Make, n8n, your own code.

Examples you can build in a few minutes (none of them exists until you build it):

| Endpoint you build | What it returns or does |
|---|---|
| `inactive-members` | members with no booking for the days you name |
| `pending-cancellations` | members with a pending cancellation and its date |
| `no-shows` | no-shows and late cancels for a class or day, with the fee |
| `failed-invoices` | invoices that failed in a period, with member and amount |
| `coach-sessions` | classes and appointments per coach for a period |
| `book-appointment` | books a PT appointment for a member (writes only after you allow it) |

Call it with your Cotlet API key, JSON in and JSON out:

```bash
curl -X POST https://cotlet.dev/api/v1/custom/inactive-members \
  -H "Authorization: Bearer $COTLET_API_KEY" -H "Content-Type: application/json" -d '{}'
```

Limits: one call runs up to 90 seconds, writes wait for your consent, payments are blocked. Start here: https://cotlet.dev/app/custom-api/new?app=pushpress

## Connect in 2 minutes

Sign up at https://cotlet.dev, sign in to PushPress once in your Cloud Browser, then add Cotlet to your AI.

### Claude

Settings → Connectors → Add custom connector, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev.

### ChatGPT

Open Plugins in ChatGPT, choose Create as a plugin, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev. No API key needed.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

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

The local server exposes guide, status, open, read, click, type and close helpers.

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

## Questions from PushPress gym owners
**Does PushPress have an API?**

Yes. Staff create a Platform API key in Core, and the SDK is marked early alpha (checked 23 Nov 2026); both need a developer. With Cotlet you ask in plain words or build your own endpoints without code.

**Is there a PushPress MCP server for ChatGPT or Claude?**

Not a hosted one. PushPress's SDK can run as a local MCP server for developers, and its AI Assistant works inside Core. Cotlet works in the ChatGPT and Claude apps and any MCP client.

**Can AI find PushPress members who stopped coming?**

Yes. Your AI lists members with no booking for the days you choose and drafts a check-in message for each. Nothing goes out until you say yes.

**Can an AI agent charge PushPress no-show fees?**

It lists the no-shows and late cancels with the fee each owes, so you charge them in one go. Charging cards stays in your hands.

**Can AI work out coach pay from PushPress?**

Yes. It totals the classes and appointments each coach ran in the period, ready for payroll.

**Can I follow up leads without paying for Grow?**

Your AI lists new leads and drafts a follow-up for each from your PushPress account, after your OK.

**How do I export my PushPress members and payment history?**

Your AI exports members, active plans and payment history from the screens your login opens, so you keep everything when you switch.

## Find it on

- [PushPress MCP server on Cotlet.dev](https://cotlet.dev/integrations/pushpress-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=pushpress&utm_content=find)
- [Does PushPress have an API or MCP server?](https://cotlet.dev/api-mcp-status/pushpress?utm_source=github&utm_medium=readme&utm_campaign=pushpress&utm_content=find)
- [PyPI package pushpress-mcp](https://pypi.org/project/pushpress-mcp/)

## More gym and fitness apps on Cotlet

- [ClubReady MCP](https://github.com/cotlet-connect/clubready-mcp)
- [Trainerize MCP](https://github.com/cotlet-connect/trainerize-mcp)
- [TrueCoach MCP](https://github.com/cotlet-connect/truecoach-mcp)
- [Pike13 MCP](https://github.com/cotlet-connect/pike13-mcp)
- [Everfit MCP](https://github.com/cotlet-connect/everfit-mcp)
