![](./quickStartImages/FirmWorks Files.png)
[Documentation](index.md)

# File Events

File Events creates Salesforce Platform Events to allow you to create business logic based on Salesforce Content record changes. Salesforce does not allow Record Triggered flows on the Files objects, but the Platform Events published from the File Events package are based on the Content Document, Content Version, and Content Document Link. This allows you to create critical business logic based upon Salesforce File inserts, updates and deletions

For reference here is a high level image of the Salesforce File Object Structure and how it works with FirmWorks Files.

![File Events Metadata 1](images/fileevents-salesforce-file-structure.png)

To get the File Events package please contact sales@getfirmworks.com or log a case with [support](https://getfirmworks.com/support/)

For more information on Salesforce Platform Event please see the Salesforce documentation below.

https://developer.salesforce.com/docs/atlas.en-us.platform_events.meta/platform_events/platform_events_intro.htm

## Configuration and Setup

Once the File Events package has been installed you can use it right away. There are no permission sets to share or licenses to manage.

## File Event Management

Specific Salesforce object events (before/after, insert, update, delete) can be disabled via an included custom metadata configuration record. This ability to turn on and off the generation of Platform events based on Content object's DML is useful to control your business process work flows (e.g. bulk data updates).


The included Custom Metadata Type to control the Platform Event generation is called 'Apex Trigger Setting'. To get to the Custom Metadata record - Enter Salesforce Setup and Type Metadata in the Quick Find and then select Custom Metadata Types. Click 'Manage records' to the left of the 'Apex Trigger Setting' Custom Metadata Type.


![File Events Metadata 1](images/fileevents-fem-metadata1.png)

There are three records in this Metadata Type, one for each sObject Involved in Salesforce Files; Content Document, Content Version, and Content Document Link. Each record controls the trigger for that object and when it will publish the Platform Event.

To Fully Disable a File from creating a Platform Event uncheck all of the action type boxes (After Insert, After Update, After Delete, After Undelete), then save the record. The Example below would mean that we would not be creating Platform Events for the Content Version Object, thus automation could not be build from it using the File Events Package.

![File Events Metadata 2](images/fileevents-fem-metadata2.png)

In the next example, we would only get Platform Events when a Content Document link was created or deleted.

![File Events Metadata 3](images/fileevents-fem-metadata3.png)

Once we have established which Platform Events to get published we can continue on to building automation using Salesforce Platform Event Triggered Flows.


## File Event Flows

Salesforce publishes excellent learning materials on the uses of Flows - Check out this free Trailhead before continuing. [
https://trailhead.salesforce.com/content/learn/trails/automate_business_processes](
https://trailhead.salesforce.com/content/learn/trails/automate_business_processes)



Platform Event Triggered flows are just like a record triggered flow but the record is just one of File Events Platform Events instead of a sObject.


To create a new Platform Event Triggered Flow go to Salesforce Setup and type Flows into the Quick Find and select Flows. then Click New Flow and Choose Platform Event Triggered Flow.  Click Create to Start Building your new Flow.


![File Events  Flows 1](images/fileevents-fef-flows1.png)

You will need to select one of the following Platform Events to start building automation with:


![File Events  Flows 2](images/fileevents-fef-flows2.png)

- **Content Document Event** - Use this event if you need to trigger automation when a Content Document is Created, Updated, Deleted, or Undeleted. This could be used when you want to change a File's Title or take an action when a File is deleted.
- **Content Document Link Event** - Use this event if yu need to trigger automation when a Content Document Link is Created. A Content Document Link connects a Salesforce record (like an Account, Case or custom objects) to a Content Document. This event could be used to take an action on a related object when a file is uploaded to it or to adjust/default a ContentVersion field based on the object it is uploaded to.  An important note - Content document links deletion events do not always fire, undelete is always unsupported.

- **Content Version Event** - Use this event if you want to trigger automation when a Content Version is Updated. This could be used to trigger automation based on a File Tag or to build or update an Analog Object to use Salesforce reporting on your tags. An important note - Content Version events do not fire insert, delete and undelete events - track these events with the Content Document events instead.


Once you have selected which Platform Event to use, you can then continue building your automation like you would with a Record Triggered Flow. The next section will describe the fields on the three Platform Events in depth.


## File Event Platform Event Glossary

### Content Document Event

Action - This field will tell you what context the platform event was created in. It can be one of the following values:

- afterInsert
- afterUpdate
- afterDelete
- afterUndelete

Content Document Id - This is the Content Document Id that created the Platform Event.

### Content Document Link Event

Action - This field will tell you what context the platform event was created in. It can be one of the following values:

- afterInsert
- afterUpdate
- afterDelete *Salesforce doesn't fire this event in all circumstances - for instance if a file is deleted there is no cascade delete events for all of its content document links.

- afterUndelete *Not Available


Content Document Id - This is the Content Document Id that created the Platform Event.

Content Document Link Id - This is the Id of the Content Document Link. This is a junction object that connects a Content Document and any other File enabled sObject.

Linked Entity Id - This is the Id for the sObject that is connected to the Content Document via the Content Document Link. This is a polymorphic lookup so it can be any object that allows files. In a Flow that uses this Id you will need to ascertain what the Object Name is before doing Get Elements to prevent errors.

### Content Version Event

Action - This field will tell you what context the platform event was created in. It can be one of the following values:

- afterInsert *Use ContentDocument insert event instead

- afterUpdate
- afterDelete *Use ContentDocument delete event instead

- afterUndelete *This currently is unsupported


Content Document Id - This is the Content Document Id that created the Platform Event.

Content Version Id - This is the Id of the Content Version. The Content Version is where the Tagging Data is stored in Salesforce. Use this object to view and update tags on the Files.

## Considerations When Using File Events

- If you are using Bulk Upload feature within FirmWorks Files (below version 0.26) you will not be able to use the Content Version Event and will need to turn fully deactivate the Content Version Apex Trigger Setting. If you do not, it could result in the following error:


![File Events  Flows 1](images/fileevents-considerations1.png)