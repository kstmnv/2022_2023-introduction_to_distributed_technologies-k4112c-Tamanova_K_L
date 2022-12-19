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

Установка драйверов для minikube:

После запуска minikube cluster вы сможете взаимодействовать с k8s, используя команду:

Команда для создания «пода»:

1. Создание сервиса для доступа к контейнеру-хранилищу

Уведомление о сервисе тип NodePort. Он отвечает за доступ к официальным ресурсам извне (из других узлов).

1. Команда port-forward позволяет прослушать порт на localhost и перенаправит на порт 8200 в кластере:


1. Команда kubectl logs позволяет просматривать логии пода. Находим токен для входа:



1. Схема организации сервисов и контейнеров:

