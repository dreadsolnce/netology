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

