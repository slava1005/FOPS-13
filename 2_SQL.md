# Домашнее задание к занятию 2. «SQL»

## Задача 1

Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, 
в который будут складываться данные БД и бэкапы.

Приведите получившуюся команду или docker-compose манифест.

```yml
version: '3'

services:
  database:
    image: postgres:12
    ports:
      - 5432:5432
    environment:
      POSTGRES_USER: sav
      POSTGRES_PASSWORD: password
      POSTGRES_DB: test_db
    volumes:
      - ./db-data/:/var/lib/postgresql/data/
      - ./db-backup/:/Work/BackUp/

```

## Задача 2

В БД из задачи 1: 
- создайте пользователя test-admin-user и БД test_db
- в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже)
- предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db
- создайте пользователя test-simple-user  
- предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE данных таблиц БД test_db

Таблица orders:
- id (serial primary key)
- наименование (string)
- цена (integer)

Таблица clients:
- id (serial primary key)
- фамилия (string)
- страна проживания (string, index)
- заказ (foreign key orders)

Приведите:
- итоговый список БД после выполнения пунктов выше,

```sql
select datname, datcollate from pg_catalog.pg_database 
```

```
datname  |datcollate|
---------+----------+
postgres |en_US.utf8|
test_db  |en_US.utf8|
template1|en_US.utf8|
template0|en_US.utf8|
```

- описание таблиц (describe)

```sql
SELECT 
   table_catalog, table_schema, table_name, column_name, data_type 
FROM 
   information_schema.columns
WHERE 
   table_name in ('orders', 'clients')
```

```
table_catalog|table_schema|table_name|column_name      |data_type|
-------------+------------+----------+-----------------+---------+
test_db      |public      |orders    |id               |integer  |
test_db      |public      |orders    |Наименование     |text     |
test_db      |public      |orders    |Цена             |integer  |
test_db      |public      |clients   |id               |integer  |
test_db      |public      |clients   |Фамилия          |text     |
test_db      |public      |clients   |Страна проживания|text     |
test_db      |public      |clients   |Заказ            |integer  |
```

- SQL-запрос для выдачи списка пользователей с правами над таблицами test_db

```sql
SELECT 
    grantee, table_name, privilege_type 
FROM 
    information_schema.table_privileges 
WHERE 
    grantee in ('test-admin-user','test-simple-user')
    and table_name in ('clients','orders')
order by 
    1,2,3;
```

- список пользователей с правами над таблицами test_db

```
grantee         |table_name|privilege_type|
----------------+----------+--------------+
test-admin-user |clients   |DELETE        |
test-admin-user |clients   |INSERT        |
test-admin-user |clients   |REFERENCES    |
test-admin-user |clients   |SELECT        |
test-admin-user |clients   |TRIGGER       |
test-admin-user |clients   |TRUNCATE      |
test-admin-user |clients   |UPDATE        |
test-admin-user |orders    |DELETE        |
test-admin-user |orders    |INSERT        |
test-admin-user |orders    |REFERENCES    |
test-admin-user |orders    |SELECT        |
test-admin-user |orders    |TRIGGER       |
test-admin-user |orders    |TRUNCATE      |
test-admin-user |orders    |UPDATE        |
test-simple-user|clients   |DELETE        |
test-simple-user|clients   |INSERT        |
test-simple-user|clients   |SELECT        |
test-simple-user|clients   |UPDATE        |
test-simple-user|orders    |DELETE        |
test-simple-user|orders    |INSERT        |
test-simple-user|orders    |SELECT        |
test-simple-user|orders    |UPDATE        |
```

## Задача 3

Используя SQL синтаксис - наполните таблицы следующими тестовыми данными:

Таблица orders

|Наименование|цена|
|------------|----|
|Шоколад| 10 |
|Принтер| 3000 |
|Книга| 500 |
|Монитор| 7000|
|Гитара| 4000|

Таблица clients

|ФИО|Страна проживания|
|------------|----|
|Иванов Иван Иванович| USA |
|Петров Петр Петрович| Canada |
|Иоганн Себастьян Бах| Japan |
|Ронни Джеймс Дио| Russia|
|Ritchie Blackmore| Russia|

Используя SQL синтаксис:
- вычислите количество записей для каждой таблицы 
- приведите в ответе:
    - запросы 
    - результаты их выполнения.

Запросы на добавление данных в таблицы:

