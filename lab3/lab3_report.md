University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)

Year: 2022/2023

Group: K4112c

Author: Tamanova K.L.

Lab: Lab3

Date of create: 21.12.2022

Date of finished: 

### Цель работы:

Ознакомиться с типами "контроллеров" развертывания контейнеров, ознакомится с сетевыми сервисами и развернуть свое веб приложение.

### Ход работы:

1. Namespace

Создадим namespace lab3ns

![image](https://user-images.githubusercontent.com/107037214/208969730-5c32e145-dd0d-48d0-a3e2-9f3a6ff3fa41.png)

1.1 Проверим создался ли namespace:

![image](https://user-images.githubusercontent.com/107037214/208969509-a119d668-53ad-49af-b2e4-77ebe0c62f7d.png)

1.2 configMap - — это объект API, используемый для хранения неконфиденциальных данных в парах ключ-значение.
Использование ConfigMaps позволяет разделять конфигурационные файлы и контейнеры с приложениями.

```kubectl apply -f lab3-configmap.yaml```

![image](https://user-images.githubusercontent.com/107037214/208969954-a460abc9-0078-4753-9f4c-92f7671416b3.png)

Проверим создался ли configmap:

![image](https://user-images.githubusercontent.com/107037214/208972228-22ddf895-9a06-4f9a-aae0-2e4d67d972ce.png)

1.3 replicaset — его цель, поддерживать стабильный набор подов реплик, работающих в любой момент времени. Таким образом, он часто используется, чтобы гарантировать доступность определенного количества идентичных подов.

```kubectl apply -f lab3-replicaset.yaml```

![image](https://user-images.githubusercontent.com/107037214/208970092-29a53cc2-2c7d-4193-bfae-fd47aacff2d6.png)

Посмотрим список запущенных подов:

![image](https://user-images.githubusercontent.com/107037214/208972749-3808a208-8c06-45ed-877f-557920fca7dd.png)

1.4 service — в Kubernetes это абстракция, определяющая политику доступа к подам.

```kubectl apply -f lab3-service.yaml```

![image](https://user-images.githubusercontent.com/107037214/208970132-0c928d68-e0dd-4d03-8ec3-b8b513b30629.png)

Посмотрим список сервисов:

![image](https://user-images.githubusercontent.com/107037214/208972978-08ccc5e0-4ef6-4b4f-80f9-1dbd548eea87.png)

Устанавливаем ingress controller:

![image](https://user-images.githubusercontent.com/107037214/208973992-ea297d56-f1a0-4f9a-a5da-b3a6caa3b6bf.png)

Вносим в hosts minikube ip и доменное имя, которое указано в ингрессе. Выполняем команду:

![image](https://user-images.githubusercontent.com/107037214/208974135-c1718a17-9e74-4776-9e35-c720275744b1.png)

2. TLS сертификат

2.1 Генерируем TLS сертификат с помощью команды:

```openssl req -x509 -newkey rsa:4096 -sha256 -nodes -keyout tls.key -out tls.crt -subj "/CN=kstamanovalab3.com" -days 365```

![image](https://user-images.githubusercontent.com/107037214/208959628-61a78802-bafa-4ce6-9901-872805df7a09.png)

req — это генерация запросов на подпись сертификата, но если мы задаём ключ «-x509», это означает, что мы генерируем самоподписанный сертификат.

Опция -newkey указывает, что нужно создать новую пару ключей, а в параметрах мы сообщаем тип rsa и сложность 4096 байт.

Опция -nodes указывает, что шифровать ключ не нужно.

-keyout - указываем в какой файл положим ключ.

Опция -out указывает на имя файла для сохранения сертификата.

-subj — этот тот кому принадлежит сертификат.

С помощью параметра -days мы указываем что сертификат будет действительным в течение 365 дней

2.2 Создание Secret с помощью команды:

```kubectl create secret tls lab3-tls --cert=tls.crt --key=tls.key```

3. Создание ingress

Ingress — это объект API, который определяет правила, разрешающие внешний доступ к сервисам в кластере.

3.1 Используем команду

```kubectl apply -f lab3-ingress.yaml```

![image](https://user-images.githubusercontent.com/107037214/208959193-351adc2a-0b21-4e27-aff4-b33efb313aa1.png)

3.2 Конфигурирем minikube для работы с ingress:

```minikube addons enable ingress```

![image](https://user-images.githubusercontent.com/107037214/208960644-0042338f-c9d3-48ce-a008-72d72752434b.png)

```minikube addons enable ingress-dns```

![image](https://user-images.githubusercontent.com/107037214/208960794-6936599a-2175-4f55-9bd8-fd7af683a5a5.png)

3.3 Добавляем IP адрес ingress и FQDN (127.0.0.1 kstamanovalab3.com) в дирикторию C:\Windows\System32\drivers\etc

3.4 Для доступа к ingress используем команду:

```minikube tunnel```

4. Результат

![image](https://user-images.githubusercontent.com/107037214/209481893-266e329d-ecd0-46fb-b590-ad070583c998.png)

Проверка сертификата:

![image](https://user-images.githubusercontent.com/107037214/209481638-8be2bf4b-55bf-45f4-8546-fbd83f0d7d58.png)

5. Схема организации сервисов и контейнеров:

![image](https://user-images.githubusercontent.com/107037214/209482013-b11b8052-7158-44f2-97dc-e509f2df227d.png)
