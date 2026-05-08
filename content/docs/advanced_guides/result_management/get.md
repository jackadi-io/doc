---
title: 'Get results'
weight: 2
---

## Usage

View complete details for a specific result:

```sh {filename="command"}
jack results get 1757611894020555151
```

```sh {filename="output"}
Request:

→ Task: plugins:sync
→ Connected targets: node1

 Node: node1

→ groupID: 1757611894020470092
→ id: 1757611894020555151
→ output:
    Unchanged:
    * Name: demo
```

This shows comprehensive information including:
* **Task Info**: Which plugin and task was executed (`plugins:sync`).
* **Connected targets**: Which nodes were targeted.
* **Node Info**: Which node executed the task.
* **Group ID**: If part of a group result, shows the group ID.
* **Result ID**: Unique identifier for this execution.
* **Output**: Complete task output (stdout content).

### Verbose output

Use the `--verbose` flag for additional node details:

```sh
jack results get 1757611894020555151 --verbose
```

## Group results

When you run a task on multiple nodes, Jackadi creates a result group that contains individual results for each node. In the result listing, group results are displayed differently when running `jack results list`:

Individual Results:
```sh
[✓] 1757611904715915620 * 2025-09-11 17:31:44
    node1
```

Group Results:
```sh
[✓] 1757611904715808178 * 2025-09-11 17:31:44
    1757611904715894360,1757611904715915620
```

Get detailed information about a group result:

```sh {filename="command"}
jack results get 1757611904715808178
```

```sh {filename="output"}
Request:

→ Task: health:ping
→ Connected targets: node1, node2

 Node: node2

→ groupID: 1757611904715808178
→ id: 1757611904715894360
→ output:
    true

 Node: node1

→ groupID: 1757611904715808178
→ id: 1757611904715915620
→ output:
    true
```

## Result status indicators

### Status symbols

Results are displayed with status indicators:
* **`[✓]`**: Successful
* **`✗`**: Failed
* **`?`**: Unknown

### Result information

Each result listing shows:
* **Unique ID**: Large numeric identifier for the result.
* **Timestamp**: Completion time in YYYY-MM-DD HH:MM:SS format.
* **Node/Group Info**: Either the executing node name or list of result IDs for group results.
