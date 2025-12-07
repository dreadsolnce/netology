Список открытых портов:
```
sudo netstat -tlpn
```
<img width="1052" height="422" alt="Снимок экрана от 2025-12-07 11-51-46" src="https://github.com/user-attachments/assets/fdc9061a-e85c-4038-a8ba-4494f4a9c65d" />


Проверить статус microk8s:
```
`microk8s status --wait-ready`
```
<img width="559" height="134" alt="Снимок экрана от 2025-12-07 11-49-04" src="https://github.com/user-attachments/assets/c0d2bb5b-97fb-45cf-8186-72b3c40f96bf" />


Получить список нод:
```
microk8s kubectl get nodes
```
<img width="426" height="110" alt="Снимок экрана от 2025-12-07 11-50-13" src="https://github.com/user-attachments/assets/a9a8d8b1-25f2-475d-bbd3-6ab1777cc5c5" />

Установить addon dashboard
```
microk8s enable dashboard
```
<img width="1234" height="807" alt="Снимок экрана от 2025-12-07 11-53-26" src="https://github.com/user-attachments/assets/9976fcce-c668-45a6-b33f-638157cefa7d" />


Список addon:
```
microk8s status
```
<img width="838" height="240" alt="Снимок экрана от 2025-12-07 11-57-47" src="https://github.com/user-attachments/assets/faa986af-fe3c-4692-acfa-1fba67fa4902" />


Вывод конфигурации:
```
microk8s config
```
<img width="532" height="668" alt="Снимок экрана от 2025-12-07 11-59-18" src="https://github.com/user-attachments/assets/786b1927-3811-4c91-86ee-24a422e632bf" />


Настройка внешнего подключения
```
sudo vim /var/snap/microk8s/current/certs/csr.conf.template
```

```
# [ alt_names ]
# Add
# IP.3 = 10.128.0.11
# IP.4 = 89.169.153.13
```
<img width="408" height="251" alt="Снимок экрана от 2025-12-07 12-03-53" src="https://github.com/user-attachments/assets/3f31d113-d188-4aef-8d8d-60a514966e4f" />

Обновить сертификаты:
```
sudo microk8s refresh-certs --cert front-proxy-client.crt
```
<img width="946" height="153" alt="Снимок экрана от 2025-12-07 12-04-54" src="https://github.com/user-attachments/assets/340d3932-11be-4070-8c47-25454a45baed" />

Обновить корневой сертификат:
```
sudo microk8s refresh-certs -e ca.crt
```
<img width="1179" height="397" alt="Снимок экрана от 2025-12-07 12-12-10" src="https://github.com/user-attachments/assets/d99d76bc-8bbb-4879-89fb-2c3afc31bcf7" />

Создать конфиг:
```
microk8s config > ~/.kube/config
```
<img width="617" height="698" alt="Снимок экрана от 2025-12-07 12-13-51" src="https://github.com/user-attachments/assets/df3286cd-43d5-408f-b924-d4e1ec72178e" />

Проброс порта для подключения к dashboard локально

```
microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443
```
<img width="1025" height="108" alt="Снимок экрана от 2025-12-07 12-15-36" src="https://github.com/user-attachments/assets/4867400e-042f-4855-bda1-db9f3daca4a5" />

Проверка локального подключения:

```
curl -k https://127.0.0.1:10443
```
<img width="749" height="528" alt="Снимок экрана от 2025-12-07 12-17-16" src="https://github.com/user-attachments/assets/8e5f25b7-e6db-4851-8a1b-bf30e0507eaf" />


Проброс порта для подключения к dashboard из любого места

