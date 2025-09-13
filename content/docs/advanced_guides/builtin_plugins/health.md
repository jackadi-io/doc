---
title: 'Health monitoring (health)'
weight: 3
---

The `health` plugin provides essential monitoring and diagnostic capabilities for Jackadi agents.

## Available tasks

### health:ping

Perform a standard health check that goes through the normal task queue.

#### Syntax
```sh
jack run <agent> health:ping [timeout=<duration>]
```

#### Example
```sh
jack run agent1 health:ping
```

#### Sample Output
```
agent1

→ output:
    Status: healthy
    Response Time: 12ms
    Uptime: 7 days, 14 hours, 23 minutes
    Load Average: 0.15, 0.10, 0.08
    Memory Usage: 2.4GB / 8GB (30%)
    Disk Usage: 45.2GB / 100GB (45%)
    Last Seen: 2025-01-15T10:30:00Z
```

The standard ping includes basic status, response time, system metrics, agent metadata, and task queue status.

### health:instant-ping

Perform an immediate health check that bypasses the task queue for faster response.

#### Syntax
```sh
jack run <agent> health:instant-ping [timeout=<duration>]
```

#### Example
```sh
jack run agent1 health:instant-ping
```

#### Sample Output
```
agent1

→ output:
    Status: healthy
    Response Time: 3ms
    Connection: active
    Agent Version: v2.1.3
    Queue Status: 5 pending tasks
    Emergency Response: available
    Timestamp: 2025-01-15T10:30:00.123Z
```

Key differences from standard ping:
- **Bypasses Queue**: Doesn't wait for other tasks to complete
- **Faster Response**: Minimal processing overhead
- **Limited Info**: Focuses on connectivity and basic status
- **Emergency Use**: Designed for urgent health verification
