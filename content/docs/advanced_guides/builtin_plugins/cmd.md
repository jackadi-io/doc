---
title: 'Command execution (cmd)'
weight: 2
---

The `cmd` allows you to run shell commands remotely and capture their output.

## Available tasks

### cmd.run

Execute a shell command on a remote node and return the complete output.

#### Syntax
```sh
jack run <node> cmd.run "<command>" [options...]
```

#### Example
```sh
jack run node1 cmd.run "ls -la /tmp"
```

#### Sample Output
```
node1

→ output:
    |
      total 0
      drwxrwxrwt 1 root root 32 Sep 10 15:01 .
      drwxr-xr-x 1 root root 24 Sep 10 15:01 ..
      srwxr-xr-x 1 root root  0 Sep 10 15:01 plugin1142520441
```
