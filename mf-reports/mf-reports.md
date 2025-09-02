# Megafon reports 
### Данный проект представляет из себя веб-сервер для форматирования отчётов, фильтрации подключений по ФИО и сотруднику. 

Оформление отчётов по следующим критериям:<br/>
  -оператор<br/>
 -тип подключения<br/>
 -даты подключения<br/>
 -ФИО сотрудника

## Инструкция по локальному развёртыванию:
1. Скачать docker 
2. Склонировать образ из Docker Hub
3. Запустить через docker run -p [локальный порт]:[80] reponame. Стандартный внутренний порт приёма соединений 80. 

## Создание systemd сервиса с развёртыванием после перезагрузки машины
```[Unit]
Description=Reestr Web Application
After=docker.service
Requires=docker.service
StartLimitIntervalSec=60
StartLimitBurst=5

[Service]
Type=simple
Restart=on-failure
RestartSec=5s
ExecStartPre=-/usr/bin/docker rm -f reestr
ExecStart=/usr/bin/docker run --rm --name reestr -p 80:80 report:latest
ExecStop=/usr/bin/docker stop -t 10 reestr
TimeoutStopSec=20
TimeoutStartSec=30
User=root
Group=docker
Environment="DOCKER_HOST=unix:///var/run/docker.sock"

[Install]
WantedBy=multi-user.target
```
В данном systemd сервисе учитывается особенность некоторых версий Docker, которая заключается в том, что после перезагрузки контейнер остаётся в системе, но не запускается. Старый контейнер удаляется и запускается новый, с флагом --rm. 


## Инструкция по использованию:
1. Скопировать отчёт подключений от нужного оператора.
2. Вставить отчёт каждого оператора в соответствующее поле. 
3. Выбрать нужные даты отчёта.
4. Выбрать тип подключения (новое подключение / перенос номера).
5. Выбрать ФИО сотрудников. 
6. Скачать полученный отчёт, сформированный по выбранным параметрам.

## Стек технологий: 
*Docker, Golang* 


