# Hardware Requirements
* assumes light evaluation use by a few users.
* minimum of 2 CPU cores, 8GB RAM, 80GB disk space. Can be a VM.
* static IP address or fixed domain name.
* port 3000 must be opened to the network (e.g. via `iptables`). Bitnobi UI uses these ports. If you are running Bitnobi in a VM, you may need to also set up "Port Forwarding" for this port.
* instructions for creating an Azure VM for a Bitnobi Pilot can be found [[here|Azure VM setup]]

# Software Prerequisites
* Ubuntu 18.04 preferred as a base Operating System.
* Docker CE. Bitnobi is distributed as docker images and so docker community edition must be installed on your target server. Docker installation instructions can be found at https://www.docker.com/get-docker .
* make sure no other server processes is using port 3000.
* (optional) access to an SMTP email server. Bitnobi needs to send an email for `forgot password` requests. Instructions for installing a simple SMTP service on ubuntu can be found at https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-postfix-as-a-send-only-smtp-server-on-ubuntu-16-04.
* (optional) Certificate signed by a CA authority for the domain name being used by the Bitnobi server. Default deployment uses a self-signed certificate.