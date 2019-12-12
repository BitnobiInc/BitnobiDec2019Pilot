Here are the steps and parameters used to create an Azure VM for deploying Bitnobi during April, 2019. The cost for running this VM for two days (mostly idle) was around $5 CAD.

### 1. Start:
* log into https://portal.azure.com with your Azure account
* navigate to: `Home > Virtual machines` and press the `Add` button

### 2. Basics
in the **Basics** section enter the following:
```
Subscription: Free trial
Resource group: Bitnobi-test
VM name: bitnobi-1
Region: East US
Image: Ubuntu Server 18.04 LTS
Size: Standard (2 vcpu, 8 GB RAM)

Admin account:
Username: bitnobi
Password: <your password>

Inbound port rules:
SSH(22)  - to allow remotely connecting to the VM once it is started
```

### 3. Disks
in the **Disks** section enter the following:
```
OS disk type: Premium SSD
```
Press the link `Create and attach a new disk` and enter the following:
```
Disk type: Standard HDD
Name: bitnobi-1_DataDisk_0
Size (GiB): 80
Source type: None (empty disk)
```

### 4. Networking:
in the **Networking** section enter the following:
```
Virtual network: Bitnobi-test-vnet
Subnet: default (10.0.0.0/24)
Public IP: bitnobi-1-ip

NIC network security group: Basic
Public inbound ports: Allow selected ports
Select inbound ports: SSH
Accelereated networking: off
Load Balancing: No
```

### 5. Management:
In the `Management` section enter the following:
```
Enable basic plan for free: Yes

Monitoring: 
Boot diagnostics: On
OS guest diagnostics: Off
Diagnostics storage account: bitnobitestdiag

Identity: off
Auto-shutdown: off
Backup: off
```

### 6. Review and Create
* Review the parameters and press `Create`.
* trial deployment took about 2 minutes.
* note down the IP address for your new VM. This will be needed to connect up to SSH.

### 7. Open more ports
Once the VM is created, go to the `Networking` section of the VM, press the `Add inbound port rule` and enter:
```
Destination port ranges: 8000
Protocol: TCP
Action: Allow
Name: Bitnobi
```
Press the `Save` button and then add another rule:
```
Destination port ranges: 8888
Protocol: TCP
Action: Allow
Name: Jupyter-notebooks
```

### 8. Log into the new VM
Use SSH (e.g. with the `putty` application on Windows, or `terminal` and `ssh` on Mac) to log into your new VM. Now you can continue with the Bitnobi deployment instructions in the [[Deploy Bitnobi]] page.

Note that your user directory will have its home in the allocated data drive (80 GB).
By default, Docker stores its images and containers in /var/lib/docker/containers.
Execute the following commands to verify that this is in your data drive:
```
cd /var/lib/docker
df -H .
```
This drive should indicate a total size of 80 GB.


