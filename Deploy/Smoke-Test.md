# Smoke test
This set of tests should be run after Bitnobi is deployed to a new server to verify that each Bitnobi service and component is working. This covers:
* signin, signout, user authentication
* visibility of dashboard
* workflow server and execution of workflow
* audit log server
* open policy engine
* python code in workflow
* backup and restore scripts

### 1. Direct browser to Bitnobi server
* verify that https: is being used and http: is denied
* no warnings about unsecure content.
* press the `(i)` icon in the upper right hand corner of the page and the `About Bitnobi` dialog box should pop up. Check that we are running the correct version of Bitnobi frontend and backend.

### 2. Signin as admin user, 
* creating an admin user is part of deployment instructions. First user to signup becomes the administrator. 
* verify that admin functions enabled.
* verify that dashboard is visible
* signout and signin

### 3. create a workflow "audit log":  AuditLog -> Result
* verify that preview working correctly, 
* save and run workflow.
* verify that workflow runs to completion.
* open workflow, Apply AuditLog and look at preview to verify that workflow start/end recorded.

### 4. create a workflow "sharing test": Datasource -> Result
* verify that initially no datasources are available. 
* exit workflow editor.
* for workflow named "audit log", click on "Workflow Access Control" button (padlock shaped icon) and use dialog box to select "Default Policy for Root User" and click "Save" button
* edit workflow "sharing test", click on the Datasource element and press "Select Datasource".
* "audit log" should now be available as a data source.

### 5. create a workflow "python test": AuditLog -> Python -> Result
* open Python editor, add in sample template
* press "Execute". should see output in console.
* press "Save", should exit back to workflow editor
* press "Apply Python". Preview should show two columns "name" and "id" with 2 rows.
* connect to Result and press "Apply"
* save and run workflow.
* verify that workflow runs to completion.

### 6. create a report "python test", using "python test" result set
* create a bar chart
* select "name" for x axis, "id" for y axis
* select "sum" for Metric
* should show justin as 1, john as 2.

### 9. signout as admin.

### 10. Perform a Backup and Restore
* SSH to the server running Bitnobi.
* attach to the `bitnobi-be` container and halt all Bitnobi processes with `pm2 delete all` but stay attached to the container and stay in directory /home/Bitnobi-V1.
* run `./bitnobiBackup.sh`. This will create a new sub-directory under "/home/Bitnobi-V1/backup" and use the current date and time in the subdirectory name.
* run `exit` to stop the container and return to host OS.
* remove the docker container with `docker rm demo.bitnobi.com:5043/bitnobi-xxx-2019-pilot`
* start up a new container as in previous deployment
run `./bitnobiRestore.sh` specifying the backup sub-directory that was created earlier. For example `./bitnobiRestore.sh backup/backup-20190503_1218`
* run `./startScript.sh`
* detach from the container and leave it running using the `CTRL-p CTRL-q` key sequence.
* go back to your browser session and refesh the page.
* login as admin user and verify that all previously created workflows are still present.



