
# Stopping Bitnobi
Open a terminal session on the server and run the `docker ps -a` command to verify that the container is still running. This should display something like:
```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                    PORTS                                            NAMES
fd816960b403        bitnobi             "/bin/bash"         4 minutes ago       Up 4 minutes              0.0.0.0:8000->8000/tcp, 0.0.0.0:8888->8888/tcp   bitnobi
e3c8952f0236        hello-world         "/hello"            18 hours ago        Exited (0) 18 hours ago                                                    infallible_wiles
```
Use the `docker attach bitnobi` command to attach your terminal session to the running container. Any commands that you type now will be executed in the container.
Hit `Enter` and type `ctrl c` to halt the Jupyter process, then type `pm2 delete all` to halt the remaining Bitnobi processes.
This should display something like:
```
pm2 delete all
[PM2] Applying action deleteProcessId on app [all](ids: 0,1,2,3)
[PM2] [auditServer](1) ✓
[PM2] [workflowserver](3) ✓
[PM2] [server](2) ✓
[PM2] [mongod](0) ✓
┌──────────┬────┬──────┬─────┬────────┬─────────┬────────┬─────┬─────┬──────┬──────────┐
│ App name │ id │ mode │ pid │ status │ restart │ uptime │ cpu │ mem │ user │ watching │
└──────────┴────┴──────┴─────┴────────┴─────────┴────────┴─────┴─────┴──────┴──────────┘
 Use `pm2 show <id|name>` to get more details about an app
```
Type `exit` to leave and halt the docker container.
Type `docker ps -a` to list the docker containers. This should show the bitnobi container status as "Exited":
```
docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
fd816960b403        bitnobi             "/bin/bash"         43 minutes ago      Exited (0) 16 seconds ago                       bitnobi
e3c8952f0236        hello-world         "/hello"            19 hours ago        Exited (0) 19 hours ago                         infallible_wiles
```
If the container does not show as "Exited" use the `docker stop bitnobi` to halt the container.

# Starting Bitnobi
First use the `docker ps -a` command to verify that the status of the bitnobi container is "Exited".
The following commands can be used to start Bitnobi if the container is halted:
```
docker start bitnobi  
docker attach bitnobi  
cd /home/BitnobiV1
./startScript.sh
```
You can detach from the container and leave it running using the `CTRL-p CTRL-q` key sequence.

