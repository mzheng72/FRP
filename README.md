cat > README.md << 'EOF'
# FRP Remote File Access

A self-hosted reverse proxy setup that let me securely access files on my personal Mac from off-campus, using [frp](https://github.com/fatedier/frp) (Fast Reverse Proxy) and a cloud VPS.

*Note: The VPS used for this project has since expired and the service is no longer live. This repo documents the configuration and setup.*

## Overview

My Mac sits behind a university/home network and isn't directly reachable from the public internet. This project used `frp` to establish a persistent tunnel from my Mac to a cloud VPS, exposing a password-protected file server so I could access files on my Mac remotely without relying on third-party cloud storage.

## How It Worked

1. **frpc** (the FRP client) ran on my Mac and connected outbound to a VPS running the FRP server (`frps`)
2. The VPS forwarded incoming requests on a specific port to my Mac through the established tunnel
3. A `static_file` plugin served a chosen local directory over HTTP, protected with HTTP Basic Auth (username/password)
4. Token-based authentication secured the connection between client and server

## Tech Stack

- **frp (Fast Reverse Proxy)** — client/server reverse proxy tool written in Go
- **VPS** — cloud server acting as the public-facing relay
- **TOML** — configuration format for the client

## Configuration

See `frpc.toml` for the client configuration structure. Real credentials (server IP, auth token, HTTP Basic Auth username/password) are excluded via `.gitignore` and replaced with placeholders here — this file is a template, not the live config.

```toml
serverAddr = "your_server_ip"
auth.token = "your_token_here"
httpUser = "your_username"
httpPassword = "your_password"
```

## Notes

- The compiled `frpc` binary and log files are intentionally excluded from this repo (`.gitignore`) — they're either build artifacts or environment-specific, not source content
- Debugged a config compatibility issue where `remotePort` was rejected by the client due to a version mismatch between the config schema and the installed `frpc` binary
- The VPS backing this project has since expired; this repo serves as a record of the setup and configuration approach

## About

Built by Minye Zheng as a personal networking/infrastructure project to practice reverse proxy setup, VPS management, and secure remote access.
EOF
