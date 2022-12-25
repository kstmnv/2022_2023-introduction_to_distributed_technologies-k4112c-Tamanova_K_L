University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)

Year: 2022/2023

Group: K4112c

Author: Tamanova K.L.

Lab: Lab1

Date of create: 11.12.2022

Date of finished: 22.12.22

### Цель рабоы:

Познакомиться с инструментами Minikube и Docker, развернуть свой первый "под".

### Ход работы:

1. Манифест для развертывания пода:

![image](https://user-images.githubusercontent.com/107037214/208406056-c6e3abf0-9fd0-44cd-851f-1c0c4d7784ff.png)

Установка драйверов для minikube:

![image](https://user-images.githubusercontent.com/107037214/208406144-148809b0-ca5a-40d3-ac37-b658d14fa1a5.png)

После запуска кластера minikube вы можете взаимодействовать с k8s, используя команду:

![image](https://user-images.githubusercontent.com/107037214/208406944-fdc46265-45c6-4b8a-86d4-77597c9c5d06.png)

Команда для создания «пода»:

![image](https://user-images.githubusercontent.com/107037214/208406999-e14f99d7-ef5f-4637-93dc-c6c5516a7a15.png)

2. Создание сервиса для доступа к контейнеру-хранилищу:

![image](https://user-images.githubusercontent.com/107037214/208407028-f74b4179-ef0e-4fd8-8ae0-b01bd41de82d.png)

Уведомление о сервисе тип NodePort. Он отвечает за доступ к официальным ресурсам извне (из других узлов).

3. Команда port-forward позволяет прослушать порт на localhost и перенаправит на порт 8200 в кластере:

![image](https://user-images.githubusercontent.com/107037214/208407060-3e4fd50a-7637-4bac-9624-3015ca6579ed.png)

![image](https://user-images.githubusercontent.com/107037214/208407241-0ad005b2-38f0-4193-88e9-52f2c22414a0.png)


4. Команда kubectl logs позволяет просматривать логии пода. Находим токен для входа:

![image](https://user-images.githubusercontent.com/107037214/208407311-c63d7707-ab1c-4d67-9480-3958f6f45069.png)

![image](https://user-images.githubusercontent.com/107037214/208407330-bc952976-6780-4aad-857e-02290da2d179.png)

![image](https://user-images.githubusercontent.com/107037214/208407424-e7fae846-dca5-4592-a200-884d3d3a540d.png)

5.	Схема организации сервисов и контейнеров:

![image](https://user-images.githubusercontent.com/107037214/208407445-cf472f09-ec4b-478c-90c1-454d799878ba.png)
