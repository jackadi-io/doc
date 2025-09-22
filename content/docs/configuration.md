---
title: 'Configuration'
weight: 3
toc: false
---

{{< tabs items="Manager,Agent" >}}
  {{< tab >}}

**Test configuration**

This minimal configuration can be used to ease testing in lab.

Not recommenced for production.

```yaml {filename="/etc/jackadi/manager.yaml"}
mtls: false  # in developement, mTLS authentication can be skipped
auto-accept-agent: true  # avoids having to accept agents manually
```

**Minimal recommended configuration for production**

```yaml {filename="/etc/jackadi/manager.yaml"}
# should listen only on the network you want to serve
address: "0.0.0.0"

# auto acceptation should be disabled,
# unless you have a strong pki management
auto-accept-agent: false

# mTLS is critical for production to both
# authenticate and encrypt communication
mtls: true
mtls-key: "/etc/jackadi/certs/manager.key"
mtls-cert: "/etc/jackadi/certs/manager.crt"
mtls-agent-ca-cert: "/etc/jackadi/certs/ca.crt"
```
  {{< /tab >}}

  {{< tab >}}

**Test configuration**

```yaml {filename="/etc/jackadi/agent.yaml"}
manager-address: "192.0.2.1"
mtls: false
```

**Minimal recommended configuration for production**

```yaml {filename="/etc/jackadi/agent.yaml"}
manager-address: "192.0.2.1"

# mTLS is critical for production to both
# authenticate and encrypt communication
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
