---
title: 'Standalone executable'
weight: 7
---

## Execute plugin as a standalone binary

You can execute plugins as standalone binary directly from the shell.

This can be used to:
- facilitate plugin development: you don't need Jackadi installed to test your plugins
- avoid Jackadi lock-in: you can execute the plugin in non-Jackadi hosts.

The syntax is similar to `jack` command:

```sh {filename="simple task"}
./my-plugin run task hello
"Hello, World! This is Jackadi distributed task execution."
```

```sh {filename="task with arg"}
./my-plugin run task find_user admin@jackadi.io
{
  "ID": 999,
  "Username": "admin",
  "Email": "admin@jackadi.io",
  "Metadata": {
    "role": "administrator"
  }
}
```

```sh {filename="task with options"}
./my-plugin run task upgrade_system DryRun=true RebootRequired=true
{
  "message": "Dry run completed - no actual changes made",
  "status": "dry-run-completed",
  "packages_updated": 47,
  "security_patches": 12,
  "reboot_required": true,
  "duration_seconds": 180,
  "dry_run": true
}
```