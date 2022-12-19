University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru) 

Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)

Year: 2022/2023

Group: K4112c

Author: Tamanova K.L.

Lab: Lab1

Date of create: 11.12.2022

Date of finished: 

**Цель работы**

Ознакомиться с инструментами Minikube и Docker, развернуть свой первый "под".

**Ход работы**

1. Манифест для развертывания пода.
![image](https://user-images.githubusercontent.com/107037214/208396405-eb161ae4-6db6-44fb-811d-27bbeb2b04a5.png)

Установка драйверов для minikube:
![image](https://user-images.githubusercontent.com/107037214/208396471-1fff5616-0987-4a3c-b25a-cd712a66250a.png)

После запуска кластера minikube вы можете взаимодействовать с k8s, используя команду:
![image](https://user-images.githubusercontent.com/107037214/208396501-bf33d8ce-74f3-4a28-903d-32cc80ba128c.png)

Команда для создания «пода»:
![image](https://user-images.githubusercontent.com/107037214/208396512-d029d475-2830-4c30-b1f2-559c8011ea6e.png)

1. Создание сервиса для доступа к контейнеру-хранилищу
![image](https://user-images.githubusercontent.com/107037214/208396549-64e2afcc-c471-4ca2-8115-afcd910cad94.png)

Уведомление о сервисе тип NodePort. Он отвечает за доступ к источникам извне (из других узлов).

1. Команда port-forward позволяет прослушать порт на localhost и перенаправит на порт 8200 в кластере:
![image](https://user-images.githubusercontent.com/107037214/208396582-7f46ee1c-c647-4a95-b13f-fd7c09474317.png)
![image](https://user-images.githubusercontent.com/107037214/208396614-e1a8df5a-e0a5-4a17-9ef4-126b321c0fe2.png)


1. Команда kubectl logs позволяет просматривать логии пода. Находим токен для входа:
![image](https://user-images.githubusercontent.com/107037214/208396671-6f1c2c4c-c953-4c8a-a724-7a2ef2ffba09.png)
![image](https://user-images.githubusercontent.com/107037214/208396701-6554c6b4-8214-4575-b3f5-8a1668856324.png)
![image](https://user-images.githubusercontent.com/107037214/208396716-145289d0-2f35-4ebe-a752-9ad32c1d95ca.png)

 

1. Схема организации сервисов и контейнеров:

![image](https://user-images.githubusercontent.com/107037214/208396734-634f6551-456c-4fb5-987f-25cbdfdc04cb.png)
