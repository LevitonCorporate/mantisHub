# MantisBT → MantisHub Enterprise Migration Proposal

**Project Scope**
Validate that MantisHub can fully replace our self-hosted (on-prem) MantisBT—matching functionality, performance, security, and compliance—so we can justify an Enterprise subscription.

---

## Table of Contents

1. [Environment Parity](#1-environment-parity)
2. [Trial Terms &amp; Conditions](#2-trial-terms--conditions)
3. [Enterprise Subscription Highlights](#3-enterprise-subscription-highlights)
4. [Key Differences — Why On-Prem Is Outdated](#4-key-differences--why-on-prem-is-outdated)
5. [Cross-Team Access &amp; Compliance](#5-cross-team-access--compliance)
6. [Single Sign-On (SSO) Integration](#6-single-sign-on-sso-integration)
7. [Customizations &amp; Plugin Audit](#7-customizations--plugin-audit)
8. [Migration Mechanisms](#8-migration-mechanisms)
9. [Detailed Migration Tasks](#9-detailed-migration-tasks)
10. [Operational Considerations](#10-operational-considerations)
11. [Conclusion &amp; Final Assessment](#11-conclusion--final-assessment)

---

## 1. Environment Parity

- **Version Match** – Select identical major.minor release (e.g., 2.25.10).
- **Schema & Custom Tables** – `mysqldump --no-data` all `mantis_*` + integration tables; import to cloud.
- **Plugins** – List from `mantis_plugin_table`; upload/enable each in MantisHub.
- **Config** – Export key rows from `mantis_config_table`; re-apply in cloud.
- **Attachments** – Sync `bug-attachments/` & `project-attachments/` to object storage; update `mc_attachment_path`.
- **Cron / Scheduled Jobs** – Recreate on-prem cron tasks in cloud scheduler.

---

## 2. Trial Terms & Conditions

| Item                | Value                                              |
| ------------------- | -------------------------------------------------- |
| Duration            | 30 days (write access)                             |
| Seats               | Up to 30 users                                     |
| Storage             | 5 GB total                                         |
| Features            | Enterprise-level (DB import, unlimited API)        |
| Limitations         | No SLA; non-extendable; write disables after trial |
| Export (post-trial) | DB dump or REST/SOAP API                           |

---

## 3. Enterprise Subscription Highlights

| Feature / Capacity                                              | What It Provides in Practice                                                                                                  | Why We**Need** It (Cost Justification)                                                                                                     |
| --------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Unlimited seats**                                       | Every Leviton dev, QA, PM, Cyber-Sec and vendor can open tickets without extra license requests or reallocations.             | Prevents the annual license-juggling cycle and eliminates “seat starvation,” which delays bug reporting (lost productivity > cost difference). |
| **Unlimited storage**                                     | Attach any file type—large PDFs, log bundles, design assets—without cleanup scripts or quota policing.                      | Stops engineering time spent pruning old attachments and removes the risk of lost evidence for defect RCA.                                       |
| **Dedicated container (4 vCPU / 32 GB RAM, auto-scales)** | Guarantees predictable performance even during release crunches and bulk API imports; scales to 8 vCPU / 64 GB automatically. | Replaces ~$15 k hardware refresh every 2-3 years and the admin time to resize VMs when usage spikes.                                             |
| **99.9 % uptime SLA & 24×7 support (1-hr critical)**     | Vendor monitors, patches and rolls back infra; Sev-1 tickets answered within 60 min, any time zone.                           | Engineering downtime averages\$750/hr (blended). One avoided 4-hour outage/year covers the annual subscription delta.                            |
| **Geo-redundant backups, 5-min RPO, 30-day PITR**         | Continuous replication across regions; restore any point in last 30 days in < 15 min.                                         | Removes custom backup scripts, off-site storage fees and legal exposure from data loss (penalties >\$10 k/incident).                             |
| **Native SAML & OIDC SSO**                                | One-click login; roles mapped from AD/Okta groups; zero passwords stored in MantisHub.                                        | Slashes help-desk tickets (~\$20 per reset) and eliminates security risk of stale local accounts.                                                |
| **SOC 2 Type II & GDPR attestation**                      | Pre-audited controls, encryption-at-rest, access-log retention (1 yr).                                                        | Replaces internal audit prep (≈ 40 hrs/yr) and shields against non-compliance fines.                                                            |
| **Audit-trail exports & BI-ready reporting API**          | Push issue metrics to Power BI/Tableau in JSON/CSV without direct DB queries.                                                 | Saves sprint time otherwise spent writing custom SQL reports; enables real-time KPIs for leadership.                                             |
| **White-label plugins & branding**                        | Vendor maintains Leviton-branded dashboards, PDF exports and custom add-ons.                                                  | Delivers polished client-facing look without dedicating front-end cycles (≈\$100/hr design/dev).                                                |

---

## 4. Key Differences — Why On-Prem Is Outdated

| Area                          | On-Prem Reality (Aging)                     | MantisHub Enterprise Advantage                  |
| ----------------------------- | ------------------------------------------- | ----------------------------------------------- |
| **Upgrades & Patching** | Manual, weekend downtime, quarterly cadence | Zero-downtime auto-updates (minor & major)      |
| **Infrastructure**      | Single VM, local RAID, no HA                | HA container, auto-scales, geo-redundant        |
| **Storage**             | 80 % capacity, manual pruning               | Unlimited object-store attachments              |
| **Backups**             | Nightly `mysqldump` to local NAS          | Geo-redundant, 5-min RPO, self-service restores |
| **SSO**                 | Legacy plugin, breaks on IdP updates        | Native SAML + OIDC with role mapping            |
| **Compliance**          | Ad-hoc log retention, manual audit prep     | SOC 2 Type II, GDPR, exportable audit trails    |
| **Plugin Lifecycle**    | Manual pull & patch, downtime for updates   | Vendor-managed lifecycle, white-label support   |
| **Cost Predictability** | CapEx spikes for hardware refresh           | Flat OpEx subscription, no hardware upkeep      |

---

## 5. Cross-Team Access & Compliance

- **Cybersecurity** – Admin login + API token; enable detailed logs; share TLS config for scans.
- **Legal / Privacy** – Provide signed ToS, DPA, SOC 2, GDPR certs; review backup retention.
- **Risk / Governance** – Verify encryption-at-rest, data residency (e.g., US-East1); log config changes.
- **Stakeholder Comms** – Read-only dashboards, scheduled email reports, Slack notifications for “Legal Review” status.

---

## 6. Single Sign-On (SSO) Integration

1. **Upload** IdP metadata (SAML) **or** OIDC issuer/credentials.
2. **Map** attributes: `email`, `displayName`.
3. **Assign** groups → roles (e.g., “MantisAdmins” → administrator).
4. **Test** provisioning and session timeout.

---

## 7. Customizations & Plugin Audit

- Export `mantis_custom_field_*`, `mantis_config_table`.
- Re-enable supported plugins; plan replacements for unsupported ones.
- Convert Drupal SQL calls to REST API endpoints.

---

## 8. Migration Mechanisms

| Path             | Description                                        | Tier Needed           |
| ---------------- | -------------------------------------------------- | --------------------- |
| DB-dump import   | Full schema + data via `mysqldump` into cloud DB | Platinum / Enterprise |
| API / CSV import | CSV/JSON →`mantisbt-importer` / REST API        | Gold +                |

---

## 9. Detailed Migration Tasks

1. **Issues** – Import `mantis_bug_*`; verify counts & sample histories.
2. **Attachments** – Import file tables; upload via API; spot-check downloads.
3. **Users / Roles** – Import user tables; validate SSO roles.
4. **Custom Fields / Config** – Import; test issue creation.
5. **Plugins** – Enable & test workflows.
6. **Notifications** – Import email/webhook settings; send test events.

---

## 10. Migration Execution & Operational Playbook

### 10.1 Pre‑Cutover Checklist

| Check                          | Command / File                                                              | Expected Result                   |
| ------------------------------ | --------------------------------------------------------------------------- | --------------------------------- |
| **Schema Checksum**      | `mysqldump --no-data mantis_db \| sha256sum` on‑prem & cloud              | Identical hashes                  |
| **Plugin Inventory**     | `SELECT plugin_name FROM mantis_plugin_table;`                            | Matches `plugins_allowlist.txt` |
| **Attachment Footprint** | `du -sh /mnt/mantis_files`                                                | ≤ S3 quota                       |
| **SSO Endpoint**         | `curl -I https://cloud/login_saml.php`                                    | HTTP 302                          |
| **API Token**            | `curl -H "Authorization:$TOKEN" https://cloud/api/rest/users?page_size=1` | HTTP 200                          |

---

### 10.2 Migration Execution Scripts

```bash
# db_migration.sh — schema then data
set -euo pipefail
mysqldump -h onprem -u mantis -p --no-data mantis_db > schema.sql
mysqldump -h onprem -u mantis -p --single-transaction --skip-triggers          mantis_db > data.sql
mysql -h cloud -u admin -p cloud_db < schema.sql
mysql -h cloud -u admin -p cloud_db < data.sql
```

```bash
# attachment_sync.sh
aws s3 sync /mnt/mantis_files/ s3://leviton-mantis-attachments/   --storage-class INTELLIGENT_TIERING --exclude "*.tmp"
```

---

### 10.3 Post‑Migration Validation

| Test            | Tool / Step                | Pass Criteria                    |
| --------------- | -------------------------- | -------------------------------- |
| Issue CRUD      | UI create → edit → close | No 500s; workflow states present |
| 1 GB Attachment | Upload & download          | SHA‑256 match; < 60 s each    |
| Email Notify    | Status change → inbox     | Mail in < 15 s                 |
| Slack Webhook   | Status change → Slack     | JSON payload `ok`              |
| Recurring Task  | Wait cron + plugin log     | Auto‑issue created              |

---

### 10.4 Backup & Disaster Recovery

```bash
# /etc/cron.d/mantis_backup – 02:00 UTC daily
0 2 * * * mantis /usr/local/bin/backup_mantis.sh
```

```bash
# backup_mantis.sh
set -euo pipefail
TS=$(date +%F_%H%M)
mysqldump -u mantis -p'StrongPass' mantis_db | gzip > /tmp/db_$TS.sql.gz
aws s3 cp /tmp/db_$TS.sql.gz s3://leviton-mantis-backups/
aws s3 sync /mnt/mantis_files/ s3://leviton-mantis-file-backups/
```

> **Quarterly Restore Drill:** Import latest dump to temp RDS; verify row counts & attachments.

---

### 10.5 Monitoring & Alerting

```yaml
scrape_configs:
 - job_name: mantis_cloud
   static_configs:
     - targets: ['cloud:9100','cloud-db:9104']
```

```yaml
groups:
- name: mantis_alerts
 rules:
 - alert: HighCPU
   expr: 100 - avg by(instance)(irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100 > 80
   for: 5m
```

Alertmanager posts to **#infra-alerts** Slack.

---

### 10.6 Network & Security Hardening

- Ingress 443 only from corp CIDRs `203.0.113.0/24`, `198.51.100.0/24`
- Egress limited to SMTP 25/443 + Slack webhook IPs
- WAF rule: block uploads > 2 GB, strip exploit patterns
- Forward audit logs to S3 prefix `audit-logs/`

---

### 10.7 Change Management & Training

- **Runbook Repo:** `github.com/leviton/mantis-docs`
- `/runbooks/restore.md`
- `/runbooks/upgrade.md`
- **Cut‑Over Comms:** Slack `#release-alerts` + email `dev@leviton.com`
- **User Training:** 30‑min Loom walkthrough + live Q&A; link in Confluence

---

## 11. Conclusion & Final Assessment

MantisHub Enterprise eliminates upgrade headaches, scaling limits, and compliance gaps in our aging on-prem setup while providing predictable OpEx, enterprise-grade SLAs, and unlimited growth headroom. Adopting it positions our team for faster delivery, stronger security, and lower total cost of ownership.
