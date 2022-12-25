University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)  
Year: 2022/2023  
Group: K4112c  
Author: Tamanova KL
Lab: Lab4
Date of create: 21.12.2022  
Date of finished: 

---

### Цель работы

Познакомиться с CNI Calico и функцией `IPAM Plugin`, изучить особенности работы CNI и CoreDNS.



- Разворачиваем minikube cluster

```bash
minikube start --nodes 2 --cni calico --kubernetes-version=v1.25.2
```

- Запустим dashboard командой:
```bash
minikube dashboard
```

- Проверим ноды
 
![image](https://user-images.githubusercontent.com/107037214/208992865-17aa51bd-a02f-48f9-9549-4e92dc5249e2.png)

- Проверим поды:

![image](https://user-images.githubusercontent.com/107037214/208992770-d8cb962e-ff4c-4acf-9734-7c47bbbb4848.png)

### 1. Сalicoctl

-  Из документации добавим файл calicoctl.yaml

![image](https://user-images.githubusercontent.com/107037214/208992947-3d26f3e6-db74-4220-92da-bcae4996a137.png)

 
## Ход работы
1. Устанавливаем плагин Calico.
- Прежде чем запустить minikube, устанавливаем плагин `CNI=Calico`, команда `minikube start --network-plugin=cni --cni=calico` позволяет создать single-node minikube cluster, нам же нужно 2 ноды. Чтобы их создать, потребуется команда `minikube start --nodes 2 -p multinode-demo`. Теперь объединяем их в одно

- `minikube start --driver=docker -p multinode-cluster --network-plugin=cni --cni=calico --nodes 2 --kubernetes-version=v1.24.0`

- Проверяем количество созданных нод при помощи команды `kubectl get nodes`:

скрин

- Проверяем работу CNI плагина Calico:

- При помощи команды `kubectl get po -n kube-system -l=k8s-app=calico-node` видим созданные поды с меткой calico-node
- 
скрин
 
- Для проверки работы Calico мы попробуем одну из функций под названием `IPAM Plugin`

- Для проверки режима IPAM для запущеных ранее нод указываем label по признаку стойки или географического расположения
 
 Соглавно инструкции по назначению ip-адресов удаляем дефолтный ippool, убеждаемся что никаких айпи-пулов не осталось:
 
скрин

- Для ранее запущенных нод добавляем labels:

скрин

- Создаём IP pool для каждого rack

скрин

- Чтобы взаимодействовать с calico, скачиваем его yaml образ с офф.сайта и деплоим в миникуб

скрин

- Далее, взаимодействуя с calico через неймспейс kube-system, разворачиваем ip pools

скрин

# Проверка назначения IP адресов

Для проверки развернём тестоый деплоймент с двумя репликами, как в предыдущих лабах

- Создаём деплой, и сервис, при помощи команды `kubectl get po -o wide` проверяем поды

скрин

Видим проблему, образ не ложится на контейнеры, и эта проблема не решается пуллом образа в докер, до того как задеплоить манифест с calico они были в состоянии `ready`.

- Чтобы понять, в чём ошибка, делаем `kubectl describe pod lab4-7cc54fb749-krdf6`

скрин

Из описания становится понятно, что сервис kubelet по какой-то причине отказывается надевать на контейнер наш образ.

- Схема организации контейнеров

![image](https://user-images.githubusercontent.com/107037214/209482965-4612f4ac-2d20-49ea-9201-e393d5cc1d20.png)
