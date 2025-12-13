# Задание 1

---
 - ***Создать Deployment приложения, состоящего из двух контейнеров (nginx и multitool), с количеством реплик 3 шт..***
```
cat deployment-web.yaml
```

<img width="657" height="595" alt="Снимок экрана от 2025-12-12 13-20-53" src="https://github.com/user-attachments/assets/7cd6d908-079b-43c6-a13b-fbf3c610fb4f" />


```
kubectl apply -f deployment-web.yaml
```

```
kubectl get deployments.apps web -o wide
```

<img width="1241" height="94" alt="Снимок экрана от 2025-12-12 13-23-05" src="https://github.com/user-attachments/assets/241567b3-3655-4a66-a9b3-fee58a30e0f0" />


- ***Создать Service, который обеспечит доступ внутри кластера до контейнеров приложения из п.1 по порту 9001 — nginx 80, по 9002 — multitool 8080.****

```
cat service-web.yaml 
```

<img width="593" height="340" alt="Снимок экрана от 2025-12-12 14-56-26" src="https://github.com/user-attachments/assets/a92d0696-cf45-4cc5-874f-a3fd6bd4dd95" />

- ***Создать отдельный Pod с приложением multitool и убедиться с помощью `curl`, что из пода есть доступ до приложения из п.1 по разным портам в разные контейнеры.***

```
cat pod-multitool.yaml 
```
<img width="541" height="242" alt="Снимок экрана от 2025-12-12 14-59-06" src="https://github.com/user-attachments/assets/65bfffcb-367b-40fc-ab37-51872b04c3b4" />

```
kubectl apply -f pod-multitool.yaml 
```

```
kubectl get pods
```

<img width="487" height="136" alt="Снимок экрана от 2025-12-12 15-00-13" src="https://github.com/user-attachments/assets/ae7dfdb9-9e6b-4829-ba66-b90ed36e99bf" />


- ***Продемонстрировать доступ с помощью `curl` по доменному имени сервиса.***

```
kubectl exec -it pod-multitool -- bash
```

```
curl http://service-web:9001
```

```
curl http://service-web:9002
```
<img width="759" height="482" alt="Снимок экрана от 2025-12-12 15-02-43" src="https://github.com/user-attachments/assets/0cf3e609-a9db-4fe5-a484-ae0a2040d92e" />


# Задание 2
____
- ***Создать отдельный Service приложения из Задания 1 с возможностью доступа снаружи кластера к nginx, используя тип NodePort.***
```
cat service-web-NodePort.yml
```

![[Снимок экрана от 2025-12-12 15-58-34.png]]

- ***Продемонстрировать доступ с помощью браузера или `curl` с локального компьютера.***
```
kubectl get services service-web -o wide
```

![[Снимок экрана от 2025-12-12 16-00-46.png]]

```
curl http://158.160.65.69:31000
```

![[Снимок экрана от 2025-12-12 16-01-29.png]]

![[Снимок экрана от 2025-12-12 16-01-49.png]]
# Задание 3. Создать Deployment приложений backend и frontend
____
- ***Создать Deployment приложения frontend из образа nginx с количеством реплик 3 шт***
```
cat deploy-front.yml
```

![[Снимок экрана от 2025-12-12 16-07-56.png]]

```
kubectl apply -f deploy-front.yml
```

```
kubectl get deployments.apps deploy-front -o wide
```

![[Снимок экрана от 2025-12-12 16-09-30.png]]

- ***Создать Deployment приложения backend из образа multitool.***
```
cat deploy-back.yml 
```

![[Снимок экрана от 2025-12-12 16-17-05.png]]

```
kubectl apply -f deploy-back.yml
```

```
kubectl get deployments.apps deploy-back -o wide
```

```
kubectl get deployments.apps
```

![[Снимок экрана от 2025-12-12 16-13-46.png]]

- ***Добавить Service, которые обеспечат доступ к обоим приложениям внутри кластера.***

```
cat service-web.yml 
```

![[Снимок экрана от 2025-12-12 16-25-03.png]]

```
kubectl apply -f service-web.yml
```

```
kubectl get services service-web -o wide
```

![[Снимок экрана от 2025-12-12 16-26-31.png]]

```
kubectl exec -it deploy-front-6c8c786b96-86pw4 -- bash
```

![[Снимок экрана от 2025-12-12 16-27-24.png]]

```
kubectl exec -it deploy-back-699dd94cb9-cg977 -- bash
```

![[Снимок экрана от 2025-12-12 16-28-16.png]]

# Задание 4. Создать Ingress и обеспечить доступ к приложениям снаружи кластера
___

- ***Включить Ingress-controller в MicroK8S.***

```
microk8s status
```

```
microk8s enable ingress
```

```
microk8s status -a ingress
```

![[Снимок экрана от 2025-12-12 16-33-44.png]]

- ***Создать Ingress, обеспечивающий доступ снаружи по IP-адресу кластера MicroK8S так, чтобы при запросе только по адресу открывался frontend а при добавлении /api - backend.***

```
cat ingress-web.yml
```

![[Снимок экрана от 2025-12-13 14-12-03.png]]

```
kubectl apply -f ingress-web.yml
```

```
kubectl get ingress -o wide
```

```
kubectl describe ingress
```

```
kubectl get ingressclasses.networking.k8s.io
```

![[Снимок экрана от 2025-12-13 14-15-06.png]]

```
curl http://app.test.com
```

![[Снимок экрана от 2025-12-13 14-17-39.png]]

```
curl http://app.test.com/api
```

![[Снимок экрана от 2025-12-13 14-18-07.png]]

```
curl -H "Host: app2.test.com" http://app.test.com/
```

![[Снимок экрана от 2025-12-13 14-18-53.png]]

```
http://app.test.com/
```

![[Снимок экрана от 2025-12-13 14-19-28.png]]

```
http://app.test.com/api
```

![[Снимок экрана от 2025-12-13 14-21-09.png]]
