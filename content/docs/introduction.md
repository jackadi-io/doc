---
title: 'Introduction'
weight: 1
---

## What is Jackadi?

Jackadi is a developer-first distributed task execution platform designed for developers with a plugin system architecture consisting of a manager and agents.

The main motivation is to create a framework where developers write tasks as pure code without abstractions or hidden behaviors. Task writing is meant to be natural and direct.

Key principles:
* **Pure Go Approach**: Tasks are written as Go code with no hidden behaviors - what you write is what you get.
* **No Runtime Dependencies**: Tasks have no runtime dependencies on other tasks; all dependencies are resolved at compile-time.
* **No Abstractions**: Task writing is natural for Go developers with minimal framework-specific knowledge needed.
* **Flexible Use Cases**: From simple package installation to complex workflows like server management and upgrades.

## Features

| | Description |
|---------|-------------|
| **Distributed Task Execution** | Execute tasks across multiple agents from a central manager. |
| **Plugin System**              | Extend functionality through custom Go plugins. |
| **Advanced Targeting**         | Target agents via list, glob, regex, advanced query. |
| **Specs Collection**           | Gather and store system information from agents. |
| **Security**                   | mTLS, agent acceptance workflow, protection against rogue agents. |
| **Developer-Friendly**         | Tasks/specs are Go function registered with a simple SDK. |

## Architecture

Agents are connected to a manager via persistent bidirectional gRPC connections.

Jackadi features a simple plugin architecture where all tasks and specs collectors are implemented as pure Go functions. The plugin system is built on top of [hashicorp/go-plugin](https://github.com/hashicorp/go-plugin/), and is made intuitive using a simple and easy to use SDK.

By default, all executed task are recorded in [BadgerDB](https://github.com/hypermodeinc/badger).

## Quick demo tour

### Quickstart

```sh
# Start the manager
manager --mtls=false

# Start an agent
agent --id="agent1" --mtls=false

# Accept the agent connection (if not using auto-accept)
jack agents list
jack agents accept agent1

# The agent should be now in "accepted" list
jack agents list

# Check agents health
jack agents health

# Run a task
jack run agent1 cmd:run "echo hello"
```

### Write my first plugin

{{% steps %}}

#### Create a Go project

```go {filename=tour.go}
package main

import "github.com/jackadi-io/jackadi/sdk"

func Hello(name string) (string, error) {
	return fmt.Sprintf("Hello %s!", name), nil
}

func main() {
	tour := sdk.NewCollection("tour")
	tour.MustRegisterTask("hello", Hello).WithDescription("Greetings.")
	sdk.MustServe(tour)
}
```

#### Compile the plugin

```sh
CGO_ENABLED=0 go build -o tour .
```
#### Put the plugin in the manager

Copy the file in the manager `/opt/jackadi/plugins` directory.

#### Synchronize the plugin to the agent
```sh
jack run agent1 plugins:sync
```

{{% /steps %}}

## Next step

{{< cards >}}
  {{< card link="basic_guides" title="Basic guides" icon="book-open" >}}
{{< /cards >}}
