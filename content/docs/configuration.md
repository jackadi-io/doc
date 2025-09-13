---
title: 'Configuration'
weight: 3
toc: false
---

{{< tabs items="Manager,Agent" >}}
  {{< tab >}}

**Development configuration**

```yaml {filename="/etc/jackadi/manager.yaml"}
mtls: false
auto-accept-agent: true
```

**Minimal recommended configuration for production**

```yaml {filename="/etc/jackadi/manager.yaml"}
address: "0.0.0.0"  # choose the listening address carefully

auto-accept-agent: false

mtls: true
mtls-key: "/etc/jackadi/certs/manager.key"
mtls-cert: "/etc/jackadi/certs/manager.crt"
mtls-agent-ca-cert: "/etc/jackadi/certs/ca.crt"
```
  {{< /tab >}}

  {{< tab >}}

**Development configuration**

```yaml {filename="/etc/jackadi/agent.yaml"}
manager-address: "192.0.2.1"
```

**Minimal recommended configuration for production**

```yaml {filename="/etc/jackadi/agent.yaml"}
manager-address: "192.0.2.1"

# Security settings (mTLS)
mtls: true
mtls-key: "/etc/jackadi/certs/agent.key"
mtls-cert: "/etc/jackadi/certs/agent.crt"
mtls-manager-ca-cert: "/etc/jackadi/certs/ca.crt"
```
  {{< /tab >}}
{{< /tabs >}}

---

{{< cards >}}
  {{< card link="../advanced_guides/configuration" title="Advanced configuration" icon="cog" >}}
{{< /cards >}}
