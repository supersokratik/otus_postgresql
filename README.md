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

bikuloffyaroslav@otus-vm-db-pg-vm:~$ sudo apt-get -y install postgresql
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  libgdbm-compat4 libllvm10 libperl5.30 libpq5 libsensors-config libsensors5
  libxslt1.1 perl perl-modules-5.30 postgresql-12 postgresql-client-12
  postgresql-client-common postgresql-common ssl-cert sysstat
Suggested packages:
  lm-sensors perl-doc libterm-readline-gnu-perl | libterm-readline-perl-perl
  make libb-debug-perl liblocale-codes-perl postgresql-doc postgresql-doc-12
  libjson-perl openssl-blacklist isag
The following NEW packages will be installed:
  libgdbm-compat4 libllvm10 libperl5.30 libpq5 libsensors-config libsensors5
  libxslt1.1 perl perl-modules-5.30 postgresql postgresql-12
  postgresql-client-12 postgresql-client-common postgresql-common ssl-cert
  sysstat
0 upgraded, 16 newly installed, 0 to remove and 0 not upgraded.
Need to get 37.8 MB of archives.
After this operation, 168 MB of additional disk space will be used.
Get:1 http://mirror.yandex.ru/ubuntu focal-updates/main amd64 perl-modules-5.30 all 5.30.0-9ubuntu0.5 [2,739 kB]
Get:2 http://mirror.yandex.ru/ubuntu focal/main amd64 libgdbm-compat4 amd64 1.18.1-5 [6,244 B]
Get:3 http://mirror.yandex.ru/ubuntu focal-updates/main amd64 libperl5.30 amd64 5.30.0-9ubuntu0.5 [3,941 kB]
Get:4 http://mirror.yandex.ru/ubuntu focal-updates/main amd64 perl amd64 5.30.0-9ubuntu0.5 [224 kB]
Get:5 http://mirror.yandex.ru/ubuntu focal/main amd64 libllvm10 amd64 1:10.0.0-4ubuntu1 [15.3 MB]
Get:6 http://mirror.yandex.ru/ubuntu focal-updates/main amd64 libpq5 amd64 12.19-0ubuntu0.20.04.1 [116 kB]
Get:7 http://mirror.yandex.ru/ubuntu focal-updates/main amd64 libsensors-config all 1:3.6.0-2ubuntu1.1 [6,052 B]
Get:8 http://mirror.yandex.ru/ubuntu focal-updates/main amd64 libsensors5 amd64 1:3.6.0-2ubuntu1.1 [27.2 kB]
Get:9 http://mirror.yandex.ru/ubuntu focal-updates/main amd64 libxslt1.1 amd64 1.1.34-4ubuntu0.20.04.1 [151 kB]
Get:10 http://mirror.yandex.ru/ubuntu focal-updates/main amd64 postgresql-client-common all 214ubuntu0.1 [28.2 kB]
Get:11 http://mirror.yandex.ru/ubuntu focal-updates/main amd64 postgresql-client-12 amd64 12.19-0ubuntu0.20.04.1 [1,054 kB]
Get:12 http://mirror.yandex.ru/ubuntu focal/main amd64 ssl-cert all 1.0.39 [17.0 kB]
Get:13 http://mirror.yandex.ru/ubuntu focal-updates/main amd64 postgresql-common all 214ubuntu0.1 [169 kB]
Get:14 http://mirror.yandex.ru/ubuntu focal-updates/main amd64 postgresql-12 amd64 12.19-0ubuntu0.20.04.1 [13.5 MB]
Get:15 http://mirror.yandex.ru/ubuntu focal-updates/main amd64 postgresql all 12+214ubuntu0.1 [3,924 B]
Get:16 http://mirror.yandex.ru/ubuntu focal-updates/main amd64 sysstat amd64 12.2.0-2ubuntu0.3 [448 kB]
Fetched 37.8 MB in 1s (44.6 MB/s)
Preconfiguring packages ...
Selecting previously unselected package perl-modules-5.30.
(Reading database ... 102667 files and directories currently installed.)
Preparing to unpack .../00-perl-modules-5.30_5.30.0-9ubuntu0.5_all.deb ...
Unpacking perl-modules-5.30 (5.30.0-9ubuntu0.5) ...
Selecting previously unselected package libgdbm-compat4:amd64.
Preparing to unpack .../01-libgdbm-compat4_1.18.1-5_amd64.deb ...
Unpacking libgdbm-compat4:amd64 (1.18.1-5) ...
Selecting previously unselected package libperl5.30:amd64.
Preparing to unpack .../02-libperl5.30_5.30.0-9ubuntu0.5_amd64.deb ...
Unpacking libperl5.30:amd64 (5.30.0-9ubuntu0.5) ...
Selecting previously unselected package perl.
Preparing to unpack .../03-perl_5.30.0-9ubuntu0.5_amd64.deb ...
Unpacking perl (5.30.0-9ubuntu0.5) ...
Selecting previously unselected package libllvm10:amd64.
Preparing to unpack .../04-libllvm10_1%3a10.0.0-4ubuntu1_amd64.deb ...
Unpacking libllvm10:amd64 (1:10.0.0-4ubuntu1) ...
Selecting previously unselected package libpq5:amd64.
Preparing to unpack .../05-libpq5_12.19-0ubuntu0.20.04.1_amd64.deb ...
Unpacking libpq5:amd64 (12.19-0ubuntu0.20.04.1) ...
Selecting previously unselected package libsensors-config.
Preparing to unpack .../06-libsensors-config_1%3a3.6.0-2ubuntu1.1_all.deb ...
Unpacking libsensors-config (1:3.6.0-2ubuntu1.1) ...
Selecting previously unselected package libsensors5:amd64.
Preparing to unpack .../07-libsensors5_1%3a3.6.0-2ubuntu1.1_amd64.deb ...
Unpacking libsensors5:amd64 (1:3.6.0-2ubuntu1.1) ...
Selecting previously unselected package libxslt1.1:amd64.
Preparing to unpack .../08-libxslt1.1_1.1.34-4ubuntu0.20.04.1_amd64.deb ...
Unpacking libxslt1.1:amd64 (1.1.34-4ubuntu0.20.04.1) ...
Selecting previously unselected package postgresql-client-common.
Preparing to unpack .../09-postgresql-client-common_214ubuntu0.1_all.deb ...
Unpacking postgresql-client-common (214ubuntu0.1) ...
Selecting previously unselected package postgresql-client-12.
Preparing to unpack .../10-postgresql-client-12_12.19-0ubuntu0.20.04.1_amd64.deb ...
Unpacking postgresql-client-12 (12.19-0ubuntu0.20.04.1) ...
Selecting previously unselected package ssl-cert.
Preparing to unpack .../11-ssl-cert_1.0.39_all.deb ...
Unpacking ssl-cert (1.0.39) ...
Selecting previously unselected package postgresql-common.
Preparing to unpack .../12-postgresql-common_214ubuntu0.1_all.deb ...
Adding 'diversion of /usr/bin/pg_config to /usr/bin/pg_config.libpq-dev by postgresql-common'
Unpacking postgresql-common (214ubuntu0.1) ...
Selecting previously unselected package postgresql-12.
Preparing to unpack .../13-postgresql-12_12.19-0ubuntu0.20.04.1_amd64.deb ...
Unpacking postgresql-12 (12.19-0ubuntu0.20.04.1) ...
Selecting previously unselected package postgresql.
Preparing to unpack .../14-postgresql_12+214ubuntu0.1_all.deb ...
Unpacking postgresql (12+214ubuntu0.1) ...
Selecting previously unselected package sysstat.
Preparing to unpack .../15-sysstat_12.2.0-2ubuntu0.3_amd64.deb ...
Unpacking sysstat (12.2.0-2ubuntu0.3) ...
Setting up postgresql-client-common (214ubuntu0.1) ...
Setting up perl-modules-5.30 (5.30.0-9ubuntu0.5) ...
Setting up libsensors-config (1:3.6.0-2ubuntu1.1) ...
Setting up libpq5:amd64 (12.19-0ubuntu0.20.04.1) ...
Setting up libllvm10:amd64 (1:10.0.0-4ubuntu1) ...
Setting up postgresql-client-12 (12.19-0ubuntu0.20.04.1) ...
update-alternatives: using /usr/share/postgresql/12/man/man1/psql.1.gz to provide /usr/share/man/man1/psql.1.gz (psql.1.gz) in auto mode
Setting up ssl-cert (1.0.39) ...
Setting up libgdbm-compat4:amd64 (1.18.1-5) ...
Setting up libsensors5:amd64 (1:3.6.0-2ubuntu1.1) ...
Setting up libxslt1.1:amd64 (1.1.34-4ubuntu0.20.04.1) ...
Setting up libperl5.30:amd64 (5.30.0-9ubuntu0.5) ...
Setting up sysstat (12.2.0-2ubuntu0.3) ...

