<h1>ROS:основы</h2>
<h3>ROS</h3>
Операционная система для роботов — это экосистема для программирования роботов, предоставляющая функциональность для распределённой работы. <br>
ROS (Robot Operating System) обеспечивает разработчиков библиотеками и инструментами для создания приложений робототехники. ROS обеспечивает аппаратную абстракцию, предлагает драйверы устройств, библиотеки, визуализаторы, обмен сообщениями, менеджеры пакетов и многое другое.<br>
<h3>Master</h3>
"Именной" сервер для соединение типа node-to-node, а также обмена сообщениями. Команда <code>roscore</code> используется для запуска мастера, когда же мастер запущен имя каждого узла (node) будет регистрироваться в нем, по запросу информацию можно достать. <br>
Мастер общается со slave-серверами через соединение XMLRPC (XML-Remote Procedure Call), что по своей сути является HTTP-based протоколом соединения, который просто не поддерживает соединение. Иными словами slave-node не поддерживает постоянное соединение с мастером, только когда нужно зарегистрировать свою информацию или запросить информацию о другом узле. XMLPRC делает ROS очень легковесным, что позволяет его использовать в больших системах, так и в маленьких. <br>
Когда вы запускайте ROS, Мастер будет сконфигурирован с указанным URI адрессом, адресс указывается в переменной ROS_MASTER_URI. По умолчанию локальный IP адресс и порт 11311<br>
<h3>Node</h3>
Узел это самый маленький юнит процессов, запущенных в ROS. Если сравнивать узел с испольняемой программой, то сравнение будет правильным. ROS требует создания отдельного узла под отдельную задачу. <br>
Как узел запускается он регистрирует информацию о себе: имя, тип сообщений, URI адресс, номер порта. Важно отметить что обмен сообщениями между узлами работает без участия мастера (соединение между узлами происходит на прямую). Мастер обеспечивает только единое пространство имен для решения вопроса как (куда) подключиться к конкретному узлу. Адрес запуска ноды, берётся из переменной окружения <code>ROS_HOSTNAME</code><br>
Узлы для связи с мастером используют протокол XMLRPC, для свзяи между друг другом XMLRPC TCPROS (уровень Transport для сообщений ROS и сервисов. Использует стандартный TCP/IP сокет для траспортировки информации)
<h3>Package</h3>
Пакет является основной единицей ROS. Любое приложение ROS оформляется в пакет, в котором определяются: конфигурация пакета, ноды необходимые для работы пакета, зависимости от других пакетов ROS.<br>
Работа с пакетами ROS очень похожа на работу с пакетами linux. Пакет ROS можно поставить готовым из репозитория пакетов, так и скачать и скомпилировать из source кода.<br>
<h3>Message</h3>
Узлы  отправляют и принимают данные между собой, согласно заданного формата. Эти данные называют сообщениями, а описание типом сообщения. <br>
Сообщения могут быть как простых типов (integer, float, boolean), так и могут состоять из сложных структур, содержащих вложенные сообщения и массивы сообщений. <br>
<h3>Topic</h3>
Тема (Topic), это один из видов обмена сообщениями, который буквально похож на тему в разговоре. Узел издателя (publisher) сначала регистрирует свою тему на мастере, а затем начинает публикацию сообщений в эту тему (topic). Узлы подписчиков, которые хотят получать информацию из этой темы при помощи мастера получают адрес этой темы и далее получают сообщения из этой темы. <br>
<h3>Publisher</h3>
Издателем называется процесс, который рассылает сообщения в рамках созданноq темы для других узлов. Один узел может содержать несколько издателей, публикующих данные в разные темы.<br>
<h3>Subscriber</h3>
The term ‘subscribe’ stands for the action of receiving relative messages corresponding to the
topic. The subscriber node registers its own information and topic with the master, and receives
publisher information that publishes relative topic from the master. Based on received publisher
information, the subscriber node directly requests connection to the publisher node and
receives messages from the connected publisher node.<br>
The topic communication is an asynchronous communication which is based on publisher
and subscriber, and it is useful to transfer certain data. Since the topic continuously transmits and receives stream of messages once connected, it is often used for sensors that must
periodically transmit data. On the other hands, there is a need for synchronous communication
with which request and response are used. Therefore, ROS provides a message synchronization
method called ‘service’. <br>
<h3>Service</h3>
The service is synchronous bidirectional communication between the service client that
requests a service regarding a particular task and the service server that is responsible for
responding to requests. <br>
<h3>Service Server</h3>
The ‘service server’ is a server in the service message communication that receives a request as
an input and transmits a response as an output. Both request and response are in the form of
messages. <br>
<h3>Service Client</h3>
The ‘service client’ is a client in the service message communication that requests service to the server and receives a response as an input. Both request and response are in the form of message. <br>
<h3>Action</h3>
The action11 is another message communication method used for an asynchronous bidirectional
communication. Action is used where it takes longer time to respond after receiving a request
and intermediate responses are required until the result is returned. The structure of action file is also similar to that of service. <br>
<h3>Action Server</h3>
The ‘action server’ is in charge of receiving goal from the client and responding with feedback
and result. Once the server receives goal from the client, it performs predefined process.<br>
<h3>Action Client</h3>
The ‘action client’ is in charge of transmitting the goal to the server and receives result or
feedback data as inputs from the action server. <br>