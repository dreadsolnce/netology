### Задание 1

***Создать Deployment приложения, состоящего из двух контейнеров и обменивающихся данными.***

___
1. Создать Deployment приложения, состоящего из контейнеров busybox и multitool.

```
cat deploy-app.yml
```

<img width="1166" height="607" alt="Снимок экрана от 2025-12-15 11-42-43" src="https://github.com/user-attachments/assets/f62f77ba-8059-4927-83aa-ef9e62678da6" />


```
kubectl apply -f deploy-app.yml
```

```
kubectl get deployments.apps deploy-app
```

<img width="669" height="87" alt="Снимок экрана от 2025-12-15 11-47-44" src="https://github.com/user-attachments/assets/145726d7-5eaf-47b7-a68b-62bf6531245e" />

___
2. Сделать так, чтобы busybox писал каждые пять секунд в некий файл в общей директории.
3. Обеспечить возможность чтения файла контейнером multitool.
4. Продемонстрировать, что multitool может читать файл, который периодоически обновляется.

```
kubectl exec -it deploy-app-5cc5fd7485-jkmhj -c container-busybox -- sh
```

```
tail -f /busybox-data/date.txt
```

<img width="337" height="211" alt="Снимок экрана от 2025-12-15 11-49-27" src="https://github.com/user-attachments/assets/38433a6d-adb9-4911-bb09-37b87e0c9d75" />

<img width="366" height="221" alt="Снимок экрана от 2025-12-15 11-49-44" src="https://github.com/user-attachments/assets/b954a2e6-56b7-465e-97a4-1097eddb5218" />


```
kubectl exec -it deploy-app-5cc5fd7485-jkmhj -c container-multitool -- bash
```

```
tail -f /multitool-data/date.txt
```

<img width="509" height="219" alt="Снимок экрана от 2025-12-15 11-51-15" src="https://github.com/user-attachments/assets/3cf3d85f-db05-45b4-9c0c-53c87545fcdd" />

<img width="532" height="222" alt="Снимок экрана от 2025-12-15 11-51-29" src="https://github.com/user-attachments/assets/fd8bf585-0ba6-4b1a-b65d-49bb5fc3677e" />


### Задание 2

***Создать DaemonSet приложения, которое может прочитать логи ноды.***

___
1. Создать DaemonSet приложения, состоящего из multitool.

```
cat daemonset-multitool.yml
```

<img width="589" height="556" alt="Снимок экрана от 2025-12-15 13-09-08" src="https://github.com/user-attachments/assets/4447eff0-7975-46c9-8567-6bb972ac28f7" />

___
2. Обеспечить возможность чтения файла `/var/log/syslog` кластера MicroK8S.
3. Продемонстрировать возможность чтения файла изнутри пода

```
kubectl exec -it daemonset-multitool-w4qsk -- bash
```

```
ssh -l ubuntu 158.160.71.158
```

<img width="1283" height="143" alt="Снимок экрана от 2025-12-15 13-12-10" src="https://github.com/user-attachments/assets/80faad56-fd1a-4a4d-bcc8-de5d52dc4e81" />

```
tail -f /mickrok8s.log
```

```
tail -f /var/log/syslog
```

<img width="1274" height="680" alt="Снимок экрана от 2025-12-15 13-13-42" src="https://github.com/user-attachments/assets/8ad56d57-d15b-4d61-900e-59e3ffaad8f1" />

## Задание 2. PV, PVC
___
### Задача

Создать Deployment приложения, использующего локальный PV, созданный вручную.

1. Создать Deployment приложения, состоящего из контейнеров busybox и multitool, использующего созданный ранее PVC

```
cat deploy-app-pvc.yml
```

<img width="595" height="620" alt="Снимок экрана от 2025-12-15 15-23-34" src="https://github.com/user-attachments/assets/cb5c4b97-6803-4334-885f-878c631a3266" />


2. Создать PV и PVC для подключения папки на локальной ноде, которая будет использована в поде.

```
cat pv.yml
```

```
cat pvc.yml
```

<img width="494" height="503" alt="Снимок экрана от 2025-12-15 15-25-55" src="https://github.com/user-attachments/assets/b58fe05f-64a2-4d51-aa60-8bad45241587" />


3. Продемонстрировать, что контейнер multitool может читать данные из файла в смонтированной директории, в который busybox записывает данные каждые 5 секунд.

```
kubectl apply -f pv.yml
```

```
kubectl apply -f pvc.yml
```

```
kubectl apply -f deploy-app-pvc.yml
```

