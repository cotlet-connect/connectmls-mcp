# connectMLS API integration and MCP server: connect connectMLS to ChatGPT, Claude and any AI agent

**connectMLS API integration and MCP server by Cotlet.dev: yes, Claude, ChatGPT and other AI agents can work in your connectMLS account.** connectMLS has no API for members, no official MCP server and no ChatGPT or Claude app; MRED listing data goes out only as licensed feeds (checked 26 Nov 2026). Cotlet.dev connects your own signed-in connectMLS account to any AI agent, so you ask in plain words and the AI helps you search listings, run comps and manage your listings.

## What you can ask

> Search connectMLS for 3 bed homes in Naperville under $500k with a basement and list the best 10 with links.

> Pull sold comps within half a mile of 410 Elm St from the last 6 months and give me a price range.

> Every morning give me today's hotsheet for my areas: new listings, price drops, back on market.

> Run reverse prospecting for my listing and draft an email to each agent whose client matches. Send after my OK.

> Which listings did my clients favorite or comment on in the portal this week?

## What MLS agents ask their AI to do in connectMLS

From MLS agents' posts on Reddit, forums, review sites and the connectMLS help center (checked 26 Nov 2026).

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Search listings for a client | "Search connectMLS for 3 bed homes in Naperville under $500k with a basement and list the best 10 with links." | When you ask |
| Sold comps for a price talk | "Pull sold comps within half a mile of 410 Elm St from the last 6 months and give me a price range." | Your own endpoint, called from a sheet or n8n |
| Morning hotsheet for my areas | "Every morning give me today's hotsheet for my areas: new listings, price drops, back on market." | Daily |
| Reverse prospecting emails | "Run reverse prospecting for my listing and draft an email to each agent whose client matches. Send after my OK." | When you ask |
| What my clients liked in the portal | "Which listings did my clients favorite or comment on in the portal this week?" | Weekly |
| Change price or status of my listing | "Change the price of my listing at 12 Oak St to $489,000 and show me the form before saving." | When you ask |
| My listings in a sheet | "Export my active and pending listings with list price, days on market and status to a sheet." | Your own endpoint, called from a sheet or n8n |
| Expired listings to call | "List listings in my zip codes that expired or were withdrawn in the last 30 days, with the listing agent." | Weekly |

Your AI asks you before it sends, books, replies or changes anything. Payments stay in your hands.

## connectMLS API

MRED offers no API for members; listing data goes out only as licensed feeds through MLS Grid, under a broker and vendor licence (checked 26 Nov 2026). With Cotlet.dev you build your own connectMLS API: show your AI agent a job once in your signed-in connectMLS account, save it as an endpoint and call it from n8n, Make, Zapier or your own code. How it works: https://cotlet.dev/integrations/api/connectmls

Endpoints you can build in a few minutes (none of them exists until you build it):

- `hotsheet`: New, price change and back on market listings for areas
- `search-listings`: Listings matching your criteria with links
- `sold-comps`: Sold listings near an address for a period
- `my-listings`: Price, days on market and status
- `expired-listings`: Expired and withdrawn listings for areas
- `client-portal-activity`: Client favorites and comments in the portal
- `update-listing-price`: A new price on your listing after your OK

Call one with your Cotlet API key: `POST https://cotlet.dev/api/v1/custom/hotsheet`. One call runs up to 90 seconds, writes wait for your OK, payments are blocked.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

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

