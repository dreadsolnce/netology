## Дополнительные задания (со звёздочкой)

### Задание 2*. Установить HA кластер

___
**Домашнее исправление**
**Создал новый кластер с ip load balancer**
```
sudo kubeadm init --apiserver-advertise-address=10.128.0.111 --pod-network-cidr 10.244.0.0/16 --apiserver-cert-extra-sans=158.160.93.162 --control-plane-endpoint=130.193.57.207 --upload-certs
```

**На второй control ноде подключился к кластеру**

```
kubeadm join 130.193.57.207:6443 --token 5cu72l.svjz9epkqop0il1c \
	--discovery-token-ca-cert-hash sha256:4a36bf1abedc889e6c66490169751918eea439d7674f97f60df6385ec509664e \
	--control-plane --certificate-key 873372a467c6aec54eab98b3bbf578c146b7686f7ea60f0dcb594889a227eb59
```

**На worker ноде подключился к кластеру**

```
kubeadm join 130.193.57.207:6443 --token 5cu72l.svjz9epkqop0il1c \
	--discovery-token-ca-cert-hash sha256:4a36bf1abedc889e6c66490169751918eea439d7674f97f60df6385ec509664e 
```

 
 Отличная задача! Настройка Kubernetes в режиме High Availability (HA) — это правильный подход для production-окружений. Мы будем использовать kubeadm для создания кластера с несколькими master-узлами (stacked control plane) и keepalived для организации виртуального IP-адреса (VIP), который будет служить единой
  точкой входа для API-сервера.

  Архитектура:
   * 3 Master-узла: На каждом будет запущен полный набор компонентов control plane, включая etcd.
   * Keepalived + HAProxy (или только Keepalived): На каждом master-узле будет работать keepalived, который будет управлять одним общим виртуальным IP (VIP). Этот VIP будет "плавающим" и всегда указывать на один из живых master-узлов.
   * Kubeadm: Будет настроен на использование этого VIP как control-plane-endpoint.

  Предполагается, что у вас есть 3 будущих master-узла, на которых уже выполнены Шаги 0, 1, 2 из предыдущей инструкции (подготовка системы, установка containerd и инструментов kubeadm/kubelet/kubectl).

  ---

  Шаг 1: Установка и настройка Keepalived (на ВСЕХ 3-х MASTER узлах)

   1. Установите Keepalived:

   1     sudo apt-get update
   2     sudo apt-get install -y keepalived

   2. Определите переменные:
       * Найдите имя вашего основного сетевого интерфейса: ip a (например, ens18, eth0).
       * Выберите виртуальный IP (VIP), который будет использоваться для кластера. Он должен быть из той же подсети, что и ваши master-узлы, и не должен быть занят.

   3. Создайте скрипт проверки для Keepalived:
      Этот скрипт будет проверять, работает ли локальный API-сервер. Если нет, keepalived передаст VIP другому узлу.

    1     cat <<EOF | sudo tee /etc/keepalived/check_apiserver.sh
    2     #!/bin/sh
    3 
    4     errorExit() {
    5         echo "*** $*" 1>&2
    6         exit 1
    7     }
    8 
    9     curl --silent --max-time 2 --insecure https://localhost:6443/livez -o /dev/null || errorExit "Error GET https://localhost:6443/livez"
   10     if ip addr | grep -q <VIRTUAL_IP>; then
   11         curl --silent --max-time 2 --insecure https://<VIRTUAL_IP>:6443/livez -o /dev/null || errorExit "Error GET https://<VIRTUAL_IP>:6443/livez"
   12     fi
   13     EOF
      Не забудьте заменить `<VIRTUAL_IP>` в скрипте на ваш реальный VIP-адрес. Затем сделайте скрипт исполняемым:

   1     sudo chmod +x /etc/keepalived/check_apiserver.sh

   4. Создайте конфигурационный файл `/etc/keepalived/keepalived.conf`:
      Конфигурация будет немного отличаться для основного и резервных master-узлов.

      Конфиг для ПЕРВОГО (главного) Master-узла (state MASTER, priority 101):

    1     vrrp_script check_apiserver {
    2         script "/etc/keepalived/check_apiserver.sh"
    3         interval 3
    4         weight 2
    5         fall 3
    6         rise 2
    7     }
    8 
    9     vrrp_instance VI_1 {
   10         state MASTER
   11         interface <INTERFACE>
   12         virtual_router_id 51
   13         priority 101
   14         advert_int 1
   15         authentication {
   16             auth_type PASS
   17             auth_pass <PASSWORD>
   18         }
   19         virtual_ipaddress {
   20             <VIRTUAL_IP>/24
   21         }
   22         track_script {
   23             check_apiserver
   24         }
   25     }

      Конфиг для ВТОРОГО и ТРЕТЬЕГО Master-узлов (state BACKUP, priority 100):

    1     vrrp_script check_apiserver {
    2         script "/etc/keepalived/check_apiserver.sh"
    3         interval 3
    4         weight 2
    5         fall 3
    6         rise 2
    7     }
    8 
    9     vrrp_instance VI_1 {
   10         state BACKUP
   11         interface <INTERFACE>
   12         virtual_router_id 51
   13         priority 100
   14         advert_int 1
   15         authentication {
   16             auth_type PASS
   17             auth_pass <PASSWORD>
   18         }
   19         virtual_ipaddress {
   20             <VIRTUAL_IP>/24
   21         }
   22         track_script {
   23             check_apiserver
   24         }
   25     }
      Замените плейсхолдеры: <INTERFACE>, <PASSWORD> (придумайте любой пароль, он должен быть одинаковым на всех узлах) и <VIRTUAL_IP>.

   5. Запустите Keepalived на всех 3-х master-узлах:
   1     sudo systemctl enable keepalived
   2     sudo systemctl start keepalived
      Проверьте, что VIP появился на главном master-узле: ip a | grep <VIRTUAL_IP>.

  Шаг 2: Инициализация ПЕРВОГО Master-узла

  Теперь, когда у нас есть стабильный VIP, мы можем инициализировать кластер.

   1 # Замените <VIRTUAL_IP> на ваш VIP
   2 sudo kubeadm init --control-plane-endpoint "<VIRTUAL_IP>:6443" --upload-certs --pod-network-cidr=192.168.0.0/16
   * --control-plane-endpoint: Указывает всем компонентам кластера использовать VIP для доступа к API.
   * --upload-certs: Загружает сертификаты кластера, чтобы другие master-узлы могли их скачать и присоединиться.

  КРИТИЧЕСКИ ВАЖНО: Сохраните ВЕСЬ вывод этой команды. Он будет содержать две разные kubeadm join команды: одну для присоединения других master-узлов, другую — для worker-узлов. Также сохраните certificate-key.

  Шаг 3: Присоединение ВТОРОГО и ТРЕТЬЕГО Master-узлов

  На втором и третьем master-узлах выполните команду kubeadm join, предназначенную для control-plane (она содержит флаг --control-plane).

   1 # Команда из вывода `kubeadm init`
   2 sudo kubeadm join <VIRTUAL_IP>:6443 --token ... --discovery-token-ca-cert-hash ... --control-plane --certificate-key ...

  Шаг 4: Настройка kubectl и установка CNI

   1. На любом из master-узлов настройте kubectl для локального пользователя:

   1     mkdir -p $HOME/.kube
   2     sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   3     sudo chown $(id -u):$(id -g) $HOME/.kube/config

   2. С этого же узла установите CNI-плагин (например, Calico):
   1     kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/calico.yaml

  Шаг 5: Проверка HA-кластера

   1. Проверьте, что все master-узлы готовы:
   1     kubectl get nodes
      Вы должны увидеть 3 узла в статусе Ready (может потребоваться пара минут после установки CNI).

   2. Тестирование отказоустойчивости:
       * Определите, на каком узле сейчас VIP: ip a | grep <VIRTUAL_IP>.
       * На этом узле остановите keepalived: sudo systemctl stop keepalived.
       * На другом master-узле проверьте, что VIP "переехал" туда: ip a | grep <VIRTUAL_IP>.
       * Во время этого процесса команда kubectl get nodes должна продолжать работать без сбоев (возможна лишь минутная задержка). Это доказывает, что ваш HA-кластер работает!

  Теперь вы можете присоединять worker-узлы, используя вторую команду kubeadm join, которую вы сохранили.