Creating config file /etc/default/sysstat with new version
update-alternatives: using /usr/bin/sar.sysstat to provide /usr/bin/sar (sar) in auto mode
Created symlink /etc/systemd/system/multi-user.target.wants/sysstat.service → /lib/systemd/system/sysstat.service.
Setting up perl (5.30.0-9ubuntu0.5) ...
Setting up postgresql-common (214ubuntu0.1) ...
Adding user postgres to group ssl-cert

Creating config file /etc/postgresql-common/createcluster.conf with new version
Building PostgreSQL dictionaries from installed myspell/hunspell packages...
Removing obsolete dictionary files:
Created symlink /etc/systemd/system/multi-user.target.wants/postgresql.service → /lib/systemd/system/postgresql.service.
Setting up postgresql-12 (12.19-0ubuntu0.20.04.1) ...
Creating new PostgreSQL cluster 12/main ...
/usr/lib/postgresql/12/bin/initdb -D /var/lib/postgresql/12/main --auth-local peer --auth-host md5
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.UTF-8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /var/lib/postgresql/12/main ... ok
creating subdirectories ... ok
selecting dynamic shared memory implementation ... posix
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting default time zone ... Etc/UTC
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok

Success. You can now start the database server using:

    pg_ctlcluster 12 main start

