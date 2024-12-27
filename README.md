Домашнее задание к занятию «Система мониторинга Zabbix. Часть 2»

Автор: Козлов Виктор


Цели задания

- Научиться создавать свои шаблоны в Zabbix, добавлять в Zabbix хосты и связывать шаблон с хостами 
- Научиться составлять кастомный дашборд 
- Научиться создавать UserParameter на Bash 
- Научиться работать с конфигурационными файлами Zabbix Server и Zabbix Agent

Выполненные задания

Задание 1: Создание шаблона 

Создан шаблон, который мониторит: 
1. Загрузку CPU в процентах 
2. Загрузку RAM в процентах 

Шаги выполнения: 
1. Перешёл в раздел Configuration > Templates
2. Нажал кнопку Create Template
3. В поле Template Name указал название, например, kozlovvs-template 
4. Перешёл в шаблон и создал два элемента данных:
   - CPU Usage: 
     - Type — Zabbix Agent
     - Key — system.cpu.util[,idle] 
     - Units — % 
   - RAM Usage:
     - Type — Zabbix Agent 
     - Key — vm.memory.size[available]
     - Units — % 

Скриншот страницы шаблона с элементами данных прикреплён
[Ссылка на Google Документ](https://docs.google.com/document/d/1EjNJ9oVAsf3TaX2Hat6xSGERnY325IyavkKLzHAMn8g/edit?usp=sharing)


Задания 2 и 3: Добавление хостов и привязка шаблонов

Шаги выполнения:
1. Установил Zabbix Agent на две виртуальные машины
2. Добавил Zabbix Server в список разрешённых серверов агентов 
3. Добавил хосты в разделе Configuration > Hosts
4. Привязал к хостам созданный шаблон и шаблон Linux by Zabbix Agent

Используемые команды:
sudo apt update
sudo apt install zabbix-agent

sudo nano /etc/zabbix/zabbix_agentd.conf

В файле zabbix_agentd.conf внёс следующие изменения:
Server=51.250.67.52
ServerActive=51.250.67.52
Hostname=kozlovvs-1 # Для первой машины 

Перезапуск сервиса:
sudo systemctl restart zabbix-agent
sudo systemctl enable zabbix-agent


Задание 4: Создание кастомного дашборда

Шаги выполнения:
1. Перешёл в раздел Monitoring > Dashboards
2. Нажал кнопку Create Dashboard
3. Указал название дашборда, kozlovvs-dashboard
4. Добавил виджеты с графиками для CPU и RAM



Используемые IP и конфигурационные файлы

Основной сервер Zabbix
- Внешний IP: 51.250.67.52
- Внутренний IP: 10.128.0.29
- Конфигурация Zabbix Server (файл /etc/zabbix/zabbix_server.conf):
DBHost=localhost 
DBName=zabbix
DBUser=zabbix
DBPassword=123456789 
ListenPort=10051 
LogFile=/var/log/zabbix/zabbix_server.log

Вторая машина Zabbix Agent
- Внешний IP: 89.169.157.35
- Внутренний IP: 10.128.0.30
- Конфигурация Zabbix Agent (файл /etc/zabbix/zabbix_agentd.conf):
Server=51.250.67.52
ServerActive=51.250.67.52
Hostname=kozlovvs-1
LogFile=/var/log/zabbix/zabbix_agentd.log
Include=/etc/zabbix/zabbix_agentd.d/*.conf


Проблемы с выполнением

В процессе выполнения задания сервер Zabbix перестал работать из-за неизвестной причины. Это произошло после добавления второго хоста и тестирования связей. Из-за этого я не успел завершить настройку и создание всех скриншотов.  
Единственные скриншоты, которые удалось сделать:
1. Страница созданного шаблона (задание 1).
2. Добавленные элементы данных для CPU и RAM.
Физически я не успею заново поднять сервер и настроить его для выполнения всех заданий с нуля, но предоставляю подробное описание всех выполненных шагов и конфигураций, которые использовались.
