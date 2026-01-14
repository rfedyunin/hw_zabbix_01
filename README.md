# Домашнее задание к занятию «Система мониторинга Zabbix» - `Федюнин Руслан`


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

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

Установите Zabbix Server с веб-интерфейсом.

Процесс выполнения

1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.

2. Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.

3. Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.

4. Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.

Требования к результатам

1. Прикрепите в файл README.md скриншот авторизации в админке.

![Screenshot_1.jpg](https://github.com/rfedyunin/hw_zabbix_01/blob/main/Screenshot_1.jpg)

2. Приложите в файл README.md текст использованных команд в GitHub.

```
`sudo apt install postgresql`

`wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_6.0+ubuntu24.04_all.deb`

`dpkg -i zabbix-release_latest_6.0+ubuntu24.04_all.deb`

`apt update`

`apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts`

`su - postgres -c 'psql --command "CREATE USER zabbix WITH PASSWORD '\'123456789\'';"'`

`su - postgres -c 'psql --command "CREATE DATABASE zabbix OWNER zabbix;"`

`zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix`

`sed -i 's/# DBPassword=/DBPassword=123456789/g' /etc/zabbix/zabbix_server.conf`

`sudo systemctl restart zabbix-server apache2`

`sudo systemctl enable zabbix-server apache2`
```

---


### Задание 2

Установите Zabbix Agent на два хоста.

Процесс выполнения

1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.

2. Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.

3. Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.

4. Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.

5. Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.


Требования к результатам
1. Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу

![Screenshot_2.jpg](https://github.com/rfedyunin/hw_zabbix_01/blob/main/Screenshot_2.jpg)

2. Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером

![Screenshot_4.jpg](https://github.com/rfedyunin/hw_zabbix_01/blob/main/Screenshot_4.jpg)

3. Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.

![Screenshot_3.jpg](https://github.com/rfedyunin/hw_zabbix_01/blob/main/Screenshot_3.jpg)

4. Приложите в файл README.md текст использованных команд в GitHub
```
sudo apt install zabbix-agent
systemctl restart zabbix-agent
systemctl enable zabbix-agent
sed -i 's/Server=127.0.0.1/Server=192.168.31.10'/g' /etc/zabbix/zabbix_agentd.conf

```
---
