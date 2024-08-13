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
4. зайти удаленным ssh (первая сессия), не забывайте про ssh-add
5. поставить PostgreSQL
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
