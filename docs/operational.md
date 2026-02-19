# Operational Ownership
MantisBT On-Prem to MantisHub Enterprise

## Purpose

This document defines day-to-day operational ownership boundaries between Leviton and the MantisHub vendor.

It describes responsibility allocation for application operation, security, data handling, incident response, and vendor coordination.

---

## Application Administration

### Leviton
- Define application usage standards
- Define project structure
- Define workflow states
- Define role model and permissions
- Manage administrative users
- Approve plugin usage

### MantisHub Vendor
- Host and operate the application
- Enforce role permissions
- Maintain application availability
- Provide administrative interfaces

---

## User and Access Management

### Leviton
- Manage users in identity provider
- Assign users to IdP groups
- Review access periodically
- Handle onboarding and offboarding
- Investigate access issues

### MantisHub Vendor
- Enforce SAML or OIDC authentication
- Map IdP attributes to user profiles
- Enforce session handling
- Prevent local password usage

---

## Authentication and Authorization

### Leviton
- Maintain IdP configuration
- Rotate IdP secrets and certificates
- Define authentication policies
- Validate role mappings

### MantisHub Vendor
- Implement authentication protocols
- Validate assertions and tokens
- Enforce authorization rules within the application

---

## Application Configuration

### Leviton
- Configure projects and categories
- Configure custom fields
- Configure notification rules
- Configure workflows

### MantisHub Vendor
- Persist configuration
- Validate configuration integrity
- Apply configuration changes safely

---

## Plugin Management

### Leviton
- Select supported plugins
- Approve plugin enablement
- Validate workflows affected by plugins

### MantisHub Vendor
- Maintain plugin compatibility
- Manage plugin lifecycle
- Apply plugin updates

---

## Data Management

### Leviton
- Define data usage requirements
- Define retention expectations
- Request exports
- Validate data completeness after migration

### MantisHub Vendor
- Store application data
- Manage database operations
- Provide supported export mechanisms
- Handle data deletion upon request

---

## Backup and Restore

### Leviton
- Define RPO and RTO requirements
- Request restore testing evidence
- Coordinate restore requests during incidents

### MantisHub Vendor
- Execute backups
- Manage backup retention
- Provide point-in-time restore capability
- Execute restores when requested

---

## Monitoring and Alerting

### Leviton
- Define alerting expectations
- Receive outage notifications
- Escalate incidents internally

### MantisHub Vendor
- Monitor application health
- Detect infrastructure and application failures
- Notify Leviton of incidents
- Maintain monitoring systems

---

## Incident Management

### Leviton
- Classify incidents
- Manage internal communications
- Handle regulatory or legal notifications
- Coordinate business response

### MantisHub Vendor
- Detect and respond to incidents
- Perform containment and remediation
- Provide incident reports
- Support root cause analysis

---

## Change and Release Management

### Leviton
- Communicate change windows
- Validate post-change functionality
- Coordinate user communication

### MantisHub Vendor
- Perform application upgrades
- Apply security patches
- Schedule maintenance windows
- Roll back failed changes

---

## Security Operations

### Leviton
- Define security requirements
- Perform vendor risk reviews
- Review audit logs
- Investigate security events

### MantisHub Vendor
- Secure infrastructure
- Patch vulnerabilities
- Maintain security controls
- Provide security documentation

---

## Compliance and Audit

### Leviton
- Define compliance scope
- Coordinate internal audits
- Review vendor evidence

### MantisHub Vendor
- Maintain SOC 2 Type II
- Maintain GDPR controls
- Provide compliance artifacts
- Support audit requests

---

## Integrations and APIs

### Leviton
- Build and maintain integrations
- Manage API token usage
- Rotate API tokens
- Monitor integration failures

### MantisHub Vendor
- Provide API availability
- Enforce rate limits
- Maintain API stability

---

## Reporting and Exports

### Leviton
- Define reporting requirements
- Schedule exports
- Consume exported data
- Maintain downstream pipelines

### MantisHub Vendor
- Provide export endpoints
- Execute exports
- Maintain export reliability

---

## Vendor Relationship Management

### Leviton
- Manage contract
- Manage renewals
- Open support tickets
- Track SLA adherence

### MantisHub Vendor
- Provide support
- Meet contractual SLAs
- Communicate service changes
- Provide account management

---

## Exit and Decommissioning

### Leviton
- Define exit requirements
- Request final data export
- Validate data completeness

### MantisHub Vendor
- Provide data exports
- Delete data upon request
- Confirm decommissioning