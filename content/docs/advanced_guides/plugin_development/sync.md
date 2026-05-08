---
title: 'Plugin synchronization'
weight: 4
---

{{< callout >}}
TODO: manage multiple version of a same plugin (using filenames, like demo-v0.0.1 and demo-v0.0.2)
{{< /callout>}}

## Overview

The plugin synchronization process in Jackadi follows this workflow:

1. Plugins are placed in the manager's plugin directory.
2. The manager maintains a configuration defining which nodes should receive which plugins.
3. Nodes request their plugin list from the manager.
4. The manager responds based on the node's identity and configured patterns.
5. Nodes download, install, and load the required plugins.

## Manager Configuration

### Plugin directory

The manager looks for plugins in its configured plugin directory:

```sh
manager --plugin-dir=/path/to/plugins
```

All plugins binaries in this directory are available for distribution to nodes.

### Plugin configuration

The manager uses a YAML configuration file located at `<config-dir>/plugins.yaml` to determine which plugins to sync to which nodes:

```yaml
"*":
  - worker-tools
"db-*":
  - database-tools
```

The configuration uses node name patterns (with glob syntax) as keys and lists of plugin names as values.

## Node plugin sync

### Automatic sync

{{< callout type="important" >}}
Not yet implemented.
{{< /callout >}}

### Manual sync

To manually trigger plugin synchronization on an node:

```sh
jack run node1 plugins.sync
```

This sync is using by default an exclusive lock.

## Troubleshooting

If a plugin isn't being loaded by an node:

1. Check if the node matches any patterns in the `plugins.yaml` file.
2. Verify the plugin binary exists in the manager's plugin directory.
3. Check the node logs for download or loading errors.
4. Execute the plugin binary manually with `--describe` and check for any errors.
5. Manually trigger a sync: `jack run node1 plugins.sync`.
