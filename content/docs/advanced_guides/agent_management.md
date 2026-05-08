---
title: 'Node management'
weight: 30
---

## Node states

Nodes in Jackadi can exist in one of three states:

| State | Description | Capabilities |
|-------|-------------|--------------|
| **Candidate** | The initial state when an node first connects to the manager, not yet approved. | Cannot execute tasks, awaits approval. |
| **Accepted** | Nodes that have been approved and can execute tasks. | Can receive and execute tasks, full system access. |
| **Rejected** | Nodes that have been denied access to the system, e.g. duplicate nodes or manually rejected. | Cannot connect or execute tasks. |

## List nodes

```sh {filename="command"}
jack nodes list
```

```sh {filename="output"}
Accepted

 • node1
 • node2

 Candidates


 Rejected
```

### Detailed view

```sh {filename="command"}
jack nodes list --verbose
```

```sh {filename="output"}
Accepted

 • node1 (172.18.0.3 MIICIjANBgkqhkiG9w...EJEX0CAwEAAQ==)
 • node2 (172.18.0.2 MIICIjANBgkqhkiG9w...HWGG0CAwEAAQ==)

 Candidates


 Rejected
```

### JSON output

```sh {filename="command"}
jack nodes list --json
```

```sh {filename="output"}
{
   "Accepted": [
      {
         "Id": "node1",
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
         "Id": "node2",
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

## Check node health

```sh {filename="command"}
jack nodes health
```

```sh {filename="output"}
Nodes

 • node1 (connected, active)
 • node2 (connected, active)
```

* `node1` is connected and had recent activity (e.g. a task has been executed).
* `node2` is connected and did not have any recent activity.
* `node3` is not connected to the manager anymore (e.g. due to service down, or network issue).

### Detailed health view

```sh {filename="command"}
jack nodes health --verbose
```

```sh {filename="output"}
Nodes

 • node1
   • state: connected, active
   • connected since: September 11, 2025 at 08:13 UTC
   • last active: September 11, 2025 at 17:02 UTC
 • node2
   • state: connected, active
   • connected since: September 11, 2025 at 08:13 UTC
   • last active: September 11, 2025 at 17:02 UTC
```

## Accept nodes

```sh {filename="command"}
jack nodes accept <node_id>
```

```sh {filename="example"}
# Accept a candidate node
$ jack nodes accept node2
```

```sh {filename="output"}
node registered: id:"node2"  address:"172.18.0.2"  certificate:"MIICIjANBgkqhkiG9w...HWGG0CAwEAAQ=="
```

### Accept with specific configuration

```sh {filename="command"}
jack nodes accept <node_id> --address <address> --certificate <cert>
```

```sh {filename="example"}
# Accept node with specific address and certificate
$ jack nodes accept node2 --address "192.168.1.101:8080" --certificate "node2.pem"
```

```sh {filename="output"}
node registered: id:"node2"  address:"192.168.1.101:8080"  certificate:"MIICIjANBgkqhkiG9w...HWGG0CAwEAAQ=="
```

## Reject nodes

```sh {filename="command"}
jack nodes reject <node_id>
```

```sh {filename="example"}
# Reject a candidate node
$ jack nodes reject node5
```

```sh {filename="output"}
node rejected: node2
```

## Remove nodes

```sh {filename="command"}
jack nodes remove <node_id>
```

```sh {filename="example"}
# Remove an accepted node
$ jack nodes remove node1
```

```sh {filename="output"}
node removed: node2
```

## Force accept rejected nodes

```sh {filename="command"}
jack nodes accept <node_id> --force
```

```sh {filename="example"}
# Force accept a previously rejected node
$ jack nodes accept node4 --force
```

```sh {filename="output"}
node registered: id:"node2"  address:"172.18.0.2"  certificate:"MIICIjANBgkqhkiG9w...HWGG0CAwEAAQ=="
```

## Auto-acceptance

For development or trusted environments, you can enable auto-acceptance on the manager:

```sh {filename="command"}
manager --auto-accept-node
```

This automatically accepts new nodes when they connect without requiring manual approval.

## Security considerations

1. **Production environments**: Disable auto-accept and manually review all node connection requests.
2. **Rogue node protection**: Jackadi prevents multiple nodes with the same ID - duplicates are automatically rejected.
3. **TLS certificates**: Use certificates for secure node authentication in production.
4. **Regular auditing**: Monitor node lists regularly to ensure only authorized nodes are connected.

## Node workflow example

```sh {filename="example"}
# 1. Start an node (from node machine)
$ node --id="web-server-01" --manager-address="manager.example.com:8080"

# 2. Check for new candidates (from manager machine)
$ jack nodes list
```

```sh {filename="output"}
Accepted

 • node1

 Candidates

 • web-server-01

 Rejected
```

```sh {filename="example"}
# 3. Accept the new node
$ jack nodes accept web-server-01
```

```sh {filename="output"}
node registered: id:"web-server-01"  address:"172.18.0.4"  certificate:"MIICIjANBgkqhkiG9w...HWGG0CAwEAAQ=="
```

```sh {filename="example"}
# 4. Verify the node is now accepted and connected
$ jack nodes health
```

```sh {filename="output"}
Nodes

 • node1 (connected, active)
 • web-server-01 (connected, inactive)
```
