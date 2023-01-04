University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)  
Year: 2022/2023  
Group: K4112c  
Author: Tamanova KL
Lab: Lab4
Date of create: 21.12.2022  
Date of finished: 29.12.2022

---

### Цель работы

Познакомиться с CNI Calico и функцией `IPAM Plugin`, изучить особенности работы CNI и CoreDNS.

## Ход работы
1. Устанавливаем плагин Calico.
- Прежде чем запустить minikube, устанавливаем плагин `CNI=Calico`, команда `minikube start --network-plugin=cni --cni=calico` позволяет создать single-node minikube cluster, нам же нужно 2 ноды. Чтобы их создать, потребуется команда `minikube start --nodes 2 -p multinode-demo`. Теперь объединяем их в одно

- `minikube start --driver=docker -p multinode-cluster --network-plugin=cni --cni=calico --nodes 2 --kubernetes-version=v1.24.0`

- Проверяем количество созданных нод при помощи команды `kubectl get nodes`:

![image](https://user-images.githubusercontent.com/107037214/209953679-a966fc93-0947-430f-97c1-dc5633a48564.png)

- Проверяем работу CNI плагина Calico:

- При помощи команды `kubectl get po -n kube-system -l=k8s-app=calico-node` видим созданные поды с меткой calico-node

![image](https://user-images.githubusercontent.com/107037214/209953820-6d1825f9-87cf-4829-803e-4a6df2acc119.png)
 
- Для проверки работы Calico мы попробуем одну из функций под названием `IPAM Plugin`

- Для проверки режима IPAM для запущеных ранее нод указываем label по признаку стойки или географического расположения
 
 Соглавно инструкции по назначению ip-адресов удаляем дефолтный ippool, убеждаемся что никаких айпи-пулов не осталось:
 
![image](https://user-images.githubusercontent.com/107037214/209961700-09ad34ee-a79c-4bbc-80e0-66f5bd78659c.png)

- Для ранее запущенных нод добавляем labels:

![image](https://user-images.githubusercontent.com/107037214/209959201-12528243-bbde-4fbd-a0e4-c07a9c28365c.png)

- Создаём IP pool для каждого rack

- Чтобы взаимодействовать с calico, скачиваем его yaml образ с офф.сайта и деплоим в миникуб

![image](https://user-images.githubusercontent.com/107037214/209959885-0091fb3f-7fad-4353-bf76-f1a15d4e9d46.png)

- Далее, взаимодействуя с calico через неймспейс kube-system, разворачиваем ip pools

``` kubectl exec -i -n kube-system calicoctl -- /calicoctl get ippools -o wide```

![image](https://user-images.githubusercontent.com/107037214/209960573-35a185bb-058d-4091-adc5-912d72c43657.png)

# Проверка назначения IP адресов

Для проверки развернём тестоый деплоймент с двумя репликами, как в предыдущих лабах

- Создаём деплой, и сервис, при помощи команды `kubectl get po -o wide` проверяем поды

![image](https://user-images.githubusercontent.com/107037214/209961312-68d9596e-57e9-4b63-8954-2721045f9425.png)

Видим проблему, образ не ложится на контейнеры, и эта проблема не решается пуллом образа в докер, до того как задеплоить манифест с calico они были в состоянии `ready`.

- Чтобы понять, в чём ошибка, делаем `kubectl describe pod lab4-7cc54fb749-krdf6`

![image](https://user-images.githubusercontent.com/107037214/209961403-42abff08-4649-4b81-9333-0bddfe14515a.png)


Из описания становится понятно, что сервис kubelet по какой-то причине отказывается надевать на контейнер наш образ.

- Схема организации контейнеров

![image](https://user-images.githubusercontent.com/107037214/209482965-4612f4ac-2d20-49ea-9201-e393d5cc1d20.png)
