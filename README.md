# Домашнее задание к занятию "Работа с данными DDL/DML`" - `Савин Алексей`  

Задание можно выполнить как в любом IDE, так и в командной строке.  

### Задание 1   
1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.  
1.2. Создайте учётную запись sys_temp.  
1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)  
1.4. Дайте все права для пользователя sys_temp.  
1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)  
1.6. Переподключитесь к базе данных от имени sys_temp.  
Для смены типа аутентификации с sha2 используйте запрос:  
`ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';`  
По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.  
1.7. Восстановите дамп в базу данных.  
1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)  
Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.  

### Решение
1.1
```
sudo apt update
sudo apt install gnupg
wget -c https://dev.mysql.com/get/mysql-apt-config_0.8.24-1_all.deb
sudo dpkg -i mysql-apt-config_0.8.24-1_all.deb
sudo apt update
sudo apt install -y mysql-server # задаём пароль для пользователя root в СУБД MySQL
sudo systemctl status mysql.service
```
![mysql-version](https://github.com/AI-Savin/12_02_DDL_DML/blob/main/img/mysql-version.png)   

1.2 Создал учётную запись `sys_temp`  

```
mysql -u root -p 
create user 'sys_temp'@'%' identified by '12345';
exit
```
![sys_temp](https://github.com/AI-Savin/12_02_DDL_DML/blob/main/img/sys_temp.png)  

1.3. Запрос на получение списка пользователей в базе данных.  

```
mysql -u root -p 
select User,Host from mysql.user;
exit
```
![user-list](https://github.com/AI-Savin/12_02_DDL_DML/blob/main/img/user_list.png)  

1.4. Дайте все права для пользователя sys_temp.  

```
mysql -u root -p 
grant ALL PRIVILEGES on *.* to 'sys_temp'@'%' with GRANT option;
flush privileges;
exit
```
![grants_sys_temp](https://github.com/AI-Savin/12_02_DDL_DML/blob/main/img/grants_sys_temp.png)

1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)  
```
mysql -u root -p 
show grants for 'sys_temp'@'%';
exit
```

![show_grants](https://github.com/AI-Savin/12_02_DDL_DML/blob/main/img/show_grants.png)  

1.6. Переподключитесь к базе данных от имени sys_temp  

```
mysql -u sys_temp -p 
alter user 'sys_temp'@'%' IDENTIFIED with mysql_native_password by '12345';
exit
```

![sys_temp_login](https://github.com/AI-Savin/12_02_DDL_DML/blob/main/img/sys_temp_login.png)  

Cкачиваем дамп базы данных
```
wget -c https://downloads.mysql.com/docs/sakila-db.zip
unzip sakila-db.zip
cd sakila-db
```

1.7. Восстановите дамп в базу данных.  

```
mysql -u sys_temp -p
show databases;
create database SakilaDB;
show databases;
use SakilaDB;
show tables;
source sakila-schema.sql;
source sakila-data.sql;
show tables;
exit
```

1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)  

```
mysql -u sys_temp -p
show databases;
use SakilaDB;
show tables;
exit
```
![Tables_sakila](https://github.com/AI-Savin/12_02_DDL_DML/blob/main/img/Tables_sakila.png)  

---

### Задание 2  

Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)  
```
Название таблицы | Название первичного ключа
customer         | customer_id
```

### Решение 2

Скришот выполнения команды  
`SELECT TABLE_NAME, COLUMN_NAME FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_SCHEMA = 'sakila' AND COLUMN_KEY = 'PRI' ORDER BY table_name;`  

![tables-primary_key](https://github.com/AI-Savin/12_02_DDL_DML/blob/main/img/tables-primary%20key.png) 

---
