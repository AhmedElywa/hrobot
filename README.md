# hrobot

CLI for [Hetzner Robot API](https://robot.hetzner.com/doc/webservice/en.html) (dedicated servers).

## Install

```sh
# Copy to your PATH
cp hrobot /usr/local/bin/

# Or symlink (with short alias)
ln -s $(pwd)/hrobot /usr/local/bin/hrobot
ln -s $(pwd)/hrobot /usr/local/bin/hr
```

Requires `curl` and `jq`.

## Setup

```sh
hrobot auth
```

Get your API credentials at **Robot → Settings → Webservice**.

## Usage

Use `hr` as a shorthand for `hrobot`.

### Servers

```sh
hr s                        # List all servers
hr server 123456            # Server details
hr rename 123456 web01      # Rename server
hr reset 123456 hw          # Hardware reset
hr rescue 123456 activate   # Activate rescue mode
```

### Network

```sh
hr ips                      # List IPs
hr rdns set 1.2.3.4 my.host.com  # Set reverse DNS
hr f 123456                 # Firewall status
hr t 123456 monthly         # Traffic stats
```

### SSH Keys

```sh
hr k                        # List SSH keys
hr key add mykey ~/.ssh/id_ed25519.pub
hr key del <fingerprint>
```

### Server Market (auction / refurbished)

Browse 700+ auction servers — no auth needed for listing.

```sh
hr m                        # List all, sorted by price
hr m show 2948626           # Server details with specs
hr m buy 2948626            # Order (with confirmation)
hr m tx                     # List transactions
```

**Filters** — combine freely:

```sh
hr m --ram 64               # 64GB+ RAM
hr m --price 50             # Under 50€/month
hr m --cpu ryzen            # AMD Ryzen CPUs
hr m --dc HEL               # Helsinki datacenter
hr m --nvme                 # NVMe drives only
hr m --ssd                  # SSD drives only
hr m --hdd                  # HDD drives only
hr m --ecc                  # ECC memory only
hr m --ram 64 --nvme --price 50 --dc HEL  # Combine filters
```

**Sort options:**

```sh
hr m --sort price           # By price (default)
hr m --sort ram             # By RAM size
hr m --sort disk            # By disk size
hr m --sort drop            # By next price drop (soonest first)
```

### Server Catalog (new / standard)

Browse standard dedicated servers with per-location pricing.

```sh
hr c                        # List all servers
hr c show AX41-NVMe         # Details, pricing, OS options
hr c buy AX41-NVMe          # Order with location selection
hr c --sort ram             # Sort by RAM instead of price
```

### Storage

```sh
hr storageboxes             # List storage boxes
hr storagebox <id>          # Storage box details
```

## Short Aliases

| Alias | Command |
|-------|---------|
| `hr` | `hrobot` |
| `s` | `servers` |
| `m` | `market` |
| `c` | `catalog` |
| `k` | `keys` |
| `f` | `firewall` |
| `t` | `traffic` |

## Configuration

Credentials are stored in `~/.config/hrobot/credentials` (chmod 600).

Override with `HROBOT_CONFIG_DIR` environment variable.

## License

MIT
