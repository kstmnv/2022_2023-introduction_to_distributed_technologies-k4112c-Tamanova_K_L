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

### 0. Запуск 

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


