# Задание - https://github.com/netology-code/smon-homeworks/blob/main/hw-02.md

### Цели задания
1.	Научиться устанавливать Zabbix Server c веб-интерфейсом
2.	Научиться устанавливать Zabbix Agent на хосты
3.	Научиться устанавливать Zabbix Agent на компьютер и подключать его к серверу Zabbix

### Задание 1
Установите Zabbix Server с веб-интерфейсом.
Процесс выполнения
1.	Выполняя ДЗ, сверяйтесь с процессом, отражённым в записи лекции.
2.	Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитории Debian 11.
3.	Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
4.	Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.
Требования к результатам
1.	Прикрепите в файл README.md скриншот авторизации в админке.
2.	Приложите в файл README.md текст использованных команд в GitHub.

### Решение:
Инструкция по настройке - https://www.zabbix.com/ru/download?zabbix=7.4&os_distribution=ubuntu&os_version=24.04&components=server_frontend_agent&db=pgsql&ws=apache
1.	Установите PostgreSQL: sudo apt install postgresql
   
3.	Установите репозиторий Zabbix
Wgethttps://repo.zabbix.com/zabbix/7.4/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.4+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.4+ubuntu24.04_all.deb
apt update

4.	Установите Zabbix сервер, веб-интерфейс и агент
apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent

6.	Создайте пользователя БД: sudo -u postgres createuser --pwprompt Zabbix
Создайте БД: sudo -u postgres createdb -O zabbix zabbix

8.	На хосте Zabbix сервера импортируйте начальную схему и данные. Вам будет предложено ввести недавно созданный пароль.
zcat /usr/share/zabbix/sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql Zabbix

10.	Настраиваем пароль DBPassword в файле /etc/zabbix/zabbix_server.conf: 
sudo nano /etc/zabbix/zabbix_server.conf
Пароль поставил DBPassword=1234

12.	Запускаем Zabbix server, Zabbix agent и веб-сервер:
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2

На хосте 10.0.2.15 находится Zabbix. Логин \ Пароль для входа Admin \ zabbix
 
![Название скриншота 1](https://github.com/Pavel24071993/HomeWork_GIT/blob/main/%D0%9C%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3/%D0%A1%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0%20%D0%BC%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3%D0%B0%20Zabbix/smon-homeworks_02/img/1.png)
![Название скриншота 2](https://github.com/Pavel24071993/HomeWork_GIT/blob/main/%D0%9C%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3/%D0%A1%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0%20%D0%BC%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3%D0%B0%20Zabbix/smon-homeworks_02/img/2.png)
### Задание 2
Установите Zabbix Agent на два хоста.
Процесс выполнения
1.	Выполняя ДЗ, сверяйтесь с процессом, отражённым в записи лекции.
2.	Установите Zabbix Agent на 2 вирт. Машины, одной из них может быть ваш Zabbix Server.
3.	Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
4.	Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
5.	Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.
Требования к результатам
1.	Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
2.	Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
3.	Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
4.	Приложите в файл README.md текст использованных команд в GitHub
   
### Решение:
Zabbix-agent установленный на моей локальной виртуальной машине. Проверяем его статус командой systemctl status zabbix-agent
![Название скриншота 2](https://github.com/Pavel24071993/HomeWork_GIT/blob/main/%D0%9C%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3/%D0%A1%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0%20%D0%BC%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3%D0%B0%20Zabbix/smon-homeworks_02/img/3.png)
Добавляю в /etc/zabbix/zabbix_agentd.conf в строку Server новый IP моей второй вирутальной машины, развёрнутов в Yandex Cloud.
![Название скриншота 2](https://github.com/Pavel24071993/HomeWork_GIT/blob/main/%D0%9C%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3/%D0%A1%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0%20%D0%BC%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3%D0%B0%20Zabbix/smon-homeworks_02/img/4.png)

1.	Следуя инструкции 
https://www.zabbix.com/ru/download?zabbix=7.4&os_distribution=ubuntu&os_version=24.04&components=agent&db=&ws= установил на вторую виртуальную машину Yandex Cloud только Zabbix-agent
2.	Настройка Zabbix agent  на удалённом хосте (Yandex Cloud)
Теперь, когда адрес агента есть в списке сервера, он начинает постоянно  пытаться с ним связаться. Посмотрите лог агента: sudo cat /var/log/zabbix/zabbix_agentd.log
В логе ошибка: failed to accept an incoming connection: connection from "212.32.192.41" rejected, allowed hosts: "127.0.0.1"
Это значит, что что коннект к Zabbix-agent, установленный на Виртуальной Машине Yandex не разрешает соединение от этого IP. 212.32.192.41 это IP моей виртуалки основной.
Добавляем в /etc/zabbix/zabbix_agentd.conf мой IP 212.32.192.41 в поле Server
 ![Название скриншота 2](https://github.com/Pavel24071993/HomeWork_GIT/blob/main/%D0%9C%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3/%D0%A1%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0%20%D0%BC%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3%D0%B0%20Zabbix/smon-homeworks_02/img/5.png)
Далее перезагружаем агент и удостоверяемся, что он заработал снова:
systemctl restart zabbix-agent.service
systemctl status zabbix-agent.service
Хост Agent Monitoring это моя локальная Виртуальная Машина. Yandex Cloud Agent облачная вируталка Яндекса с установленным Zabbix_Agenтом.
IP Yandex Cloud – 158.160.204.0 , его указываю в Интерфейсе.
![Название скриншота 2](https://github.com/Pavel24071993/HomeWork_GIT/blob/main/%D0%9C%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3/%D0%A1%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0%20%D0%BC%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3%D0%B0%20Zabbix/smon-homeworks_02/img/6.png)
![Название скриншота 2](https://github.com/Pavel24071993/HomeWork_GIT/blob/main/%D0%9C%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3/%D0%A1%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0%20%D0%BC%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3%D0%B0%20Zabbix/smon-homeworks_02/img/7.png)
Почистил, т.к. уже стал слишком нечитаемым. Перезапустил сервис агента. Лог:
![Название скриншота 2](https://github.com/Pavel24071993/HomeWork_GIT/blob/main/%D0%9C%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3/%D0%A1%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0%20%D0%BC%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3%D0%B0%20Zabbix/smon-homeworks_02/img/8.png)
![Название скриншота 2](https://github.com/Pavel24071993/HomeWork_GIT/blob/main/%D0%9C%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3/%D0%A1%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0%20%D0%BC%D0%BE%D0%BD%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3%D0%B0%20Zabbix/smon-homeworks_02/img/9.png)
 
### Задание 3 со звёздочкой*
Установите Zabbix Agent на Windows (компьютер) и подключите его к серверу Zabbix.
Требования к результатам
1.	Приложите в файл README.md скриншот раздела Latest Data, где видно свободное место на диске C:
