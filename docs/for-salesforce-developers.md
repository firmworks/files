---
title: "For Salesforce Developers"
description: "How FirmWorks Files fits a developer toolkit: standard objects, File Events, invocable actions, flow components and deployable configuration."
---
<img src="images/firmworksfiles.svg" alt="FirmWorks Files" width="64" height="64"/>

[Back To Documentation](index.md)


## For Salesforce Developers

### Built on standard objects — nothing to migrate
- Every file is a normal `ContentVersion`; every tag is a normal custom field on it. Reports,
  list views, Data Loader, and existing integrations keep working.
- No custom storage object, no external service dependency, no callouts in the core package.

### File Events — trigger Flows and Apex from file activity
- Packaged triggers on `ContentVersion`, `ContentDocument`, and `ContentDocumentLink` publish platform
  events (`EV_Content_Version_Event__e`, `EV_Content_Document_Event__e`,
  `EV_Content_Document_Link_Event__e`) carrying the action (insert, update, delete, undelete) and the
  relevant Ids.
- Subscribe from Flow, Apex, or any external system via Pub/Sub API — decoupled from the trigger
  transaction.
- Each trigger event is switchable per operation in `EV_Apex_Trigger_Setting__mdt`, and the
  `Service_Class__c` field lets you substitute your own implementation of `EV_ITriggerService`
  without touching the package.
- Invocable actions that solve the "deleted records are gone in Flow" problem: **Fetch Related
  Records For A Deleted Content Document** and **Fetch Latest ContentVersion Record For A Deleted
  Content Document**.
- Six Flow templates ship as starting points: Content Version event, Content Document event, Content
  Document Link event, File Report event, Public Link creation and email, and an Upload & Validate
  screen flow.

### Flow-ready invocables
- **Get Public Links For Files** — create or fetch `ContentDistribution` links for a record's files
  with expiration and password options, ready to merge into an email.
- **Get FirmWorks File Report Results** — run any saved File Report from Flow and branch on records
  with or without matching documents.

### Components with full App Builder and Flow metadata
- `Content Viewer (LWC)` exposes typed properties (record Id, Id collection, configuration name via an
  Apex datasource, tab style, delete toggle, height, image width) on record pages, app pages,
  Experience Cloud, and Flow screens, with a custom property editor for Flow.
- Aura components for File Viewer, Tag & Upload, Record Report, and Notes implement
  `force:hasRecordId`, `lightning:isUrlAddressable`, `lightning:availableForFlowScreens`, and
  Experience Cloud interfaces.
- Flow-screen variants of File Viewer, Tag & Upload, and Record Report accept and return values so
  uploads can be validated before a Flow advances.

### Configuration as deployable metadata
- Every behavior lives in Custom Metadata (`File_Viewer_Configuration__mdt`,
  `Report_Configuration__mdt`, `ContentRecordTypeMapping__mdt`, `EV_Apex_Trigger_Setting__mdt`).
  Build in a sandbox, retrieve, version in source control, deploy with the CLI.
- Configurations reference field API names and JSON, so they survive org refreshes and can be
  generated or audited programmatically.

### Extensibility surface
- Global Apex request/response classes for the invocables (`PublicLinkRequest`, `PublicLinkResult`,
  `FileReportInvocableRequest`, `FileReportInvocableResponse`).
- Global `EV_ITriggerService` interface for plugging custom trigger services into the packaged
  dispatcher.
- Platform events for File Viewer, File Report, and Note Manager activity so external monitoring or
  automation can react to user actions.
- Works with Files Connect external references (`ContentLocation = 'E'`), with implementation notes
  in `docs/filesconnect`.

### Performance and scale
- Server-side paging, configurable results-per-page, and search parameters designed for orgs with
  tens of thousands of documents.
- Scheduled reports run as Batch Apex to stay within governor limits on large data volumes.
- Enhanced upload uses the REST API for large multi-file drops rather than the 10-file Lightning
  limit.

---
