# MantisHub Repository

## Overview

This repository supports Leviton’s implementation and documentation of the MantisHub platform, including:

* Source Integration (GitHub ↔ MantisHub)
* Workflow configuration
* Project structure
* Documentation and governance
* Integration validation

This repository serves as the official source of record for configuration, documentation, and integration testing related to MantisHub within the LevitonCorporate GitHub organization.

---

## Purpose

The objectives of this repository are to:

1. Document MantisHub configuration and workflows.
2. Validate GitHub Source Integration with MantisHub.
3. Demonstrate commit-to-issue traceability.
4. Maintain documentation for internal governance and operational transparency.

---

## GitHub ↔ MantisHub Integration

This repository is connected to MantisHub using the Source Integration plugin.

Integration capabilities include:

* Importing commits into MantisHub
* Linking commits to issues via issue ID references
* Optional automatic issue status transitions based on commit keywords

### Commit Reference Format

To link a commit to a Mantis issue, use one of the following formats in the commit message:

```
#123
Fixes #123
Resolves #123
Refs #123
```

Example:

```
Fixes #432111 - Updated documentation workflow
```

---

## Branch Strategy

Primary branch:

```
main
```

Additional branches may be used for:

* Documentation updates
* Workflow configuration changes
* Controlled integration testing

---

## Governance

All changes to this repository should:

* Reference a MantisHub issue when applicable
* Follow internal documentation standards
* Support traceability between GitHub commits and MantisHub tickets