<img width="657" height="137" alt="Снимок экрана от 2025-12-15 15-27-58" src="https://github.com/user-attachments/assets/8ad17639-9023-42b8-b3eb-241b812a2afe" />


```
kubectl exec -it deploy-app-7f89fc6b98-28vl4 -c container-multitool -- bash
```

```
tail -f /multitool-data/date.txt
```

<img width="956" height="386" alt="Снимок экрана от 2025-12-15 15-30-18" src="https://github.com/user-attachments/assets/d41f0b3e-5b69-4c9b-a2d1-409d6ef9c8d2" />


```
kubectl describe pv
```

<img width="551" height="365" alt="Снимок экрана от 2025-12-15 15-45-30" src="https://github.com/user-attachments/assets/c5bb18cf-3acf-4cac-95e3-94a241d312e4" />


**Статус - bound (связано)**

4. Удалить Deployment и PVC. Продемонстрировать, что после этого произошло с PV. Пояснить, почему. (Используйте команду `kubectl describe pv`)
```
kubectl delete deployments.apps deploy-app
```

```
kubectl delete persistentvolumeclaims pvc-data
```

<img width="766" height="135" alt="Снимок экрана от 2025-12-15 15-37-25" src="https://github.com/user-attachments/assets/908a597e-a852-495c-ba20-5e8d9ee159cd" />

```
kubectl describe pv
```

<img width="579" height="326" alt="Снимок экрана от 2025-12-15 15-38-21" src="https://github.com/user-attachments/assets/4b21dd1c-1d4f-49a1-bccc-4e9379ca3a2b" />

**Статус Released (освобождено)**

5. Продемонстрировать, что файл сохранился на локальном диске ноды. Удалить PV. Продемонстрировать, что произошло с файлом после удаления PV. Пояснить, почему

```
ls -l /tmp/data/date.txt
``` 

<img width="502" height="259" alt="Снимок экрана от 2025-12-15 15-49-03" src="https://github.com/user-attachments/assets/3db12db4-a1f9-4989-a7aa-1277d5a8269e" />

```
kubectl delete persistentvolume pv-data
```

<img width="710" height="66" alt="Снимок экрана от 2025-12-15 15-49-50" src="https://github.com/user-attachments/assets/d23bb982-8550-48e0-a6c0-41e8322d4ac8" />


```
ls -l /tmp/data/date.txt
```

<img width="500" height="253" alt="Снимок экрана от 2025-12-15 15-50-23" src="https://github.com/user-attachments/assets/48988ec4-18e2-4c0f-9be9-51b712aec933" />


**Файл никуда не делся после удаления pv-data, т.к. в настройках pv указан параметр persistentVolumeReclaimPolicy: Retain, что значит, что после удаления PV ресурсы из  
внешних провайдеров автоматически не удаляются**

___
## Задание 3. StorageClass

### Задача

1. Создать Deployment приложения, использующего PVC, созданный на основе StorageClass.

```
cat deploy-app-svc.yml
```

<img width="537" height="612" alt="Снимок экрана от 2025-12-15 16-41-33" src="https://github.com/user-attachments/assets/d585108f-2347-439f-bcd6-1e2e2c7b3439" />


2. Создать SC и PVC для подключения папки на локальной ноде, которая будет использована в поде.

```
cat sc.yml
```

```
cat pvc-sc.yml
```

<img width="549" height="440" alt="Снимок экрана от 2025-12-15 16-43-35" src="https://github.com/user-attachments/assets/fe588966-c0a3-4dd8-a9ce-b7faed235923" />


```
cat pv-sc.yml 
```

<img width="842" height="282" alt="Снимок экрана от 2025-12-15 16-56-51" src="https://github.com/user-attachments/assets/0b07ee62-846c-4a2e-bc81-12aa0f869747" />


3. Продемонстрировать, что контейнер multitool может читать данные из файла в смонтированной директории, в который busybox записывает данные каждые 5 секунд.

```
kubectl exec -it deploy-app-sc-644ccdbbf7-b6z9z -c container-multitool -- bash
```

```
tail -f /multitool-data/date.txt
```

<img width="1029" height="407" alt="Снимок экрана от 2025-12-15 16-47-21" src="https://github.com/user-attachments/assets/a560eb29-bec7-4ea5-a2da-58ad19785469" />

<img width="1141" height="246" alt="Снимок экрана от 2025-12-15 17-00-04" src="https://github.com/user-attachments/assets/b7ec4d19-5c62-4ae9-88c9-93502a611ef4" />
















