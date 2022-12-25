University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)

Year: 2022/2023

Group: K4112c

Author: Tamanova K.L.

Lab: Lab2

Date of create: 20.12.2022

Date of finished: 22.12.22

### Цель работы:

Ознакомиться с типами "контроллеров" развертывания контейнеров, ознакомится с сетевыми сервисами и развернуть свое веб приложение.

### Ход работы:

1. Создание deployment и сервиса:

С помощью команды kubectl apply -f lab2.yaml :

![image](https://user-images.githubusercontent.com/107037214/208655545-cc5a2456-0600-4013-9115-e1aa222508d3.png)

Проверка создания подов:

![image](https://user-images.githubusercontent.com/107037214/208656365-83871ec2-2c1a-4403-b44c-168bc3371bb0.png)

Проверка создания сервиса:

![image](https://user-images.githubusercontent.com/107037214/208656546-77d857c1-d133-4eed-bdbb-7a96635ddb37.png)

2. Подключение через веб-браузер

Для работы выбран тип сервиса LoadBalancer. Подключение к контейнеру происходит через сервис, данную процедуру можно выполнить с помощью следующей команды:

![image](https://user-images.githubusercontent.com/107037214/208656867-2ea157da-8fd7-4d66-a7af-cff22fbcb747.png)

3. Проверка на странице в веб браузере переменных:

![image](https://user-images.githubusercontent.com/107037214/208663664-d6240953-5f1c-4427-89a3-51fecbd8b78b.png)
![image](https://user-images.githubusercontent.com/107037214/208661081-df026c59-93d0-4272-87e4-b29d0750b27f.png)

Поля REACT_APP_USERNAME и REACT_APP_COMPANY_NAME остаются не изменными, поскольку это константы, указанные в манифесте, а поле Container name изменяется, т.к. сервис распределяет запросы между двумя репликами.

4. Логи контейнеров:

![image](https://user-images.githubusercontent.com/107037214/208660079-2a440598-8be8-4661-b876-da1d2c8da463.png)

5. Схема организации сервисов и контейнеров:

![image](https://user-images.githubusercontent.com/107037214/208664994-fd0f126c-a8dc-4af3-aca4-69e18231e04b.png)
