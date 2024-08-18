# otus_postgresql
learning course
## Цель:
1. научиться работать в Яндекс Облаке
2. научиться управлять уровнем изолции транзации в PostgreSQL и понимать особенность работы уровней read commited и repeatable read

**Описание/Пошаговая инструкция выполнения домашнего задания:**
1. создать новый проект в Яндекс облако или на любых ВМ, докере
2. далее создать инстанс виртуальной машины с дефолтными параметрами
**Обзор**
```
Идентификатор fv49egohbnhq7mt5bn3t
Статус Provisioning
Имя otus-vm-db-pg-vm
Дата создания 13.08.2024, в 22:18
Внутренний FQDN otus-vm-db-pg-vm.ru-central1.internal
Зона доступности ru-central1-d
```

**Доступ**

```
Сервисный аккаунт otus
Доступ через OS Login Включен
Для подключения к машине с помощью CLI используйте команду:
yc compute ssh --name otus-vm-db-pg-vm --folder-id b1g0r5thu63jnf6d0uer
```
**Ресурсы**
```
Платформа Intel Ice Lake
Гарантированная доля vCPU 100%
​vCPU 2 
RAM 4 ГБ
Объём дискового пространства 10 ГБ
Приложение Marketplace Ubuntu 20.04 LTS OS Login
Тип тарификации Free
```
**Сетевой интерфейс**
```
Внутренний IPv4 10.131.0.6
Публичный IPv4 51.250.37.122
Подсеть otus-vm-db-pg-vm1-ru-central1-d
Группа безопасности default-sg-enpibb205c3fetlej9b5
```
**Резервное копирование**
``` 
Cloud Backup Не подключён
```
**Мониторинг**
``` 
Агент для поставки метрик нет
```
**Дополнительно**
``` 
Доступ к серийной консоли запрещен
```
3. добавить свой ssh ключ в metadata ВМ
4. зайти удаленным ssh (первая сессия), не забывайте про ssh-add
```
PS C:\Users\Ярослав> yc init
Welcome! This command will take you through the configuration process.
Pick desired action:
 [1] Re-initialize this profile 'default' with new settings
 [2] Create a new profile
Please enter your numeric choice: 1
Please go to https://oauth.yandex.ru/authorize?response_type=token&client_id=1a6990aa636648e9b2ef855fa7bec2fb in order to obtain OAuth token.
 Please enter OAuth token: [y0_AgAAAAASGGZA*******************************1-SDDXwhBHci-Ow] y0_AgAAAAASGGZAAATuwQAAAAEM26w7AADcD4-hmoJCoat1-SDDXwhBHci-Ow
Please select cloud to use:
 [1] cloud-bikuloff-yaroslav (id = b1g8k9upo38h35mq9gv0)
 [2] cloud-bikuloff-yaroslav (id = b1gurpg1okjkg6qocg0v)
 [3] otus (id = b1g600003cdim5vffe0e)
Please enter your numeric choice: 3
Your current cloud has been set to 'otus' (id = b1g600003cdim5vffe0e).
Please choose folder to use:
 [1] otus-vm-db-pg-vm1 (id = b1g0r5thu63jnf6d0uer)
 [2] Create a new folder
Please enter your numeric choice: 1
Your current folder has been set to 'otus-vm-db-pg-vm1' (id = b1g0r5thu63jnf6d0uer).
Do you want to configure a default Compute zone? [Y/n] y
Which zone do you want to use as a profile default?
 [1] ru-central1-a
 [2] ru-central1-b
 [3] ru-central1-c
 [4] ru-central1-d
 [5] Don't set default zone
Please enter your numeric choice: 1
Your profile default Compute zone has been set to 'ru-central1-a'.
PS C:\Users\Ярослав> yc compute ssh --name otus-vm-db-pg-vm --folder-id b1g0r5thu63jnf6d0uer
Warning: Permanently added 'compute.fv49egohbnhq7mt5bn3t' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.4.0-190-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

Creating directory '/home/bikuloffyaroslav'.
bikuloffyaroslav@otus-vm-db-pg-vm:~$

**sudo nano /etc/postgresql/14/main/postgresql.conf**

# If external_pid_file is not explicitly set, no extra PID file is written.
external_pid_file = '/var/run/postgresql/14-main.pid'                   # write>
                                        # (change requires restart)


#------------------------------------------------------------------------------
# CONNECTIONS AND AUTHENTICATION
#------------------------------------------------------------------------------

# - Connection Settings -

listen_addresses = '*'          # what IP address(es) to listen on;
                                        # comma-separated list of addresses;
File Name to Write: /etc/postgresql/14/main/postgresql.conf


sudo nano /etc/postgresql/14/main/pg_hba.conf
# Database administrative login by Unix domain socket
local   all             postgres                                peer

# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     peer
# IPv4 local connections:
host    all             all             0.0.0.0/0            scram-sha-256
# IPv6 local connections:
host    all             all             ::1/128                 scram-sha-256
# Allow replication connections from localhost, by a user with the

```
5. поставить PostgreSQL
```
 ssh-keygen -t ed25519
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\Ярослав/.ssh/id_ed25519): yc_key.pub
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in yc_key.pub.
Your public key has been saved in yc_key.pub.pub.
The key fingerprint is:
SHA256:dceIZZpqKvSfBNPWa6t2+mgKz7GwTNquyqRjjwfXhy4 Ярослав@DESKTOP-ETLBR6V
The key's randomart image is:
+--[ED25519 256]--+
|            o    |
|           * o   |
|          = o o  |
|       . + . .   |
|    o + S .      |
| . o + B   .     |
| .o * = . o      |
|=..E O =o+..     |
|+==+* =+*=o      |
+----[SHA256]-----+

bikuloffyaroslav@otus-vm-db-pg-vm:~$ sudo apt-get -y install postgresql-14
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  libcommon-sense-perl libjson-perl libjson-xs-perl libpq5
  libtypes-serialiser-perl postgresql-client-14 postgresql-client-common
  postgresql-common
Suggested packages:
  postgresql-doc-14
The following NEW packages will be installed:
  libcommon-sense-perl libjson-perl libjson-xs-perl libtypes-serialiser-perl
  postgresql-14 postgresql-client-14
The following packages will be upgraded:
  libpq5 postgresql-client-common postgresql-common
3 upgraded, 6 newly installed, 0 to remove and 6 not upgraded.
Need to get 18.2 MB of archives.
After this operation, 60.5 MB of additional disk space will be used.
Get:1 http://mirror.yandex.ru/ubuntu focal/main amd64 libjson-perl all 4.02000-2 [80.9 kB]
Get:2 http://mirror.yandex.ru/ubuntu focal/main amd64 libcommon-sense-perl amd64 3.74-2build6 [20.1 kB]
Get:3 http://mirror.yandex.ru/ubuntu focal/main amd64 libtypes-serialiser-perl all 1.0-1 [12.1 kB]
Get:4 http://mirror.yandex.ru/ubuntu focal/main amd64 libjson-xs-perl amd64 4.020-1build1 [83.7 kB]
Get:5 http://apt.postgresql.org/pub/repos/apt focal-pgdg/main amd64 postgresql-common all 262.pgdg20.04+1 [240 kB]
Get:6 http://apt.postgresql.org/pub/repos/apt focal-pgdg/main amd64 postgresql-client-common all 262.pgdg20.04+1 [94.8 kB]
Get:7 http://apt.postgresql.org/pub/repos/apt focal-pgdg/main amd64 libpq5 amd64 16.4-1.pgdg20.04+1 [216 kB]
Get:8 http://apt.postgresql.org/pub/repos/apt focal-pgdg/main amd64 postgresql-client-14 amd64 14.13-1.pgdg20.04+1 [1,626 kB]
Get:9 http://apt.postgresql.org/pub/repos/apt focal-pgdg/main amd64 postgresql-14 amd64 14.13-1.pgdg20.04+1 [15.8 MB]
Fetched 18.2 MB in 2s (7,659 kB/s)
Preconfiguring packages ...
Selecting previously unselected package libjson-perl.
(Reading database ... 106476 files and directories currently installed.)
Preparing to unpack .../0-libjson-perl_4.02000-2_all.deb ...
Unpacking libjson-perl (4.02000-2) ...
Preparing to unpack .../1-postgresql-common_262.pgdg20.04+1_all.deb ...
Leaving 'diversion of /usr/bin/pg_config to /usr/bin/pg_config.libpq-dev by postgresql-common'
Unpacking postgresql-common (262.pgdg20.04+1) over (214ubuntu0.1) ...
Preparing to unpack .../2-postgresql-client-common_262.pgdg20.04+1_all.deb ...
Unpacking postgresql-client-common (262.pgdg20.04+1) over (214ubuntu0.1) ...
Selecting previously unselected package libcommon-sense-perl.
Preparing to unpack .../3-libcommon-sense-perl_3.74-2build6_amd64.deb ...
Unpacking libcommon-sense-perl (3.74-2build6) ...
Selecting previously unselected package libtypes-serialiser-perl.
Preparing to unpack .../4-libtypes-serialiser-perl_1.0-1_all.deb ...
Unpacking libtypes-serialiser-perl (1.0-1) ...
Selecting previously unselected package libjson-xs-perl.
Preparing to unpack .../5-libjson-xs-perl_4.020-1build1_amd64.deb ...
Unpacking libjson-xs-perl (4.020-1build1) ...
Preparing to unpack .../6-libpq5_16.4-1.pgdg20.04+1_amd64.deb ...
Unpacking libpq5:amd64 (16.4-1.pgdg20.04+1) over (12.19-0ubuntu0.20.04.1) ...
Selecting previously unselected package postgresql-client-14.
Preparing to unpack .../7-postgresql-client-14_14.13-1.pgdg20.04+1_amd64.deb ...
Unpacking postgresql-client-14 (14.13-1.pgdg20.04+1) ...
Selecting previously unselected package postgresql-14.
Preparing to unpack .../8-postgresql-14_14.13-1.pgdg20.04+1_amd64.deb ...
Unpacking postgresql-14 (14.13-1.pgdg20.04+1) ...
Setting up postgresql-client-common (262.pgdg20.04+1) ...
Installing new version of config file /etc/postgresql-common/supported_versions ...
Setting up libpq5:amd64 (16.4-1.pgdg20.04+1) ...
Setting up libcommon-sense-perl (3.74-2build6) ...
Setting up postgresql-client-14 (14.13-1.pgdg20.04+1) ...
Merging postmaster.1.gz alternatives into psql.1.gz link group ...
update-alternatives: updating alternative /usr/share/postgresql/12/man/man1/psql.1.gz because link group psql.1.gz has changed slave links
update-alternatives: using /usr/share/postgresql/14/man/man1/psql.1.gz to provide /usr/share/man/man1/psql.1.gz (psql.1.gz) in auto mode
Setting up libtypes-serialiser-perl (1.0-1) ...
Setting up libjson-perl (4.02000-2) ...
Setting up libjson-xs-perl (4.020-1build1) ...
Setting up postgresql-common (262.pgdg20.04+1) ...
Replacing config file /etc/postgresql-common/createcluster.conf with new version
'/etc/apt/trusted.gpg.d/apt.postgresql.org.gpg' -> '/usr/share/postgresql-common/pgdg/apt.postgresql.org.gpg'
Setting up postgresql-14 (14.13-1.pgdg20.04+1) ...
Creating new PostgreSQL cluster 14/main ...
/usr/lib/postgresql/14/bin/initdb -D /var/lib/postgresql/14/main --auth-local peer --auth-host scram-sha-256 --no-instructions
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.UTF-8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /var/lib/postgresql/14/main ... ok
creating subdirectories ... ok
selecting dynamic shared memory implementation ... posix
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting default time zone ... Etc/UTC
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok
Processing triggers for systemd (245.4-4ubuntu3.23) ...
Processing triggers for man-db (2.9.1-1) ...
Processing triggers for libc-bin (2.31-0ubuntu9.16) ...
bikuloffyaroslav@otus-vm-db-pg-vm:~$ pg_lsclusters
Ver Cluster Port Status Owner    Data directory              Log file
bikuloffyaroslav@otus-vm-db-pg-vm:~$ pg_lsclustersostgresql-12-main.log
Ver Cluster Port Status Owner    Data directory              Log file
14  main    5433 online postgres /var/lib/postgresql/14/main /var/log/postgresql/postgresql-14-main.log
```
6. зайти вторым ssh (вторая сессия)
7. запустить везде psql из под пользователя postgres
```
bikuloffyaroslav@otus-vm-db-pg-vm:~$ sudo -u postgres psql
psql (14.13 (Ubuntu 14.13-1.pgdg20.04+1), server 12.19 (Ubuntu 12.19-0ubuntu0.20.04.1))
Type "help" for help.

postgres=# \l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
-----------+----------+----------+-------------+-------------+-----------------------
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
(3 rows)

(END)

postgres=# alter user postgres password 'postgres';
ALTER ROLE

bikuloffyaroslav@otus-vm-db-pg-vm:~$ exit
logout
Connection to 51.250.37.122 closed.
ERROR: exit status 127


                      client-trace-id: a567208d-f4b1-4ef9-9bad-8d37d1d450d2

                                                                           Use client-trace-id for investigation of issues in cloud support
                   If you are going to ask for help of cloud support, please send the following trace file: C:\Users\Ярослав\.config\yandex-cloud\logs\2024-08-17T13-17-50.461-yc_compute_ssh.txt

PS C:\Users\Ярослав> yc compute instances list
+----------------------+------------------+---------------+---------+---------------+-------------+
|          ID          |       NAME       |    ZONE ID    | STATUS  |  EXTERNAL IP  | INTERNAL IP |
+----------------------+------------------+---------------+---------+---------------+-------------+
| fv49egohbnhq7mt5bn3t | otus-vm-db-pg-vm | ru-central1-d | RUNNING | 51.250.37.122 | 10.131.0.6  |
+----------------------+------------------+---------------+---------+---------------+-------------+
```
8. выключить auto commit
```
postgres=# AUTOCOMMIT = 'off'
```
### сделать

