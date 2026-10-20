---
title: "For Salesforce Administrators"
description: "What FirmWorks Files gives administrators: search, tagging, viewers, sharing, reporting, Notes, configuration and security."
---
<img src="images/shared/firmworksfiles.svg" alt="FirmWorks Files" width="64" height="64"/>

[Back To Documentation](index.md)

## For Salesforce Administrators

### File Manager — find any file in seconds
- Full-text search across file title and tagged fields (3+ characters, SOSL-backed), combined with
  field filters, related-record filters, number ranges, relative date filters (today, this quarter,
  last N days, custom), Is Null / Not Null on any field, and include/exclude file types.
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
- "Search All Null Values" surfaces documents that slipped through without classification.
- Bookmarkable searches — "Launch Last Search" produces a URL users can share or save.
- **Bulk actions** on any selection from a result set: edit tags with find and replace and revert,
  link and unlink records, change owner, create public links with expiry and passwords, remove links,
  **Download Files** (zipped), **Download Data** (CSV of file metadata), **Download Relationships**,
  and delete. Each is switched on per configuration. See [Bulk Actions](bulk-actions.md).
- **Versions tab** on every file with a viewer and download per version, and optional pruning of old
  versions to recover storage.
- Available as a tab, on any Lightning record page, app page, home page, Experience Cloud page, Flow
  screen, or record action.

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
- Enhanced mode uploads three files at a time with a bounded memory footprint, so thousand-file
  drops stay responsive, and tags ride along with each insert.
- Post-upload actions: close, show results, or jump straight into File Manager.
- Full support for **record types on ContentVersion**, with a utility that keeps record-type picklist
  values in sync (Apex cannot read these natively).

### Content Viewer — files where the work happens
- Drop-in Lightning Web Component for record pages, app pages, Experience Cloud, and Flow screens.
- Shows files for the current record, a specific list of Ids, or a Flow collection.
- Five presentation styles: default tabs, scoped tabs, vertical tabs, carousel, tile, with a
  configurable page size and optional title bars.
- Renders Salesforce Notes as well as files; optional delete button; height and image-width controls.
- Previews PDFs, Office documents, images, HEIC photos, video, audio, text and Outlook message
  details; see [Supported File Types](supported-file-types.md).
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
- **Include View Statistics** adds public link counts, views, downloads and dates to every document
  row, so you can see which shared documents are actually opened.
- Every saved report has a stable **Report Identifier** that components, flows and events reference.
- **Record Reporter** component shows a single record's compliance status on its own page, or inside a
  Flow screen as a validation gate.
- **Report Runner** inside File Manager lets end users run a saved report and then filter the
  resulting documents further.
- "View SOQL Syntax" shows the generated query for review, with Copy SOQL.

### FirmWorks Notes
- A rich note editor built on Salesforce Notes with autosave, tags, search, and related records.
- **Version history** with restore, side-by-side **compare** between versions, duplicate, and
  **Create PDF Copy**.
- Notes appear in File Manager alongside files, so a matter or account has one document timeline.

### Configurator — no-code control of every component
- Visual editor (FirmWorks Files Configurator tab) that writes `File_Viewer_Configuration__mdt` records:
  display fields, filter fields, required fields, read-only fields, dependent visibility, lookup
  sub-queries, default values, allowed filter objects, related schema paths, sharing defaults,
  bulk actions, file detail tabs, report-runner settings, and view settings.
- Create as many configurations as you have audiences; assign one per component instance in App
  Builder from a picklist.
- Clone existing configurations; deactivate without deleting; see **Where This Configuration Is
  Used** before changing one.

### Files Dashboard — the state of the org on one page
- Storage meters, license seats and expiry, permission set assignments, configuration, report and
  schedule counts, automations on files, custom taxonomy fields, untagged files, and the Lightning
  pages that place FirmWorks components, each linked to where it is managed. See
  [Files Dashboard](files-dashboard.md).

### Component Tools — a guided move to Lightning Web Components
- Every component is now an LWC. Component Tools inventories the pages, flows, quick actions and
  tabs still on the retired Aura versions, shows how each setting carries over, and replaces them in
  place with undo. See [Upgrading to Lightning Web Components](component-tools.md).

### Security, permissions, and setup
- Permission sets for **Files User**, **Configurator**, **Reporting**, **Experience User**,
  **File Events**, **Notes User** and **Component Tools**, plus ready-made permission set groups
  (Admin, User, Reporter, Experience User).
- Custom permissions gate every administrative surface so UI reflects what a user can actually do.
- Apex runs `with sharing`; user-supplied HTML is sanitized with DOMPurify.
- Passed repeated AppExchange security reviews (history retained in the repo).
- License management through standard Installed Package "Manage Licenses"; optional modules
  (File Events, Notes) are toggled by feature parameters.
- A Home tab walks admins through licenses, permission sets, configuration, reporting, and support.
- All UI text is in Custom Labels with English, Spanish, Mexican Spanish, and French translations.

---