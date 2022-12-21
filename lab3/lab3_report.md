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

1. Создание компонентов

1.1 configMap - — это объект API, используемый для хранения неконфиденциальных данных в парах ключ-значение.
Использование ConfigMaps позволяет разделять конфигурационные файлы и контейнеры с приложениями.

kubectl apply -f lab3-configmap.yaml

![image](https://user-images.githubusercontent.com/107037214/208952404-46e538da-b364-4425-aa6e-657fec34d873.png)

1.2 replicaset — его цель, поддерживать стабильный набор подов реплик, работающих в любой момент времени. Таким образом, он часто используется, чтобы гарантировать доступность определенного количества идентичных подов.

kubectl apply -f lab3-replicaset.yaml

![image](https://user-images.githubusercontent.com/107037214/208953285-4bce39f6-706d-43f7-9127-f4d529491971.png)

1.3 service — в Kubernetes это абстракция, определяющая политику доступа к подам.

kubectl apply -f lab3-service.yaml

![image](https://user-images.githubusercontent.com/107037214/208953474-9fb28b79-7a1b-4454-bf0e-22a5d317814e.png)

2. TLS сертификат

2.1 Генерируем TLS сертификат с помощью команды:

openssl req -x509 -newkey rsa:4096 -sha256 -nodes -keyout tls.key -out tls.crt -subj "/CN=kstamanovalab3.com" -days 365

![image](https://user-images.githubusercontent.com/107037214/208959628-61a78802-bafa-4ce6-9901-872805df7a09.png)

req — это генерация запросов на подпись сертификата, но если мы задаём ключ «-x509», это означает, что мы генерируем самоподписанный сертификат.

Опция -newkey указывает, что нужно создать новую пару ключей, а в параметрах мы сообщаем тип rsa и сложность 4096 байт.

Опция -nodes указывает, что шифровать ключ не нужно.

-keyout - указываем в какой файл положим ключ.

Опция -out указывает на имя файла для сохранения сертификата.

-subj — этот тот кому принадлежит сертификат.

С помощью параметра -days мы указываем что сертификат будет действительным в течение 365 дней

2.2 Создание Secret с помощью команды:

kubectl create secret tls lab3-tls --cert=tls.crt --key=tls.key

3. Создание ingress

Ingress — это объект API, который определяет правила, разрешающие внешний доступ к сервисам в кластере.

3.1 Используем команду

kubectl apply -f lab3-ingress.yaml

![image](https://user-images.githubusercontent.com/107037214/208959193-351adc2a-0b21-4e27-aff4-b33efb313aa1.png)

3.2 Конфигурирем minikube для работы с ingress:

minikube addons enable ingress

![image](https://user-images.githubusercontent.com/107037214/208960644-0042338f-c9d3-48ce-a008-72d72752434b.png)

minikube addons enable ingress-dns


3.3 Добавляем IP адрес ingress и FQDN (127.0.0.1 kstamanovalab3.com) в дирикторию C:\Windows\System32\drivers\etc

3.4 Для доступа к ingress используем команду:

minikube tunnel

4. Результат

картинка

Проверка сертификата:

картинка

5. Схема организации сервисов и контейнеров:

картинка
