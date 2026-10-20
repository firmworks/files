---
title: "FirmWorks Notes"
description: "Tagged, searchable notes built on Salesforce Enhanced Notes, with version compare, restore and PDF conversion."
---
<img src="images/shared/firmworksfiles.svg" alt="FirmWorks Files" width="64" height="64"/>

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
- [Exporting notes with their tags](#exporting-notes-with-their-tags)
- [Read-only notes](#read-only-notes)
- [Sharing notes with records](#sharing-notes-with-records)
- [Opening a note by URL](#opening-a-note-by-url)
- [Notes on a record page or in the utility bar](#notes-on-a-record-page-or-in-the-utility-bar)

## Setup

1. **Enable Enhanced Notes.** Setup > Notes Settings > Enable Notes. Notes are ContentNote records, so this Salesforce setting is required.
2. **Enable the FirmWorks Notes license feature.** FirmWorks turns this on for your org. If it is off, the Note Manager shows "FirmWorks Notes License Was Not Found". Contact <support@getfirmworks.com>.
3. **Assign permissions.** Give users the **FirmWorks Notes User** permission set, or one of the FirmWorks Files Admin, User or Reporter permission set groups, which include it. See [Permissions and Licensing](permissions.md).
4. **Optional: create a configuration.** A [FirmWorks Files Configuration](configuration.md) controls which tag fields appear on notes and which fields can be searched. The Filter Fields step and the Suggested Field Relationships setting apply to Notes. Set the configuration on the component's **FirmWorks Notes Configuration Developer Name** property. **Component Height** sets a fixed CSS height such as `700px`; left blank, the component fills the window from where it sits.

The **Note Manager** tab is part of the FirmWorks Files app. To add it to another app, use Setup > App Manager > (app) > Navigation Items.

Notes are not available to Experience Cloud users.

## The Note Manager

The Note Manager is one fixed-height frame with two areas:

- **Notes Explorer** on the left: the list of notes, with search and sort controls above it and a count such as "24 notes sorted by Last Modified".
- **Note Editor** on the right: the selected note, with its tags and the Sharing, Related Records and Version History panels.

The explorer can be collapsed to give the editor the full width. On narrow screens and in Salesforce Mobile it collapses by itself.

Controls in the explorer:

| Control | Action |
|---|---|
| Plus | Create New Note |
| Magnifying glass | Show or hide the search panel |
| Sort Items | Change the sort order of the list. **Remember** keeps the choice. |

Controls in the editor:

| Control | Action |
|---|---|
| Auto Save | Turn automatic saving on or off. With it off, use **Save Note**. |
| Show Tags | Show or hide the note's tag fields. |
| Advanced Formatting | Show or hide the extra toolbar options. |
| Show Sharing, Show Versions | Open the Sharing or Version History panel. |
| Note Actions | Duplicate, Create PDF Copy, Show In FileViewer, Delete. |

## Creating and editing notes

Click the plus icon to create a note. Give it a title and start typing. Notes autosave as you type; the editor tracks unsaved changes and saves them after a short pause. A note needs both a title and content before it saves.

Type **@** followed by a record name, for example `@Acme`, to link records as you write. The mention opens the Related Records section with that name searched.

**Refresh Current Note (Losing Changes)** reloads the note from the server and discards anything not yet saved.

If another user saves a version of the note you have open, the editor is notified in real time so you do not overwrite their work.

## Searching notes

Open the search panel with the magnifying glass icon.

- **Search Notes For...** searches note titles and content.
- Below the search box, one filter appears for each tag field in your configuration's Filter Fields. Picklists and multi-select picklists show their values, checkboxes offer N/A, Unchecked and Checked, and date fields offer date ranges.

## Tagging notes

Tag fields appear beside the note body. They are the custom Content Version fields chosen in the configuration's Filter Fields step. Values save with the note.

Because notes and files share the same fields, a File Report can include notes, and File Manager can find notes by their tags. File Manager excludes notes from results by default through the **Exclude File Types** view setting (`snote`). Remove that value from the configuration to see notes alongside files; they show an **Open In NoteManager** button.

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

## Exporting notes with their tags

A [File Report](file-reporting.md) exports notes as a spreadsheet with their tag values and their text.

1. Build a report whose document criteria match the notes you want, for example a tag value that only notes carry. In Step 2, add the tag fields you want as columns.
2. Run the report and switch the results to the **Details** view, one row per note.
3. Open the grid's actions menu and choose **Download Data With Note Contents**.

The download is a CSV named after the report. Each row holds the record the note is linked to, the note's title and Ids, the tag columns you chose, and a final **Note Contents** column with the note's text as plain text, line breaks kept. The menu item appears only in the Details view and only when the results contain notes. Notes you cannot read are exported as "[Note contents unavailable]". Note text is loaded fifty notes at a time with a progress message, so a large export takes a moment.

The same report can be placed on a page or flow screen with [File Report Results](component-reference.md#file-report-results), or [scheduled](file-reporting.md#scheduling-reports).

## Read-only notes

When the configuration's **Check Record Access** setting is on, a note you cannot edit opens read only: the title is locked, there is no Save Note, and the notice "You can view this file but can't edit its details." shows above the editor. Delete is left out of the note menu when you cannot delete the note. See [Read-only files](component-appendix.md#read-only-files) for how access is decided.

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

File Manager's **Open In NoteManager** button uses these parameters to open a note in the Note Manager.

## Notes on a record page or in the utility bar

The **FirmWorks Notes** component can be added to any Lightning record page, App page or Home page. On a record page it shows the notes linked to that record and links new notes to it. Set **FirmWorks Notes Configuration Developer Name** to choose the tag fields and **Component Height** to fix its height. See the [Component Reference](component-reference.md#firmworks-notes).

The same component can be a **utility item**: Setup > App Manager > edit the app > Utility Items > Add Utility Item > FirmWorks Notes. Notes then open in a panel from the utility bar on every page of the app. The utility item takes the same configuration name and Component Height; left blank, the height fills the utility panel. A utility item has no record context, so it shows every note the user can see, and new notes are linked to records through the Related Records panel.
