This set of tests should be run if Bitnobi is deployed to two servers (or VMs) to verify Federation components are working.

### Prerequisites:
* run the "Smoke Test" on each of the servers to verify that Bitnobi services and components are working correctly.
* get sample csv file from http://demo.bitnobi.com/sample-csv-files/

### Overview:
One of the two Bitnobi servers will act as "Originating Bitnobi" and other server as "Remote Bitnobi".

### Steps:
1. Direct a browser to "Remote Bitnobi" and Sign Up a new user with ID "serviceAccount1", then Signout. Record the password used for later reference.
2. Login as administrator to "Remote Bitnobi" and navigate to `Admin > Resources Management > Users`. Edit the attributes of user "serviceAccount1" and sets the "organization", "role", and "department" attributes to the value "remote-system-access". 
3. Login as administrator to "Originating Bitnobi" and navigate to `Admin > Network Management`. Create a new Bitnobi Server entry for "Remote Bitnobi" populating fields for hostname (or IP address), port number (8000 for default deployment), Description, user "serviceAccount1" and password (assigned during account SignUp) and press `Submit`.
4. An entry should appear in the `Bitnobi Servers` page and status should be "online". A status of "offline" indicates that Originating Bitnobi could not connect to Remote Bitnobi. In this case verify that the hostname, port, userid and password are correct and that there is no firewall blocking access. A status of "forbidden" means that the remote connection was established, but the "serviceAccount1" userid does not have the required "remote-system-access" role.
5. As administrator in "Remote Bitnobi", create a workflow called "sample-100". Edit the workflow and drag over an `Importer` element to the canvas and upload the sample file "out100.csv" then press `Apply`. Attach a `Result` element to the `Importer`, attach the default policy and press `Apply`. The preview area should be populated with 5 columns of sample data. `Save` and `Run` the workflow and verify that it completes. Attach the policy "Default Policy For Root User" to the workflow.
6. Go back to your session as administrator in "Originating Bitnobi". Create a workflow called "federation test" and start editing it.
7. Drag an `External DS` element on the canvas, press the `Open Input Setting` button and this will open a dialog box. Select "Remote Bitnobi" as the remote source, press `Search` and the "sample-100" result set should display in the Available Data list. Press the `->` button on this entry and then `Save`. The dialog box will then close. Press the `Apply` button and 5 columns of data should appear in the `Preview Data` area. This preview data is being relayed from the "Remote Bitnobi" server.
8. Drag a `Data Mover` element onto the canvas, attach it to the `External DS` element, and press `Apply` for the `Data Mover` element. Preview data should be populated.
9. Drag an `Importer` element onto the canvas and upload the sample file "out100.csv" then press `Apply`. Preview data should be populated.
10. Drag a `Union` element onto the canvas and connect the `Data Mover` and `Importer` as inputs then press `Apply`. Preview data should be populated.
11. Attach a `Result` element to the `Union`, attach the default policy and press `Apply`. The preview area should be populated with 5 columns of sample data. `Save` and `Run` the workflow and verify that it completes.


