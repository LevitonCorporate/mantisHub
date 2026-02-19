# Migration Checklist
MantisBT On-Prem to MantisHub Enterprise

## Pre-Migration Inventory

- [ ] Record MantisBT version
- [ ] Record PHP version
- [ ] Record MySQL version
- [ ] Capture `mantis_config_table`
- [ ] Capture custom config overrides (`config_inc.php`)
- [ ] Export list of enabled plugins
- [ ] Export plugin versions
- [ ] Export custom field definitions
- [ ] Export project list and IDs
- [ ] Export user list and role mappings
- [ ] Inventory integrations using direct SQL
- [ ] Inventory cron jobs
- [ ] Inventory filesystem paths used by MantisBT
- [ ] Measure total attachment size
- [ ] Measure database size
- [ ] Identify peak usage windows

---

## Trial Environment Preparation

- [ ] Create MantisHub trial instance
- [ ] Confirm region and data residency
- [ ] Verify admin access
- [ ] Enable REST API
- [ ] Generate API token
- [ ] Confirm attachment upload limits
- [ ] Confirm plugin availability
- [ ] Confirm export capabilities
- [ ] Confirm SSO support level

---

## Authentication and Identity

- [ ] Select SAML or OIDC
- [ ] Export IdP metadata
- [ ] Upload IdP metadata to MantisHub
- [ ] Map IdP attributes to user fields
- [ ] Map IdP groups to roles
- [ ] Test user auto-provisioning
- [ ] Test role assignment
- [ ] Test session timeout
- [ ] Test logout behavior

---

## Database Migration

- [ ] Export schema only
- [ ] Export data
- [ ] Validate row counts
- [ ] Validate character encoding
- [ ] Validate foreign keys
- [ ] Import schema
- [ ] Import data
- [ ] Validate issue counts
- [ ] Validate issue history
- [ ] Validate custom fields
- [ ] Validate project mappings

---

## Attachment Migration

- [ ] Inventory attachment directories
- [ ] Inventory orphaned files
- [ ] Sync attachments to object storage
- [ ] Import attachment metadata
- [ ] Validate attachment links
- [ ] Validate download permissions
- [ ] Validate file integrity
- [ ] Spot-check large files

---

## Plugin and Customization

- [ ] Enable supported plugins
- [ ] Disable unsupported plugins
- [ ] Document plugin gaps
- [ ] Validate workflows impacted by plugins
- [ ] Validate notification rules
- [ ] Validate transitions
- [ ] Validate automation rules

---

## Integration Validation

- [ ] REST API authentication
- [ ] REST API issue create
- [ ] REST API issue update
- [ ] REST API search
- [ ] Webhook delivery
- [ ] Email notifications
- [ ] Export endpoints
- [ ] External reporting pipelines

---

## User Acceptance Validation

- [ ] Create issue
- [ ] Edit issue
- [ ] Change status
- [ ] Assign user
- [ ] Upload attachment
- [ ] Download attachment
- [ ] Search issues
- [ ] Filter issues
- [ ] View history

---

## Cutover Preparation

- [ ] Define freeze window
- [ ] Communicate freeze
- [ ] Disable on-prem writes
- [ ] Perform final data sync
- [ ] Validate final counts
- [ ] Enable MantisHub access
- [ ] Notify users

---

## Post-Cutover Validation

- [ ] Confirm write access
- [ ] Confirm SSO enforcement
- [ ] Confirm notifications
- [ ] Confirm integrations
- [ ] Confirm exports
- [ ] Confirm audit logs
- [ ] Confirm backups active

---

## Rollback Readiness

- [ ] On-prem instance preserved
- [ ] Backup verified
- [ ] Restore procedure documented
- [ ] Rollback communication drafted