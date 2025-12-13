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

<img width="416" height="310" alt="Снимок экрана от 2025-12-12 15-58-34" src="https://github.com/user-attachments/assets/d4edc969-e405-4fff-a6c7-4535d5358a26" />


- ***Продемонстрировать доступ с помощью браузера или `curl` с локального компьютера.***
```
kubectl get services service-web -o wide
```

<img width="803" height="86" alt="Снимок экрана от 2025-12-12 16-00-46" src="https://github.com/user-attachments/assets/5ef38c4b-fb15-452e-9aa7-35434c0db259" />

```
curl http://158.160.65.69:31000
```
<img width="657" height="457" alt="Снимок экрана от 2025-12-12 16-01-29" src="https://github.com/user-attachments/assets/dd904abc-e9a3-49a1-a7eb-8f9797b42c87" />

<img width="1225" height="314" alt="Снимок экрана от 2025-12-12 16-01-49" src="https://github.com/user-attachments/assets/77cc4ad2-5b96-4e3d-8a8e-cc02cc2300ff" />

# Задание 3. Создать Deployment приложений backend и frontend
____
- ***Создать Deployment приложения frontend из образа nginx с количеством реплик 3 шт***
```
cat deploy-front.yml
```

<img width="541" height="402" alt="Снимок экрана от 2025-12-12 16-07-56" src="https://github.com/user-attachments/assets/72418c8b-a9d2-40e2-94b9-6604ff8b238f" />

```
kubectl apply -f deploy-front.yml
```

```
kubectl get deployments.apps deploy-front -o wide
```

<img width="848" height="133" alt="Снимок экрана от 2025-12-12 16-09-30" src="https://github.com/user-attachments/assets/76d013c3-8ec0-46f3-9d7c-5ecbd71dea73" />


- ***Создать Deployment приложения backend из образа multitool.***
```
cat deploy-back.yml 
```

<img width="554" height="473" alt="Снимок экрана от 2025-12-12 16-17-05" src="https://github.com/user-attachments/assets/4be1c255-55cb-45a2-a882-c86a71a2c6af" />

```
kubectl apply -f deploy-back.yml
```

```
kubectl get deployments.apps deploy-back -o wide
```

```
kubectl get deployments.apps
```

<img width="1069" height="200" alt="Снимок экрана от 2025-12-12 16-13-46" src="https://github.com/user-attachments/assets/becf257d-8818-4416-baed-dfa82cf33148" />


- ***Добавить Service, которые обеспечат доступ к обоим приложениям внутри кластера.***

```
cat service-web.yml 
```

<img width="592" height="330" alt="Снимок экрана от 2025-12-12 16-25-03" src="https://github.com/user-attachments/assets/dfbc84fa-96ef-4300-a73a-38f7c10f5409" />

```
kubectl apply -f service-web.yml
```

```
kubectl get services service-web -o wide
```

<img width="824" height="59" alt="Снимок экрана от 2025-12-12 16-26-31" src="https://github.com/user-attachments/assets/27420b14-c4f6-4a2c-a81b-a8de84f16cd0" />

```
kubectl exec -it deploy-front-6c8c786b96-86pw4 -- bash
```

<img width="773" height="495" alt="Снимок экрана от 2025-12-12 16-27-24" src="https://github.com/user-attachments/assets/db550980-357f-4c19-a4e7-392573f92c86" />

```
kubectl exec -it deploy-back-699dd94cb9-cg977 -- bash
```

<img width="804" height="494" alt="Снимок экрана от 2025-12-12 16-28-16" src="https://github.com/user-attachments/assets/800b405e-d587-41aa-9d80-2a53ca1c42fd" />

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

<img width="350" height="70" alt="Снимок экрана от 2025-12-12 16-33-44" src="https://github.com/user-attachments/assets/76acb8ce-1d4c-4f37-971f-e6a71bdafb8a" />


- ***Создать Ingress, обеспечивающий доступ снаружи по IP-адресу кластера MicroK8S так, чтобы при запросе только по адресу открывался frontend а при добавлении /api - backend.***

```
cat ingress-web.yml
```

<img width="660" height="756" alt="Снимок экрана от 2025-12-13 14-12-03" src="https://github.com/user-attachments/assets/27a6fdf4-b1ca-47c9-a7c7-7fa2ff7675a5" />

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

<img width="910" height="569" alt="Снимок экрана от 2025-12-13 14-15-06" src="https://github.com/user-attachments/assets/4c60fa9a-6137-471e-8311-929d74abcd64" />


```
curl http://app.test.com
```

<img width="695" height="135" alt="Снимок экрана от 2025-12-13 14-17-39" src="https://github.com/user-attachments/assets/c3cdabba-c446-4fec-8ff5-2fee581c4560" />

```
curl http://app.test.com/api
```

<img width="695" height="135" alt="Снимок экрана от 2025-12-13 14-17-39" src="https://github.com/user-attachments/assets/c90b874e-3396-4842-b91f-d45c920d3d67" />

```
curl -H "Host: app2.test.com" http://app.test.com/
```

<img width="952" height="105" alt="Снимок экрана от 2025-12-13 14-18-53" src="https://github.com/user-attachments/assets/37dc9d2f-609e-4140-bc2e-b4df2572bf11" />

```
http://app.test.com/
```

<img width="711" height="311" alt="Снимок экрана от 2025-12-13 14-19-28" src="https://github.com/user-attachments/assets/6646649e-7b46-4b04-ab82-0674469ca906" />

```
http://app.test.com/api
```

<img width="1257" height="203" alt="Снимок экрана от 2025-12-13 14-21-09" src="https://github.com/user-attachments/assets/b43aef5f-2f50-4b27-afb2-185b47866ce3" />

