Federated Bitnobi servers allows a Bitnobi user logged into one Bitnobi server (Originating Bitnobi) to create a workflow that can merge data from one or more Remote Bitnobi servers with access controlled through policies. An example of such a configuration is below:

[[images/bitnobi-federation-configuration-1.png]]

In the diagram above, there are 2 separate organizations (Org1, Org2) that have each deployed a Bitnobi server. Each organization has its own private databases and needs to protect this data from getting into the hands of unauthorized users.
There are data owners in each organization that use Bitnobi to set data access policies. Each organization has its own Bitnobi administrator. A Bitnobi user logged into Org1 can create a workflow in Bitnobi-Org1 that merges data from Bitnobi-Org2 as well as local data.

Here are the steps to configure the federation of these servers:

1. The administrator of Bitnobi-Org1 browses to Bitnobi-Org2 and creates a new account. This will become the service account for Bitnobi-Org1 to access Bitnobi-Org2.
2. The administrator of Bitnobi-Org1 contacts the administrator of Bitnobi-Org2 and requests to convert the new account into a remote-system-access account.
3. If the request is approved, the administrator of Bitnobi-Org2 logs into Bitnobi, navigates to "Admin > Resources Management > Users" and sets the "organization", "role", and "department" attributes for the new account to the value "remote-system-access" and notifies the Bitnobi-Org1 administrator when it is done.
4. The administrator of Bitnobi-Org1 logs into Bitnobi-Org1 and navigates to the "Admin > Network Management > Create New" page and creates an entry for connecting to Bitnobi-Org2 by entering a descriptive name for the connection, hostname and port for Bitnobi-Org2, userid and password of the service account and then pressing "Submit". A new entry should now appear in the "Bitnobi Servers" page listing the properties of the new connection, and its status (online or offline). If connection shows an "offline" status then there was a problem connecting to Bitnobi-Org2. Check that the hostname, port, userid and password were entered correctly and fix any typos. 

There is a tutorial video available at http://demo.bitnobi.com/videos/bitnobi-configuring-federation-2019-01-11.mp4 that provides an example of going through the above steps .

