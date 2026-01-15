### Задание 1. Выбрать стратегию обновления приложения и описать ваш выбор

**Выбор стратегии: Recreate**

Наиболее подходящей и безопасной стратегией в заданных условиях является ***"Recreate"***.


> [!NOTE]
> *Причины выбора именно этой стратегии:*
> - Т.к. у нас критически мало запаса свободных ресурсов, а этот подход (recreate) не требует никаких дополнительных ресурсов. В каждый момент времени работает только ОДНА версия приложения.
> - Т.к. новые и старые версии приложения не совместимы, а данная стратегия подразумевает полное удаление (отключение) старого деплоймента (приложения) и только потом создание нового с новой версией, то проблема их несовместимости полностью исключается.

### Задание 2. Обновить приложение

1. Создать deployment приложения с контейнерами nginx и multitool. Версию nginx взять 1.19. Количество реплик — 5.

```
cat <<EOF >> deploy.yml 

apiVersion: apps/v1
kind: Deployment
metadata:
  name: deoloy 
  labels:
    app: app
spec:
  replicas: 5 # Колличество подов 
  selector:
    matchLabels:
      app: app
  template:
    metadata:
      labels:
        app: app
    spec:
      containers:
      - name: nginx-container
        image: nginx:1.19
        ports:
        - containerPort: 80
          name: port-nginx
      
      - name: multitool-container
        image: praqma/network-multitool:alpine-extra
        ports:
        - containerPort: 8080 
          name: port-multi
        env:
        - name: HTTP_PORT
          value: "8080"

EOF
```

<img width="438" height="185" alt="Снимок экрана от 2026-01-15 11-20-39" src="https://github.com/user-attachments/assets/cc153f85-6545-4227-89b7-1878f71fe1ec" />

<img width="990" height="164" alt="Снимок экрана от 2026-01-15 11-25-27" src="https://github.com/user-attachments/assets/bf4b2f71-096e-4cdd-9911-2ee43415017f" />


2. Обновить версию nginx в приложении до версии 1.20, сократив время обновления до минимума. Приложение должно быть доступно.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deoloy
  labels:
    app: app
spec:
  replicas: 5 # Колличество подов 
  strategy:
    type: RollingUpdate # Указываем тип стратегии
    rollingUpdate:
      maxUnavailable: 4 # Максимум 1 под может быть недоступен (или 25% от 3 = 0, но округляется вверх)
      maxSurge: 5       # Максимум 1 новый под может быть создан (или 25% от 3 = 0, но округляется вверх)
  selector:
    matchLabels:
      app: app
  template:
    metadata:
      labels:
        app: app
    spec:
      containers:
      - name: nginx-container
        image: nginx:1.20
        ports:
        - containerPort: 80
          name: port-nginx

      - name: multitool-container
        image: praqma/network-multitool:alpine-extra
        ports:
        - containerPort: 8080
          name: port-multi
        env:
        - name: HTTP_PORT
          value: "8080"

```

<img width="999" height="398" alt="Снимок экрана от 2026-01-15 12-17-37" src="https://github.com/user-attachments/assets/7ca8b55b-71d5-4c4a-bd50-81d2b4e337ff" />

<img width="1003" height="156" alt="Снимок экрана от 2026-01-15 12-17-49" src="https://github.com/user-attachments/assets/e623413e-0599-4554-8099-f8e37ed85071" />


3. Попытаться обновить nginx до версии 1.28, приложение должно оставаться доступным.

<img width="307" height="120" alt="Снимок экрана от 2026-01-15 12-19-01" src="https://github.com/user-attachments/assets/a15cdb8a-ec59-42cf-a9ff-b5852aedc2a5" />

<img width="1041" height="143" alt="Снимок экрана от 2026-01-15 12-20-33" src="https://github.com/user-attachments/assets/2f57e5ed-6281-40f1-956d-55f208ee4867" />


4. Откатиться после неудачного обновления.

```
kubectl rollout history deployment deoloy 
```

<img width="460" height="168" alt="Снимок экрана от 2026-01-15 12-30-55" src="https://github.com/user-attachments/assets/dcfb5708-4693-408a-884f-971f8bcb3b3b" />


```
kubectl rollout undo deployment deoloy
```

<img width="1004" height="128" alt="Снимок экрана от 2026-01-15 12-26-16" src="https://github.com/user-attachments/assets/317b3d59-1a0c-440c-93e8-36ba27b17ad3" />


```
kubectl rollout history deployment deoloy
```

<img width="460" height="177" alt="Снимок экрана от 2026-01-15 12-31-46" src="https://github.com/user-attachments/assets/65c75e14-ab8b-466f-80be-b01421b1cd3a" />

