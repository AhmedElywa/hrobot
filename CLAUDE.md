# CLAUDE.md

## Project Overview

hrobot is a Bash CLI for the [Hetzner Robot API](https://robot.hetzner.com/doc/webservice/en.html) — managing dedicated servers, IPs, firewall, rescue mode, SSH keys, storage boxes, and server ordering (both auction and new).

## Architecture

Single-file Bash script (`hrobot`) with no dependencies beyond `curl` and `jq`. Credentials stored in `~/.config/hrobot/credentials`.

**Data sources:**
- Robot API (`robot-ws.your-server.de`) — server management, ordering, auth-required operations
- Public Hetzner feeds (no auth needed):
  - `live_data_sb_EUR.json` — server auction/market listings (700+ servers)
  - `live_data_en_EUR.json` — standard server catalog (17 products)

## Commands

```bash
# Short alias: hr = hrobot, subcommands: s m c k f t
hr help              # Show all commands
hr s                 # List servers (alias: servers)
hr server <num>      # Server details
hr reset <num> [sw|hw|man]  # Reset server
hr rescue <num> [status|activate|deactivate|last]
hr ips               # List IPs
hr rdns [list|get|set|del]  # Reverse DNS
hr f <num>           # Firewall status (alias: firewall)
hr k                 # SSH keys (alias: keys)
hr t <num> [daily|monthly|yearly]  # Traffic stats

# Server market (auction) — no auth for listing
hr m                 # List auction servers
hr m --ram 64 --nvme --price 50  # Filter
hr m --sort ram      # Sort by ram|price|disk|drop
hr m show <id>       # Server details
hr m buy <id>        # Order (with confirmation)
hr m tx              # Transactions

# Server catalog (new) — no auth for listing
hr c                 # List standard servers
hr c show <name>     # Details with pricing/OS
hr c buy <id>        # Order (with confirmation)
hr c --sort ram      # Sort by ram|price|name
```

## Development

- All code lives in the single `hrobot` file
- Robot API base: `https://robot-ws.your-server.de`
- Auth: HTTP Basic Auth over HTTPS
- Output: colored tables in TTY, plain text/JSON for pipes (`[ -t 1 ]` check)
- Tables are responsive — truncate to terminal width with `tput cols`
- Destructive commands (reset, cancel) require confirmation prompts
- Order commands require typing `YES` to confirm

## Key Functions

- `api()` — HTTP request wrapper with error handling for all status codes
- `table()` — table formatter: title, column headers, jq expression, terminal-width-aware
- `_sb_fetch()` / `_catalog_fetch()` — fetch public Hetzner JSON feeds
- `_select_keys()` — shared SSH key selection for buy flows
- `_market_detail()` / `_catalog_detail()` — rich detail views from public feed data

## API Reference

Full API docs: https://robot.hetzner.com/doc/webservice/en.html

**Key endpoints:**
- `POST /order/server_market/transaction` — order auction server (params: `product_id`, `authorized_key[]`)
- `POST /order/server/transaction` — order standard server (params: `product_id`, `authorized_key[]`, `location`)
- Traffic endpoint uses short date formats: day=`YYYY-MM-DDTHH`, month=`YYYY-MM-DD`, year=`YYYY-MM`

## Security

- API access is IP-restricted to the Hetzner server (95.217.230.59)
- Credentials file is chmod 600
- Config directory is chmod 700

## Guidelines

- Keep it as a single file — no external dependencies beyond curl/jq
- All destructive operations must prompt for confirmation
- Use colored output for terminal, plain for pipes
- Error handling: parse HTTP status codes, show meaningful messages
- Capture API responses before piping (avoid die-in-subshell bug)
- Market/catalog listing use public feeds — no auth required
- Only buy/transactions commands need auth
