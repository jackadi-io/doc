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
    agent1

[✓] 1757611904715894360 * 2025-09-11 17:31:44
    agent2

[✓] 1757611904715808178 * 2025-09-11 17:31:44
    1757611904715894360,1757611904715915620

[✓] 1757611900836610128 * 2025-09-11 17:31:40
    agent1

[✓] 1757611900836582446 * 2025-09-11 17:31:40
    agent2

[✓] 1757611900836474724 * 2025-09-11 17:31:40
    1757611900836610128,1757611900836582446

[✓] 1757611894020555151 * 2025-09-11 17:31:34
    agent1

[✓] 1757611894020470092 * 2025-09-11 17:31:34
    1757611894020555151

Showing 8 results (limit: 100, offset: 0)
```

By default, this shows up to 100 of the most recent results with:
* **Result ID**: Unique identifier for each task execution
* **Timestamp**: When the task completed
* **Agent/Group**: Either the agent that executed the task, or for group results, the comma-separated list of individual result IDs
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

## Agent filtering

Filter results from specific agents using the `--targets` option:

```sh
# Single agent
jack results list --targets agent1

# Multiple agents (comma-separated)
jack results list --targets agent1,agent2,agent3
```

The `--targets` option accepts:
- Single agent ID: `--targets agent1`
- Multiple agent IDs: `--targets agent1,agent2,agent3`

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