1. в первой сессии новую таблицу и наполнить ее данными create table persons(id serial, first_name text, second_name text); insert into persons(first_name, second_name) values('ivan', 'ivanov'); insert into persons(first_name, second_name) values('petr', 'petrov'); commit;
```
postgres=# create table persons (id serial, first_name text, second_name text);
CREATE TABLE                                                                   )
postgres=# insert into persons(first_name, second_name) values('ivan', 'ivanov');
INSERT 0 1
postgres=# insert into persons(first_name, second_name) values('petr', 'petrov');
INSERT 0 1
postgres=# commit;
WARNING:  there is no transaction in progress
COMMIT
```

2. посмотреть текущий уровень изоляции: show transaction isolation level
3. начать новую транзакцию в обоих сессиях с дефолтным (не меняя) уровнем изоляции
4. в первой сессии добавить новую запись insert into persons(first_name, second_name) values('sergey', 'sergeev');
5. сделать select from persons во второй сессии
6. видите ли вы новую запись и если да то почему?
7. завершить первую транзакцию - commit;
8. сделать select from persons во второй сессии
9. видите ли вы новую запись и если да то почему?
10. завершите транзакцию во второй сессии
11. начать новые но уже repeatable read транзации - set transaction isolation level repeatable read;
12. в первой сессии добавить новую запись insert into persons(first_name, second_name) values('sveta', 'svetova');
13. сделать select* from persons во второй сессии*
14. видите ли вы новую запись и если да то почему?
15. завершить первую транзакцию - commit;
16. сделать select from persons во второй сессии
17. видите ли вы новую запись и если да то почему?
18. завершить вторую транзакцию
19. сделать select * from persons во второй сессии
20. видите ли вы новую запись и если да то почему?
