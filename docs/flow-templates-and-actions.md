---
title: "Flow Templates and Invocable Actions"
description: "The six packaged flow templates and four invocable actions, with their inputs and outputs."
---
<img src="images/firmworksfiles.svg" alt="FirmWorks Files" height="100"/>

[Back To Documentation](index.md)

# Flow Templates and Invocable Actions

The package ships six flow templates and four invocable actions for use in Flow Builder. This page lists each with its inputs and outputs. For screen components used inside flows, see [FirmWorks Files and Flows](fileviewer-and-flow.md) and the [Component Reference](component-reference.md).

- [Flow templates](#flow-templates)
- [Invocable actions](#invocable-actions)
  - [Get Public Links For Files](#get-public-links-for-files)
  - [Get FirmWork's File Report Results](#get-firmworks-file-report-results)
  - [Fetch Related Records For A Deleted Content Document](#fetch-related-records-for-a-deleted-content-document)
  - [Fetch Latest ContentVersion Record For A Deleted Content Document](#fetch-latest-contentversion-record-for-a-deleted-content-document)

## Flow templates

Templates appear in Setup > Flows under the **Templates** tab. Open one, **Save As** a new flow, then edit and activate your copy. Templates themselves are not active and should not be.

| Template | Type | Purpose |
|---|---|---|
| FirmWorks Content Document Event Flow Template | Platform event triggered on Content Document Event | Branches on the event action (insert, update, delete, undelete). The delete branch shows how to recover the records a deleted file was linked to, using the two deleted-document actions below. |
| FirmWorks Content Document Link Event Flow Template | Platform event triggered on Content Document Link Event | Branches on the type of record the file was linked to (Account, Opportunity, Case). The Opportunity branch builds a Content Document Link to the parent Account so the file appears there too. |
| FirmWorks Content Version Event Flow Template | Platform event triggered on Content Version Event | Applies a file naming convention and branches on tags (file type, description, first version). Ships intentionally incomplete so you must finish the loop before activating. |
| FirmWorks File Report Event Flow Template | Platform event triggered on File Report Event | Compares the event's Report Name to an input variable and shows two reactions: send an email to the record owner, or post to Chatter on the record. |
| FirmWorks Files Public Link Creation and Send Flow Template | Autolaunched subflow with a `recordId` input | Creates password-protected public links for a record's files that expire in 10 days, builds an HTML list of links, and emails it. Call it from another flow. |
| FirmWorks Files Upload and Validate Screen Flow Template | Screen flow | One screen with File Report Runner For Flow Records above File Upload & Tagger For Flows, so users see what is missing while they upload. Inputs: `recordId`, `varFWFConfigurationName`, `varFWFReportName`. |

Before activating a copy:

- Replace the placeholder email recipient `support@getfirmworks.com` in the File Report and Public Link templates.
- Replace the placeholder Account Id `001000000000000` in the Content Document Link template with a lookup of the Opportunity's Account.
- In the Content Version template, set the loop's collection and decide what the update should do.

The File Events templates require the [File Events](file-events.md) triggers to be enabled, or the flows will never fire.

## Invocable actions

All four actions are in the **Files** category of the Action element. Each accepts and returns collections, so they can be called once for many records.

### Get Public Links For Files

Creates or returns public links (Content Distributions) for files. Use it to send links instead of attachments. See [Create and Send Public Links in a Flow](fileviewer-and-flow.md#create-and-send-public-links-in-a-flow) for a worked example.

Inputs:

| Input | Required | Type | Meaning |
|---|---|---|---|
| Record Id | Yes | Id | A Content Version Id (that file), a Content Document Id (its latest version), or any other record Id (every file linked to the record). |
| FirmWorks Files Configuration Name | No | Text | When Record Id is a record, apply this configuration's Default Filter to choose which files get links. |
| Link Title | No | Text | Title of the public link. Blank uses the file's title. |
| Password Protect Link | No | Boolean | Have Salesforce generate a password. Ignored if the org's Content Deliveries setting forces passwords. |
| Expiration DateTime | No | Date/Time | When the links stop working. |
| Return Most Recent Link | No | Boolean | Reuse an existing link for the file instead of creating a new one. |

Output: **Links**, a collection with one entry per file:

| Field | Meaning |
|---|---|
| Content Distribution Id | The Content Distribution record. |
| Id of the Original Request | The Record Id you passed in. |
| Content Document Id | The file. |
| Content Version Id | The version the link points to. |
| Name | The link title. |
| Password | The generated password, if any. |
| Distribution Public Url | The link to view the file. |
| Content Download Url | A direct download link. Blank when the link is password protected. |
| Expires On | The expiration date/time. |

The action fails with "Public links are not supported" if Content Deliveries are disabled in the org, and with "Invalid Configuration Name" if the configuration does not exist.

### Get FirmWork's File Report Results

Runs a saved [File Report](file-reporting.md) for one record and returns whether the record is in the results. Use it for a headless compliance check inside any flow.

Inputs:

| Input | Required | Type | Meaning |
|---|---|---|---|
| Report Name | Yes | Text | The saved report's name. |
| Record Id | Yes | Id | The record to evaluate. |
| Report Metadata Record Id | No | Text | The report's custom metadata Id. Pass it from a variable rather than hard coding. |

Outputs:

| Output | Type | Meaning |
|---|---|---|
| Has Error | Boolean | True if the report could not run. |
| Error Message | Text | Why. "Unable to locate the requested report" means the name did not match a saved report. |
| Report Result Return Type | Text | The report's record return setting: all, with documents, or without documents. |
| Record Id is Valid For Report | Boolean | True when the record meets the report's object criteria (Step 1 of the report). |
| Result | Boolean | True when the record is valid and is in the report's result set. |
| Documents | Collection | The matching documents, each with Id of ContentVersion, Id of ContentDocument and Title of Document. |

### Fetch Related Records For A Deleted Content Document

Flows cannot query deleted records. When a Content Document Event arrives with action `afterDelete`, this action returns the records the deleted file had been linked to so you can update them.

| Input | Required | Type | Meaning |
|---|---|---|---|
| Content Document Id | Yes | Id | The deleted Content Document. |

| Output | Type | Meaning |
|---|---|---|
| Content Document Id | Id | Echo of the input. |
| Linked Entity Ids | Id collection | The records the file was linked to before deletion. |

Requires read access to Content Document Link. Salesforce restricts queries on this object, so the action runs one query per Content Document and cannot be bulkified further.

### Fetch Latest ContentVersion Record For A Deleted Content Document

Returns the latest version of a deleted file, including its tag field values, so a flow can read what was deleted.

| Input | Required | Type | Meaning |
|---|---|---|---|
| Content Document Id | Yes | Id | The deleted Content Document. |

| Output | Type | Meaning |
|---|---|---|
| Content Document Id | Id | Echo of the input. |
| Content Version | Content Version record | The latest version with every field the running user can read, except the file body. |

Both deleted-document actions require the **FirmWorks File Events** permission set, which is included in every FirmWorks Files permission set group.
