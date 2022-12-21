University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)

Year: 2022/2023

Group: K4112c

Author: Tamanova K.L.

Lab: Lab3

Date of create: 21.12.2022

Date of finished: 

Цель работы:

Ознакомиться с типами "контроллеров" развертывания контейнеров, ознакомится с сетевыми сервисами и развернуть свое веб приложение.

Ход работы:

1. Создаем configmap:

![image](https://user-images.githubusercontent.com/107037214/208981612-01e4e6df-2f74-40dc-941b-9ece6ad6efd8.png)

Проверяем создался ли:

![image](https://user-images.githubusercontent.com/107037214/208982088-89817544-316f-4846-b321-ea549eb868ec.png)

2. Создаем replicaset:

![image](https://user-images.githubusercontent.com/107037214/208982248-6f18e049-8958-48b0-b722-13ac840b0acb.png)

![image](https://user-images.githubusercontent.com/107037214/208982538-ab3f8ff9-0a6d-46d4-afca-f8503ec8650e.png)

2. Включаем ingress

![image](https://user-images.githubusercontent.com/107037214/208982820-21a50037-5919-4ab3-94d7-db7268c1bbac.png)

Устанавливаем ingress controller:

![image](https://user-images.githubusercontent.com/107037214/208976726-a62190d1-723a-40cc-8b7b-cb42aef9a738.png)

После чего создаем файл сервиса и ингресса, вносим изменения в деплоймент и выполняем команду:

![image](https://user-images.githubusercontent.com/107037214/208976934-5b44005f-3847-4a67-a0c6-8f5b7b34ca75.png)

Вносим в hosts minikube ip и доменное имя, которое указано в ингрессе. Выполняем команду:

![image](https://user-images.githubusercontent.com/107037214/208977095-4c0d2c4f-3068-4e10-a970-f2ad1ff188c9.png)

Проверяем работоспособность нашего ингресса - вводим это доменное имя в браузер и видим, что все работает корректно - наш ингресс смог достучаться до сервиса, а сервис до пода:

