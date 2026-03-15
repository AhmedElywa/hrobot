# hrobot

CLI for [Hetzner Robot API](https://robot.hetzner.com/doc/webservice/en.html) (dedicated servers).

## Install

```sh
# Copy to your PATH
cp hrobot /usr/local/bin/

# Or symlink
ln -s $(pwd)/hrobot /usr/local/bin/hrobot
```

Requires `curl` and `jq`.

## Setup

```sh
hrobot auth
```

Get your API credentials at **Robot → Settings → Webservice**.

## Usage

```sh
hrobot servers              # List all servers
hrobot server 123456        # Server details
hrobot rename 123456 web01  # Rename server
hrobot reset 123456 hw      # Hardware reset
hrobot rescue 123456 activate # Activate rescue mode
hrobot ips                  # List IPs
hrobot rdns set 1.2.3.4 my.host.com  # Set reverse DNS
hrobot firewall 123456      # Firewall status
hrobot keys                 # List SSH keys
hrobot traffic 123456 monthly # Traffic stats
hrobot storageboxes         # List storage boxes
hrobot order products       # Browse available servers
```

Run `hrobot help` for all commands.

## Configuration

Credentials are stored in `~/.config/hrobot/credentials` (chmod 600).

Override with `HROBOT_CONFIG_DIR` environment variable.

## License

MIT
