# MantisHub ↔ GitHub Source Integration (Developer Notes)

## Goal

Enable traceability between GitHub code changes and MantisHub issues by:

- Importing commits (changesets) from GitHub into MantisHub.
- Linking commits to Mantis issues via commit message references (e.g., `Fixes #123`).
- Optionally updating issue status based on commit keywords.
- Receiving push events via GitHub Webhooks to reduce manual imports and keep changesets current.

This document describes the **exact configuration**, the **expected behaviors**, and a **verification + troubleshooting checklist**.

---

## Components

### 1) MantisHub: Source Integration plugin
- Owns repository definitions (GitHub repo URL, username, primary branches).
- Maintains branch mappings.
- Imports commits into Mantis as **Changesets**.
- Exposes the webhook endpoint for inbound SCM events.

### 2) GitHub Repository
- The repo whose commits we want to appear in MantisHub.
- Must have a webhook configured to call MantisHub’s Source Integration webhook endpoint.
- Must allow MantisHub to read the repo (public repo = simplest; private repo typically requires GitHub App creds / token setup).

### 3) GitHub Webhook
- Sends push events to MantisHub.
- Should be configured as JSON payload and push-only for minimal noise.

---

## Canonical Webhook Endpoint (MantisHub)

Mantis plugins are routed through `plugin.php`, and the Source Integration plugin registers a `Source/webhook` handler.

**Webhook URL format:** `https://<your-instance>.mantishub.io/plugin.php?page=Source/webhook`
---

**For Leviton MantisHub instance:** `https://leviton.mantishub.io/plugin.php?page=Source/webhook`

### Does the endpoint change?
- The `/plugin.php?page=Source/webhook` portion is stable as long as the Source Integration plugin remains enabled.
- Only the base hostname changes between environments/instances (e.g., `leviton.mantishub.io`).

---

## MantisHub Repository Configuration (Required)

Path:
**MantisHub → Manage → Source Integration → Repositories → (Repository) Manage Repository / Update Repository**

Minimum fields:

- **Type:** GitHub
- **URL:** `https://github.com/<OrgOrUser>/<Repo>.git`
- **GitHub Username:** `<OrgOrUser>`
- **GitHub Repository Name:** `<Repo>`
- **Primary Branches:** `main` (or `*` to allow all branches)

### Branch Mappings
Add mapping for the branch you care about (at minimum `main`):
- **Branch:** `main`
- **Strategy:** choose one consistent strategy (team-dependent)
  - Example: `Explicit Version` or `Furthest Release Date`
- Version / Regex usually left blank unless you’re tying branches/tags to Versions.

### Import Controls
On the repo Manage screen:
- **Import Latest Data**: imports the newest commits.
- **Import Everything**: imports all available commits (use carefully).

Expected on success:
- “Imported X changesets…”

---

## GitHub Webhook Configuration (Required)

Path:
**GitHub → Repo → Settings → Webhooks → Add webhook**

Required values:

- **Payload URL:** `https://leviton.mantishub.io/plugin.php?page=Source/webhook`
- **Content type:** `application/json`
- **SSL verification:** Enabled
- **Events:** `Just the push event`
- **Active:** checked

### Secret
- Optional.
- Only use if you also set the same value in MantisHub repo config:
- MantisHub repo field: **GitHub Webhook Secret**
- GitHub webhook field: **Secret**
- If you’re not 100% sure it’s being validated correctly, leave both blank initially to reduce variables.

---

## Expected Behaviors

### A) Webhook delivery (GitHub)
After any push to the configured repo/branch, GitHub will create a delivery record.

Expected:
- `Request URL` = `https://leviton.mantishub.io/plugin.php?page=Source/webhook`
- `X-Github-Event: push`
- `Content-Type: application/json`
- Delivery has a successful status (GitHub UI shows success).

### B) Changeset creation (MantisHub)
MantisHub should show new commits under:
**Source → Changesets**

Expected:
- New commit appears as a new changeset entry.
- Author/commit message appear.
- Diff/File buttons show changed files.

### C) Issue linkage (commit message parsing)
When a commit message references a Mantis issue ID, MantisHub links the changeset to the issue.

Common patterns:
- `Fixes #123`
- `Resolves #123`
- `Refs #123`
- `#123`

Expected:
- The issue shows linked changeset(s).
- Optional: issue status changes if configured.

---

## Verification Checklist (In Order)

### 1) MantisHub can pull repo
- Go to repo Manage page
- Click **Import Latest Data**
- Confirm it imports commits (non-zero changesets)

✅ If this works, Mantis has repo read access and configuration is valid.

### 2) Webhook is firing
- GitHub → Webhooks → Recent Deliveries
- Confirm a new delivery appears after a push
- Confirm payload includes the new commit SHA

✅ If this works, GitHub → Mantis connectivity exists.

### 3) Webhook triggers changeset creation without manual import
- Push a new commit
- Refresh Mantis **Source → Changesets**
- Confirm the new commit appears **without** clicking Import

✅ If this works, webhook ingestion is working end-to-end.

---

## Known Failure Mode: Webhook Delivers, Changesets Don’t Update

**Symptom:**
- GitHub shows a successful push delivery (payload includes new commit SHA)
- Mantis Changesets page does **not** show the new commit
- Manual **Import Latest Data** *does* bring it in

**Meaning:**
- Webhook endpoint is reachable, but Mantis may not be processing the webhook into an import job automatically, OR it is failing silently.

### Quick Troubleshooting

1) Confirm Content-Type
- Must be `application/json` on GitHub webhook.

2) Confirm repo alignment
- Webhook is configured on the same GitHub repo that MantisHub repository definition points to.

3) Confirm branch alignment
- Pushed branch is included in:
- Primary Branches setting (e.g., `main` or `*`)
- Branch mappings include the pushed branch if required by configuration.

4) Force a delta import
- Click **Import Latest Data**
- If the commit appears after import, repo access is fine and the webhook is the missing link.

5) Check Source plugin / system logs (admin)
- If available, review server/plugin logs around webhook delivery timestamps.

---

## Demonstration Script (Quick)

1) In MantisHub, open **Source → Changesets** in one tab.
2) In GitHub, open **Settings → Webhooks → Recent Deliveries** in one tab.
3) Make a small change (README) and push to `main`.
4) Show:
 - GitHub delivery created for `push` event.
 - Payload includes commit SHA.
5) In MantisHub:
 - Refresh Changesets.
 - If commit appears: webhook ingestion confirmed.
 - If not: click Import Latest Data to show repo access + import works, then log follow-up item to investigate webhook ingestion.

---

## Current Environment Notes (As Configured)

- Repo URL (Mantis): `https://github.com/LevitonCorporate/mantisHub.git`
- Primary Branches: `main`
- Webhook URL (GitHub): `https://leviton.mantishub.io/plugin.php?page=Source/webhook`
- Webhook Content-Type: `application/json`
- Events: Push only

---

## Next Steps (Recommended)

1) Create 2 test issues in a test project (e.g., `DEL_Testing`)
2) Commit with messages:
 - `Refs #<ID> - test linkage`
 - `Fixes #<ID> - test status transition`
3) Confirm:
 - Changesets link to issues
 - Status transitions behave as expected (if enabled)
4) Document the final commit keywords and any project-level configuration required.

---
---