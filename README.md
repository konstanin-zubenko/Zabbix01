Домашнее задание к занятию «Система мониторинга Zabbix» Зубенко Константин Викторович

Задание 1
Установите Zabbix Server с веб-интерфейсом.

Процесс выполнения
Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.
Требования к результатам
Прикрепите в файл README.md скриншот авторизации в админке.
Приложите в файл README.md текст использованных команд в GitHub.

Ответ:


Устанавливаем PostgreSQL: sudo apt install postgresql
`Набор команд с сайта Zabbix:
скачиваем репазиторий с Zabbix: wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-2+ubuntu22.04_all.deb
устанавливаем пакет репазитория с Zabbix:
dpkg -i zabbix-release_7.0-2+ubuntu22.04_all.deb
обновляем репазиторий:
apt update'
'устанавливаем zabbix-server-pgsql zabbix-frontend-php php8.1-pgsql zabbix-apache-conf zabbix-sql-scripts: apt install zabbix-server-pgsql zabbix-frontend-php php8.1-pgsql zabbix-apache-conf zabbix-sql-scripts'
'Создал базу данных: sudo -u postgres createuser --pwprompt zabbix sudo -u postgres createdb -O zabbix zabbix'
'На хосте Zabbix сервера импортировал начальную схему и данные: zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix'
'Отредактировал файл /etc/zabbix/zabbix_server.conf: DBPassword=090807
'Запустил процессы Zabbix сервера: systemctl restart zabbix-server apache2 systemctl enable zabbix-server apache2 systemctl status zabbix-server apache2'
'Авторизовался через веб интерфейс: http://89.169.172.205/zabbix/ Логин и пароль для входа в админ-панель — Admin\zabbix/

'Скриншот авторизации в админке'

![alt text](https://github.com/konstanin-zubenko/Zabbix01/blob/main/img/51.png)

Задание 2
Установите Zabbix Agent на два хоста.

Процесс выполнения
Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.
Требования к результатам
Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
Приложите в файл README.md текст использованных команд в GitHub

Ответ:

'Установил Zabbix Agent на 2 хоста (cozu@84.201.141.11 и cozu@89.169.172.205):'

`Зашел по SSH на 1й хост: ssh -l cozu@84.201.141.11
'Установил репозиторий Zabbix: wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-2+ubuntu22.04_all.deb' dpkg -i zabbix-release_7.0-2+ubuntu22.04_all.deb apt update'
'Установил Zabbix агент: apt install zabbix-agent'
'Запустил процесс Zabbix агента: systemctl restart zabbix-agent systemctl enable zabbix-agent systemctl status zabbix-agent.service - проверил статус'
'Всё тоже самое сделал со 2й машиной: cozu@89.169.172.205
'Добавил Zabbix Server в список разрешенных серверов Zabbix Agentов.'

'На обоих хостах внес изменения в файл zabbix_agentd.conf (Server=10.129.0.33) sudo nano /etc/zabbix/zabbix_agentd.conf'
'Добавил Zabbix Agentов в раздел Configuration > Hosts Zabbix Servera.

'СКРИНШОТЫ'

'Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу: 
![alt text](https://github.com/konstanin-zubenko/Zabbix01/blob/main/img/56.png)

'Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером: Команда: tail -f /var/log/zabbix/zabbix_agentd.log 
![alt text](https://github.com/konstanin-zubenko/Zabbix01/blob/main/img/57.png)
![alt text](https://github.com/konstanin-zubenko/Zabbix01/blob/main/img/58.png)'

'Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные: 
![alt text](https://github.com/konstanin-zubenko/Zabbix01/blob/main/img/55.png)`

'Приложите в файл README.md текст использованных команд в GitHub: git clone https://github.com/konstanin-zubenko/Zabbix01.git git commit -m "Zabbix" git push main'