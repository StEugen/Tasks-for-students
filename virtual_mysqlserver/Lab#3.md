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
Пример запроса<br>
<pre>
SELECT customer.customer_id, customer.name, COUNT(`order`.order_id) AS total_orders
FROM customer
JOIN `order` ON customer.customer_id = `order`.customer_id
GROUP BY customer.customer_id, customer.name;
</pre>

5. Добавьте новую таблицу под названием product со столбцами для product_id, product_name, price и description. Добавьте новую таблицу с именем order_item со столбцами для order_id, product_id, quantity и subtotal. Настройте отношения внешнего ключа между столбцом product_id в таблице order_item и столбцом product_id в таблице product, а также между столбцом order_id в таблице order_item и столбцом order_id в таблице order для обеспечения ссылочной целостности.

6. Вставьте данные в таблицы product и order_item. Убедитесь, что каждый элемент заказа имеет действительный order_id, соответствующий заказу в таблице заказов, и действительный product_id, соответствующий продукту в таблице product.

7. Напишите запрос, чтобы получить общую выручку, полученную от каждого продукта, по всем заказам. используйте функцию SUM чтобы получить сумму, как выручку.   
Пример запроса:<br>
<pre>
SELECT product.product_id, product.product_name, SUM(order_item.subtotal) AS total_revenue
FROM product
JOIN order_item ON product.product_id = order_item.product_id
GROUP BY product.product_id, product.product_name;
</pre>

8. Напишите запрос, чтобы получить общую выручку, полученную каждым клиентом по всем заказам.
Пример запроса: <br> 
<pre>
SELECT customer.customer_id, customer.name, SUM(order.total_price) AS total_revenue
FROM customer
JOIN `order` ON customer.customer_id = `order`.customer_id
GROUP BY customer.customer_id, customer.name;
</pre>

