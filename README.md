# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Александров Денис`- Часть 1

---
Задание 1
Установите Zabbix Server с веб-интерфейсом.

Процесс выполнения
Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии.
Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.

# Задание 1. Установите Zabbix Server с веб-интерфейсом

### 1. Установка базы данных PostgreSQL

sudo apt install postgresql


### 2. Установка и распаковка репозитория

wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_6.0+ubuntu24.04_all.deb

dpkg -i zabbix-release_latest_6.0+ubuntu24.04_all.deb

apt update

### 3. Установка заббикс сервера, веб-интерфейса

apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts

### 4. Создание базы данных и пользователя

sudo -u postgres createuser --pwprompt zabbix

sudo -u postgres createdb -O zabbix zabbix


### 5. Импорт данных на сервер Zabbix

zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix


### 6. Настройка пароля бызы данных

nano /etc/zabbix/zabbix_server.conf далее в файле напротив DBPassword= указываем наш пароль


### 7. Рестарт и добавление в автозагрузку веб сервера и Заббикс сервера

systemctl restart zabbix-server apache2

systemctl enable zabbix-server apache2

### Скриншот авторизации в админке

![alt text](https://github.com/AlexDen1990/homework_gitlab_8-03-hw/blob/main/img/1.png)

---

# Задание 2. Установка Zabbix Agent на два хоста

В качестве одного из хостов использована машина с Zabbix серверо, в качестве второго - чистая ВМ.

### 1. Установка заббикс агента

sudo apt install zabbix-agent


### 2. Рестарт и добавление в автозагрузку Заббикс агента

systemctl restart zabbix-agent

systemctl enable zabbix-agent


### 3. Настраиваем параметры подключения заббикс агента к серверу (прописываем адреса хостов)

sed -i 's/Server=127.0.0.1/Server=192.168.1.43/g' /etc/zabbix/zabbix_agent.conf

sed -i 's/Server=127.0.0.1/Server=192.168.1.46/g' /etc/zabbix/zabbix_agent.con

### Скриншот раздела Узлы сети
![alt text](https://github.com/AlexDen1990/homework_gitlab_8-03-hw/blob/main/img/2.png)

### Cкриншот лога zabbix agent, где видно, что он работает с сервером
![alt text](https://github.com/AlexDen1990/homework_gitlab_8-03-hw/blob/main/img/4.png)

### Cкриншот раздела Мониторинг > последние данные для обоих VM машин, где видны поступающие от агентов данные.
![alt text](https://github.com/AlexDen1990/homework_gitlab_8-03-hw/blob/main/img/3.png)

