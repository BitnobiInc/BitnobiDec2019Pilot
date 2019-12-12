The `bitobiBackup.sh` and `bitnobiRestore.sh` commands allow saving all Bitnobi state and user defined content so that it can be later restored into a new docker instance of Bitnobi. This allows you to do things like:
* move the Bitnobi installation to another server while preserving all users, policies, workflows, etc.
* duplicate the state of a Bitnobi system into another docker container
* examine Bitnobi state for some previous date.
* recover from a catastrophic hardware or software failure 

Some limitation of the current implementation:
* May not restore properly if the version of Bitnobi code has changed significantly.
* Need to temporarily halt Bitnobi while backup or restore is in progress
* Need to create a new docker container before Restoring.

### Backup
1. attach to the Bitnobi container and halt all Bitnobi processes as described in [[Stopping Bitnobi|Start and Stop Bitnobi]] but stay attached to the container and stay in directory /home/Bitnobi-V1.
2. run `./bitnobiBackup.sh`. This will create a new sub-directory under "/home/Bitnobi-V1/backup" and use the current date and time in the subdirectory name. For example running `./bitnobiBackup.sh` on July 27 at 12:18 would generate a subdirectory "backup-20180727_1218".
4. run `./startScript.sh`
5. detach from the container and leave it running using the `CTRL-p CTRL-q` key sequence.

### Restore
1. halt and delete the previous Bitnobi container. The `stop` and `rm` commands take a few seconds to complete:
```
docker stop bitnobi
docker rm bitnobi
```
2. start up a new Bitnobi container:
```
docker run -it \
-p 8000:8000 -p 8888:8888 \
--name bitnobi \
--cap-add IPC_LOCK \
--mount type=bind,source="$(pwd)"/bitnobiBackup,target=/home/Bitnobi-V1/backup \
--mount type=bind,source=/etc/localtime,target=/etc/localtime,readonly \
bitnobi /bin/bash
```
3. run `./bitnobiRestore.sh` specifying which backup sub-directory to use. For example `./bitnobiRestore.sh backup/backup-20180727_1218`
4. run `./startScript.sh`
5. detach from the container and leave it running using the `CTRL-p CTRL-q` key sequence.