<img src="images/firmworksfiles.svg" alt="FirmWorks Files" height="200"/>

[Back To Documentation](index.md)

# Tips And Known Issues

## Known Issues

### Filter Lookup Limited Support

Filtered Lookups are not currently supported in the following FirmWorks Files components/views:

- FirmWorks Files FileViewer - Tile View
- File Tagger Button for Upload

This is due to an issue with how Salesforce allows us to access filtered lookup criteria via apex

## Tips

### Permission Set to Query All Files

If you are in a FirmWorks Files component and think you may not be seeing all Files in your search criteria, that could be the case! You mmy need to give you or a sub set of your users a specific permission.

NOTE: This permission can assigned to Users with View All Data permission and it will allow the user the ability to query ContentDocument and ContentVersion and retrieve all files in the org. Please be careful who you give this permission to

In order to enable this follow the Salesforce article here: https://help.salesforce.com/s/articleView?id=000381258&type=1

### Salesforce Images Low Quality Render

#### Occasionally Salesforce will render poor quality images of a document (for instance a pdf)

![Poor Quality Image](images/knownissues/imagequality/badrender.png)

Luckily Salesforce provides a way to regenerate images.

1. First we need to get the Content Document Id of the File
    - This can be done in a number of ways - the easiest is to use Salesforce's file image to open the modal window, navigate to view document and get the id from the url.
    - ![Find The Content Document Id](images/knownissues/imagequality/getcontentdocumentid.gif)
2. Give the content id to an administrator or a someone with access to setup.
3. From within the setup section
    1. Go to regenerate previews
    2. Enter the id and click 'Regenerate Preview'
    3. Wait a few minutes for the image to regenerate.


![Regenerate Images](images/knownissues/imagequality/regenerate-preview.gif)


#### Using The Alternate Viewer

If using Google's Chrome browser window and you have permissions (check with your internal security officer first) you can use an extension such as:

<span style="font-size:larger;color:darkred; background-color:lightyellow">
***IMPORTANT! FirmWorks does not support or endorse any browser extensions. The following extension does not publish security or privacy information and there for by installing it you are using it at your own risk!***</span>

- [PDF-Viewer](https://chrome.google.com/webstore/detail/pdf-viewer/oemmndcbldboiebfnladdacbdfmadadm/related?hl=en-US) to open pdf's in screen with FileViewer's Alternate Viewer button.

![Using Browser Extensions](images/knownissues/imagequality/browser-extension.gif)

#### File Sharing and Access Explained

Access to a Salesforce Files is determine by two different aspects of the associated Content Document Links:

- **ShareType** - The determined is how the user can see the files. The option selected will determine if the file is visible to the user.
    - **V (Viewer in FileViewer)** - Viewer Permission: the user can only view the file but not update tags or edit the
    - **I (Record in FileViewer)** - Inferred Permission: The user can interact with the file based on how they can interact with the record associated in teh Content Document Link. If they have read permission to the object that is linked to the document they can view the file but not edit it. If they have read/write permission to the object, they can view and edit tags on the document.

- **Visibility** - This determines if the document is visible to internal users or internal and external users based on the option selected. There are exceptions to visible that can be found on the [Salesforce Documentation](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_contentdocumentlink.htm).
    - AllUsers - The file is available to everyone with access  based on the ShareType
    - InternalUsers - The file is available to only infernal user who have access based on the ShareType.

#### Salesforce Files Object Structure

Below is an Entity Relationship Diagram (ERD) of the Salesforce Files Schema. This is not all of them but it contains the majority of the relvant object for FileViewer use.

![Salesforce Files ERD](images/knownissues/salesforce-files-erd.png)