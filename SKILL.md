---
name: winget-skill
description: Manage Windows applications using winget
---

# winget Package Manager

Automate application lifecycle management on Windows using the official Windows Package Manager CLI (`winget`).

## When to Use

- Searching for software packages by name, ID, or tag.
- Installing, upgrading, or uninstalling Windows applications.
- Managing software sources or exporting/importing system environment manifests.
- Resolving package conflicts, handling silent installations, and bypassing hash check failures when requested.

## Common Operations & Commands

### 1. Package Search
Search for an application using strict or fuzzy match:

```
winget search <package_name>
```

### 2. Package Installation

Install a package by explicit ID (preferred over name to prevent installing ambiguous apps):

```
winget install --id <Package.ID> --exact --accept-package-agreements --accept-source-agreements
```

* **Silent Install:** Add `--silent` or `-h` to avoid UI popups.
* **Custom Location:** Add `-o <path>` or `--location <path>` if requested.

### 3. Package Upgrade

Check for available upgrades across all installed software:

```
winget upgrade
```

Upgrade a specific application:

```
winget upgrade --id <Package.ID> --accept-package-agreements --accept-source-agreements
```

Upgrade all installed applications at once:

```
winget upgrade --all --accept-package-agreements --accept-source-agreements
```

### 4. Package Uninstallation

Uninstall an application by ID or exact name:

```
winget uninstall --id <Package.ID>
```

### 5. Environment Backup & Restore

Export currently installed software to a JSON manifest:

```
winget export -o packages.json
```

Import software list from a JSON manifest:

```
winget import -i packages.json --accept-package-agreements --accept-source-agreements
```

## Critical Rules & Best Practices

1. **Prefer `--id` over free text:** Always resolve the exact `Package.ID` using `winget search` first to avoid installing unintended software (e.g., use `--id Microsoft.VisualStudioCode` instead of installing `vscode`).
2. **Auto-accept Agreements:** Always include `--accept-package-agreements --accept-source-agreements` in non-interactive terminal sessions to avoid prompt blocking.
3. **Elevated Privileges (Admin):** Software requiring system-level installation (MSI/exe installers in `Program Files`) must be run in an administrative terminal session. If execution fails due to permissions, inform the user to re-run in an Admin shell.
4. **Scope Control:** If installing for a single user without admin rights, append `--scope user` if supported by the package manifest.

## Troubleshooting & Edge Cases

* **Multiple Packages Found:** If a search/install query yields ambiguous results, stop and ask the user to specify the exact `Package.ID` from the returned list.
* **Hash Mismatch Failure:** If the downloaded installer fails hash validation, use `--force` to bypass (warn the user about potential security risks first).
* **Source Refresh Issues:** If source catalogs fail to load, reset sources with:

```
winget source reset --force
```

<!-- OB-S:_pmb98J_a7WbzMJmczFpoQ -->
