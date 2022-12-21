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

1. Создаем деплоймент и конфигмап

![image](https://user-images.githubusercontent.com/107037214/208976059-d3952c73-e873-4d06-b916-76e53fac77a3.png)

2. Включаем ingress

![image](https://user-images.githubusercontent.com/107037214/208976595-d2e60e9b-4390-4c6b-a0da-f5930a536505.png)

Устанавливаем ingress controller:

![image](https://user-images.githubusercontent.com/107037214/208976726-a62190d1-723a-40cc-8b7b-cb42aef9a738.png)

После чего создаем файл сервиса и ингресса, вносим изменения в деплоймент и выполняем команду:

![image](https://user-images.githubusercontent.com/107037214/208976934-5b44005f-3847-4a67-a0c6-8f5b7b34ca75.png)

Вносим в hosts minikube ip и доменное имя, которое указано в ингрессе. Выполняем команду:

![image](https://user-images.githubusercontent.com/107037214/208977095-4c0d2c4f-3068-4e10-a970-f2ad1ff188c9.png)

Проверяем работоспособность нашего ингресса - вводим это доменное имя в браузер и видим, что все работает корректно - наш ингресс смог достучаться до сервиса, а сервис до пода:

