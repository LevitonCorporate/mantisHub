# Architecture Review
MantisBT On-Prem to MantisHub Enterprise

## Purpose

This document records the architecture of the existing on-prem MantisBT deployment and the proposed MantisHub Enterprise deployment.

It documents:
- System structure and dependencies
- Differences in architecture and control boundaries
- Known constraints and tradeoffs
- Items validated during trial versus vendor-provided guarantees

This document does not provide recommendations or final decisions.

---

## Current Architecture (On-Prem)

### System Overview
- Application: MantisBT
- Hosting: On-prem virtual machine
- Database: MySQL hosted on the same VM
- File storage: Local filesystem
- Availability: Single-node deployment

### Characteristics
- Application, database, and storage are co-located
- Scaling requires manual VM resizing
- Maintenance and upgrades require scheduled downtime
- Backup and restore processes are custom and manually operated

---

## Target Architecture (MantisHub Enterprise)

### System Overview
- Application: MantisHub (vendor managed)
- Hosting: Dedicated containerized environment
- Database: Managed relational database
- File storage: Object storage
- Availability: Multi-node, vendor-managed

### Characteristics
- Compute, database, and storage are managed as separate services
- Upgrades and patching are performed by the vendor
- Monitoring and alerting are vendor-managed
- Uptime and support are governed by contract

---

## Authentication and Identity

### On-Prem
- Plugin-based SSO integration
- Local user records maintained in application database
- Role assignment handled within MantisBT

### MantisHub
- Native SAML or OIDC integration
- User identity sourced from external IdP
- Role mapping based on IdP group membership
- No local password storage

---

## Data and Integration Model

### Database Access

On-prem deployment allows:
- Direct SQL access for reporting and integrations

MantisHub deployment:
- Does not allow direct database access
- Provides REST API and export mechanisms for data access

### Impacted Areas
- SQL-based reports
- Direct database integrations
- Ad hoc data queries

These require redesign using supported interfaces.

---

## File Storage and Attachments

### On-Prem
- Files stored on local disk
- Capacity managed manually
- Backup coupled to filesystem snapshots

### MantisHub
- Files stored in object storage
- Capacity managed by vendor
- Backup handled as part of managed service

---

## Backup and Recovery

### On-Prem
- Scheduled database dumps
- File backups handled separately
- Restore process is manual

### MantisHub
- Automated backups
- Point-in-time recovery supported
- Backup retention and restore processes defined by vendor

---

## Security and Compliance

### Observed Controls
- TLS enforced for transport
- Encryption at rest
- Centralized audit logging

### Artifacts
- SOC 2 Type II documentation
- GDPR alignment documentation
- Data Processing Agreement available from vendor

Full compliance validation is dependent on contract review.

---

## Scalability and Performance

### On-Prem
- Fixed resource allocation
- Manual scaling
- Performance dependent on VM sizing

### MantisHub
- Elastic compute resources
- Storage scales independently
- Performance characteristics managed by vendor

Stress testing under full production load was not performed during trial.

---

## Plugins and Customization

### On-Prem
- Custom and third-party plugins
- Manual installation and updates
- Direct database access available to plugins

### MantisHub
- Vendor-supported plugins only
- Plugin lifecycle managed by vendor
- No direct database access for plugins

Unsupported plugins require replacement or workflow changes.

---

## Risks and Tradeoffs

| Area | Description |
|-----|-------------|
| Database Access | Direct SQL access is not available |
| Plugin Compatibility | Some legacy plugins are unsupported |
| Vendor Dependence | Infrastructure and upgrades are vendor controlled |
| Load Validation | Full production load not validated during trial |

---

## Open Items

- Contractual review of SLA and support terms
- Final plugin compatibility inventory
- Migration sequencing and rollback definition
- Legal and security sign-off