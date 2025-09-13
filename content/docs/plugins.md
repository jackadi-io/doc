---
title: 'Plugins'
weight: 4
---

## Builtins

Jackadi comes up with few builtins:
* `cmd:`
* `health:`
* `plugins:`
* `specs:`

More details can be found [here](/docs/advanced_guides/builtin_plugins).

## Creating a plugin

{{< callout >}}
A collection can serve both tasks and spec collectors.
{{< /callout >}}

### Creating a task

A task can be a simple operation like creating a systemd service, or a more complex workflow.

It is a function having a simple signature:
1. **Context Parameter** (optional): If present, must be the first parameter. Allows for cancellation and timeout handling.
2. **Options Parameter** (optional): If present, must be the first parameter (if no context) or second parameter (if context is present). A pointer to a struct that implements the `Options` interface.
3. **Positional Arguments** (optional): Zero or more arguments for positional CLI parameters.
4. **Return Values**: Always returns two values - a result of any serializable type and an error.

Example of a plugin serving one collection which has one task:
```go {filename="demo.go"}
package main

import "github.com/jackadi-io/jackadi/sdk"

func Hello() (string, error){
	return "hello world!", nil
}

func main() {
	collection := sdk.New("demo")
	collection.MustRegisterTask("hello", Hello).WithSummary("Simple hello world")
	sdk.MustServe(collection)
}
```

Usage:
```sh {filename="command"}
jack run <agent> demo:hello
```

For more details, check out the advanced [task writing guide](TODO).

### Creating spec collectors

Spec collectors are function returning information about the asset (e.g. hardware specs, resource usage, OS version etc...).

Their signature is similar to tasks:
1. **Context Parameter** (optional): Used for cancellation and timeout handling.
2. **Return Values**: Always returns two values:
   - A data structure containing the collected information.
   - An error value (or nil if successful).

```go {filename="demo.go"}
package main

import "github.com/jackadi-io/jackadi/sdk"

func SystemInfoCollector() (map[string]string, error){
	info := map[string]string{
		{"distribution": "opensuse"},
		{"version": "tumbleweed"},
		{"kernel": "6.16"},
	}
	return info, nil
}

func main() {
	collection := sdk.New("demo")
	collection.MustRegisterSpecCollector("system-info", SystemInfoCollector)
	sdk.MustServe(collection)
}
```

Once declared, they are regularly collected by the agent (every 60s by default).

Usage:
```sh {filename="commands"}
jack run <agent> specs:list
jack run <agent> specs:all

# we can get all specs provided system-info
jack run <agent> specs:get system-info
# or get a specific field
jack run <agent> specs:get system-info.distribution
```
