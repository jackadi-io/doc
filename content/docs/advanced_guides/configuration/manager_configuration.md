---
title: 'Manager configuration'
weight: 1
---

## File locations

The manager searches for configuration files in this order:
- `/etc/jackadi/manager.yaml` (system-wide, recommended for production)
- `$HOME/.jackadi/manager.yaml` (user-specific)
- `./manager.yaml` (current directory, good for development)

You can also specify a custom config file using the `--config` flag:
```sh
manager --config /path/to/custom-manager.yaml
```

## Example

Here's a complete `manager.yaml` configuration file with explanations:

```yaml
# Network settings
address: "0.0.0.0"
port: "40080"
plugin-server-port: "40081"

# Directory settings
config-dir: "/etc/jackadi"
plugin-dir: "/opt/jackadi/plugins"

# Agent management
auto-accept-agent: false

# Security settings (mTLS)
mtls:
  enabled: true
  key: "/etc/jackadi/certs/manager.key"
  cert: "/etc/jackadi/certs/manager.crt"
  agent-ca-cert: "/etc/jackadi/certs/ca.crt"
```

## Environment variables

All configuration options can be set via environment variables using the prefix `JACKADI_MANAGER_` and converting to UPPER_SNAKE_CASE:

```sh
# Network configuration
export JACKADI_MANAGER_ADDRESS="0.0.0.0"
export JACKADI_MANAGER_PORT="40080"
export JACKADI_MANAGER_PLUGIN_SERVER_PORT="40081"

# Security settings
export JACKADI_MANAGER_MTLS_ENABLED="true"
export JACKADI_MANAGER_MTLS_KEY="/etc/jackadi/certs/manager.key"
export JACKADI_MANAGER_MTLS_CERT="/etc/jackadi/certs/manager.crt"
export JACKADI_MANAGER_MTLS_AGENT_CA_CERT="/etc/jackadi/certs/ca.crt"
export JACKADI_MANAGER_AUTO_ACCEPT_AGENT="false"

# Directory paths
export JACKADI_MANAGER_CONFIG_DIR="/etc/jackadi"
export JACKADI_MANAGER_PLUGIN_DIR="/opt/jackadi/plugins"
```

## Command-line options

The manager supports extensive command-line configuration:

```sh
manager [OPTIONS]
```

| Option | Default | Description |
|--------|---------|-------------|
| `--config-dir` | /etc/jackadi | Configuration directory |
| `--address` | 127.0.0.1 | Manager listen address |
| `--port` | 40080 | Manager listen port |
| `--plugin-dir` | /opt/jackadi/plugins | Plugin inventory directory |
| `--plugin-server-port` | 40081 | Port used to serve plugins |
| `--auto-accept-agent` | false | Auto-accept new agent connections |
| `--mtls.enabled` | true | Secure connections using mTLS |
| `--mtls.key` | | Manager TLS key filepath |
| `--mtls.cert` | | Manager TLS certificate filepath |
| `--mtls.agent-ca-cert` | | Agent CA certificate filepath |
| `--config` | | Configuration file path |
| `--version, -v` | | Print version information |

## Plugin sync configuration

The manager uses a YAML configuration file located at `<config-dir>/plugins.yaml` to determine which plugins to sync to which agents. This enables fine-grained control over plugin distribution based on agent patterns.

Basic plugin distribution ():
```yaml {filename="/etc/jackadi/plugins.yaml"}
"*":
  - linux

"worker-*":
  - deployment
  - monitoring
  - maintenance
```

Currently, plugin binaries must be placed directly within the directory (flat structure, without subdirectories).

```sh
/opt/jackadi/plugins/
├── plugin1
└── plugin2
```