Ver Cluster Port Status Owner    Data directory              Log file
12  main    5432 down   postgres /var/lib/postgresql/12/main /var/log/postgresql/postgresql-12-main.log
update-alternatives: using /usr/share/postgresql/12/man/man1/postmaster.1.gz to provide /usr/share/man/man1/postmaster.1.gz (postmaster.1.gz) in auto mode
Setting up postgresql (12+214ubuntu0.1) ...
Processing triggers for systemd (245.4-4ubuntu3.23) ...
Processing triggers for man-db (2.9.1-1) ...
Processing triggers for libc-bin (2.31-0ubuntu9.16) ...
Запускаем обновление 
bikuloffyaroslav@otus-vm-db-pg-vm:~$ sudo apt update && sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list' && wget --quiet -0 - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - && sudo apt-get update && sudo apt -y install postgresql-14
Hit:1 http://mirror.yandex.ru/ubuntu focal InRelease
Get:2 http://mirror.yandex.ru/ubuntu focal-updates InRelease [128 kB]
Get:3 http://security.ubuntu.com/ubuntu focal-security InRelease [128 kB]
Hit:4 http://mirror.yandex.ru/ubuntu focal-backports InRelease
Get:5 http://mirror.yandex.ru/ubuntu focal-updates/main amd64 Packages [3,495 kB]
Get:6 http://mirror.yandex.ru/ubuntu focal-updates/main i386 Packages [1,016 kB]
Get:7 http://mirror.yandex.ru/ubuntu focal-updates/main Translation-en [543 kB]
Get:8 http://mirror.yandex.ru/ubuntu focal-updates/main amd64 c-n-f Metadata [17.7 kB]
Get:9 http://mirror.yandex.ru/ubuntu focal-updates/restricted amd64 Packages [3,152 kB]
Get:10 http://mirror.yandex.ru/ubuntu focal-updates/restricted Translation-en [442 kB]
Get:11 http://mirror.yandex.ru/ubuntu focal-updates/universe i386 Packages [801 kB]
Get:12 http://mirror.yandex.ru/ubuntu focal-updates/universe amd64 Packages [1,219 kB]
Get:13 http://mirror.yandex.ru/ubuntu focal-updates/universe Translation-en [294 kB]
Get:14 http://mirror.yandex.ru/ubuntu focal-updates/universe amd64 c-n-f Metadata [27.8 kB]
Get:15 http://security.ubuntu.com/ubuntu focal-security/main amd64 Packages [3,125 kB]
Get:16 http://security.ubuntu.com/ubuntu focal-security/main i386 Packages [796 kB]
Get:17 http://security.ubuntu.com/ubuntu focal-security/main Translation-en [464 kB]
Get:18 http://security.ubuntu.com/ubuntu focal-security/main amd64 c-n-f Metadata [14.1 kB]
Get:19 http://security.ubuntu.com/ubuntu focal-security/restricted amd64 Packages [3,034 kB]
Get:20 http://security.ubuntu.com/ubuntu focal-security/restricted Translation-en [425 kB]
Get:21 http://security.ubuntu.com/ubuntu focal-security/universe amd64 Packages [999 kB]
Get:22 http://security.ubuntu.com/ubuntu focal-security/universe i386 Packages [674 kB]
Get:23 http://security.ubuntu.com/ubuntu focal-security/universe Translation-en [212 kB]
Get:24 http://security.ubuntu.com/ubuntu focal-security/universe amd64 c-n-f Metadata [21.0 kB]
Fetched 21.0 MB in 4s (5,268 kB/s)
Reading package lists... Done
Building dependency tree
Reading state information... Done
14 packages can be upgraded. Run 'apt list --upgradable' to see them.
wget: invalid option -- '0'
Usage: wget [OPTION]... [URL]...

