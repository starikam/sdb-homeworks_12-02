# Домашнее задание к занятию «Работа с данными (DDL/DML)» - Викторов Михаил

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1
1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.

1.2. Создайте учётную запись sys_temp. 

1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)

1.4. Дайте все права для пользователя sys_temp. 

1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

1.6. Переподключитесь к базе данных от имени sys_temp.

Для смены типа аутентификации с sha2 используйте запрос: 
```sql
ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.

1.7. Восстановите дамп в базу данных.

1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*


### Задание 2
Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)
```
Название таблицы | Название первичного ключа
customer         | customer_id
```


## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 3*
3.1. Уберите у пользователя sys_temp права на внесение, изменение и удаление данных из базы sakila.

3.2. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*

## Ответы

### Задание 1
### 1.1
Ставим MySQL 9 на wsl в Docker:
```
docker run --name mysql-viktorov \
  -e MYSQL_ROOT_PASSWORD=mypass \
  -e MYSQL_DATABASE=viktorov \
  -p 3306:3306 \
  -d mysql:9.0
 ``` 

<img width="1087" height="173" alt="image" src="https://github.com/user-attachments/assets/9cf895ea-8dac-464b-930c-c127276db8ca" />

### 1.2
Подключается к контейнеру и создаем пользователя
``` docker exec -it mysql-sakila mysql -u root -p ```

<img width="768" height="342" alt="image" src="https://github.com/user-attachments/assets/02866359-1488-4d3d-a95d-e914afa37f81" />

### 1.3
```
SELECT User, Host FROM mysql.user;
```

<img width="398" height="255" alt="image" src="https://github.com/user-attachments/assets/a5780f68-c504-4915-a8e3-ac7835eea307" />

### 1.4
```
GRANT ALL PRIVILEGES ON *.* TO 'sys_temp'@'localhost';
FLUSH PRIVILEGES;
```

<img width="578" height="147" alt="image" src="https://github.com/user-attachments/assets/3568b14e-6ef2-40f5-98a1-1df2ff2f5718" />

### 1.5
``` 
SHOW GRANTS FOR 'sys_temp'@'localhost';
```

<img width="1403" height="770" alt="image" src="https://github.com/user-attachments/assets/78e2c60c-e126-46e6-a29f-561ddc670a2d" />

### 1.6
Переподключаемся к БД
``` 
sudo docker exec -it mysql-viktorov mysql -u sys_temp -p
```

<img width="778" height="290" alt="image" src="https://github.com/user-attachments/assets/00145163-85ca-4865-8ecc-d2640c51a4e3" />

Качаем дамп.

### 1.7
Копируем в контейнер скачанный дамп:
```
sudo docker cp sakila-data.sql mysql-viktorov:/tmp/
sudo docker cp sakila-schema.sql mysql-viktorov:/tmp/
```


<img width="851" height="150" alt="image" src="https://github.com/user-attachments/assets/fcdccf44-e67a-4d33-bb53-d9f0643cf78b" />

Восстанавливаем дамп:
```
SOURCE /tmp/sakila-schema.sql;
SOURCE /tmp/sakila-data.sql;
```

<img width="383" height="203" alt="image" src="https://github.com/user-attachments/assets/9bd5dfb7-8411-448e-8ca1-4c26d41245e7" />

<img width="372" height="232" alt="image" src="https://github.com/user-attachments/assets/73b41720-acaf-475c-bbea-90b098ee1b27" />

### 1.8
```
USE sakila;
SHOW TABLES;
```

<img width="316" height="635" alt="image" src="https://github.com/user-attachments/assets/bcbfd283-43dc-48ef-8773-90ba94805c1a" />

### Задание 2

Выполним запрос к БД:
```
SELECT 
    TABLE_NAME, 
    COLUMN_NAME AS PRIMARY_KEY
FROM 
    information_schema.KEY_COLUMN_USAGE
WHERE 
    TABLE_SCHEMA = 'sakila'
    AND CONSTRAINT_NAME = 'PRIMARY'
ORDER BY 
    TABLE_NAME;
```

Получим ответ в виде таблицы:

<img width="302" height="475" alt="image" src="https://github.com/user-attachments/assets/6823adbe-ab04-4b0b-9730-18a2980c12b4" />

```
+---------------+--------------+
| TABLE_NAME    | PRIMARY_KEY  |
+---------------+--------------+
| actor         | actor_id     |
| address       | address_id   |
| category      | category_id  |
| city          | city_id      |
| country       | country_id   |
| customer      | customer_id  |
| film          | film_id      |
| film_actor    | actor_id     |
| film_actor    | film_id      |
| film_category | film_id      |
| film_category | category_id  |
| film_text     | film_id      |
| inventory     | inventory_id |
| language      | language_id  |
| payment       | payment_id   |
| rental        | rental_id    |
| staff         | staff_id     |
| store         | store_id     |
+---------------+--------------+
```


