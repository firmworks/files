---
title: "FirmWorks Notes"
description: "Tagged, searchable notes built on Salesforce Enhanced Notes, with version compare, restore and PDF conversion."
---
<img src="images/firmworksfiles.svg" alt="FirmWorks Files" height="100"/>

[Back To Documentation](index.md)

# FirmWorks Notes

FirmWorks Notes is a note editor built on Salesforce Enhanced Notes. Notes are stored as Salesforce Files (ContentVersion records of type SNOTE), so they can be tagged with the same custom fields as your documents, searched, shared with records, versioned, and converted to PDF.

- [Setup](#setup)
- [The Note Manager](#the-note-manager)
- [Creating and editing notes](#creating-and-editing-notes)
- [Searching notes](#searching-notes)
- [Tagging notes](#tagging-notes)
- [Version history, compare and restore](#version-history-compare-and-restore)
- [Note actions](#note-actions)
- [Sharing notes with records](#sharing-notes-with-records)
- [Opening a note by URL](#opening-a-note-by-url)
- [Notes on a record page](#notes-on-a-record-page)

## Setup

1. **Enable Enhanced Notes.** Setup > Notes Settings > Enable Notes. Notes are ContentNote records, so this Salesforce setting is required.
2. **Enable the FirmWorks Notes license feature.** FirmWorks turns this on for your org. If it is off, the Note Manager shows "FirmWorks Notes License Was Not Found". Contact <support@getfirmworks.com>.
3. **Assign permissions.** Give users the **FirmWorks Notes User** permission set, or one of the FirmWorks Files Admin, User or Reporter permission set groups, which include it. See [Permissions and Licensing](permissions.md).
4. **Optional: create a configuration.** A [FirmWorks Files Configuration](configuration.md) controls which tag fields appear on notes and which fields can be searched. The Filter Fields step and the Suggested Field Relationships setting apply to Notes. Set the configuration on the component's **FirmWorks Notes Configuration Developer Name** property.

The **Note Manager** tab is part of the FirmWorks Files app. To add it to another app, use Setup > App Manager > (app) > Navigation Items.

Notes are not available to Experience Cloud users.

## The Note Manager

The Note Manager has three areas:

- **Left:** the list of notes, with search and sort controls above it.
- **Center:** the note editor.
- **Right:** Sharing, Related Records or Version History, depending on which panel is open.

On narrow screens and in Salesforce Mobile the list collapses so the editor has room.

Icons across the top:

| Icon | Action |
|---|---|
| Plus | Create New Note |
| Magnifying glass | Show or hide the search panel |
| Sort arrows | Change the sort order of the list |
| Document preview | Show or hide the Sharing panel |

## Creating and editing notes

Click the plus icon to create a note. Give it a title and start typing. Notes autosave as you type; the editor tracks unsaved changes and saves them after a short pause.

**Refresh Current Note (Losing Changes)** reloads the note from the server and discards anything not yet saved.

If another user saves a version of the note you have open, the editor is notified in real time so you do not overwrite their work.

## Searching notes

Open the search panel with the magnifying glass icon.

- **Search Notes For...** searches note titles and content.
- Below the search box, one filter appears for each tag field in your configuration's Filter Fields. Picklists and multi-select picklists show their values, checkboxes offer N/A, Unchecked and Checked, and date fields offer date ranges.

## Tagging notes

Tag fields appear beside the note body. They are the custom Content Version fields chosen in the configuration's Filter Fields step. Values save with the note.

Because notes and files share the same fields, a File Report can include notes, and FileViewer can find notes by their tags. FileViewer excludes notes from results by default through the **Exclude File Types** view setting (`snote`). Remove that value from the configuration to see notes alongside files.

## Version history, compare and restore

Every save of a note creates a new Salesforce file version. Open **Version History** to see them. Each row shows the version number, Content Version Id, when and by whom it was modified, and a text preview.

- **Compare** opens two versions side by side with insertions and deletions highlighted.
- **Restore** makes the selected version the current one. It does this by adding a new version with that content. No history is deleted.

## Note actions

Each note in the list has a row menu:

| Action | What it does |
|---|---|
| Duplicate | Creates a copy of the note, including its tags. |
| Create PDF Copy | Converts the note to a PDF file linked to the same records. The PDF is tagged with the note's field values. Running it again on the same note adds a new version to the existing PDF instead of creating a second file. |
| Show In FileViewer | Opens the File Search tab with this note selected. After Create PDF Copy, the PDF opens the same way. |
| Delete | Deletes the note. |

Salesforce's PDF renderer does not support every font style. The conversion strips font family and size so the PDF renders reliably.

## Sharing notes with records

Click the document preview icon to open the **Sharing** panel. It lists the records the note is linked to and lets you add or remove links. New notes created from a record page are linked to that record with the record's sharing settings.

The **Related Records** tab shows records mentioned in the note. When your configuration has [Suggested Field Relationships](configuration.md#step-10-sharing), related records such as an Opportunity's Account are suggested for one-click linking.

Text in a note that matches a record name shows a **Search** button that opens the Related Records section for that record.

## Opening a note by URL

The Note Manager tab accepts two URL parameters:

| Parameter | Value |
|---|---|
| `c__noteId` | The ContentVersion or ContentDocument Id of the note to open. |
| `c__configurationName` | The developer name of the configuration to apply. |

Example: `/lightning/n/firmworks__FW_Notes?c__noteId=068XXXXXXXXXXXXXXX`

FileViewer's row action for a note uses these parameters to open it in the Note Manager.

## Notes on a record page

The **FirmWorks Notes** component can be added to any Lightning record page, App page, Home page, or as a quick action. On a record page it shows the notes linked to that record and links new notes to it. Set **FirmWorks Notes Configuration Developer Name** to choose the tag fields. See the [Component Reference](component-reference.md#firmworks-notes).
