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








