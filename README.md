# otus_postgresql
learning course
## Цель:
1. научиться работать в Яндекс Облаке
2. научиться управлять уровнем изолции транзации в PostgreSQL и понимать особенность работы уровней read commited и repeatable read

**Описание/Пошаговая инструкция выполнения домашнего задания:**
1. создать новый проект в Яндекс облако или на любых ВМ, докере
2. далее создать инстанс виртуальной машины с дефолтными параметрами
3. добавить свой ssh ключ в metadata ВМ
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
