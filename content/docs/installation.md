---
title: 'Installation'
weight: 2
---

## Linux packages (recommended)

1. Download and install Jackadi. Installation files (`.deb` and `.rpm`) are available in the [GitHub releases](https://github.com/jackadi-io/jackadi/releases).
2. Edit the configuration (see [Configuration](/docs/configuration) page).
3. Start the service and ensure it is running:
```sh
# for the manager
systemctl enable --now jackadi-manager
systemctl status jackadi-manager

# for the agent
systemctl enable --now jackadi-agent
systemctl status jackadi-agent
```

## Manual installation

{{< callout type="warning" >}}
To install Jackadi manually, you will need to manually create all the directories to make it work (see [File Structure]({{<ref "#file-structure" >}}) section to get the list of directories to create).
{{< /callout >}}

{{< callout type="tips" >}}
More details can be found in [Manual installation guide](/docs/advanced_guides/manual_installation/).
{{< /callout >}}

### Install binaries

All the binaries can be found in [GitHub releases](https://github.com/jackadi-io/jackadi/releases).

### Install from source

1. Clone the repository:
```sh
git clone https://github.com/jackadi-io/jackadi.git && cd jackadi
```
2. Run the build:
```sh
make build
```
3. The three binaries can be found in `dist/` directory.

## File structure

Once everything is installed, you should have the following directory layout:

| Manager | Agent |
|---------|-------|
| {{< filetree/container >}}
  {{< filetree/folder name="/etc/jackadi" >}}
    {{< filetree/file name="manager.yaml" >}}
    {{< filetree/file name="plugins.yaml" >}}
    {{< filetree/file name="registry.json" >}}
    {{< filetree/folder name="tls" >}}{{< /filetree/folder >}}
  {{< /filetree/folder >}}
  {{< filetree/folder name="/var/lib/jackadi" >}}
    {{< filetree/folder name="database" >}}{{< /filetree/folder >}}
  {{< /filetree/folder >}}
    {{< filetree/folder name="/opt/jackadi" >}}
      {{< filetree/folder name="plugins" >}}
      {{< filetree/file name="plugin1" >}}
      {{< filetree/file name="plugin2" >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}} | {{< filetree/container >}}
  {{< filetree/folder name="/etc/jackadi" >}}
    {{< filetree/file name="agent.yaml" >}}
    {{< filetree/folder name="tls" >}}{{< /filetree/folder >}}
  {{< /filetree/folder >}}
  {{< filetree/folder name="/var/lib/jackadi" >}}
    {{< filetree/folder name="database" >}}{{< /filetree/folder >}}
      {{< filetree/folder name="plugins" >}}
      {{< filetree/file name="plugin1" >}}
      {{< filetree/file name="plugin2" >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}
|