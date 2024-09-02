PS C:\Users\Ярослав> yc compute ssh --name compute-vm-2-2-20-hdd-1724909041159 --folder-id b1gd7tphql41ehp4g4ua
```
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.4.0-192-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro
Last login: Thu Aug 29 05:49:22 2024 from 217.107.126.235
```
bikuloffyaroslav@compute-vm-2-2-20-hdd-1724909041159:~$ curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh get-docker.sh && rm get-docker.sh && sudo usermod -aG docker $USER && newgrp docker
```
# Executing docker install script, commit: 0d6f72e671ba87f7aa4c6991646a1a5b9f9dae84
Warning: the "docker" command appears to already exist on this system.

If you already have Docker installed, this script can cause trouble, which is
why we're displaying this warning and provide the opportunity to cancel the
installation.

If you installed the current Docker package using this script and are using it
again to update Docker, you can safely ignore this message.

You may press Ctrl+C now to abort this script.
+ sleep 20
+ sh -c apt-get update -qq >/dev/null
+ sh -c DEBIAN_FRONTEND=noninteractive apt-get install -y -qq ca-certificates curl >/dev/null
+ sh -c install -m 0755 -d /etc/apt/keyrings
+ sh -c curl -fsSL "https://download.docker.com/linux/ubuntu/gpg" -o /etc/apt/keyrings/docker.asc
+ sh -c chmod a+r /etc/apt/keyrings/docker.asc
+ sh -c echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu focal stable" > /etc/apt/sources.list.d/docker.list
+ sh -c apt-get update -qq >/dev/null
+ sh -c DEBIAN_FRONTEND=noninteractive apt-get install -y -qq docker-ce docker-ce-cli containerd.io docker-compose-plugin docker-ce-rootless-extras docker-buildx-plugin >/dev/null
+ sh -c docker version
Client: Docker Engine - Community
 Version:           27.2.0
 API version:       1.47
 Go version:        go1.21.13
 Git commit:        3ab4256
 Built:             Tue Aug 27 14:15:09 2024
 OS/Arch:           linux/amd64
 Context:           default

Server: Docker Engine - Community
 Engine:
  Version:          27.2.0
  API version:      1.47 (minimum version 1.24)
  Go version:       go1.21.13
  Git commit:       3ab5c7d
  Built:            Tue Aug 27 14:15:09 2024
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.7.21
  GitCommit:        472731909fa34bd7bc9c087e4c27943f9835f111
 runc:
  Version:          1.1.13
  GitCommit:        v1.1.13-0-g58aa920
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

================================================================================

To run Docker as a non-privileged user, consider setting up the
Docker daemon in rootless mode for your user:

    dockerd-rootless-setuptool.sh install

Visit https://docs.docker.com/go/rootless/ to learn about rootless mode.


To run the Docker daemon as a fully privileged service, but granting non-root
users access, refer to https://docs.docker.com/go/daemon-access/

WARNING: Access to the remote API on a privileged Docker daemon is equivalent
         to root access on the host. Refer to the 'Docker daemon attack surface'
         documentation for details: https://docs.docker.com/go/attack-surface/

================================================================================
```
bikuloffyaroslav@compute-vm-2-2-20-hdd-1724909041159:~$ sudo docker network create pg-net
```
f5bd98f4ef0fd301c3c7ca409a2ead1996d13c5f0adc9f2b331876c29dfc08d3
```
g-server --network pg-net -e POSTGRES_PASSWORD=postgres -d -p 5432:5432 -v /var/lib/postgres:/var/lib/postgresql/data postgres:15
```
Unable to find image 'postgres:15' locally
15: Pulling from library/postgres
e4fff0779e6d: Pull complete
ac2c82f5abbf: Pull complete
6423808c0870: Pull complete
9fc1a9646316: Pull complete
f486755b3b27: Pull complete
e3f131c9ffee: Pull complete
bb37c85245b8: Pull complete
af85de36d2ba: Pull complete
3fb6cac099e8: Pull complete
31efa1bde9c4: Pull complete
15bf20fe9460: Pull complete
3216df30c32c: Pull complete
cecbd5a08338: Pull complete
a733d89613cc: Pull complete
Digest: sha256:0836104ba0de8d09e8d54e2d6a28389fbce9c0f4fe08f4aa065940452ec61c30
Status: Downloaded newer image for postgres:15
039a2ee0094a319eb6fb0e60c92b2dc5b6c2b1dff641e4c05b7597331790c81a
```
bikuloffyaroslav@compute-vm-2-2-20-hdd-1724909041159:~$ sudo docker run -it --rm --network pg-net --name pg-client postgres:15 psql -h pg-server -U postgres
Password for user postgres:
```
psql (15.8 (Debian 15.8-1.pgdg120+1))
Type "help" for help.
```

postgres=# CREATE DATABASE otus;
```
CREATE DATABASE
postgres=#
```
postgres=# \conninfo
```
You are connected to database "postgres" as user "postgres" on host "pg-server" (address "172.18.0.2") at port "5432".
postgres=#
```
postgres=# \l
```
   Name    |  Owner   | Encoding |  Collate   |   Ctype    | ICU Locale | Locale Provider |   Access privileges
-----------+----------+----------+------------+------------+------------+-----------------+-----------------------
 otus      | postgres | UTF8     | en_US.utf8 | en_US.utf8 |            | libc            |
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |            | libc            |
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 |            | libc            | =c/postgres          +
           |          |          |            |            |            |                 | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 |            | libc            | =c/postgres          +
           |          |          |            |            |            |                 | postgres=CTc/postgres
(4 rows)
```
остановить контейнер с сервером
```
bikoffyaroslav@compute-vm-2-2-20-hdd-1724909041159:~$ sudo docker stop ca7f8ec830aa
ca7f8ec830aa
```
удалить контейнер
```
bikuloffyaroslav@compute-vm-2-2-20-hdd-1724909041159:~$ sudo docker rm ca7f8ec83830aa
ca7f8ec830aa
```
создать его заново 
```
bikuloffyaroslav@compute-vm-2-2-20-hdd-1724909041159:~$ sudo docker ps -a
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS          PORTS                                       NAMES
05b593543bf4   postgres:15   "docker-entrypoint.s…"   29 minutes ago   Up 29 minutes   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   pg-server
bikuloffyaroslav@compute-vm-2-2-20-hdd-1724909041159:~$
```

Проверяем, что все ранее созданные данные остались на месте:

  ```
                                                  List of databases
       Name        |  Owner   | Encoding |  Collate   |   Ctype    | ICU Locale | Locale Provider |   Access privileges
-------------------+----------+----------+------------+------------+------------+-----------------+-----------------------
 otus              | postgres | UTF8     | en_US.utf8 | en_US.utf8 |            | libc            |
 postgres          | postgres | UTF8     | en_US.utf8 | en_US.utf8 |            | libc            |
 readme_to_recover | postgres | UTF8     | en_US.utf8 | en_US.utf8 |            | libc            |
 template0         | postgres | UTF8     | en_US.utf8 | en_US.utf8 |            | libc            | =c/postgres          +
                   |          |          |            |            |            |                 | postgres=CTc/postgres
 template1         | postgres | UTF8     | en_US.utf8 | en_US.utf8 |            | libc            | =c/postgres          +
                   |          |          |            |            |            |                 | postgres=CTc/postgres
(5 rows)

(END)
```
