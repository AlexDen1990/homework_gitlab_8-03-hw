# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Александров Денис`

---

### Задание 1. Установите Zabbix Server с веб-интерфейсом

Согласно заданию выбрана последняя версия Zabbix 6.4

1. Установка базы данных PostgreSQL

sudo apt install postgresql


2. Установка и распаковка репозитория

wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_6.0+ubuntu24.04_all.deb

dpkg -i zabbix-release_latest_6.0+ubuntu24.04_all.deb

apt update
