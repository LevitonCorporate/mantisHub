# MantisHub Repository

## Overview

This repository supports Leviton’s MantisHub implementation, including:

- GitHub ↔ MantisHub Source Integration
- Workflow documentation
- Governance and process documentation
- Change traceability validation

This repository serves as the official source of record for MantisHub configuration and integration documentation within the LevitonCorporate GitHub organization.

---

## GitHub ↔ MantisHub Integration

This repository is connected to MantisHub using the Source Integration plugin.

Integration capabilities:

- Importing commits into MantisHub
- Linking commits to issues via issue references
- Optional automatic issue state transitions

### Commit Reference Format

To link a commit to a Mantis issue, include the issue ID in the commit message:

Fixes #123
Resolves #123
Refs #123
#123

Example:

Fixes #432111 - Updated documentation workflow

---

## Primary Branch

main

---

## Governance

All commits should reference a Mantis issue when applicable to maintain traceability between GitHub and MantisHub.
EOF
