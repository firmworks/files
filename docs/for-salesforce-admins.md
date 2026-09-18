---
title: "For Salesforce Administrators"
description: "What FirmWorks Files gives administrators: search, tagging, viewers, sharing, reporting, Notes, configuration and security."
---
<img src="images/firmworksfiles.svg" alt="FirmWorks Files" height="100"/>

[Back To Documentation](index.md)

## For Salesforce Administrators

### File Viewer — find any file in seconds
- Full-text search across file title and tagged fields (3+ characters, SOSL-backed), combined with
  field filters, related-record filters, relative date ranges (today, this quarter, last year, custom),
  and include/exclude file types.
- Filter by **any record the file is linked to**, across any object you allow — Accounts, Cases,
  custom objects — and follow **related schema paths** (e.g. Account → Opportunities → Contracts) so
  a user sees every document in a hierarchy, not just the ones on the current record.
- List view and tile view, in-page preview via Salesforce renditions, HEIC image support, and a
  "Show in Browser's Viewer" fallback for anything else.
- **Scalable viewer**: read and inspect documents in a resizable viewer directly on the record page.
  No preview modal, no navigating to the file and back; fewer clicks and the user stays in the
  record's context.
- Inline editing of tags directly in the results grid, with "Use Last Modified Values" to speed up
  bulk tagging sessions.
- Resizable, remembered layout: image width, list height, and column widths persist per user.
- "Show Untagged Files" surfaces documents that slipped through without classification.
- Bookmarkable searches — "Launch Last Search" produces a URL users can share or save.
- Bulk actions on a result set: **Download Files** (zipped), **Download Data** (CSV of file metadata),
  **Download Relationships**, **Create Public Links**, **Create Public Links with Passwords**,
  **Remove Public Links**.
- Available as a tab, on any Lightning record page, app page, home page, utility bar, Experience Cloud
  page, Flow screen, or quick action.

### Tag & Upload — files arrive classified
- Global or object-specific quick action: users pick the file's attributes first, then upload, so
  nothing lands untagged.
- **Standard mode** (up to 10 files) and **Enhanced mode** for large drops: select hundreds of files,
  run duplicate detection by title and size, choose "set all as new files" or "set all as new
  versions," remove oversize files in one click.
- Required fields, default values, read-only fields, and **dependent field visibility** (show
  "Contract Type" only when "Category = Legal") — all declared in configuration.
- Sharing decided at upload time: Inferred vs Viewer share type, and Internal vs All Users visibility
  for Experience Cloud, with sensible org-level defaults.
- **Suggested links** propose related records (e.g. the Account behind an Opportunity) so a file can
  be linked to everything it belongs to in one step.
- Post-upload actions: close, show results, or jump straight into File Viewer.
- Full support for **record types on ContentVersion**, with a utility that keeps record-type picklist
  values in sync (Apex cannot read these natively).

### Content Viewer — files where the work happens
- Drop-in Lightning Web Component for record pages, app pages, Experience Cloud, and Flow screens.
- Shows files for the current record, a specific list of Ids, or a Flow collection.
- Five presentation styles: default tabs, scoped tabs, vertical tabs, carousel, tile.
- Renders Salesforce Notes as well as files; optional delete button; height and image-width controls.
- Bound to a named configuration so different pages can show different fields to different audiences.
- **Curated file lists**: show only the files that match a configuration's filter, or files drawn from
  related records, on each record page. Stock Salesforce has no way to curate the Files related list.

### Sharing, links, and distribution
- **Link manager** on every file: see every record it is linked to, add links by searching any allowed
  object, remove links, change owner, and accept suggested links.
- **Public links** (ContentDistribution) with expiration date, optional password, custom link title,
  view count and last-viewed tracking, and one-click copy of link, download link, or password.
- **Download Records Files** component: pull every file on a record as a zip, with a stop button for
  large jobs.
- Per-configuration switches decide which distribution actions each audience can see (create link,
  delete link, download data, download files, sharing details).

### File Reporting — auditing and compliance without SOQL
- A guided four-step report builder: pick an object, filter its records, define one or more **filter
  sets** describing the documents you expect, choose a result type, run.
- Result types answer the compliance question directly: **All Records**, **Records With Documents**,
  or **Records Without Documents** ("which open Opportunities have no signed SOW?").
- Filter sets combine with AND / OR / custom evaluation patterns like `(1 OR 2) AND 3`, plus an
  **AND HAS** mode that requires every filter set to be satisfied by at least one document.
- Grouped and detail views, show/hide files inline, navigate to record or document, open results in
  File Viewer, download the data or the files.
- Save reports, save-as, and **schedule** them. Each scheduled run publishes a platform event
  (`File_Report_Event__e`) with the record Ids and counts, so a Flow can email owners, post to
  Chatter, or create tasks — a template Flow is included.
- **Record Report** component shows a single record's compliance status on its own page, or inside a
  Flow screen as a validation gate.
- **Report Runner** inside File Viewer lets end users run a saved report and then filter the resulting
  documents further.
- "View SOQL Syntax" shows the generated query for review.

### FirmWorks Notes
- A rich note editor built on Salesforce Notes with autosave, tags, search, and related records.
- **Version history** with restore, side-by-side **compare** between versions, duplicate, and
  **Create PDF Copy**.
- Notes appear in File Viewer alongside files, so a matter or account has one document timeline.

### Configurator — no-code control of every component
- Visual editor (File Viewer Configurator tab) that writes `File_Viewer_Configuration__mdt` records:
  display fields, filter fields, required fields, read-only fields, dependent visibility, lookup
  sub-queries, default values, allowed filter objects, related schema paths, sharing defaults,
  distribution actions, report-runner settings, and view settings.
- Create as many configurations as you have audiences; assign one per component instance in App
  Builder from a picklist.
- Clone existing configurations; deactivate without deleting.

### Security, permissions, and setup
- Permission sets for **Files User**, **Configurator**, **Reporting**, **Experience User**,
  **File Events**, and **Notes User**, plus ready-made permission set groups (Admin, User, Reporter,
  Experience User).
- Custom permissions gate every administrative surface so UI reflects what a user can actually do.
- Apex runs `with sharing`; user-supplied HTML is sanitized with DOMPurify.
- Passed repeated AppExchange security reviews (history retained in the repo).
- License management through standard Installed Package "Manage Licenses"; optional modules
  (File Events, Notes) are toggled by feature parameters.
- A Home tab walks admins through licenses, permission sets, configuration, reporting, and support.
- All UI text is in Custom Labels with English, Spanish, Mexican Spanish, and French translations.

---