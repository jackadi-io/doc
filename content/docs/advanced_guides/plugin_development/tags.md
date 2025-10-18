---
title: 'Tags'
weight: 6
---

## Jackadi tag

`jackadi` tag is available for both input and output structures. It enables you to customize the keys, similar to `json` tag.

## Tag in Options structure

In this example, a task uses the tag:

```go
type UpgradeOptions struct {
	DryRun          bool `jackadi:"dry-run"`
	SecurityOnly    bool
	ExcludePackages []string
	RebootRequired  bool
	BackupBefore    bool
}

func UpgradeSystem(ctx context.Context, options *UpgradeOptions) (map[string]any, error) {
	// ...
}
```

When calling the task, the key can be either be `DryRun` or `dry-run`:

```sh {filename="task with options using jackadi tags"}
jack run agent1 upgrade_system DryRun=true
{
  "message": "Dry run completed - no actual changes made"
  "status": "dry-run-completed",
  "packages_updated": 47,
  "security_patches": 12,
  "reboot_required": false,
  "duration_seconds": 180,
  "dry_run": true,
}
```

```sh {filename="task with options using jackadi tags"}
jack run agent1 upgrade_system dry-run=true
{
  "message": "Dry run completed - no actual changes made"
  "status": "dry-run-completed",
  "packages_updated": 47,
  "security_patches": 12,
  "reboot_required": false,
  "duration_seconds": 180,
  "dry_run": true,
}
```

## Tag in output structures

Output structures also support `jackadi` tag to customize the output keys:

```go
type User struct {
	ID       int64             `jackadi:"id"`
	Username string            `jackadi:"username"`
	Email    string            `jackadi:"email"`
	Metadata map[string]string `jackadi:"metadata,omitempty"`
}

func FindUserByEmail(email string) (*User, error) {
	// ...
}
```

When calling this task, the keys of the output will be customized:

```sh
jack run agent1 get_users
[
  {
    "id": 1,
    "username": "alice",
    "email": "alice@jackadi.io"
  },
  {
    "id": 2,
    "username": "bob",
    "email": "bob@jackadi.io"
  },
  {
    "id": 3,
    "username": "charlie",
    "email": "charlie@jackadi.io"
  }
]
```