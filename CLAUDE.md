# CLAUDE.md

## Project Overview

hrobot is a Bash CLI for the [Hetzner Robot API](https://robot.hetzner.com/doc/webservice/en.html) — managing dedicated servers, IPs, firewall, rescue mode, SSH keys, storage boxes, and ordering.

## Architecture

Single-file Bash script (`hrobot`) with no dependencies beyond `curl` and `jq`. Credentials stored in `~/.config/hrobot/credentials`.

## Commands

```bash
hrobot help          # Show all commands
hrobot servers       # List servers
hrobot server <num>  # Server details
hrobot reset <num> [sw|hw|man]  # Reset server
hrobot rescue <num> [status|activate|deactivate]
hrobot ips           # List IPs
hrobot rdns [list|get|set|del]  # Reverse DNS
hrobot firewall <num>  # Firewall status
hrobot keys          # SSH keys
hrobot order products  # Browse servers
```

## Development

- All code lives in the single `hrobot` file
- API base: `https://robot-ws.your-server.de`
- Auth: HTTP Basic Auth over HTTPS
- Output: JSON via `jq`, tables via `column -t`
- Destructive commands (reset, cancel) require confirmation prompts

## API Reference

Full API docs: https://robot.hetzner.com/doc/webservice/en.html

## Security

- API access is IP-restricted to the Hetzner server (95.217.230.59)
- Credentials file is chmod 600
- Config directory is chmod 700

## Guidelines

- Keep it as a single file — no external dependencies beyond curl/jq
- All destructive operations must prompt for confirmation
- Use colored output for terminal, plain for pipes (`[ -t 1 ]` check)
- Error handling: parse HTTP status codes, show meaningful messages
