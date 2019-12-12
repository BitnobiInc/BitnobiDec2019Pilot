The Bitnobi UI displays lists of Workflows, Policies and Reports that you have created and provides a common UI for managing lists of these items.

### Common List Behaviours
* **Property Select Button Row** - a user can hide/unhide list item properties (e.g. Name, Description, User ) by clicking on the corresponding item in the top row of buttons ().
* **Filter Field** - immediately below the property select button row is the filter field. Any text that you enter here will match against the name/description/user columns and only those rows that have matching text will be displayed.
* **Column Sort** - clicking on a column header will sort the list order based on ascending/descending/unsorted order for that column.
* **Common Properties** - lists for Workflows, Policies and Reports will display properties for:
  * `Name` - name assigned to the item.
  * `Description` - description assigned to the item.
  * `User` - the Bitnobi user that created this item.
  * `Created` - the date and time when the item was created.
* **Paging** - if there are a large number of list items, you can page through the list by using the arrows in the lower right hand corner. You can also change the number of items per page.
* **Remember List Settings** - The UI will remember your settings for filtering, sorting, paging but only for your current session. Once you signout and signin again these will revert to the default settings.
* **Create** - button in the lower right hand corner to create a new item (Workflow, Policy or Report respectively)

### Workflow List
In addition to the common properties, Workflow List displays properties for:
* **Progress** - displays a progress bar for workflow execution. If a workflow has not been run, displays an empty bar.
* **Options** - These include:
  * `Open Canvas Editor` - opens the Workflow Canvas Editor
  * `Rename Workflow` - allows the workflow name and description to be changed
  * `Workflow Access Control` - allows policies to be attached to a workflow
  * `Workflow Info` - 
  * `Exportables` - allows setting export capabilities for the other users that use this workflow as a datasource.
  * `Delete Workflow` - 

### Policy List
In addition to the common properties, Policy List displays properties for:
* **Options** - These include:
  * `Edit Policy` - opens the Policy Editor
  * `Policy Info` - 
  * `Delete Policy` - 

### Report List
In addition to the common properties, Report List displays properties for:
* **Options** - These include:
  * `Open Report Canvas Editor` - opens the Report Canvas Editor
  * `Rename Report` - allows the report name and description to be changed
  * `Report Info` - 
  * `Delete Report` - 