This repo is a real MCP server. It speaks MCP over stdio, lists its connectMLS tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"connectmls":{"command":"uvx","args":["connectmls-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t connectmls-mcp .
docker run -i --rm -e COTLET_API_KEY connectmls-mcp
```

Tools: `connectmls_guide`, `connectmls_status`, `connectmls_open`, `connectmls_read`, `connectmls_click`, `connectmls_type`, `connectmls_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install connectmls-mcp        # or run it once: uvx connectmls-mcp connect
connectmls-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 connectmls_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 connectmls_agent.py status    # is connectMLS ready in your Cotlet.dev account
python3 connectmls_agent.py guide     # connectMLS screens and sign-in link
python3 connectmls_agent.py read      # open connectMLS, read the page, close the browser
python3 connectmls_agent.py tools     # list Cotlet.dev tools
python3 connectmls_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from connectMLS users

**Does connectMLS have an API?**

MRED offers no API for members; listing data goes out only as licensed feeds through MLS Grid, under a broker and vendor licence (checked 26 Nov 2026). With Cotlet.dev you build your own connectMLS API on your signed-in account and call it from n8n, Make or your own code: https://cotlet.dev/integrations/api/connectmls

**How do I connect connectMLS to ChatGPT?**

Add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to connectMLS once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words. No code and no connectMLS API key.

**Is there a way to use Claude with connectMLS?**

Yes. In Claude Code run the one-line `claude mcp add` command above; in the Claude app add `https://cotlet.dev/mcp/v1` as a custom connector. Claude then works in your own connectMLS account.

**Is there a connectMLS MCP server?**

connectMLS has no official MCP server for your account. Cotlet.dev is a hosted MCP server that works in your own connectMLS account for you, with Claude, ChatGPT, Cursor, Codex, n8n, Make and Zapier.

**Can ChatGPT or Claude search connectMLS for me?**

Yes. With Cotlet.dev your AI searches connectMLS in your own signed-in account, with your filters, and returns the listings with links. It sees what your login sees.

**Does MRED or connectMLS have an API or MCP server for agents?**

No (checked 26 Nov 2026). MRED listing data goes out only as licensed feeds through MLS Grid. Cotlet.dev works in your own connectMLS account, and you can build your own endpoints such as sold comps or a hotsheet.

## Find it on

- [connectMLS on Cotlet.dev](https://cotlet.dev/integrations/mcp/connectmls?utm_source=github&utm_medium=readme&utm_campaign=connectmls&utm_content=find)
- [PyPI package connectmls-mcp](https://pypi.org/project/connectmls-mcp/)
- [Odel](https://odel.app/modules/cotlet/connectmls)
- [mcpbeat](https://mcpbeat.com/mcp-servers/cotlet/connectmls/)

## Also works with

- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Garmin Connect MCP](https://github.com/cotlet-connect/garmin-mcp)
- [Housecall Pro MCP](https://github.com/cotlet-connect/housecall-pro-mcp)
- [Medium MCP](https://github.com/cotlet-connect/medium-mcp)
- [Crexi MCP](https://github.com/cotlet-connect/crexi-mcp)
- [DistroKid MCP](https://github.com/cotlet-connect/distrokid-mcp)
- [LinkedIn MCP](https://github.com/cotlet-connect/linkedin-mcp)
- [EstimateOne MCP](https://github.com/cotlet-connect/estimateone-mcp)
- [Google Search Console MCP](https://github.com/cotlet-connect/google-search-console-mcp)
- [Lichess MCP](https://github.com/cotlet-connect/lichess-mcp)
- [Strava MCP](https://github.com/cotlet-connect/strava-mcp)
- [StreamElements MCP](https://github.com/cotlet-connect/streamelements-mcp)
- [Meetup MCP](https://github.com/cotlet-connect/meetup-mcp)
- [AccuRanker MCP](https://github.com/cotlet-connect/accuranker-mcp)
- [GetYourGuide MCP](https://github.com/cotlet-connect/getyourguide-mcp)
- [Idealista MCP](https://github.com/cotlet-connect/idealista-mcp)
- [Riverside MCP](https://github.com/cotlet-connect/riverside-mcp)
- [Udemy MCP](https://github.com/cotlet-connect/udemy-mcp)
- [Faire MCP](https://github.com/cotlet-connect/faire-mcp)
- [Teachable MCP](https://github.com/cotlet-connect/teachable-mcp)
- [Vagaro MCP](https://github.com/cotlet-connect/vagaro-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Zoopla MCP](https://github.com/cotlet-connect/zoopla-mcp)
- [Dribbble MCP](https://github.com/cotlet-connect/dribbble-mcp)
- [Hostelworld MCP](https://github.com/cotlet-connect/hostelworld-mcp)
- [Mashvisor MCP](https://github.com/cotlet-connect/mashvisor-mcp)
- [Rent Manager MCP](https://github.com/cotlet-connect/rent-manager-mcp)
- [ThriveCart MCP](https://github.com/cotlet-connect/thrivecart-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Booksy MCP](https://github.com/cotlet-connect/booksy-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [Milanote MCP](https://github.com/cotlet-connect/milanote-mcp)
- [Partiful MCP](https://github.com/cotlet-connect/partiful-mcp)
- [Contractor Foreman MCP](https://github.com/cotlet-connect/contractor-foreman-mcp)
- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [ZenMaid MCP](https://github.com/cotlet-connect/zenmaid-mcp)
- [AroFlo MCP](https://github.com/cotlet-connect/aroflo-mcp)
- [Simpro MCP](https://github.com/cotlet-connect/simpro-mcp)
- [Workiz MCP](https://github.com/cotlet-connect/workiz-mcp)
- [Kickserv MCP](https://github.com/cotlet-connect/kickserv-mcp)
- [Service Autopilot MCP](https://github.com/cotlet-connect/service-autopilot-mcp)
- [Mindbody MCP](https://github.com/cotlet-connect/mindbody-mcp)
- [DaySmart MCP](https://github.com/cotlet-connect/daysmart-mcp)
- [Acuity Scheduling MCP](https://github.com/cotlet-connect/acuity-scheduling-mcp)
- [Jane App MCP](https://github.com/cotlet-connect/jane-app-mcp)
- [FieldEdge MCP](https://github.com/cotlet-connect/fieldedge-mcp)
- [Phorest MCP](https://github.com/cotlet-connect/phorest-mcp)
- [DINGG MCP](https://github.com/cotlet-connect/dingg-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [PhotoShelter MCP](https://github.com/cotlet-connect/photoshelter-mcp)
- [RazorSync MCP](https://github.com/cotlet-connect/razorsync-mcp)
- [ShootProof MCP](https://github.com/cotlet-connect/shootproof-mcp)
- [Invoice2go MCP](https://github.com/cotlet-connect/invoice2go-mcp)
- [Appointy MCP](https://github.com/cotlet-connect/appointy-mcp)
- [Contractor+ MCP](https://github.com/cotlet-connect/contractor-plus-mcp)
- [Petpooja MCP](https://github.com/cotlet-connect/petpooja-mcp)
- [Teachers Pay Teachers MCP](https://github.com/cotlet-connect/teachers-pay-teachers-mcp)
- [Bloom MCP](https://github.com/cotlet-connect/bloom-mcp)
- [hipages MCP](https://github.com/cotlet-connect/hipages-mcp)
- [ServiceSeeking MCP](https://github.com/cotlet-connect/serviceseeking-mcp)
- [MyJobQuote MCP](https://github.com/cotlet-connect/myjobquote-mcp)
- [MyBuilder MCP](https://github.com/cotlet-connect/mybuilder-mcp)
- [Yelp for Business MCP](https://github.com/cotlet-connect/yelp-for-business-mcp)
- [Rosie MCP](https://github.com/cotlet-connect/rosie-mcp)
- [Roofr MCP](https://github.com/cotlet-connect/roofr-mcp)
- [FieldPulse MCP](https://github.com/cotlet-connect/fieldpulse-mcp)
- [YourTradebase MCP](https://github.com/cotlet-connect/yourtradebase-mcp)
- [JobNimbus MCP](https://github.com/cotlet-connect/jobnimbus-mcp)
- [Ascora MCP](https://github.com/cotlet-connect/ascora-mcp)
- [eLocal MCP](https://github.com/cotlet-connect/elocal-mcp)
- [Alto MCP](https://github.com/cotlet-connect/alto-mcp)
- [Rightmove Plus MCP](https://github.com/cotlet-connect/rightmove-plus-mcp)
- [TurboTenant MCP](https://github.com/cotlet-connect/turbotenant-mcp)
- [Innago MCP](https://github.com/cotlet-connect/innago-mcp)
- [Property Finder MCP](https://github.com/cotlet-connect/property-finder-mcp)
- [LoopNet MCP](https://github.com/cotlet-connect/loopnet-mcp)
- [Dotloop MCP](https://github.com/cotlet-connect/dotloop-mcp)
- [SkySlope MCP](https://github.com/cotlet-connect/skyslope-mcp)
- [ShowingTime MCP](https://github.com/cotlet-connect/showingtime-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/mcp/connectmls?utm_source=github&utm_medium=readme&utm_campaign=connectmls

Start free: https://cotlet.dev/signup?app=connectmls&utm_source=github&utm_medium=readme&utm_campaign=connectmls
