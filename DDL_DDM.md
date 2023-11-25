## Домашнее задание к занятию «Работа с данными (DDL/DML)» - "Вячеслав Савилов"

### Задание 1
### 1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.
Предварительно нужно установить пакет gnupg, приусловии что его устанавливали ранее:
```
apt-get install gnupg
```
Установим MySQL APT репозиторий, переходим на страничку:
```
https://dev.mysql.com/downloads/
```
Последний пакет называется mysql-apt-config_0.8.25-1_all.deb, копируем ссылку на него. Загрузим пакет:
```
cd /tmp

wget -c https://dev.mysql.com/get/mysql-apt-config_0.8.25-1_all.deb

ls -fla | grep mysql
```
Установим пакет:
```
dpkg -i mysql-apt-config_0.8.25-1_all.deb
```
После установки пакета в /etc/apt/source.list.d/ добавится mysql.list.

Обновляем репозиторий:
```
apt-get update
```
Установим MySQL сервер.
```
apt-get install mysql-server
```
В процессе установки нас просят установить пароль пользователя root для MySQL. Выбираем плагин аутентификации по умолчанию. Рекомендуется Strong Password Encryption. Ok. Установка завершена.

### 1.2. Создайте учётную запись sys_temp.

Вход в консоль MySQL: 
```
mysql -u root -p

CREATE USER 'sys_temp'@'localhost' IDENTIFIED BY 'password';
```
### 1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)
```
mysql> SELECT user FROM mysql.user
```
![1_2](https://github.com/slava1005/FOPS-13/assets/114395964/f954b87d-692d-450c-a946-6fdd2ac134ec)

### 1.4. Дайте все права для пользователя sys_temp.
```
mysql> GRANT ALL PRIVILEGES ON . TO 'sys_temp'@'localhost';
```
### 1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)
```
mysql> SHOW GRANTS FOR 'sys_temp'@'localhost';
```

![1_5](https://github.com/slava1005/FOPS-13/assets/114395964/95c6804b-d87f-45d3-ae05-c4a9c0ff7883)

### 1.6. Переподключитесь к базе данных от имени sys_temp. Для смены типа аутентификации с sha2 используйте запрос:
```
ALTER USER 'sys_temp'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
### 1.6.1. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.

### 1.7. Восстановите дамп в базу данных.

![1_7](https://github.com/slava1005/FOPS-13/assets/114395964/549a7a61-3a6d-4430-a0a8-9f540304a132)

### 1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)
![1_8](https://github.com/slava1005/FOPS-13/assets/114395964/3d7a2404-69cb-41cc-a0e9-3e70c4b95c7d)
![1_8_1](https://github.com/slava1005/FOPS-13/assets/114395964/6ef8977d-add8-49d7-b115-0fd01f338c99)
![1_8_2](https://github.com/slava1005/FOPS-13/assets/114395964/e3a60cbe-1e7d-4b00-943e-b3605ed380e7)
 
 Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.

### Задание 2 Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)

Название таблицы | Название первичного ключа customer | customer_id
![2](https://github.com/slava1005/FOPS-13/assets/114395964/34f5fa7e-08b2-4767-a903-67ce6b425b65)

