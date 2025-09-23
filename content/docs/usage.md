---
title: 'Usage'
weight: 5
---

## Agent management

### List agents

```sh {filename="command"}
jack agents list
```

```sh  {filename="output"}
 Accepted
 • agent1
 • agent3

 Candidates
 • agent2

 Rejected
 • agent1
```

### Manage agents

```sh {filename="command"}
jack agents <accept|reject|remove> <agent>
```

```sh {filename="output"}
$ jack agents accept agent2
```

### Agents health

```sh {filename="command"}
jack agents health
```

```sh {filename="output"}
 Agents

 • agent1 (connected, active)
 • agent2 (connected, inactive)
 • agent3 (disconnected, inactive)
```

* `agent1` is connected and had recent activity (e.g. a task has been executed).
* `agent2` is connected and did not have any recent activity.
* `agent3` is not connected to the manager anymore (e.g. due to service down, or network issue).

## Run a task

{{< callout type="tips" >}}
All targeting modes are documented [here](/docs/advanced_guides/task_execution/targeting/).
{{< /callout >}}

```sh {filename="command"}
jack run <agent> <plugin:task> <arg1> <argN>
```

### One target

```sh {filename="example"}
# Check the health of agent1
$ jack run agent1 health:ping

```

```sh {filename="output"}
agent1

→ output:
   true
```

### Multiple targets

```sh {filename="example"}
# Execute the 'uptime' command on all agent matching 'agent*'
$ jack run agent* cmd:run "uptime"
```

```sh {filename="output"}
agent1

→ output:
    |
       14:49:15 up  4:58,  0 user,  load average: 0.13, 0.52, 0.65

agent2

→ output:
    |
       14:49:15 up  4:58,  0 user,  load average: 0.13, 0.52, 0.65

agent3

→ output:
    |
        14:49:15 up  4:58,  0 user,  load average: 0.13, 0.52, 0.65
```

## Get job results

### List jobs

```sh {filename="command"}
jack results list
```

```sh {filename="output"}
 Task results

[✓] 1757515773331361206 - 2025-09-10 14:49:33
    agent1

[✓] 1757515773331318817 - 2025-09-10 14:49:33
    agent2

[✓] 1757515755520084702 - 2025-09-10 14:49:15
    agent2

[✓] 1757515755520078922 - 2025-09-10 14:49:15
    agent1

[✓] 1757515755519920865 - 2025-09-10 14:49:15
    1757515755520078922,1757515755520084702
```

### Get one job result

```sh {filename="command"}
jack results get <ID>
```

```sh {filename="example"}
# Show the result of the job '1757515755519920865'
$ jack results get 1757515755519920865
```

```sh {filename="output"}
 Request:

→ Task: cmd:run
→ Connected targets: agent2, agent1

 Agent: agent1

→ groupID: 1757515755519920865
→ id: 1757515755520078922
→ output:
    |
       14:49:15 up  4:58,  0 user,  load average: 0.13, 0.52, 0.65

 Agent: agent2

→ groupID: 1757515755519920865
→ id: 1757515755520084702
→ output:
    |
       14:49:15 up  4:58,  0 user,  load average: 0.13, 0.52, 0.65

```


## Manage plugins

### List installed plugins

```sh {filename="command"}
jack run <agent> plugins:list
```

```sh {filename="example"}
# List installed plugins on agent1
$ jack run agent1 plugins:list
```

```sh {filename="output"}
agent1

→ output:
   - specs
   - cmd
   - health
   - plugins
```

### Install/update/remove plugins
```sh {filename="command"}
jack run <agent> plugins:sync
```

```sh {filename="example"}
# Sync plugins to agent1
$ jack run agent1 plugins:sync
```

```sh {filename="output"}
agent1

→ output:
    Added:
    - Name: demo
```

During sync, states can be: `Added`, `Deleted`, `Unchanged`, `Updated`.

### Get version of a plugin

```sh {filename="command"}
jack run <agent> plugins:version
```

```sh {filename="example"}
# Get version information of the 'demo' plugin installed on agent1
$ jack run agent1 plugins:version demo
```

```sh {filename="output"}
agent1

→ output:
    BuildTime: "2025-09-10T15:01:00Z"
    Commit: v0.0.6
    GoVersion: go1.25.1
    PluginVersion: v0.0.6
```

### Print the help for a plugin

```sh {filename="command"}
jack run <agent> plugins:help <plugin>
```

```sh {filename="example"}
# Print the help of the 'demo' plugin installed on agent1
$ jack run agent1 plugins:help demo
```

```sh {filename="output"}
agent1

→ output:
    demo: |
        hello                 Simple hello world task
        configure_service     Configure a system service
        monitor_health        Monitor system health metrics
        create_user           Create a new user account
        get_system_version    Get system version (returns string)
        get_connection_count  Get active connections (returns int64)
        is_maintenance_mode   Check maintenance mode (returns bool)
        get_cpu_usage         Get CPU usage (returns float64)
        list_services         List active services (returns []string)
        get_users             Get user list (returns []User)
        get_env_vars          Get environment variables (returns map[string]string)
        get_metrics           Get system metrics (returns map[string]any)
        get_server_info       Get server information (returns ServerInfo)
        get_reboot_history    Get reboot history (returns [3]string)
        get_db_stats          Get database statistics (returns map[string]DatabaseStats)
        find_user             Find user by email (returns *User)
        upgrade_system        Upgrade system packages
```

### Print the help for a task

```sh {filename="command"}
jack run <agent> plugins:help <plugin:task>
```

```sh {filename="example"}
# Print the help of the task 'configure_service'
# from 'demo' plugin installed on agent1
$ jack run agent1 plugins:help demo:configure_service
```

```sh {filename="output"}
agent1

→ output:
   configure_service: |+
     Summary:
       Configure a system service

     Description:
       Configures a named service with regional settings and timeout controls.

     Usage:
       jack run <target> demo:configure_service <serviceName>

     Arguments:
     serviceName  string   e.g. webserver-pro

     Lock Mode: write
```

### More advanced usage

{{< cards >}}
  {{< card link="/docs/advanced_guides/task_execution" title="Task Execution" icon="play" >}}
  {{< card link="/docs/advanced_guides/system_specs" title="System Specs" icon="chip" >}}
{{< /cards >}}
