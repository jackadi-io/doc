---
title: 'System specifications (specs)'
weight: 4
---

The `specs` plugin provides specs management for Jackadi agents.

## Available tasks

### specs:list

List all available specification profiles defined on the agent.

#### Syntax
```sh
jack run <agent> specs:list
```

#### Example
```sh
jack run agent1 specs:list
```

#### Sample Output
```
agent1

→ output:
    - demo
    - production-web
    - database-server
```

### specs:all

Retrieve all specification data from all profiles defined on the agent.

#### Syntax
```sh
jack run <agent> specs:all
```

#### Example
```sh
jack run agent1 specs:all
```

#### Sample Output
```yaml
agent1

→ output:
    demo:
      hardware:
        CPUCores: "4"
        DiskUsage: "42.8"
        Hostname: demo-server-01
        IPAddresses:
        - 192.168.1.10
        - 10.0.0.15
        IsProduction: false
        LastReboot: "2024-01-15T06:30:00Z"
        MemoryGB: "16"
        Services:
        - systemd
        - networkd
        - resolved
      network:
        DNS:
        - 8.8.8.8
        - 1.1.1.1
        Interfaces:
        - IPAddress: 192.168.1.10/24
          MAC: 00:50:56:12:34:56
          Name: eth0
          State: up
          Type: ethernet
        Routes:
        - 0.0.0.0/0 via 192.168.1.1
      os:
        architecture: x86_64
        distribution: opensuse-tumbleweed
        kernel: 6.7.1-1-default
        os: linux
        version: "20240115"
      software:
        critical_packages:
        - name: webserver-pro
          version: 2.4.x-demo
        - name: datastore-engine
          version: 15.x-demo
        last_update: "2024-01-10T14:22:00Z"
        package_manager: zypper
        security_updates: "3"
        total_packages: "2847"
```

### specs:get

Retrieve specifications for a specific profile.

#### Syntax
```sh
jack run <agent> specs:get <profile_name>
```

#### Example
```sh
jack run agent1 specs:get demo
```

#### Sample Output
```yaml
agent1

→ output:
    hardware:
      CPUCores: "4"
      DiskUsage: "42.8"
      Hostname: demo-server-01
      IPAddresses:
      - 192.168.1.10
      - 10.0.0.15
      IsProduction: false
      LastReboot: "2024-01-15T06:30:00Z"
      MemoryGB: "16"
      Services:
      - systemd
      - networkd
      - resolved
    network:
      DNS:
      - 8.8.8.8
      - 1.1.1.1
      Interfaces:
      - IPAddress: 192.168.1.10/24
        MAC: 00:50:56:12:34:56
        Name: eth0
        State: up
        Type: ethernet
      Routes:
      - 0.0.0.0/0 via 192.168.1.1
    os:
      architecture: x86_64
      distribution: opensuse-tumbleweed
      kernel: 6.7.1-1-default
      os: linux
      version: "20240115"
    software:
      critical_packages:
      - name: webserver-pro
        version: 2.4.x-demo
      package_manager: zypper
      security_updates: "3"
      total_packages: "2847"
```
