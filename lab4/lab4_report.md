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


- Посмоотрим доступные пулы ip
```bash
kubectl get ippools -o wide
NAME                  AGE
default-ipv4-ippool   4m17s
```

- Удалим дефолтный пулл 
```bash
kubectl exec -i -n kube-system calicoctl -- /calicoctl delete ippools default-ipv4-ippool
NAME                  AGE
Successfully deleted 1 'IPPool' resource(s)
```

- Добавим labelы
```bash
kubectl label nodes minikube zone=parnas
NAME                  AGE
node/minikube labeled
```

```bash
kubectl label nodes minikube-m02 zone=kupchino
NAME                  AGE
node/minikube labeled
```
- Добавим labelы свои пулы из манифеста:
```yaml
apiVersion: crd.projectcalico.org/v1
kind: IPPool
metadata:
  name: zone-parnas-ippool
spec:
  cidr: 10.244.102.1/24
  ipipMode: Always
  natOutgoing: true
  nodeSelector: zone == "parnas"

---
apiVersion: crd.projectcalico.org/v1
kind: IPPool
metadata:
  name: zone-kupchino-ippool 
spec:
  cidr: 10.244.102.2/24
  ipipMode: Always
  natOutgoing: true
  nodeSelector: zone == "kupchino"
  
```
```bash
kubectl apply -f ippool.yaml
NAME                  AGE
ippool.crd.projectcalico.org/zone-parnas-ippool unchanged
ippool.crd.projectcalico.org/zone-kupchino-ippool created
```


```bash
kubectl get ippools -o wide
NAME                   AGE
zone-kupchino-ippool   3s
zone-parnas-ippool     53s
```



### 2. ConfigMap, ReplicaSet
- Создадим configMap
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: env-cm
data:
    ReactAppUserName: Ivan
    ReactAppCompanyName: ITMO
```
```bash
kubectl apply -f env-configmap.yaml
configmap/env-cm created
```
- Создадим ReplicaSet

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: lab4app-replicaset
  labels:
    app: lab4app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: lab4app
  template:
    metadata:
      labels:
        app: lab4app
    spec:        
      containers:
      - name: lab4-container
        image: ifilyaninitmo/itdt-contained-frontend:master
        ports:
        - containerPort: 3000
        env:
        - name: REACT_APP_USERNAME
          valueFrom:
            configMapKeyRef:
              name: env-cm
              key: ReactAppUserName
        - name: REACT_APP_COMPANY_NAME
          valueFrom:
            configMapKeyRef:
              name: env-cm
              key: ReactAppCompanyName
```
```bash
kubectl apply -f lab4app-ReplicaSet.yaml
replicaset.apps/lab4app-replicaset created
```




### 3. Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: lab4-service
spec:
  selector:
    app: lab4app
  type: LoadBalancer
  ports:
    - protocol: TCP
      port: 9090
      targetPort: 3000
      
```

```bash
kubectl apply -f service.yaml
service/lab4-service created
```


```bash
minikube service lab4-service
|-----------|--------------|-------------|---------------------------|
| NAMESPACE |     NAME     | TARGET PORT |            URL            |
|-----------|--------------|-------------|---------------------------|
| default   | lab4-service |        9090 | http://192.168.49.2:31407 |
|-----------|--------------|-------------|---------------------------|
```

```bash
kubectl exec -i -n kube-system calicoctl -- /calicoctl get ippool -o wide
NAME                   CIDR              NAT    IPIPMODE   VXLANMODE   DISABLED   SELECTOR             
zone-kupchino-ippool   10.244.102.2/24   true   Always     Never       false      zone == "kupchino"   
zone-parnas-ippool     10.244.102.1/24   true   Always     Never       false      zone == "parnas"   

```

### 4. Запуск

- Запустим работу сервиса 
```bash
minikube service lab4-service
```
>![0](img/2.jpg)
>![0](img/3.jpg)


Пропингуем командой:
```bash
kubectl exec -ti lab4app-replicaset-c54sf -- sh / lab4app # ping 10.244.102.71 -c 5
```
