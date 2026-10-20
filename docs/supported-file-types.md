---
title: "Supported File Types"
description: "Which file types FirmWorks Files previews in place, which open in the Browser's Viewer, how uploads are restricted by type, and the limits that apply."
---
<img src="images/shared/firmworksfiles.svg" alt="FirmWorks Files" width="64" height="64"/>

[Back To Documentation](index.md)

# Supported File Types

FirmWorks Files stores every file as a standard Salesforce file, so any type Salesforce accepts can be uploaded, tagged, searched, shared and downloaded. What differs by type is how the file is **previewed**. This page lists what the viewers can show and the limits involved.

- [How previews work](#how-previews-work)
- [File type reference](#file-type-reference)
- [The Browser's Viewer](#the-browsers-viewer)
- [Restricting upload types](#restricting-upload-types)
- [Experience Cloud](#experience-cloud)

## How previews work

The viewers in File Manager, Content Viewer and Single Content Record Viewer choose how to show a file from its extension:

- **Salesforce renditions.** For PDFs, Office documents and most images, Salesforce generates page images after upload and the viewer shows them page by page. Renditions are generated asynchronously, so a file uploaded moments ago may show "No Preview Available" with a **Retry Preview** button until Salesforce has finished.
- **Native browser playback** for video and audio.
- **Text rendering** for plain text, notes, HTML and data files, read directly from the file.
- **The Browser's Viewer**, a fallback that streams the file to the browser and lets the browser render it. Used automatically for HEIC photos and on request through **Show In Browser's Viewer** for anything else. See [The Browser's Viewer](#the-browsers-viewer).

When none of these can show the file, the viewer says "No Preview Available" and offers a download link.

## File type reference

| Type | Extensions | Preview in place | How | Show In Browser's Viewer |
|---|---|---|---|---|
| PDF | pdf | Yes | Rendition, page by page | Yes |
| Word | doc, docx, dot, dotx, docm, dotm | Yes | Rendition | Only if the browser can render it |
| Excel | xls, xlsx, xlt, xltx, xla, xlsm, xltm, xlam, xlsb | Yes | Rendition | Only if the browser can render it |
| PowerPoint | ppt, pptx, pot, potx, pps, ppsx, ppa, ppam, pptm, potm, ppsm | Yes | Rendition | Only if the browser can render it |
| Images | png, jpg, bmp, tiff, tif, svg | Yes | Rendition, thumbnail fallback | Yes |
| Animated GIF | gif | Yes | Shown directly so the animation plays | Yes |
| HEIC photos | heic | Yes | Always through the Browser's Viewer, converted in the browser when it cannot show HEIC itself | Always |
| Video | mp4, webm, mov, m4v, ogg | Yes | Native player | mp4, webm, ogg, mov |
| Other video | avi, mpg, mpeg | No | | Yes, if the browser can render it |
| Audio | mp3, wav | Yes | Native player | Yes |
| Plain text | txt | Yes | Text, up to 2 MB | Yes |
| Text and data | html, htm, xml, json, csv, md | Yes | Text, up to 2 MB | No |
| Salesforce Notes | snote | Yes | Rich text | No, use **Open In NoteManager** |
| Outlook messages | msg | Metadata only | Subject, from, to, cc, bcc, dates, importance and attachment names are read in the browser. The body is not shown. Up to 2 MB. | No |
| CAD | dxf, dwf, dwg, acd | No | | Yes, if the browser can render it |
| Anything else | | No | "No Preview Available" with Retry and download | No |

Text previews and Outlook messages larger than 2 MB show "Contents too large to display" instead of the content. Download the file to read it.

Notes can be converted to PDF with **Create PDF Copy** in the Note Manager; see [FirmWorks Notes](notes.md#note-actions). No other conversion is built in.

## The Browser's Viewer

**Show In Browser's Viewer** in a file's menu opens the file in an embedded browser frame. The frame fetches the file's bytes and hands them to the browser, which renders what it can: PDFs, images, video and audio reliably, Office documents only when the browser or a plug-in supports them. Large PDFs, video, audio, HEIC photos and Files Connect external files are the usual reasons to use it. If the browser cannot display the file, the frame says so and offers a download.

Requirements:

- The user needs the **API Enabled** permission. Without it the frame shows an "API is disabled" message and a download link. See [Troubleshooting](troubleshooting.md#api-is-disabled-for-this-user).
- The option is not offered for Notes or Outlook messages, which have their own viewers.

Files stored in external systems through Files Connect are fetched through the Files Connect API and displayed the same way.

## Restricting upload types

By default the upload components accept any file. To limit them, set **Allowed File Types** in the configuration's Upload Dialogs settings or on the component, as a comma-separated list of extensions such as `.pdf,.png,.jpg`. Blank, `any` or `all` means no restriction. See [Configuration](configuration.md#step-5-view-settings).

The list is passed to the browser's file picker as a hint, so the picker shows only matching files. It is not enforced on the server. A user who drags a file in, or switches the picker to All Files, can still upload another type. Use a File Report or a [File Event](file-events.md) flow if you need to enforce a type after upload.

**Maximum File Size** in the same settings rejects larger files before upload starts. Salesforce's own size limits still apply.

## Experience Cloud

Previews, downloads and the Browser's Viewer work in Experience Cloud sites, including sites on a custom domain. The same type rules apply. Two points to note:

- The Browser's Viewer, and therefore HEIC previews, need the external user to have **API Enabled**. See [Experience Cloud users](permissions.md#experience-cloud-users).
- Notes are not available in Experience Cloud.
