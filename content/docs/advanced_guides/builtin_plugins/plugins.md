---
title: 'Plugin management (plugins)'
weight: 1
---

The `plugins` plugin provides essential functionality for managing plugins on Jackadi nodes.

## Available tasks

### plugins.list

List all available plugins on an node.

#### Syntax
```sh
jack run <node> plugins.list
```

#### Example
```sh
jack run node1 plugins.list
```

#### Sample Output
```
node1

→ output:
    - cmd
    - health
    - specs
    - plugins
```

### plugins.help

Get help documentation for plugins and tasks.

#### Syntax
```sh
# Get help for a plugin
jack run <node> plugins.help <plugin_name>

# Get help for a specific task
jack run <node> plugins.help <plugin_name.task_name>
```

#### Examples

**Plugin-level help:**
```sh
jack run node1 plugins.help cmd
```

Sample output:
```
node1

→ output:
    cmd: |
        run
```

**Task-level help:**
```sh
jack run node1 plugins.help cmd.run
```

Sample output:
```
node1

→ output:
    Task: cmd:run
    Description: Execute a shell command and return output
    Arguments:
      command (string, required): The shell command to execute
      timeout (duration, optional): Maximum execution time (default: 30s)
      shell (string, optional): Shell to use (default: /bin/sh)
```

### plugins.version

Get detailed version information for a specific plugin.

#### Syntax
```sh
jack run <node> plugins.version <plugin_name>
```

#### Example
```sh
jack run node1 plugins.version cmd
```

#### Sample Output
```
node1

→ output:
    Plugin: cmd
    Version: v1.2.3
    Build Time: 2025-01-15T10:30:00Z
    Platform: linux/amd64
```

### plugins.sync

You can synchronize the plugins on the node using, following `plugins.yaml` configuration on the manager.

It will:
* Add new plugins not installed on the node.
* Update existing plugins if necessary.
* Remove unwanted plugins.

#### Syntax
```sh
jack run <node> plugins.sync
```

#### Lock Mode
This task automatically uses `--lock-mode exclusive` because it modifies the plugin system and may restart or reload plugins.

#### Example
```sh
jack run node1 plugins.sync
```

#### Sample Output
```
node1

→ output:
    Starting plugin synchronization...

    Checking for updates:
      cmd: v1.2.2 → v1.2.3 (update available)
      health: v1.1.0 (up to date)
      specs: v1.0.4 → v1.0.5 (update available)

    Downloading updates:
      ✓ cmd v1.2.3 downloaded (2.4MB)
      ✓ specs v1.0.5 downloaded (1.1MB)

    Installing updates:
      ✓ cmd v1.2.3 installed successfully
      ✓ specs v1.0.5 installed successfully

    Synchronization completed successfully.
    Updated: 2 plugins
    Unchanged: 1 plugin
    Total time: 15.2s
```
