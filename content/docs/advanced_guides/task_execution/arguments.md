---
title: 'Argument passing'
weight: 2
---

## Types of arguments

Jackadi supports different ways to pass arguments to tasks:

### Positional arguments

Positional arguments are passed in order after the task name:

```sh
jack run agent1 myplugin:task2 John 30
```

In this example:
- `John` is the first positional argument
- `30` is the second positional argument

The task receives these arguments in the order they are provided.

### Named options (key-value pairs)

Named arguments use the `key=value` syntax:

```sh
jack run agent1 myplugin:task3 name="John" age=30
```

This passes:
- `name` with value `"John"`
- `age` with value `30`

These key-value pairs are passed to the task argument implementing `sdk.Option`. See more details [here](/docs/advanced_guides/plugin_developement/writing_tasks).

### Mixed arguments

You can combine positional and named arguments:

```sh
jack run agent1 myplugin:task4 John age=30
```

This passes:
- `John` as the first positional argument
- `age=30` as a named argument
