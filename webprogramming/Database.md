# Database

## Полезные ссылки
- <a href='https://www.sqlite.org/cli.html'>Руководство пользования CLI SQLite'а</a>
- <a href='https://sqlite.org/index.html'>SQLite официальный сайт</a>
- <a href='https://www.digitalocean.com/community/tutorials/how-to-use-flask-sqlalchemy-to-interact-with-databases-in-a-flask-application'>подсоединение SQLite к Flask app</a>



## База данных простыми словами
Ба́за да́нных — совокупность данных, хранимых в соответствии со схемой данных, манипулирование которыми выполняют в соответствии с правилами средств моделирования данных

## Database
- SQLite

## ТЗ для создания базы данных заметок
- скачайте SQLite для Windows
- Укажите SQLite в переменной PATH
- Создайте базу данных  db.sqlite3
- Создайте Таблицу Tasks
	- details: таблица должна содердать поля: id (целые числа, primary key), title(строка, ограничение по символам 30, не нулевое), content(строка, ограничение по символам 255, не нулевое)
- Connect Backend с базой данных
