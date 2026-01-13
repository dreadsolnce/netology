## Чеклист готовности к домашнему заданию
### Установка RKE2 (Rancher Kubernetes Engine)

> [!NOTE]
> RKE2 (Rancher Kubernetes Engine) — это дистрибутив Kubernetes, поставляемый и запускаемый целиком в виде контейнерного приложения (Docker). Использование RKE позволяет избежать сложностей, связанных с установкой и первоначальной настройкой Kubernetes.

[Ссылка на сайт с инструкцией](https://clo.ru/help/containerization/installation/rke2)

#### Установка ноды control plane

```
cat <<EOF >>config.server.yaml

write-kubeconfig-mode: "0644"
node-ip:
  - 10.129.0.11
node-external-ip:
  - 158.160.21.30
advertise-address:
  - 10.129.0.11
cni:
  - calico
disable:
  - rke2-canal
EOF

```

```
curl -sfL https://get.rke2.io | sudo sh -
```

```
sudo mkdir -p /etc/rancher/rke2/
```

```
sudo cp config.server.yaml /etc/rancher/rke2/config.yaml
```

```
export KUBECONFIG=/etc/rancher/rke2/rke2.yaml
```

```
export PATH=$PATH:/var/lib/rancher/rke2/bin
```

```
sudo systemctl enable --now rke2-server.service
```


> [!IMPORTANT]
> Запуск RKE2 в среднем занимает около 5 минут.

> [!TIP]
>  - Проверить статус RKE2 можно с помощью следующих команд:
>  `journalctl -u rke2-server -f`  
   `kubectl cluster-info`
>  - Получите токен авторизации для подключения нод к кластеру
>  `cat /var/lib/rancher/rke2/server/node-token` 

<img width="1008" height="171" alt="Снимок экрана от 2026-01-13 10-38-43" src="https://github.com/user-attachments/assets/aecd7fca-f83b-4cda-80f5-38b44dd58bf1" />


```
source .bashrc
```

#### Установка ноды worker plane

```
cat <<EOF >>config.agent.yaml 

server: https://10.129.0.11:9345
token: K106a4836c44091b389676e53634dc1d0621c865355e551c01d35fc353ead404aae::server:1f629b03dd3b3b8c1441a368...
node-ip:
  - 10.129.0.12
node-external-ip:
  - 158.160.71.58
cni:
  - calico
disable:
  - rke2-canal
EOF

```

```
curl -sfL https://get.rke2.io | INSTALL_RKE2_TYPE="agent" sudo sh -
```
  
```
sudo mkdir -p /etc/rancher/rke2/
```

```
sudo cp config.agent.yaml /etc/rancher/rke2/config.yaml
```

```
sudo systemctl enable --now rke2-agent.service
```

> [!TIP]
> - Проверьте статус подключения рабочей ноды на сервере control node с помощью команды
> `kubectl get nodes`
> - Назначьте статус для рабочей ноды
> `kubectl label nodes worker node-role.kubernetes.io/worker=true`

```
kubectl label nodes wn node-role.kubernetes.io/worker=true
```

```
kubectl get nodes
```

<img width="521" height="114" alt="Снимок экрана от 2026-01-13 12-15-22" src="https://github.com/user-attachments/assets/11eb68b9-6975-496d-a022-e8b8ba43423d" />

## Задание 1. Создать сетевую политику или несколько политик для обеспечения доступа

1. **Создать deployment'ы приложений frontend, backend и cache и соответсвующие сервисы.**

***fronend:***

```
cat <<EOF >>deploy-frontend.yml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  labels:
    app: label-front 
  namespace: app                     
spec:
  replicas: 2 # Колличество подов 
  selector:
    matchLabels:
      app: label-front
  template:
    metadata:
      labels:
        app: label-front
    spec:
      containers:
        - name: multitool
          image: praqma/network-multitool:alpine-extra
          ports:
            - containerPort: 80
              name: port

EOF
```

```
kubectl apply -f deploy-frontend.yml
```

```
kubectl -n app get deployments.apps frontend -o wide
```

<img width="972" height="96" alt="Снимок экрана от 2026-01-13 12-17-36 1" src="https://github.com/user-attachments/assets/579490dc-68db-4861-a1aa-bffcbf5f866f" />


```
kubectl -n app get pods -o wide
```

<img width="969" height="79" alt="Снимок экрана от 2026-01-13 12-18-46" src="https://github.com/user-attachments/assets/28cd2a4f-59e5-4795-b114-fa8b632b883d" />


```
cat <<EOF >>service-frontend.yml

apiVersion: v1
kind: Service
metadata:
  name: service-frontend
  namespace: app
spec:
  selector:
    app: label-front
  ports:
  - name: web-port-80
    protocol: TCP
    port: 80
    targetPort: port

EOF
```

```
kubectl apply -f service-frontend.yml
```

```
kubectl -n app get services service-frontend -o wide
```

<img width="716" height="63" alt="Снимок экрана от 2026-01-13 12-07-44" src="https://github.com/user-attachments/assets/0ecf3334-3c60-4cc9-9649-b7d43bb36648" />


```
curl http://10.43.18.202
```

<img width="680" height="79" alt="Снимок экрана от 2026-01-13 12-20-28" src="https://github.com/user-attachments/assets/e7a9671d-54df-4d22-bfd6-c57d0625c379" />


***backend и cache:***


> [!NOTE]
> По аналогии создаем deployment и service для backend и cache

2. 
3. **Разместить поды в namespace app.**
```
cat <<EOF >>namespace.yml

apiVersion: v1
kind: Namespace
metadata:
  name:  app     # Имя вашего нового пространства имен
  labels:
    app: deploy

EOF
```

```
kubectl apply -f namespace.yml
```

```
kubectl get namespaces app -o wide
```

<img width="395" height="75" alt="Снимок экрана от 2026-01-13 11-33-26" src="https://github.com/user-attachments/assets/d0d810de-5bed-473c-96e8-2bcb590b95f7" />


4. **Создать политики, чтобы обеспечить доступ frontend -> backend -> cache. Другие виды подключений должны быть запрещены.**

```
cat <<EOF >>policy-deny.yml

apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata: 
  name: policy-deny
  namespace: app
spec:
  podSelector: {}
  policyTypes:
    - Ingress

```

```
cat <<EOF >>policy-allow-backend.yml 

apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata: 
  name: policy-back
  namespace: app
spec:
  podSelector: 
    matchLabels:
      app: label-back
  policyTypes:
    - Ingress
  ingress:
    - from:
      - podSelector:
          matchLabels:
            app: label-front

```

```
cat <<EOF >>policy-allow-cache.yml 

apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata: 
  name: policy-cache
  namespace: app
spec:
  podSelector: 
    matchLabels:
      app: label-cache
  policyTypes:
    - Ingress
  ingress:
    - from:
      - podSelector:
          matchLabels:
            app: label-back

```

5. **Продемонстрировать, что трафик разрешён и запрещён.**

```
kubectl -n app get deployments.apps -o wide
```

<img width="992" height="120" alt="Снимок экрана от 2026-01-13 16-00-16" src="https://github.com/user-attachments/assets/a23365e3-eecd-4f30-bc68-7cc02eec05c7" />


```
kubectl -n app get services -o wide
```

<img width="777" height="121" alt="Снимок экрана от 2026-01-13 16-03-22" src="https://github.com/user-attachments/assets/83e9d8fd-b4ae-4fec-888d-ac2ce76cbae5" />


```
kubectl -n app get pods -o wide
```

<img width="972" height="184" alt="Снимок экрана от 2026-01-13 16-05-34" src="https://github.com/user-attachments/assets/1029359e-5d7d-4597-a1b0-d5a0640a4a45" />


```
kubectl -n app get networkpolicies.networking.k8s.io -o wide
```

<img width="618" height="119" alt="Снимок экрана от 2026-01-13 16-06-22" src="https://github.com/user-attachments/assets/62dd854e-a6f5-4a80-9084-bd0792de64b8" />


```
kubectl -n app exec -it deployments/frontend -- bash
```

<img width="679" height="152" alt="Снимок экрана от 2026-01-13 16-08-34" src="https://github.com/user-attachments/assets/5563f956-2a33-4805-85fe-e72924427b5d" />


```
kubectl -n app exec -it deployments/backend -- bash
```

<img width="669" height="152" alt="Снимок экрана от 2026-01-13 16-09-52" src="https://github.com/user-attachments/assets/1754fa45-3670-4403-a295-6ebb103b2ade" />


```
kubectl -n app exec -it deployments/cache -- bash
```

<img width="535" height="151" alt="Снимок экрана от 2026-01-13 16-11-07" src="https://github.com/user-attachments/assets/3e258c23-5f42-44f0-b46b-1d68541bf6a3" />

