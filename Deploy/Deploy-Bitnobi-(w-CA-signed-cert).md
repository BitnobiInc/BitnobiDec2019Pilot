If you have a signed SSL certificate from a Certificate Authority that you wish to use with Bitnobi, you need take some extra steps during deployment. 

Log into the server and open a terminal window for the following steps. 


### 1. Remove Old Bitnobi Deployment
If you have a previously installed Bitnobi container and image you need to remove it prior to installing a new one.
The docker command `docker ps -a` will list which containers are present and `docker images` will list the images loaded on the server.

If you are getting permission errors trying to run docker on linux, please have a look at:
https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user

Bitnobi runs one docker container for the front-end and another for the backend. The following commands will remove the existing bitnobi containers and images:
```
docker stop bitnobi-fe
docker stop bitnobi-be
docker rm bitnobi-be
docker rmi <bitnobi fe image name> <bitnobi be image name>
```

### 2. Obtain Bitnobi Docker Images
login to the Bitnobi docker repository using your assigned userid/password and then pull the image to your server.
```
docker login demo.bitnobi.com:5043
docker pull demo.bitnobi.com:5043/bitnobi-fe-oct-2019-pilot
docker pull demo.bitnobi.com:5043/bitnobi-oct-2019-pilot
```
The pull requests can take a few minutes to complete.

### 3. Create Bitnobi Directories
Create a directory for holding Bitnobi shell commands, logs and backup files. For example:
```
cd ~
mkdir bitnobi
cd bitnobi
mkdir backup
mkdir logs
```
Now we create a directory and copy over the certificate into it and name the files `key.pem` and `cert.pem`. For example if you used the "letsencrypt" site to create certificates with the subdomain name "demo.bitnobi.com" the steps would be:
```
mkdir bitnobiSslCerts
sudo cp /etc/letsencrypt/live/demo.bitnobi.com/privkey1.pem bitnobiSslCerts/key.pem
sudo cp /etc/letsencrypt/live/demo.bitnobi.com/fullchain.pem bitnobiSslCerts/cert.pem
```

### 4. Create and Run the Backend Container
Create a docker bridge network to allow private communications between the Bitnobi containers:
```
docker network create --driver bridge bitnobi-nw
```
The docker command `docker network ls` can be used to inspect the networks.

Go to the bitnobi folder (`cd ~/bitnobi`) and create a bash script `start-bitnobi-be.sh` with the following contents:
```
#!/bin/bash    #!/bin/bash

rm "$(pwd)"/logs/*

docker run -it \
--name bitnobi-be \
--cap-add IPC_LOCK \
--mount type=bind,source="$(pwd)"/backup,target=/home/Bitnobi-V1/backup \
--mount type=bind,source="$(pwd)"/logs,target=/root/.pm2/logs \
--mount type=bind,source=/etc/localtime,target=/etc/localtime,readonly \
--mount type=bind,source="$(pwd)"/bitnobiSslCerts,target=/home/Bitnobi-V1/DPprototype/config/sslcerts,readonly \
--network=bitnobi-nw \
demo.bitnobi.com:5043/bitnobi-oct-2019-pilot /bin/bash
```
* The -p options expose port 8000 for the main Bitnobi UI and port 8888 for the Jupyter notebooks server.
* The --name option gives the name "bitnobi-be" to the new docker container
* the --cap-add IPC_LOCK allows the vault process to lock memory to prevent it from swapping out.
* the --mount backup option causes the backup directory in the container to map to the `~/bitnobi/backup` directory in the host. This allows the `bitnobiBackup.sh` command to store backups in the host filesystem rather than inside the container.
* the --mount logs option causes the logs directory in the container to map to the `~/bitnobi/logs` directory in the host. This makes it easier to get at the logs in the host filesystem rather than having to go to the docker container.
* the --mount localtime option causes the container to use the same timezone as the host
* the --mount bitnobiSslCerts will over-ride the default self-signed certificate in the Bitnobi image with your signed CA certificate.
* the --network=bitnobi-nw option connects the container to the `bitnobi-nw` bridge network that we created in a previous step.

Once you have created this file, make it executable and run it:
```
  chmod a+x start-bitnobi-be.sh
  ./start-bitnobi-be.sh
```
This will create the container then attach to a `bash` shell running in your new container. Any commands that you type now will be executed in the docker container.

### 5. Initialize Email Settings
Bitnobi needs access to an SMTP email server to send email to the user when a "forgot password" request is made. Bitnobi will otherwise run fine without this and you can skip this step if you do not have an email server available. 
Run the following command and it will prompt you for:
* an "mail from" value (e.g. bitnobi@demo.bitnobi.com), 
* the hostname or IP address of your SMTP server (e.g. demo.bitnobi.com)
* port number for SMTP server. If you are using the default port (25) then you can leave this value blank.
```
./email-init-env.sh
```
This command will create the file `set_email_env.sh` which will be used by the `startScript.sh` command later. Should you need to change the email server settings at some later date, you can run this command again and the next invocation of `startScript.sh` will pick up the new values.

