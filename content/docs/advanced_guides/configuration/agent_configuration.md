---
title: 'Agent configuration'
weight: 2
---

## File locations

The agent searches for configuration files in this order:
- `/etc/jackadi/agent.yaml` (system-wide, recommended for production)
- `$HOME/.jackadi/agent.yaml` (user-specific, good for development)
- `./agent.yaml` (current directory)

Custom configuration file:
```sh
agent --config /path/to/custom-agent.yaml
```

## Example

Complete `agent.yaml` configuration with explanations:

```yaml
# Agent identification
agent-id: "my-agent-01"

# Manager connection settings
manager-address: "127.0.0.1"
manager-port: "40080"
reconnect-delay: 10

# Plugin configuration
plugin-dir: "/var/lib/jackadi/plugins"
plugin-server-port: "40081"

# Security settings (mTLS)
mtls:
  enabled: true
  key: "/etc/jackadi/certs/agent.key"
  cert: "/etc/jackadi/certs/agent.crt"
  manager-ca-cert: "/etc/jackadi/certs/ca.crt"

# Custom DNS resolvers (optional)
custom-resolvers:
  - "8.8.8.8:53"
  - "1.1.1.1:53"
```

## Environment variables

All configuration options can be set via environment variables using the prefix `JACKADI_AGENT_` and converting to UPPER_SNAKE_CASE:

```sh
# Agent identification
export JACKADI_AGENT_AGENT_ID="prod-web-01"

# Manager connection
export JACKADI_AGENT_MANAGER_ADDRESS="manager.company.com"
export JACKADI_AGENT_MANAGER_PORT="40080"
export JACKADI_AGENT_RECONNECT_DELAY="5"

# Plugin configuration
export JACKADI_AGENT_PLUGIN_DIR="/opt/agent/plugins"
export JACKADI_AGENT_PLUGIN_SERVER_PORT="40081"

# Security
export JACKADI_AGENT_MTLS_ENABLED="true"
export JACKADI_AGENT_MTLS_KEY="/etc/jackadi/certs/agent.key"
export JACKADI_AGENT_MTLS_CERT="/etc/jackadi/certs/agent.crt"
export JACKADI_AGENT_MTLS_MANAGER_CA_CERT="/etc/jackadi/certs/ca.crt"

# Network (optional)
export JACKADI_AGENT_CUSTOM_RESOLVERS="10.0.1.100:53,10.0.1.101:53"
```

**Container deployment example:**
```sh
# Docker environment variables
docker run -d \
  -e JACKADI_AGENT_AGENT_ID="container-agent-$(hostname)" \
  -e JACKADI_AGENT_MANAGER_ADDRESS="manager.internal" \
  -e JACKADI_AGENT_MTLS_ENABLED="true" \
  -v /etc/jackadi/certs:/certs:ro \
  jackadi/agent
```

## Command-line options

The agent provides comprehensive command-line configuration:

```sh
agent [OPTIONS]
```

| Option | Default | Description |
|--------|---------|-------------|
| `--id` | hostname | Set agent ID |
| `--manager-address` | 127.0.0.1 | Manager address |
| `--manager-port` | 40080 | Manager port |
| `--reconnect-delay` | 10 | Delay in seconds between reconnection attempts |
| `--plugin-dir` | /var/lib/jackadi/plugins | Directory for plugins |
| `--plugin-server-port` | 40081 | Port for plugin server |
| `--custom-resolvers` | | Custom DNS resolvers for GRPC connections (comma-separated) |
| `--mtls.enabled` | true | Use mTLS for secure connection |
| `--mtls.key` | | Agent TLS key filepath |
| `--mtls.cert` | | Agent TLS certificate filepath |
| `--mtls.manager-ca-cert` | | Manager CA certificate filepath |
| `--config` | | Configuration file path |
| `--version, -v` | | Print version information |

## Custom DNS resolution

For environments with complex networking, Jackadi supports custom DNS resolution:

```sh
agent --manager-address=manager.lan \
  --custom-resolvers="192.0.2.1:53,192.0.2.2:53"
```

**Configuration examples:**
```yaml
custom-resolvers:
  - "192.0.2.1:53"
  - "192.0.2.2:53"
```
