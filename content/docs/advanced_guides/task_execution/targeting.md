---
title: 'Nodes targeting'
weight: 1
---

## Running a task on a single node

The basic syntax for running a task is:

```sh
jack run <node_name> <plugin.task> [arguments...]
```

Example:
```sh
jack run node1 health.ping

node1

→ output:
    true
```

## Targeting methods overview

Jackadi provides several ways to target multiple nodes:

### 1. Single node targeting

Target a specific node by name:
```sh
jack run -t node1 example.task1
```

### 2. List-based targeting

Target multiple specific nodes:
```sh
jack run -l "node1,node2,node3" example.task1
```

### 3. Pattern-based targeting

**Glob Patterns (default):**
```sh
jack run -g "worker-*" example.task1
jack run "web-*" example.task1  # -g is default
```

**Regular Expressions:**
```sh
jack run -e "node[0-9]+" example.task1
jack run -e "^(web|api)-server-[0-9]{2}$" example.task1
```

### 4. Query targeting

The most advanced targeting method using hostname and/or specs:

#### Exact match

The operator for an exact match is `==`.

```sh {filename="example"}
jack run -q "id==hostname1" example.task1
```

#### List of nodes

The operator for a list is `==`. The elements in the list are separated by a comma.

```sh {filename="example"}
jack run -q "id==node1,node2" example.task1
```

#### Glob pattern

The operator for a glob pattern is `=~`.

```sh {filename="example"}
jack run -q "id=~node*" example.task1
```

#### Regex pattern

The operator for regex filtering is `=~`, followed by `/your_regex/`.

```sh {filename="example"}
jack run -q "id=~/^node.*$/" example.task1
```

#### Logical operators

You can combine multiple filters together using `AND` and `OR`.

The order of execution follows standard binary/mathematical logic: `AND` operations are processed before `OR`.

```sh {filename="example"}
jack run -q "id==node1 AND specs.dev.hardware.Vendor==Lenovo" example.task1
jack run -q "id==node1 OR specs.dev.hardware.Type==Laptop" example.task1
```

{{< callout type="important" >}}
Logic with parentheses is not implemented yet.
{{< /callout >}}

#### Using nested spec

When the used spec is nested, like a map or a struct, you can access to a specific leaf using `.`.

```sh
jack run -q "specs.system.os.kernel.version=~'6.17.*'" kernel-specific.task
```

You can also access to an element of a list:

```sh
jack run -q "specs.system.network.interfaces[0].status==up" network.configure
```

### 4. Target from a file

You can import a list of target from a file (one node per line).

``` {filename="node.txt"}
node1
node2
node3
```

```sh
jack run -f ./nodes.txt cmd.run "pwd"
```

## Execute plugins as standalone binaries

Plugins can be executed directly from the terminal without Jackadi, see [Standalone executable](/docs/advanced_guides/plugin_development/standalone_executable).
