# Задание - https://github.com/netology-code/smon-homeworks/blob/main/hw-02.md
###Цели задания
1.	Научиться устанавливать Zabbix Server c веб-интерфейсом
2.	Научиться устанавливать Zabbix Agent на хосты
3.	Научиться устанавливать Zabbix Agent на компьютер и подключать его к серверу Zabbix
Задание 1
Установите Zabbix Server с веб-интерфейсом.
Процесс выполнения
1.	Выполняя ДЗ, сверяйтесь с процессом, отражённым в записи лекции.
2.	Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитории Debian 11.
3.	Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
4.	Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.
Требования к результатам
1.	Прикрепите в файл README.md скриншот авторизации в админке.
2.	Приложите в файл README.md текст использованных команд в GitHub.

Решение:
Инструкция по настройке - https://www.zabbix.com/ru/download?zabbix=7.4&os_distribution=ubuntu&os_version=24.04&components=server_frontend_agent&db=pgsql&ws=apache
1.	Установите PostgreSQL: sudo apt install postgresql
2.	Установите репозиторий Zabbix
Wgethttps://repo.zabbix.com/zabbix/7.4/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.4+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.4+ubuntu24.04_all.deb
	apt update
3.	Установите Zabbix сервер, веб-интерфейс и агент
apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
4.	Создайте пользователя БД: sudo -u postgres createuser --pwprompt Zabbix
Создайте БД: sudo -u postgres createdb -O zabbix zabbix
5.	На хосте Zabbix сервера импортируйте начальную схему и данные. Вам будет предложено ввести недавно созданный пароль.
zcat /usr/share/zabbix/sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql Zabbix
6.	Настраиваем пароль DBPassword в файле /etc/zabbix/zabbix_server.conf: 
sudo nano /etc/zabbix/zabbix_server.conf
Пароль поставил DBPassword=1234
7.	Запускаем Zabbix server, Zabbix agent и веб-сервер:
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2

На хосте 10.0.2.15 находится Zabbix. Логин \ Пароль для входа Admin \ zabbix
 
 
 
Задание 2
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
Решение:
Zabbix-agent установленный на моей локальной виртуальной машине. Проверяем его статус командой systemctl status zabbix-agent
 
Добавляю в /etc/zabbix/zabbix_agentd.conf в строку Server новый IP моей второй вирутальной машины, развёрнутов в Yandex Cloud.
 
 
1.	Следуя инструкции 
https://www.zabbix.com/ru/download?zabbix=7.4&os_distribution=ubuntu&os_version=24.04&components=agent&db=&ws= установил на вторую виртуальную машину Yandex Cloud только Zabbix-agent
2.	Настройка Zabbix agent  на удалённом хосте (Yandex Cloud)
Теперь, когда адрес агента есть в списке сервера, он начинает постоянно  пытаться с ним связаться. Посмотрите лог агента: sudo cat /var/log/zabbix/zabbix_agentd.log
В логе ошибка: failed to accept an incoming connection: connection from "212.32.192.41" rejected, allowed hosts: "127.0.0.1"
Это значит, что что коннект к Zabbix-agent, установленный на Виртуальной Машине Yandex не разрешает соединение от этого IP. 212.32.192.41 это IP моей виртуалки основной.
Добавляем в /etc/zabbix/zabbix_agentd.conf мой IP 212.32.192.41 в поле Server
 
Далее перезагружаем агент и удостоверяемся, что он заработал снова:
systemctl restart zabbix-agent.service
systemctl status zabbix-agent.service
Хост Agent Monitoring это моя локальная Виртуальная Машина. Yandex Cloud Agent облачная вируталка Яндекса с установленным Zabbix_Agenтом.
IP Yandex Cloud – 158.160.204.0 , его указываю в Интерфейсе.
 
 
 
Почистил, т.к. уже стал слишком нечитаемым. Перезапустил сервис агента. Лог:
 
 
Задание 3 со звёздочкой*
Установите Zabbix Agent на Windows (компьютер) и подключите его к серверу Zabbix.
Требования к результатам
1.	Приложите в файл README.md скриншот раздела Latest Data, где видно свободное место на диске C:










# Домашнее задание к занятию "`Git`" - `Александров Павел Павлович


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://i126.fastpic.org/big/2025/1202/f6/1179efa7aa13ecf18061bc17cc04c4f6.jpg)

---

### Задание 1
1.	Зарегистрируйте аккаунт на GitHub.
2.	Создайте новый отдельный публичный репозиторий. Обязательно поставьте галочку в поле «Initialize this repository with a README».
3.	Склонируйте репозиторий, используя https протокол git clone ....
4.	Перейдите в каталог с клоном репозитория.
5.	Произведите первоначальную настройку Git, указав своё настоящее имя и email: git config --global user.name и git config --global user.email johndoe@example.com.
6.	Выполните команду git status и запомните результат.
7.	Отредактируйте файл README.md любым удобным способом, переведя файл в состояние Modified.
8.	Ещё раз выполните git status и продолжайте проверять вывод этой команды после каждого следующего шага.
9.	Посмотрите изменения в файле README.md, выполнив команды git diff и git diff --staged.
10.	Переведите файл в состояние staged или, как говорят, добавьте файл в коммит, командой git add README.md.
11.	Ещё раз выполните команды git diff и git diff --staged.
12.	Теперь можно сделать коммит git commit -m 'First commit'.
13.	Сделайте git push origin master.
В качестве ответа добавьте ссылку на этот коммит в ваш md-файл с решением.

