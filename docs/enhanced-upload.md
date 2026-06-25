[Documentation](index.md)

# Enhanced Upload (Formerly Bulk Upload)

This mode requires users to have API access. It performs SOAP calls for files less than 35mb in size and REST calls for files larger than 35mb in size.

## Upload thousands of files in one session

Useful for organizations that need to upload thousands of documents to a record without having to use Salesforce native interface allowing 10-25 files at a time.

## Duplicate Detection And Versioning Control

Detects if a file is already related to a record. Checks the title and size of the document. If there is a match on title - versioning options appear to allow the user to upload as a new document or as a new version of the existing document.

![Duplicate Detection](images/features/upload/duplicate_detect_versioning_options.png)

## Maximum File Size Enforcement

Aids in user experience so that the time to upload extremely large files is not wasted with Server Side validation.

### Client side detection of files that are larger than specified in the FirmWork Files Configuration

![Max File Size](images/features/upload/max_file_size_enforced_in_client.png)

### Warning message complete with the limit

![Max File Size Enforcment](images/features/upload/max_file_size_enforced_in_client_warning.png)

## Easily remove already uploaded files

When selecting all of the files in a local directory - easily remove files that have already been previously uploaded - reducing the need to hunt and select individual files. Simply select all of the files and then remove any possible duplicates with the action menu.

![alt text](images/features/upload/action_menu_remove_files_from_upload.png)


## Troubleshoooting "Awaiting Registration" Hanging

Refer to [Troubleshooting](troubleshooting.md)