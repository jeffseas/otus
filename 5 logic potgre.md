# Логический уровень Postgre

1. создайте новую базу данных testdb
```sql
CREATE DATABASE testdb
```

2. зайдите в созданную базу данных под пользователем postgres
создайте новую схему testnm
создайте новую таблицу t1 с одной колонкой c1 типа integer
вставьте строку со значением c1=1

```sql
CREATE SCHEMA testnm;
CREATE TABLE t1(c1 integer);
INSERT INTO t1 values(1);
```

3. создайте новую роль readonly
дайте новой роли право на подключение к базе данных testdb
дайте новой роли право на использование схемы testnm
дайте новой роли право на select для всех таблиц схемы testnm
создайте пользователя testread с паролем test123
дайте роль readonly пользователю testread
зайдите под пользователем testread в базу данных testdb

```sql
grant connect on DATABASE testdb TO readonly;
grant usage on SCHEMA testnm to readonly;
grant SELECT on all TABLEs in SCHEMA testnm TO readonly;
CREATE USER testread with password 'test123';
grant readonly TO testread;
```

4. сделайте select * from t1;
получилось? (могло если вы делали сами не по шпаргалке и не упустили один существенный момент про который позже)
напишите что именно произошло в тексте домашнего задания
у вас есть идеи почему? ведь права то дали?

```sql
нет, не получилось
ОШИБКА:  нет доступа к таблице t1
SQL state: 42501

таблица по умолчанию создалась в public
```


5. вернитесь в базу данных testdb под пользователем postgres
удалите таблицу t1
создайте ее заново но уже с явным указанием имени схемы testnm
вставьте строку со значением c1=1
зайдите под пользователем testread в базу данных testdb
сделайте select * from testnm.t1;
получилось?
есть идеи почему?

```sql
DROP TABLE t1;
CREATE TABLE testnm.t1(c1 integer);
INSERT INTO testnm.t1 values(1);
```
не получилось, так как доступ только для существующих на тот момент времени таблиц 

6. как сделать так чтобы такое больше не повторялось? 
ALTER default privileges in SCHEMA testnm grant SELECT on TABLES to readonly; 

7. сделайте select * from testnm.t1;

не получилось, так как ALTER default будет действовать для новых таблиц а grant SELECT on all TABLEs in SCHEMA testnm TO readonly отработал только для существующих 

8. теперь попробуйте выполнить команду create table t2(c1 integer); insert into t2 values (2);
 получилось, так как схема первая для поиска public, и все действия даются для роли public и каждый пользователь может создавать объекты в схеме public любой базы данных
```sql
REVOKE CREATE on SCHEMA public FROM public; 
REVOKE ALL on DATABASE testdb FROM public; 
```
9. теперь попробуйте выполнить команду create table t3(c1 integer); insert into t2 values (2);
расскажите что получилось и почему

теперь без прав не получится