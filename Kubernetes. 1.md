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