```
microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443 --address 0.0.0.0
```
<img width="1232" height="55" alt="Снимок экрана от 2025-12-07 12-19-10" src="https://github.com/user-attachments/assets/145486be-5f1d-4b5a-aa03-5d7137e29878" />
<img width="1176" height="914" alt="Снимок экрана от 2025-12-07 12-20-30" src="https://github.com/user-attachments/assets/51330617-a6be-4bf3-ae2f-2a11e49e2d1a" />
Поделючение к dashboard
<img width="644" height="506" alt="Снимок экрана от 2025-12-07 12-22-30" src="https://github.com/user-attachments/assets/d887b1b6-2bae-4608-83a4-fe438b23dc41" />

```
microk8s status -a rbac
```
<img width="395" height="73" alt="Снимок экрана от 2025-12-07 12-22-14" src="https://github.com/user-attachments/assets/106b5d80-dd54-482b-9b59-8824b1fc203c" />

Получение токена для подключения
```
microk8s kubectl describe secrets -n kube-system microk8s-dashboard-token 
```
<img width="939" height="401" alt="Снимок экрана от 2025-12-07 12-25-07" src="https://github.com/user-attachments/assets/6bf2d13c-1a32-47ec-8946-028383f0da36" />
Подключение к dashboard
<img width="425" height="342" alt="Снимок экрана от 2025-12-07 12-25-38" src="https://github.com/user-attachments/assets/71a62f78-7674-4cb1-b1d5-e723e790b6b7" />
<img width="1142" height="553" alt="Снимок экрана от 2025-12-07 12-26-58" src="https://github.com/user-attachments/assets/57888956-092d-42d4-babe-a7ed14afd404" />

Установка kubectl:
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
```
<img width="551" height="148" alt="Снимок экрана от 2025-12-07 12-29-12" src="https://github.com/user-attachments/assets/880503ad-af08-453b-b29e-5b7dde46cbb8" />

```
chmod +x ./kubectl
```

```
sudo mv ./kubectl /usr/local/bin/kubectl
```

```
source <(kubectl completion bash)
```

```
echo "source <(kubectl completion bash)" >> ~/.bashrc
```

Создаем файл конфига для kubectl (можно не создавать т.к. мы его создали до этого просто как пример)
```
microk8s config > .kube/config
```

Проверка получаем информацию о подах
```
kubectl get pods -A
```
<img width="933" height="197" alt="Снимок экрана от 2025-12-07 13-17-08" src="https://github.com/user-attachments/assets/50e868c0-5444-4303-85ae-c028736fca16" />


```
kubectl version
```
<img width="305" height="115" alt="Снимок экрана от 2025-12-07 13-18-05" src="https://github.com/user-attachments/assets/e5b1738a-323d-4410-9864-6b44684fe610" />

Подключение к кластеру через kubectl c другой машины:
```
mkdir .kube
```

```
scp ubuntu@89.169.153.13:/home/ubuntu/.kube/config ~/.kube/
```
<img width="1325" height="68" alt="Снимок экрана от 2025-12-07 13-22-02" src="https://github.com/user-attachments/assets/6f0dc048-5ebe-4455-adaf-c91b93203e61" />

```
ls .kube/
```
<img width="280" height="96" alt="Снимок экрана от 2025-12-07 13-22-44" src="https://github.com/user-attachments/assets/548342e6-2705-46f9-baf6-d01c8194ab5f" />

В конфиге изменить айпи адрес на внешний адрес кластера microk8s

```
vim .kube/config
```
<img width="409" height="83" alt="Снимок экрана от 2025-12-07 13-29-26" src="https://github.com/user-attachments/assets/6120bc48-4632-4ccd-a818-0c838ccfc73d" />


Проверить подключение к кластеру
```
kubectl get pods -A
```
<img width="971" height="251" alt="Снимок экрана от 2025-12-07 13-30-10" src="https://github.com/user-attachments/assets/5c8e6cd9-c309-45ff-ac19-0fd1925a6761" />

Более подробная информация о нодах
```
kubectl get nodes -o wide
```
<img width="1325" height="97" alt="Снимок экрана от 2025-12-07 13-31-31" src="https://github.com/user-attachments/assets/8c2d0764-f842-47f3-93c3-7ff2ac4a3b6a" />








