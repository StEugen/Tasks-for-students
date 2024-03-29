# Frontend

## Полезные ссылки
- <a href='https://jinja.palletsprojects.com/en/3.1.x/'>Jinja официальный сайт</a>
- <a href='https://en.wikipedia.org/wiki/Frontend_and_backend'>Frontend и Backend Wikipedia</a>
- <a href='https://ru.wikipedia.org/wiki/HTML'>HTML в Wikipedia</a>
- <a href='https://www.w3schools.com/html/'>W3School Tutorial HTML</a>
- <a href='http://htmlbook.ru'>HTML book, справочник HTML на русском</a>
- <a href='https://ru.wikipedia.org/wiki/CSS'>CSS в Wikipedia</a>
- <a href='https://www.w3schools.com/css/'>W3School CSS Tutorials</a> 
- <a href='https://getbootstrap.com'>Официальный сайт Bootstrap</a>


## Фронтэнд простыми словами
Что такое фронтэнд (frontend) - это часть веб-приложения, с которой взаимодействует пользователь, с помощью фронтэнда происходит ввод данных пользователем, репрезентация данных из бекэнда. 

## Frontend Stack:
- HTML/CSS - языки разметки для создания документов 
- Jinja - template-engine, движок для генерирования шаблонов
- Дополнительно (не обязательно использовать):
    - Bootstrap - свободный набор инструментов для создания сайтов и веб-приложений. Включает в себя HTML и CSS шаблоны.

## ТЗ для создания фронтэнда приложения заметок
- В папке <code>template/</code> создать <code>base.html</code> файл
    - details: данный файл будет хранить в себе основные данные о страницы, все остальные страницы будут генерироваться с использованием данного файла. Использовать Jinja для этого.

- В папке <code>template/</code> создать файл <code>index.html</code>
    - details: файл должен содержать форму для добавления заметок, отображать существующие заметки, также генерировать кнопки "удалить", "изменить" для заметок, кнопки должны отправлять запрос к определенному URL соотвественно.

- В папке <code>template/</code> создать файл <code>edit.html</code>
    - details: файл должен содержать форму для изменения заметки, должен отправлять <code>POST</code> запрос к соответствующему URL для изменения заметки

- Отредактировать отображение формы и заметок, центрировать, на странице index.html
    - details: создать файл <code>index.css</code> в папке <code>static/css</code>, который будет отвечать за стили на странице <code>index.html</code>. Заметки и форма должны отображаться по центру, приветствуется визуальное отделение одной заметки от другой для удобства пользователя. Подключить файл стилей в файле <code>index.html</code>

- Отредактировать отображение формы, центрировать, на странице edit.html
    - details: создать файл <code>edit.css</code>, который будет отвечать за стили на странице <code>edit.html</code>. Форма должна отображаться по центру, приветствуется визуальное выделение формы для удобства пользователя. Подключить файл стилей в файле <code>edit.html</code>

- Дополнительно (не обязательно): Применить библиотеку <code>Bootstrap</code> при созданнии фронтэнда. 