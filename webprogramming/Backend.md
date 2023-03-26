# Backend

## Полезные ссылки
- <a href='https://en.wikipedia.org/wiki/Frontend_and_backend'>Frontend и Backend Wikipedia</a>
- <a href='https://tproger.ru/translations/rendering-on-the-web/'>Про виды рендеринга: Клиент и Сервер</a>
- <a href='https://flask.palletsprojects.com/en/2.2.x/'>Flask официальный сайт</a>
- <a href='https://flask-sqlalchemy.palletsprojects.com/en/3.0.x/models/'> Официальный сайт SQLAlchemy</a>
- <a href='https://flask-sqlalchemy-russian.readthedocs.io/ru/latest/models.html'>Readthedoc.io декларирование Models</a>
- <a href='https://ru.wikipedia.org/wiki/HTTP'>HTTP в Wikipedia, искать какие запросы бывают, интересуют GET и POST</a>

## Бекэнд простыми словами
Что такое бэкэнд (backend) - это часть веб-приложения, которая отвечает за логику и доступ к данным приложения. Также, может отвечать за рендер страниц, при условии, что используется подход Серверного рендера (Server-side rendering).

## Backend Stack:
- Flask - микро-фреймворк для создания веб-приложений на языке программирования Python
- SQLAlchemy - библиотека для Flask, чтобы успешно контактировать с базами данных на основе SQL

## Установка зависимостей
<code>pip install Flask</code><br>
<code>pip install flask_sqlalchemy</code>


## ТЗ для создания бекенда приложения заметок
- Создать файлы необходимые для проекта:
    - details: создать основной файл программы <code>app.py</code>, создать файл <code>model.py</code> для моделей. Создать файл <code>README.md</code> содержащий информацию о проекте

- Создать все необходимые директории
    - details: создать директорию <code>static</code> для хранения статических файлов, создать директорию <code>static/css</code> для хранения файлов стилей, создать директорию <code>template</code> для хранения шаблонов и страниц html.

- Создать модель для заметок:
    - details: создать модель Todo в файле model.py. Использовать SQLAlchemy для этого. Модель должна содержать поля <code> id</code>, которое будет являться <code>Primary key</code>, поле <code>title</code>,которое будет содержать в себе короткое описание заметки (ограничение по символам: 30), <code>content</code> поле должно включать в себя подробное описание заметки (ограничение по символам: 255)

- Создать основной путь для веб-приложения (root route: /):
    - details: root route для отображения страницы, создать запрос к базе данных, чтобы отобразить заметки. Передавать их на страницу для рендера

- Создать путь для добавления заметок (add route: /add):
    - details: Использовать метод POST для создания запроса к серверу, считывать title и content из формы, которая передается при отправке (submit'е). Заносить данные в базу данных

- Создать путь для удаления заметок (delete route: /delete/<int:id>)
    - details: Должен получать id заметки, фильтровать заметки по полученному id, если заметка есть, то удалить ее, после чего возвратить пользователя на страницу

- Создать путь для редактирования заметок (edit route: /edit:<int:id>)
    - details: Должен получать <code>id</code> заметки, фильтровать по <code>id</code>, работает с двумя методами <code>POST and GET</code>, если <code>POST</code> запрос, то редактировать заметку, если <code>GET</code> запрос, то возвращать страницу <code>edit.html</code>
    <pre> 
    ## code snippet
    if request.method == "POST":
        todo.title = request.form['title']
        todo.content = request.form['content']
        
        db.session.commit()
    </pre>

- Обязательно использовать проверку, что app.py файл, является основным файлом для запуска программы:
    - hint: Using the <code>if __name__ == '__main__':</code> (name and main have __ before and after) statement is a best practice in Python when writing scripts that can be used as standalone applications or imported as modules into other programs.
    When you execute a Python script directly, such as by running python app.py in the terminal, the __name__ variable is set to '__main__'. On the other hand, if you import the script into another program using the import statement, the __name__ variable will be set to the name of the module.
    Using the if __name__ == '__main__': statement ensures that the code inside the block is only executed if the script is being run as the main program. This can be useful in situations where you want to perform certain actions only when the script is being run directly, such as starting a web server or running a standalone command-line application.
    Additionally, using this statement can also help avoid unintended side effects when the script is imported as a module into another program, since the code inside the block will not be executed in that case.