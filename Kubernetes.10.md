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

> [!tip] Проверка статуса
>  - Проверить статус RKE2 можно с помощью следующих команд:
>  `journalctl -u rke2-server -f`  
   `kubectl cluster-info`
>  - Получите токен авторизации для подключения нод к кластеру
>  `cat /var/lib/rancher/rke2/server/node-token` 

  