Note that if you are using a `postfix` mail server on your host, you will need to
modify its configuration to accept SMTP relay from docker containers.
To do this edit the `/etc/postfix/main.cf` file to modify the `mynetworks` line to contain:
```
      mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128 172.16.0.0/12
```
and then restart postfix:
```
    sudo service postfix restart
```

### 6. Initialize vault with CA signed certificate
We need to call the `vault-init.sh` with your Certificate domain name as a parameter before calling `startScript.sh`. For example if we have the subdomain name "demo.bitnobi.com" the steps would be
```
cd /home/Bitnobi-V1
./vault-init.sh demo.bitnobi.com
```

### 7. Start Bitnobi Processes

Now we need to start the Bitnobi processes in the backend container. Enter the following commands:
```
cd /home/Bitnobi-V1
./startScript.sh
```
this will start several processes and should generate output similar to:
```
┌────────────────┬────┬──────┬─────┬────────┬─────────┬────────┬───────┬───────────┬──────┬──────────┐
│ App name       │ id │ mode │ pid │ status │ restart │ uptime │ cpu   │ mem       │ user │ watching │
├────────────────┼────┼──────┼─────┼────────┼─────────┼────────┼───────┼───────────┼──────┼──────────┤
│ auditServer    │ 1  │ fork │ 47  │ online │ 0       │ 1s     │ 0%    │ 54.6 MB   │ root │ disabled │
│ mongod         │ 0  │ fork │ 23  │ online │ 0       │ 5s     │ 0%    │ 77.7 MB   │ root │ disabled │
│ server         │ 2  │ fork │ 63  │ online │ 0       │ 1s     │ 38.5% │ 37.6 MB   │ root │ disabled │
│ workflowserver │ 3  │ fork │ 80  │ online │ 0       │ 0s     │ 0%    │ 11.6 MB   │ root │ disabled │
└────────────────┴────┴──────┴─────┴────────┴─────────┴────────┴───────┴───────────┴──────┴──────────┘
 Use `pm2 show <id|name>` to get more details about an app
```
Now press the key sequence `ctrl-p ctrl-q` to detach the container and return to the host shell.
You can now run the `docker ps -a` command to verify that the container is still running, and it should return something like:
```
$ docker ps -a
CONTAINER ID        IMAGE                                             COMMAND                  CREATED             STATUS              PORTS                                            NAMES
822b74420347        demo.bitnobi.com:5043/bitnobi-oct-2019-pilot      "/bin/bash"              About an hour ago   Up About an hour    8000/tcp, 8888/tcp                               bitnobi-be
```

### 8. Create and Run the Frontend container
Create a directory for nginx configuration and logs:
```
cd ~/bitnobi
mkdir nginx
cd nginx
mkdir conf.d
mkdir logs
```
Within the `conf.d` folder, create a file named `default.conf` with the following contents:
```
server {
    listen       80 ssl;
    listen       [::]:80 ssl;
    server_name  localhost;
    client_max_body_size  1000M;

    ssl_certificate     /etc/ssl/certs/cert.pem;
    ssl_certificate_key /etc/ssl/private/key.pem;

    # proxy Bitnobi API calls to backend server
    location /api/ {
       proxy_pass "https://bitnobi-be:8000/";
    }

    # proxy Bitnobi socket io calls to backend server
    location /socket.io/ {
        proxy_pass "https://bitnobi-be:8000";
        proxy_redirect off;

        proxy_http_version 1.1;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
        try_files $uri $uri/ /index.html;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}

```

Go to the bitnobi folder (`cd ~/bitnobi`) and create a bash script `start-bitnobi-fe.sh` with the following contents:
```
#!/bin/bash

rm "$(pwd)"/nginx/logs/*

docker run -d --rm -p 3000:80 \
--name bitnobi-fe \
--mount type=bind,source="$(pwd)"/nginx/logs,target=/var/log/nginx \
--mount type=bind,source="$(pwd)"/nginx/conf.d,target=/etc/nginx/conf.d \
--mount type=bind,source=/etc/localtime,target=/etc/localtime,readonly \
--mount type=bind,source="$(pwd)"/bitnobiSslCerts/cert.pem,target=/etc/ssl/certs/cert.pem,readonly \
--mount type=bind,source="$(pwd)"/bitnobiSslCerts/key.pem,target=/etc/ssl/private/key.pem,readonly \
--network=bitnobi-nw \
demo.bitnobi.com:5043/bitnobi-fe-oct-2019-pilot
```
Once you have created this file, make it executable and run it:
```
  chmod a+x start-bitnobi-fe.sh
  ./start-bitnobi-fe.sh
```
This runs the bitnobi-fe image in detached mode and automatically starts nginx. Mounts the nginx configuration from `~/bitnobi/nginx/conf.d/default.conf`. Using `docker stop bitnobi-fe` will stop the container and delete it. 

### 9. Open Bitnobi UI
Now open a Chrome browser and navigate to `https://<server name>:3000`. This should open the Bitnobi main page.
Note that you should not see any browser warnings about unsafe HTTPS certificates.

### 10. Create Admin User
The first user to sign up to Bitnobi will become the `admin` user. Please sign up now and define a username and password for the admin user.


