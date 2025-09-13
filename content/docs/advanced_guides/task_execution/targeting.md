---
title: 'Agents targeting'
weight: 1
---

## Running a task on a single agent

The basic syntax for running a task is:

```sh
jack run <agent_name> <plugin:task> [arguments...]
```

Example:
```sh
jack run agent1 health:ping

agent1

→ output:
    true
```

## Targeting methods overview

Jackadi provides several ways to target multiple agents:

### 1. Single agent targeting

Target a specific agent by name:
```sh
jack run -t agent1 example:task1
```

### 2. List-based targeting

Target multiple specific agents:
```sh
jack run -l "agent1,agent2,agent3" example:task1
```

### 3. Pattern-based targeting

**Glob Patterns (default):**
```sh
jack run -g "worker-*" example:task1
jack run "web-*" example:task1  # -g is default
```

**Regular Expressions:**
```sh
jack run -e "agent[0-9]+" example:task1
jack run -e "^(web|api)-server-[0-9]{2}$" example:task1
```

### 4. Query targeting

The most advanced targeting method using hostname and/or specs:

```sh
# Exact match for a single hostname
jack run -q "id==hostname1" example:task1
```

```sh
# Query using specs
jack run -q "id==hostname1" example:task1
```


```sh
# Match multiple hostnames (comma-separated)
jack run -q "id==agent1,agent2" example:task1
```

```sh
# Match hostnames using glob pattern
jack run -q "id=~agent1*" example:task1
```

```sh
# Combine conditions with logical operators
jack run -q "id==agent1 AND specs.dev.hardware.Vendor==Lenovo" example:task1
jack run -q "id==agent1 OR specs.dev.hardware.Type==Laptop" example:task1
```

```sh
# Using nested spec
jack run -q "specs.system.os.kernel.version=~'6.17.*'" kernel-specific:task
```

```sh
# Accessing spec Array element access
jack run -q "specs.system.network.interfaces[0].status==up" network:configure
```

{{< callout type="important" >}}
Logic with Parentheses not implemented yet.
{{< /callout >}}
