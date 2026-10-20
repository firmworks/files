---
title: "Additional Setup"
description: "Adding tabs to apps, creating tabs, placing components on record pages, and setting up Experience Cloud."
---
<img src="images/shared/firmworksfiles.svg" alt="FirmWorks Files" width="64" height="64"/>

[Back To Documentation](index.md)

# Additional Setup

Setup tasks beyond the [Quick Start Guide](quick-start.md): adding tabs to apps, creating your own tabs, placing components on pages, and setting up Experience Cloud.

- [Adding the FirmWorks Files tabs to an app](#adding-the-firmworks-files-tabs-to-an-app)
- [Creating a new tab](#creating-a-new-tab)
- [Placing components on a record page](#placing-components-on-a-record-page)
- [FirmWorks Files Configurations](#firmworks-files-configurations)
- [Experience Cloud](#experience-cloud)

## Adding the FirmWorks Files tabs to an app

The package includes a **FirmWorks Files** app with six tabs: FirmWorks Files Home, FirmWorks Files Dashboard, File Search, Note Manager, File Report and FirmWorks Files Configurator. A seventh tab, FirmWorks Files Component Tools, is not in the app and is reached from the App Launcher by users with its permission set. To add any of these tabs to another app:

1. Setup > App Manager > find the app > **Edit**.

   ![App Manager](images/advanced-config/tab-setup1.png)

2. Choose **Navigation Items**, move the tab from Available Items to Selected Items, and save.

   ![Navigation items](images/advanced-config/tab-setup2.png)

## Creating a new tab

You can build your own tab around a FirmWorks Files component, for example a File Search tab that uses a specific configuration.

1. Setup > Tabs > Lightning Component Tabs > **New**.
2. Choose the component: `firmworks:fileManager` for search and viewing, `firmworks:contentNotesManager` for notes, `firmworks:filesDashboard` for the dashboard or `firmworks:filesHome` for the landing page. A Tag and Upload button tab uses the Aura `firmworks:FileTaxonomyLauncher`, which is still supported for tabs.
3. Give the tab a label and name, then choose which profiles and apps see it.

![Lightning Component Tab](images/advanced-config/custom-tab-setup1.png)

A configuration named with the tab's API name becomes that tab's default configuration. See [Configuration naming](configuration.md#step-2-name).

## Placing components on a record page

Open the Lightning record page in Lightning App Builder and drag the component from the **Custom - Managed** section of the palette. Every component and setting is described in the [Component Reference](component-reference.md):

| Component | Use it for |
|---|---|
| [FirmWorks File Manager](component-reference.md#firmworks-file-manager) | Searching, viewing, tagging, sharing, bulk actions and downloading a record's files. |
| [File Tagger Button](component-reference.md#file-tagger-button) | A button that opens the Tag and Upload screen. Compact. |
| [File Tagger](component-reference.md#file-tagger) | The Tag and Upload screen directly on the page. |
| [Single Content Record Viewer](component-reference.md#single-content-record-viewer) | Showing one specific file. |
| [Content Viewer](component-reference.md#content-viewer) | Showing all of a record's files as tabs, a carousel or tiles. |
| [File Report Results](component-reference.md#file-report-results) | Showing a saved File Report's results. |
| [Record Reporter](component-reference.md#record-reporter) | Telling users whether this record is compliant with a File Report. |
| [Download Records Files](component-reference.md#download-records-files) | A button to download all of the record's files. |
| [FirmWorks Notes](component-reference.md#firmworks-notes) | Notes linked to the record. |

Repeat for each object's record pages where FirmWorks Files is used. Standard component visibility rules apply to all of them. Pages built before 2.1 carry the Aura versions of these components; the [Component Tools](component-tools.md) tab lists them and replaces them in place.

## FirmWorks Files Configurations

**Configure components with a FirmWorks Files Configuration, not with their individual settings.** Create the configuration once in the Configurator tab and choose it in each component's **Configuration: Name** setting. Configurations control display fields, filter fields, required and read-only fields, conditional visibility, defaults, default filters, sharing, related-record searching, upload behavior, view settings and which actions users see. Most of those have no equivalent setting on the component. See [FirmWorks Files Configuration](configuration.md) and [Use a configuration first](component-reference.md#use-a-configuration-first).

Name a configuration after an object (`Account`) and it becomes the default for that object's record pages and its Tag & Upload quick action, so you may not need to set Configuration: Name on the component at all.

## Experience Cloud

You need an active Experience Cloud site. See Salesforce's [Experience Cloud setup](https://help.salesforce.com/s/articleView?id=sf.networks_setup_maintain_communities.htm&type=5) documentation if you do not have one.

### Users

Give external users a package license and the **FirmWorks_Files_ExperienceUser** permission set group. It contains the Experience User permission set, which grants every Apex class and Visualforce page the components need. You do not need to edit profile class access by hand. Users who will use Enhanced Upload or the Browser Viewer also need **API Enabled**. Details are in [Permissions and Licensing](permissions.md#experience-cloud-users).

### Pages

1. Open Experience Builder and navigate to the page.
2. Open the Components panel and scroll to **Custom Components**.
3. Drag **FirmWorks File Manager**, **File Tagger Button**, **File Tagger**, **Content Viewer**, **File Report Results**, **Record Reporter**, **FirmWorks Notes** or **Download Records Files** onto the page.

![Experience Builder](images/advanced-config/experience-setup1.png)

4. Set **Record Id** to `{!recordId}` on record detail pages. Experience components do not receive the record Id automatically.
5. Choose a **Configuration: Name** rather than filling in the component's field settings, or name a configuration in lower case after the object (for example `account`) to make it the default for that object's detail page.
6. Publish the site.

### Sharing

Files uploaded by Experience Cloud users are always shared with **All Users** visibility so internal users can see them. To let external users see files uploaded internally, those files must have All Users visibility and the user must have access to the linked record. See [File Sharing and Access Explained](known-issues.md#file-sharing-and-access-explained).

### Enhanced Upload

Enhanced Upload embeds a Visualforce page. If your org has clickjack protection enabled for Visualforce pages, add your site's domain as a trusted domain. See [Troubleshooting](troubleshooting.md#enhanced-upload-hangs-on-awaiting-registration).
