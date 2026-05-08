---
title: 'List results'
weight: 1
---

## Usage

Show the most recent task results:

```sh {filename="command"}
jack results list
```

```sh {filename="output"}
Task results

[✓] 1757611904715915620 * 2025-09-11 17:31:44
    node1

[✓] 1757611904715894360 * 2025-09-11 17:31:44
    node2

[✓] 1757611904715808178 * 2025-09-11 17:31:44
    1757611904715894360,1757611904715915620

[✓] 1757611900836610128 * 2025-09-11 17:31:40
    node1

[✓] 1757611900836582446 * 2025-09-11 17:31:40
    node2

[✓] 1757611900836474724 * 2025-09-11 17:31:40
    1757611900836610128,1757611900836582446

[✓] 1757611894020555151 * 2025-09-11 17:31:34
    node1

[✓] 1757611894020470092 * 2025-09-11 17:31:34
    1757611894020555151

Showing 8 results (limit: 100, offset: 0)
```

By default, this shows up to 100 of the most recent results with:
* **Result ID**: Unique identifier for each task execution
* **Timestamp**: When the task completed
* **Node/Group**: Either the node that executed the task, or for group results, the comma-separated list of individual result IDs
* **Status indicator**: `[✓]` for successful completion

## Datetime filtering

Filter results by date using the `--from` and `--to` options:

```sh
# Results from a specific date
jack results list --from "2025-09-11"

# Results from date and time
jack results list --from "2025-09-11 17:30:00"

# Results within a date range
jack results list --from "2025-09-11 17:30:00" --to "2025-09-11 17:32:00"
```

## Node filtering

Filter results from specific nodes using the `--targets` option:

```sh
# Single node
jack results list --targets node1

# Multiple nodes (comma-separated)
jack results list --targets node1,node2,node3
```

The `--targets` option accepts:
- Single node ID: `--targets node1`
- Multiple node IDs: `--targets node1,node2,node3`

## Pagination

Control how many results are displayed and navigate through large result sets:
* **`--limit`**: Maximum number of results to return (default: 100, max: 100)
* **`--offset`**: Starting position for pagination (default: 0)

```sh {filename="example"}
# Show only 10 results
jack results list --limit 10

# Show next 10 results (skip first 10)
jack resultviewings list --limit 10 --offset 10

# Maximum limit is 100
jack results list --limit 100
```
