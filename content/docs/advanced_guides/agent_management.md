---
title: 'Agent management'
weight: 30
---

## Agent states

Agents in Jackadi can exist in one of three states:

| State | Description | Capabilities |
|-------|-------------|--------------|
| **Candidate** | The initial state when an agent first connects to the manager, not yet approved. | Cannot execute tasks, awaits approval. |
| **Accepted** | Agents that have been approved and can execute tasks. | Can receive and execute tasks, full system access. |
| **Rejected** | Agents that have been denied access to the system, e.g. duplicate agents or manually rejected. | Cannot connect or execute tasks. |

## List agents

```sh {filename="command"}
jack agents list
```

```sh {filename="output"}
Accepted

 • agent1
 • agent2

 Candidates


 Rejected
```

### Detailed view

```sh {filename="command"}
jack agents list --verbose
```

```sh {filename="output"}
Accepted

 • agent1 (172.18.0.3 MIICIjANBgkqhkiG9w...EJEX0CAwEAAQ==)
 • agent2 (172.18.0.2 MIICIjANBgkqhkiG9w...HWGG0CAwEAAQ==)

 Candidates


 Rejected
```

### JSON output

```sh {filename="command"}
jack agents list --json
```

```sh {filename="output"}
{
   "Accepted": [
      {
         "Id": "agent1",
         "Address": "172.18.0.3",
         "Certificate": "MIICIjANBgkqhkiG9w...EJEX0CAwEAAQ==",
         "IsConnected": true,
         "Since": {
            "Seconds": 1757578390,
            "Nanos": 564026075
         },
         "LastMsg": {
            "Seconds": 1757610077,
            "Nanos": 274820288
         },
         "IsActive": true
      },
      {
         "Id": "agent2",
         "Address": "172.18.0.2",
         "Certificate": "MIICIjANBgkqhkiG9w...HWGG0CAwEAAQ==",
         "IsConnected": true,
         "Since": {
            "Seconds": 1757578390,
            "Nanos": 569209812
         },
         "LastMsg": {
            "Seconds": 1757610077,
            "Nanos": 274888616
         },
         "IsActive": true
      }
   ],
   "Candidates": null,
   "Rejected": null
}
```

## Check agent health

```sh {filename="command"}
jack agents health
```

```sh {filename="output"}
Agents

 • agent1 (connected, active)
 • agent2 (connected, active)
```

* `agent1` is connected and had recent activity (e.g. a task has been executed).
* `agent2` is connected and did not have any recent activity.
* `agent3` is not connected to the manager anymore (e.g. due to service down, or network issue).

### Detailed health view

```sh {filename="command"}
jack agents health --verbose
```

```sh {filename="output"}
Agents

 • agent1
   • state: connected, active
   • connected since: September 11, 2025 at 08:13 UTC
   • last active: September 11, 2025 at 17:02 UTC
 • agent2
   • state: connected, active
   • connected since: September 11, 2025 at 08:13 UTC
   • last active: September 11, 2025 at 17:02 UTC
```

## Accept agents

```sh {filename="command"}
jack agents accept <agent_id>
```

```sh {filename="example"}
# Accept a candidate agent
$ jack agents accept agent2
```

```sh {filename="output"}
agent registered: id:"agent2"  address:"172.18.0.2"  certificate:"MIICIjANBgkqhkiG9w...HWGG0CAwEAAQ=="
```

### Accept with specific configuration

```sh {filename="command"}
jack agents accept <agent_id> --address <address> --certificate <cert>
```

```sh {filename="example"}
# Accept agent with specific address and certificate
$ jack agents accept agent2 --address "192.168.1.101:8080" --certificate "agent2.pem"
```

```sh {filename="output"}
agent registered: id:"agent2"  address:"192.168.1.101:8080"  certificate:"MIICIjANBgkqhkiG9w...HWGG0CAwEAAQ=="
```

## Reject agents

```sh {filename="command"}
jack agents reject <agent_id>
```

```sh {filename="example"}
# Reject a candidate agent
$ jack agents reject agent5
```

```sh {filename="output"}
agent rejected: agent2
```

## Remove agents

```sh {filename="command"}
jack agents remove <agent_id>
```

```sh {filename="example"}
# Remove an accepted agent
$ jack agents remove agent1
```

```sh {filename="output"}
agent removed: agent2
```

## Force accept rejected agents

```sh {filename="command"}
jack agents accept <agent_id> --force
```

```sh {filename="example"}
# Force accept a previously rejected agent
$ jack agents accept agent4 --force
```

```sh {filename="output"}
agent registered: id:"agent2"  address:"172.18.0.2"  certificate:"MIICIjANBgkqhkiG9w...HWGG0CAwEAAQ=="
```

## Auto-acceptance

For development or trusted environments, you can enable auto-acceptance on the manager:

```sh {filename="command"}
manager --auto-accept-agent
```

This automatically accepts new agents when they connect without requiring manual approval.

## Security considerations

1. **Production environments**: Disable auto-accept and manually review all agent connection requests.
2. **Rogue agent protection**: Jackadi prevents multiple agents with the same ID - duplicates are automatically rejected.
3. **TLS certificates**: Use certificates for secure agent authentication in production.
4. **Regular auditing**: Monitor agent lists regularly to ensure only authorized agents are connected.

## Agent workflow example

```sh {filename="example"}
# 1. Start an agent (from agent machine)
$ agent --id="web-server-01" --manager-address="manager.example.com:8080"

# 2. Check for new candidates (from manager machine)
$ jack agents list
```

```sh {filename="output"}
Accepted

 • agent1

 Candidates

 • web-server-01

 Rejected
```

```sh {filename="example"}
# 3. Accept the new agent
$ jack agents accept web-server-01
```

```sh {filename="output"}
agent registered: id:"web-server-01"  address:"172.18.0.4"  certificate:"MIICIjANBgkqhkiG9w...HWGG0CAwEAAQ=="
```

```sh {filename="example"}
# 4. Verify the agent is now accepted and connected
$ jack agents health
```

```sh {filename="output"}
Agents

 • agent1 (connected, active)
 • web-server-01 (connected, inactive)
```
