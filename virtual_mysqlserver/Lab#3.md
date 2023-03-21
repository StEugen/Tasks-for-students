# Laboratory No.3

Disclaimer:<br>
Все переменные указанные в {} являются ПЕРЕМЕННЫМИ, значит их надо заменить на актуальные названия переменных и убрать {}

Полезная литература:<br>
- <a href='https://www.mysqltutorial.org/mysql-foreign-key/'>MySQL Tutorial</a>
- <a href='https://en.wikipedia.org/wiki/Foreign_key'>Wikipedia</a>
- <a href='http://www.mysql.ru/docs/man/ANSI_diff_Foreign_Keys.html'>Руководство на русском</a>
- <a href='https://www.w3schools.com/MySQL/mysql_primarykey.asp'>W3School PRIMARY KEY</a>
- <a href='https://www.w3schools.com/MySQL/mysql_foreignkey.asp'>W3School FOREIGN KEY</a>
- <a href='https://www.w3schools.com/MySQL/mysql_join.asp'>W3School JOIN</a>
- <a href='https://www.w3schools.com/MySQL/mysql_select.asp'>W3School SELECT FROM</a>

В данной лабораторной работе, вы попробуйте создать базу данных клиентов магазина

## Техническое задание
Создать базу данных клиентов и заказов, провести ряд запросов к базе данных<br><br>

1. Создайте базу данных под названием shop и две таблицы customer и order. Таблица customer должна содержать столбцы для customer_id, name, email и phone_number. Таблица заказов должна содержать столбцы для order_id, customer_id, order_date и total_price. Не забудьте добавить <code>PRIMARY KEY</code> (в качестве PRIMARY KEY используйте поле отвечающее за id). Настройте связь внешнего ключа между столбцом customer_id в таблице order и столбцом customer_id в таблице customer, чтобы обеспечить ссылочную целостность. Для того чтобы сделать ссылку в order (customer_id в order на customer_id в customer) вам понадобиться <code>FOREIGN KEY</code>:<br>
<pre>
customer_id INT NOT NULL,
FOREIGN KEY (customer_id) REFERENCES customer(customer_id)
</pre> 
 

2. Вставьте данные в таблицы клиентов и заказов. Вставьте пять случайных клиентов, и пять случайных заказов. Убедитесь, что каждый заказ имеет действительный идентификатор customer_id, который соответствует клиенту в таблице customer. (Используйте INSERT для выполнения данной задачи) 

3. Напишите запрос для получения всех заказов для конкретного клиента, объединив таблицы order и customer в столбце customer_id.
Пример запроса:
<pre>
SELECT order.order_id, order.order_date, order.total_price
FROM order
JOIN customer ON order.customer_id = customer.customer_id
WHERE customer.name = 'John Doe';
</pre>

4. Напишите запрос, чтобы получить всех клиентов, которые разместили заказ, вместе с общим количеством размещенных ими заказов.