### Решение:
![Название скриншота 1](https://github.com/Pavel24071993/Pavel24071993-HomeWork_8-01/blob/main/img/1.jpg)
![Название скриншота 2](https://github.com/Pavel24071993/Pavel24071993-HomeWork_8-01/blob/main/img/2.jpg)
![Название скриншота 3](https://github.com/Pavel24071993/Pavel24071993-HomeWork_8-01/blob/main/img/3.jpg)
![Название скриншота 4](https://github.com/Pavel24071993/Pavel24071993-HomeWork_8-01/blob/main/img/4.jpg)
После перенастройки репозитория под SSH ключ удалось выполнить коммит в origin репозиторий.

![Название скриншота 5](https://github.com/Pavel24071993/Pavel24071993-HomeWork_8-01/blob/main/img/5.jpg)

---

### Задание 2
1.	Создайте файл. gitignore (обратите внимание на точку в начале файла) и проверьте его статус сразу после создания.
2.	Добавьте файл. gitignore в следующий коммит git add....
3.	Напишите правила в этом файле, чтобы игнорировать любые файлы .pyc, а также все файлы в директории cache.
4.	Сделайте коммит и пуш.
В качестве ответа добавьте ссылку на этот коммит в ваш md-файл с решением.

### Решение:
![Название скриншота 6](https://github.com/Pavel24071993/Pavel24071993-HomeWork_8-01/blob/main/img/6.jpg)
![Название скриншота 7](https://github.com/Pavel24071993/Pavel24071993-HomeWork_8-01/blob/main/img/7.jpg)
![Название скриншота 16](https://github.com/Pavel24071993/Pavel24071993-HomeWork_8-01/blob/main/img/16.jpg)


---

### Задание 3
1.	Создайте новую ветку dev и переключитесь на неё.
2.	Создайте в ветке dev файл test.sh с произвольным содержимым.
3.	Сделайте несколько коммитов и пушей в ветку dev, имитируя активную работу над файлом в процессе разработки.
4.	Переключитесь на основную ветку.
5.	Добавьте файл main.sh в основной ветке с произвольным содержимым, сделайте комит и пуш . Так имитируется продолжение общекомандной разработки в основной ветке во время разработки отдельного функционала в dev ветке.
6.	Сделайте мердж dev ветки в основную с помощью git merge dev. Напишите осмысленное сообщение в появившееся окно комита.
7.	Сделайте пуш в основной ветке.
8.	Не удаляйте ветку dev.
В качестве ответа прикрепите ссылку на граф коммитов https://github.com/ваш-логин/ваш-репозиторий/network в ваш md-файл с решением.
Ваш граф комитов должен выглядеть аналогично скриншоту:

![Название скриншота 8](https://github.com/Pavel24071993/Pavel24071993-HomeWork_8-01/blob/main/img/8.jpg)
![Название скриншота 9](https://github.com/Pavel24071993/Pavel24071993-HomeWork_8-01/blob/main/img/9.jpg)
![Название скриншота 10](https://github.com/Pavel24071993/Pavel24071993-HomeWork_8-01/blob/main/img/10.jpg)
![Название скриншота 11](https://github.com/Pavel24071993/Pavel24071993-HomeWork_8-01/blob/main/img/11.jpg)
Прямая ссылка на мой граф - https://github.com/Pavel24071993/HomeWork_GIT/network
![Название скриншота 11](https://github.com/Pavel24071993/Pavel24071993-HomeWork_8-01/blob/main/img/12.jpg)

### Задание 4
Сэмулируем конфликт. Перед выполнением изучите документацию.
Что нужно сделать:
1.	Создайте ветку conflict и переключитесь на неё.
2.	Внесите изменения в файл test.sh.
3.	Сделайте коммит и пуш.
4.	Переключитесь на основную ветку.
5.	Измените ту же самую строчку в файле test.sh.
6.	Сделайте коммит и пуш.
7.	Сделайте мердж ветки conflict в основную ветку и решите конфликт так, чтобы в результате в файле оказался код из ветки conflict.
В качестве ответа на задание прикрепите ссылку на граф коммитов https://github.com/ваш-логин/ваш-репозиторий/network в ваш md-файл с решением.

### Решение:
Внёс изменения в первую строчку файла test.sh в ветке conflict.

![Название скриншота 13](https://github.com/Pavel24071993/Pavel24071993-HomeWork_8-01/blob/main/img/13.jpg)
Внёс изменения в первую строчку файла test.sh в ветке main.
![Название скриншота 14](https://github.com/Pavel24071993/Pavel24071993-HomeWork_8-01/blob/main/img/14.jpg)
Можно вручную отредактировать конфликтный файл, можно принять стороны из текущей ветки или из той, откуда мержим. Я сделал командой git checkout --theirs test.sh
![Название скриншота 15](https://github.com/Pavel24071993/Pavel24071993-HomeWork_8-01/blob/main/img/15.jpg)


