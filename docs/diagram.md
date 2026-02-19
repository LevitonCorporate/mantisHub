# Architecture Diagram

## Current State (On-Prem MantisBT)

[ Users ]
    |
    v
[ MantisBT Web App ]
    |
    +--> [ MySQL Database ]
    |
    +--> [ Local Filesystem ]
           - bug attachments
           - project attachments

- Single VM
- Application, database, and storage co-located
- SSO via plugin
- Backups via scheduled dumps and file copies

---

## Target State (MantisHub Enterprise)

[ Users ]
    |
    v
[ Identity Provider (SAML / OIDC) ]
    |
    v
[ MantisHub Application Layer ]
    |
    +--> [ Managed Database ]
    |
    +--> [ Object Storage ]
           - attachments

- Vendor-managed containerized application
- Database and storage managed as separate services
- Native SSO integration
- Backups and monitoring handled by vendor

---

## Integration Access

- REST API
- Webhooks
- Export endpoints

No direct database access.
