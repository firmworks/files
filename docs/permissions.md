---
title: "Permissions and Licensing"
description: "Package licenses, permission sets and groups, custom permissions, license-controlled features, and Experience Cloud user requirements."
---
<img src="images/shared/firmworksfiles.svg" alt="FirmWorks Files" width="64" height="64"/>

[Back To Documentation](index.md)

# Permissions and Licensing

Everything a user needs before a FirmWorks Files component will work for them: a package license, a permission set or permission set group, and for some features a license option that FirmWorks enables.

- [Assigning licenses](#assigning-licenses)
- [Permission set groups](#permission-set-groups)
- [Permission sets](#permission-sets)
- [Custom permissions](#custom-permissions)
- [Files Dashboard and Component Tools access](#files-dashboard-and-component-tools-access)
- [License-controlled features](#license-controlled-features)
- [Experience Cloud users](#experience-cloud-users)
- [Record types on Content Version](#record-types-on-content-version)

## Assigning licenses

Every user who opens a FirmWorks Files component needs a package license.

**Setup > Installed Packages > FirmWorks Files > Manage Licenses > Add Users**

The **FirmWorks Files Home** tab lists these steps with shortcuts for users who hold the Configurator permission.

## Permission set groups

The quickest way to grant access is a permission set group. Each group bundles the permission sets a role needs.

| Group | Contains | Give it to |
|---|---|---|
| FirmWorks Files Admin | FirmWorks Files, FirmWorks Files Configurator, FirmWorks Files Reporting, FirmWorks Notes User, FirmWorks File Events | Administrators who create configurations and reports |
| FirmWorks Files User | FirmWorks Files, FirmWorks Notes User, FirmWorks File Events | Everyday users who search, tag, upload and share files |
| FirmWorks Files Reporter | FirmWorks Files, FirmWorks Files Reporting, FirmWorks Notes User, FirmWorks File Events | Users who build and schedule File Reports |
| FirmWorks_Files_ExperienceUser | FirmWorks Files - Experience User, FirmWorks File Events | Experience Cloud (external) users |

To assign a group: **Setup > Users > Permission Set Groups > (group) > Manage Assignments > Add Assignments**.

## Permission sets

Use individual permission sets when a group grants more than you want.

| Permission set | Label in Setup | Grants |
|---|---|---|
| FileViewer | FirmWorks Files | The FirmWorks Files app, the FirmWorks Files Home and File Search tabs, all viewer, tagging, upload, sharing and download classes, the Browser Viewer and Batch File Support pages, and read access to the FirmWorks Files Configuration, Report Configuration and Content Record Type Mapping metadata types. |
| FileViewer_Configurator | FirmWorks Files Configurator | The FirmWorks Files Configurator and FirmWorks Files Dashboard tabs, configuration deployment classes, the Configuration References check, and the Record Type utility page. |
| FileViewer_Reporting | FirmWorks Files Reporting | The File Report and FirmWorks Files Dashboard tabs, report building, scheduling and batch classes, and create access to the File Report Event. |
| FileViewer_Component_Tools | FirmWorks Files Component Tools | The FirmWorks Files Component Tools and FirmWorks Files Dashboard tabs and the pages they read metadata through. Not in any permission set group; assign it directly to the administrators who migrate Aura placements. They also need View Setup and Configuration, Customize Application and API Enabled. See [Upgrading to Lightning Web Components](component-tools.md#prerequisites). |
| FileViewer_Experience | FirmWorks Files - Experience User | The classes and pages an external user needs for FileViewer, File Tagger Button For Upload, Record's Content Viewer, File Report Runner For Records, File Report Results, Enhanced Upload and the Browser Viewer. Does not grant the app or the Configurator. |
| NoteViewer | FirmWorks Notes User | The Note Manager tab, the Notes classes, and read access to the FirmWorks Notes Event. |
| EV_FileEvents | FirmWorks File Events | The Content Apex Trigger Setting metadata type, the three file platform events, and the two deleted-document invocable actions. |

All permission sets also grant the custom permission of the same name (next section).

## Custom permissions

The package uses custom permissions to decide what to show, not to secure data. For example the FirmWorks Files Home tab hides the Configurator shortcut from users without the Configurator permission.

| Custom permission | Label | Granted by |
|---|---|---|
| FileViewer_Viewer | FirmWorks Files Viewer | FirmWorks Files |
| FileViewer_Configurator | FirmWorks Files Configurator | FirmWorks Files Configurator |
| FileViewer_Reporting | FirmWorks Files Reporting | FirmWorks Files Reporting |
| FileViewer_Experience | FirmWorks Files Experience Viewer | FirmWorks Files - Experience User |
| FileViewer_Component_Tools | FirmWorks Files Component Tools | FirmWorks Files Component Tools |
| Notes_User | FirmWorks Notes User | FirmWorks Notes User |
| FirmWorks_File_Events | FirmWorks File Events | FirmWorks File Events |

## Files Dashboard and Component Tools access

| Tab | Granted by |
|---|---|
| FirmWorks Files Home | FirmWorks Files, FirmWorks Files - Experience User, FirmWorks Files Configurator, FirmWorks Files Reporting |
| FirmWorks Files Dashboard | FirmWorks Files Configurator, FirmWorks Files Reporting, FirmWorks Files Component Tools. Everyday users with only the FirmWorks Files permission set do not see it. |
| FirmWorks Files Component Tools | FirmWorks Files Component Tools, plus the View Setup and Configuration, Customize Application and API Enabled permissions. |

The dashboard's file counts cover only the files the user can see unless the user also has **Query All Files**. See [Permission Set to Query All Files](known-issues.md#permission-set-to-query-all-files).

There is no separate permission for [bulk actions](bulk-actions.md). They are enabled per configuration and run with the user's own access to the files.

## License-controlled features

Two features are switched on per customer by FirmWorks as part of the license. They cannot be enabled from Setup.

| Feature | When it is off, users see |
|---|---|
| File Platform Events | "FirmWorks File Events Is Not Enabled! Please Contact FirmWorks to Enable this Feature" on the Configurator's File Events tab. The file triggers publish nothing. |
| FirmWorks Notes | "FirmWorks Notes License Was Not Found" in place of the Note Manager. |

Contact <support@getfirmworks.com> to enable either feature.

## Experience Cloud users

External users need:

1. A package license.
2. The **FirmWorks_Files_ExperienceUser** permission set group, or the **FirmWorks Files - Experience User** permission set.
3. **API Enabled** on their profile or permission set if they will use Enhanced Upload or the Browser Viewer. Standard upload and Salesforce's own file previews do not need it.

The Experience User permission set grants every Apex class and Visualforce page the components need. You do not need to add them to profiles by hand.

The Configurator, File Reporting builder, Files Dashboard, Component Tools and FirmWorks Notes are not available to Experience Cloud users.

Files uploaded by Experience Cloud users are always shared with **All Users** visibility so internal users can see them. See [Sharing and Visibility](configuration.md#step-10-sharing) for how to default the share type.

## Record types on Content Version

If you use record types on the Content Version object, picklist values that depend on the record type are not available to Apex. The package works around this by caching the values in the **Content Record Type Mapping** custom metadata type.

Whenever you add a record type or change record-type-specific picklist values, a user with the FirmWorks Files Configurator permission set must refresh the cache:

1. Open `/apex/firmworks__RecordTypeFetcher` in your org.
2. Click **Query Values And Save To Custom Metadata**.
3. Wait for the deployment to finish. The page links to Deployment Status.

Until this is done, users may see stale or missing picklist values for record-type-controlled fields.
