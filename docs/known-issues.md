---
title: "Tips and Known Issues"
description: "Known limitations, and tips on permissions, preview quality, file sharing and the Salesforce Files object model."
---
<img src="images/shared/firmworksfiles.svg" alt="FirmWorks Files" width="64" height="64"/>

[Back To Documentation](index.md)

# Tips And Known Issues

## Known Issues

### Filter Lookup Limited Support

Filtered Lookups are not currently supported in the following FirmWorks Files components/views:

- File Manager - Tile View
- File Tagger Button

This is due to an issue with how Salesforce allows us to access filtered lookup criteria via Apex. Release 0.68 added support for lookup filters and for SOQL sub-queries on lookup filter fields, set in the [Filtering step](configuration.md#step-4-filtering) of a configuration. If a filtered lookup still misbehaves in your org, please report it to <support@getfirmworks.com>.

### Upload type restrictions are a hint, not a rule

**Allowed File Types** narrows the browser's file picker but is not enforced on the server, so a user can still upload another type by dragging it in or choosing All Files in the picker. Use a File Report or a File Event flow to catch unwanted types after upload. See [Restricting upload types](supported-file-types.md#restricting-upload-types).

### Quick actions and custom tabs stay on Aura

Component Tools cannot change the component behind a quick action or a custom tab, so the packaged **Tag & Upload** action and any tab you built on an Aura component keep using it. They continue to work. See [What Component Tools cannot change](component-tools.md#what-component-tools-cannot-change).

### Same-screen components missing from a Flow property editor

Flow Builder hands a screen's components to a custom property editor only after the screen has been saved, so a component you just added or renamed on the same screen is not offered as a resource yet. Click **Done** on the screen, then open the screen editor again. See [Seeing same-screen components in the resource list](fileviewer-and-flow.md#seeing-same-screen-components-in-the-resource-list).

## Tips

### Permission Set to Query All Files

If you are in a FirmWorks Files component and think you may not be seeing all Files in your search criteria, that could be the case! You may need to give yourself or a subset of your users a specific permission. The [Files Dashboard](files-dashboard.md) tells you at the top of the page whether you have it, and its file counts cover only the files you can see without it.

NOTE: This permission can be assigned to Users with View All Data permission and it will allow the user the ability to query ContentDocument and ContentVersion and retrieve all files in the org. Please be careful who you give this permission to

In order to enable this follow the Salesforce article here: https://help.salesforce.com/s/articleView?id=000381258&type=1

### Salesforce Images Low Quality Render

#### Occasionally Salesforce will render poor quality images of a document (for instance a pdf)

![Poor Quality Image](images/known-issues/badrender.png)

Luckily Salesforce provides a way to regenerate images.

1. First we need to get the Content Document Id of the File
    - This can be done in a number of ways - the easiest is to use Salesforce's file image to open the modal window, navigate to view document and get the id from the url.
    - ![Find The Content Document Id](images/known-issues/getcontentdocumentid.gif)
2. Give the content id to an administrator or someone with access to Setup.
3. From within the setup section
    1. Go to regenerate previews
    2. Enter the id and click 'Regenerate Preview'
    3. Wait a few minutes for the image to regenerate.


![Regenerate Images](images/known-issues/regenerate-preview.gif)


#### Using the Browser Viewer

Instead of waiting for a regenerated preview, choose **Show In Browser's Viewer** from the file's menu. The Browser Viewer streams the file itself to your browser, so PDFs render with the browser's own viewer at full quality. It requires the API Enabled permission. See [Previewing files](component-appendix.md#previewing-files).

#### File Sharing and Access Explained

Access to a Salesforce Files is determine by two different aspects of the associated Content Document Links:

- **ShareType** - This determines how the user can see the files. The option selected will determine if the file is visible to the user.
    - **V (Viewer in File Manager)** - Viewer Permission: the user can only view the file but not update tags or edit the file
    - **I (Record in File Manager)** - Inferred Permission: The user can interact with the file based on how they can interact with the record associated in the Content Document Link. If they have read permission to the object that is linked to the document they can view the file but not edit it. If they have read/write permission to the object, they can view and edit tags on the document.

- **Visibility** - This determines if the document is visible to internal users or internal and external users based on the option selected. There are exceptions to visible that can be found on the [Salesforce Documentation](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_contentdocumentlink.htm).
    - AllUsers - The file is available to everyone with access  based on the ShareType
    - InternalUsers - The file is available to only internal users who have access based on the ShareType.

#### Salesforce Files Object Structure

Below is an Entity Relationship Diagram (ERD) of the Salesforce Files Schema. This is not all of them but it contains the majority of the relevant objects for File Manager use.

![Salesforce Files ERD](images/known-issues/salesforce-files-erd.png)