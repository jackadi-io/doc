---
title: 'Optimization'
weight: 5
---

## Plugin synchronization

If you deploy a large number of agents, reducing the size of a plugin binary can be beneficial for bandwidth usage and duration of plugin synchronization.

### Plugin size optimization

We recommend using [UPX](https://upx.github.io/), which can shrink binaries by up to 70%:

```sh
upx --best --lzma ./my_plugin
```

### Mono plugin

Instead of installing a lot of plugins, you can limit the number of plugins by centralizing all tasks/specs you want into a dedicated plugin.

See [Reusing functions from other plugins]()
