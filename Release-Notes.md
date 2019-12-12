# Release Notes
### Bitnobi December 2019 Pilot release

### Overview:
This distribution of Bitnobi software is intended for customer pilot deployments. The expectation is that the customer will use it for about a month for testing & evaluation and then will undeploy it.
This distribution is not intended for extended enterprise use. 

* For deployment instructions, please see the wiki: https://github.com/BitnobiInc/BitnobiDec2019Pilot
* For customer support issues, please contact: hassan@bitnobi.com


### Key features
* attribute based policies to control data sharing in a multi-tenant data ecosystem.
* workflows perform a sequence of operations to assemble the data to be shared.
* graphical workflow editor 
* workflow operations extensible through Python
* basic data visualization and charting built in.
* supports data sources from SQL databases (currently MySQL, SQL Server and Azure SQL Database), CSV files and JSON files.
* maintains audit log of user actions and datasource accesses. Audit log is accessible by admin user only. Audit log can  be used as a datasource in a workflow for log analysis.
* uses Hashicorp Vault for storing remote Bitnobi passwords and SQL datasource passwords.
* uses Open Policy Agent for authorization and policy management.

### Key features planned for next release:
* interfaces to external packages such as Watson Analytics for data visualization,
and Jupyter Notebooks for algorithm exploration.
* supports federation of multiple Bitnobi servers. A workflow running on one Bitnobi server can merge data from one or more external Bitnobi servers with access controlled through policies.

### Recent changes
* new UI implementation using Angular 7 and Google Material Design components. 
Also uses nginx to serve UI pages and provide a reverse-proxy to the backend container. 
The purpose here was to improve the code maintainability and move away from the 
old Angular.JS framework that had its final release in June 2018.
* this distribution installs everything into two docker containers: one for UI, one for backend.


### Known limitations
* designed to have up to 20 users with max of 5 users simultaneously signed in and actively using Bitnobi. 
* has been load tested with 8 users simultaneously running workflows with 100K row data sources. 
* for reasonable performance we recommend a maximum data set size of around 1 million rows. Has been load tested with a 10 million row dataset.
* has datasource drivers for MySQL, SQL Server and Azure SQL relational database, CSV and JSON files.