Try `wget --help' for more options.
gpg: no valid OpenPGP data found.

bikuloffyaroslav@otus-vm-db-pg-vm:~$ apt list --upgradable
Listing... Done
busybox-initramfs/focal-updates,focal-security 1:1.30.1-4ubuntu6.5 amd64 [upgradable from: 1:1.30.1-4ubuntu6.4]
busybox-static/focal-updates,focal-security 1:1.30.1-4ubuntu6.5 amd64 [upgradable from: 1:1.30.1-4ubuntu6.4]
curl/focal-updates,focal-security 7.68.0-1ubuntu2.23 amd64 [upgradable from: 7.68.0-1ubuntu2.22]
krb5-locales/focal-updates,focal-updates,focal-security,focal-security 1.17-6ubuntu4.6 all [upgradable from: 1.17-6ubuntu4.4]
libcurl4/focal-updates,focal-security 7.68.0-1ubuntu2.23 amd64 [upgradable from: 7.68.0-1ubuntu2.22]
libgssapi-krb5-2/focal-updates,focal-security 1.17-6ubuntu4.6 amd64 [upgradable from: 1.17-6ubuntu4.4]
libk5crypto3/focal-updates,focal-security 1.17-6ubuntu4.6 amd64 [upgradable from: 1.17-6ubuntu4.4]
libkrb5-3/focal-updates,focal-security 1.17-6ubuntu4.6 amd64 [upgradable from: 1.17-6ubuntu4.4]
libkrb5support0/focal-updates,focal-security 1.17-6ubuntu4.6 amd64 [upgradable from: 1.17-6ubuntu4.4]
libssl1.1/focal-updates,focal-security 1.1.1f-1ubuntu2.23 amd64 [upgradable from: 1.1.1f-1ubuntu2.22]
linux-generic/focal-updates,focal-security 5.4.0.192.190 amd64 [upgradable from: 5.4.0.190.188]
linux-headers-generic/focal-updates,focal-security 5.4.0.192.190 amd64 [upgradable from: 5.4.0.190.188]
linux-image-generic/focal-updates,focal-security 5.4.0.192.190 amd64 [upgradable from: 5.4.0.190.188]
openssl/focal-updates,focal-security 1.1.1f-1ubuntu2.23 amd64 [upgradable from: 1.1.1f-1ubuntu2.22]

```
6. зайти вторым ssh (вторая сессия)
7. запустить везде psql из под пользователя postgres
8. выключить auto commit

### сделать

1. в первой сессии новую таблицу и наполнить ее данными create table persons(id serial, first_name text, second_name text); insert into persons(first_name, second_name) values('ivan', 'ivanov'); insert into persons(first_name, second_name) values('petr', 'petrov'); commit;
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
