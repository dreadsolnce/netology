# Задание 1. Установить кластер k8s с 1 master node

## Установка k8s - control-node

***Отключить swap:***

```
sudo swapoff -a
```

```
sudo vim /etc/fstab
```

```
sudo mount -a
```

```
sudo rm /swapfile
```

***Включить IPv4 forwarding:***

```
sudo sysctl -w net.ipv4.ip_forward=1
```

```
sudo vim /etc/sysctl.conf
```

`+++ net.ipv4.ip_forward = 1`

```
sudo sysctl -p
```

```
cat /proc/sys/net/ipv4/ip_forward
```

***Установка среды для работы контейнеров***

```
sudo apt update && sudo apt install -y containerd
```

***Настройка containerd:***

- **Создаем директорию для конфига**
```
sudo mkdir -p /etc/containerd
```

- **Генерируем конфиг по умолчанию и сохраняем**
```
sudo containerd config default | sudo tee /etc/containerd/config.toml
```

- **Критически важный шаг:** *Меняем Cgroup Driver на "systemd", как рекомендует Kubernetes*.
```
sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml
```

```
sudo systemctl restart containerd.service
```

```
sudo systemctl status containerd.service
```

***Установка зависимостей***

```
sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates curl gpg
```

***Подключение репозитория kubernetes***

```
sudo mkdir -p -m 755 /etc/apt/keyrings 
```

```
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

```
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

***Установка основных компонентов***

```
sudo apt update && sudo apt install -y kubelet kubeadm kubectl
```

**Включите сервис kubelet:***  

```
sudo systemctl enable --now kubelet
```

***Инициализация master-ноды***:

```
sudo kubeadm init --apiserver-advertise-address=10.129.0.21 --pod-network-cidr 10.244.0.0/16 --apiserver-cert-extra-sans=158.160.77.70 --control-plane-endpoint=cluster_ip_address
```

> [!NOTE] Описание
> 10.129.0.21 — адрес, который слушает apiserver
> 10.244.0.0/16 — сеть для подов
> 158.160.77.70 — внешний адрес для подключения через kubectl
> cluster_ip_address — кластерный IP-адрес (адрес LoadBalancer) для HA control plane

***Вывод***

<img width="965" height="507" alt="Снимок экрана от 2025-12-25 12-04-49" src="https://github.com/user-attachments/assets/4fa9a812-9c8e-4dfd-8ae3-2678abc7ae58" />

**После успешного выполнения команда покажет токен для подключения** **worker****-****нод**

<img width="944" height="297" alt="Снимок экрана от 2025-12-25 12-14-30" src="https://github.com/user-attachments/assets/9273d368-508c-4f86-889b-c8a0d5b33d77" />


***Подключаем kubectl к k8s***

```
kubectl version --client
```

```
mkdir -p $HOME/.kube
```

```
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
```

```
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

> [!important] Важно
> В конфигурационном файле изменить ip адрес с внешнего на внутренний

```
source <(kubectl completion bash)
```

```
echo "source <(kubectl completion bash)" >> ~/.bashrc
```

```
source .bashrc
```

***Проверяем через kubectl***



***Установить сетевой плагин calico***

```
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/operator-crds.yaml
```

```
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/tigera-operator.yaml
```

```
curl -O https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/custom-resources-bpf.yaml
```


> [!NOTE] Замечание
> Изменить ip адрес в скачанном файле custom-resources-bpf.yaml на сеть подов 10.244.0.0/16 с которой мы инициализировали наш кластер

<img width="353" height="152" alt="Снимок экрана от 2025-12-25 17-14-28" src="https://github.com/user-attachments/assets/0edaf2ad-642a-4188-82a5-c104e98e45d9" />


```
kubectl create -f custom-resources-bpf.yaml
```

***отслеживаем процесс установки:***
```
watch kubectl get tigerastatus
```

<img width="559" height="188" alt="Снимок экрана от 2025-12-25 17-15-43" src="https://github.com/user-attachments/assets/c07582fd-af9d-4958-9ba2-bf52d4363385" />


***Проверяем статус подов чтобы они были запущены***

```
kubectl get pods -A
```

<img width="846" height="272" alt="Снимок экрана от 2025-12-25 17-17-00" src="https://github.com/user-attachments/assets/80ccdece-45f3-4b8c-8180-7704cad706a9" />


***Проверяем статус нашего кластера***

```
kubectl get nodes
```

<img width="503" height="77" alt="Снимок экрана от 2025-12-25 17-18-38" src="https://github.com/user-attachments/assets/1625e21a-baae-48dc-831a-181285a75bde" />



> [!NOTE] Заключение
> Если наш cluster STATUS Ready можно переходить к установке на рабочие ноды

***Команда для создания токена для подключения к copntol-node***

```
kubeadm token create --print-join-command
```

## Установка k8s - worker-node

```
sudo swapoff -a
```

```
sudo vim /etc/fstab
```

```
sudo mount -a
```

```
sudo rm /swapfile
```

```
sudo sysctl -w net.ipv4.ip_forward=1
```

```
sudo vim /etc/sysctl.conf
```

`+++ net.ipv4.ip_forward = 1`

```
sudo sysctl -p
```

```
cat /proc/sys/net/ipv4/ip_forward
```

```
sudo apt update && sudo apt install -y containerd
```

```
sudo mkdir -p /etc/containerd
```

```
sudo containerd config default | sudo tee /etc/containerd/config.toml
```

```
sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml
```

```
sudo systemctl restart containerd.service
```

```

sudo systemctl status containerd.service
```

```
sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates curl gpg
```

```
sudo mkdir -p -m 755 /etc/apt/keyrings 
```

```
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

```
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

```
sudo apt update && sudo apt install -y kubelet kubeadm kubectl
```

```
sudo systemctl enable --now kubelet
```

```
sudo kubeadm join 158.160.86.32:6443 --token zea04u.fczk9bwo4094r8vi --discovery-token-ca-cert-hash sha256:144656bcad096108b5ba017b91f54ac07d4feee627e858e0da3154b418395b3f 
```

## Развернутый кластер:

<img width="481" height="163" alt="Снимок экрана от 2025-12-25 17-54-07" src="https://github.com/user-attachments/assets/41ab5248-d66b-461e-9d78-10392c097289" />

<img width="416" height="300" alt="Снимок экрана от 2025-12-25 17-54-19" src="https://github.com/user-attachments/assets/3b0e3581-77a9-41c2-9512-22ac60643c92" />