```sql
INSERT INTO orders VALUES (1, 'Шоколад', 10), (2, 'Принтер', 3000), (3, 'Книга', 500), (4, 'Монитор', 7000), (5, 'Гитара', 4000);

INSERT INTO clients VALUES (1, 'Иванов Иван Иванович', 'USA'), (2, 'Петров Петр Петрович', 'Canada'), (3, 'Иоганн Себастьян Бах', 'Japan'), (4, 'Ронни Джеймс Дио', 'Russia'), (5, 'Ritchie Blackmore', 'Russia');
```

Запросы вычисляющие количество записей в каждой таблице:

```
select count(1) from clients

count|
-----+
    5|

select count(1) from orders

count|
-----+
    5|
```

## Задача 4

Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys свяжите записи из таблиц, согласно таблице:

|ФИО|Заказ|
|------------|----|
|Иванов Иван Иванович| Книга |
|Петров Петр Петрович| Монитор |
|Иоганн Себастьян Бах| Гитара |

Приведите SQL-запросы для выполнения данных операций.

```sql
UPDATE clients SET "Заказ" = (SELECT id FROM orders WHERE "Наименование"='Книга') WHERE "Фамилия"='Иванов Иван Иванович';
UPDATE clients SET "Заказ" = (SELECT id FROM orders WHERE "Наименование"='Монитор') WHERE "Фамилия"='Петров Петр Петрович';
UPDATE clients SET "Заказ" = (SELECT id FROM orders WHERE "Наименование"='Гитара') WHERE "Фамилия"='Иоганн Себастьян Бах';
```

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод данного запроса.
 
Подсказка - используйте директиву `UPDATE`.

```sql
SELECT c."Фамилия", c."Страна проживания", o."Наименование"  FROM clients c JOIN orders o ON c.Заказ = o.id;
```

Результат запроса:

```
Фамилия             |Страна проживания|Наименование|
--------------------+-----------------+------------+
Иванов Иван Иванович|USA              |Книга       |
Петров Петр Петрович|Canada           |Монитор     |
Иоганн Себастьян Бах|Japan            |Гитара      |
```

## Задача 5

Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 
(используя директиву EXPLAIN).

Приведите получившийся результат и объясните, что значат полученные значения.

```sql
explain SELECT c."Фамилия", c."Страна проживания", o."Наименование"  FROM clients c JOIN orders o ON c.Заказ = o.id;
```

Результат:

```
QUERY PLAN                                                             |
-----------------------------------------------------------------------+
Hash Join  (cost=37.00..57.24 rows=810 width=96)                       |
  Hash Cond: (c."Заказ" = o.id)                                        |
  ->  Seq Scan on clients c  (cost=0.00..18.10 rows=810 width=68)      |
  ->  Hash  (cost=22.00..22.00 rows=1200 width=36)                     |
        ->  Seq Scan on orders o  (cost=0.00..22.00 rows=1200 width=36)|
```

1. Построчно прочитана таблица orders.
2. Создан кеш по полю id для таблицы orders.
3. Прочитана таблица clients.
4. Для каждой строки по полю "заказ" будет проверено, соответствует ли она чему-то в кеше orders:
- если соответствия нет - строка будет пропущена;
- если соответствие есть, то на основе этой строки и всех подходящих строках кеша СУБД сформирует вывод.


## Задача 6

Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. Задачу 1).

Остановите контейнер с PostgreSQL (но не удаляйте volumes).

Поднимите новый пустой контейнер с PostgreSQL.

Восстановите БД test_db в новом контейнере.

Приведите список операций, который вы применяли для бэкапа данных и восстановления. 

```
root@76f73d65a9b0:/# export PGPASSWORD=password && pg_dumpall -h localhost -U test-admin-user > /Work/BackUp/test_db.sql

root@76f73d65a9b0:/# ls /Work/BackUp/
test_db.sql

docker-compose stop

docker run --rm -d -e POSTGRES_USER=test-admin-user -e POSTGRES_PASSWORD=password -e POSTGRES_DB=test_db -v /Users/avb/git/FOps13/06-02-sql/db-backup/:/Work/backup --name psql2 postgres:12

docker ps
CONTAINER ID   IMAGE         COMMAND                  CREATED         STATUS         PORTS      NAMES
d7f54ff5ada3   postgres:12   "docker-entrypoint.s…"   5 seconds ago   Up 4 seconds   5432/tcp   psql2

docker exec -it d7f54ff5ada3 bash
root@d7f54ff5ada3:/# ls /Work/backup/
test_db.sql
root@d7f54ff5ada3:/# export PGPASSWORD=password && psql -h localhost -U test-admin-user -f /Work/backup/test_db.sql test_db
```