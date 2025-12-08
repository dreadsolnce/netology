# Задание 1
___
```
cat echo-server.yaml
```
<img width="565" height="206" alt="Снимок экрана от 2025-12-08 13-11-02" src="https://github.com/user-attachments/assets/ef1e8559-6259-44c0-a03b-0b0c222b23a1" />

```
kubectl apply -f echo-server.yaml
```

```
kubectl get pods -n default
```
<img width="680" height="139" alt="Снимок экрана от 2025-12-08 11-37-27" src="https://github.com/user-attachments/assets/ee5bfc1e-9bbc-4edf-b8c7-6aa6f28a925b" />

```
kubectl port-forward -n default echoserver 8888:8080
```
<img width="800" height="83" alt="Снимок экрана от 2025-12-08 11-38-45" src="https://github.com/user-attachments/assets/5ae79eca-8823-4aae-a1eb-37b458059529" />

```
curl http://127.0.0.1:8888
```
<img width="825" height="101" alt="Снимок экрана от 2025-12-08 11-40-03" src="https://github.com/user-attachments/assets/ab304edc-a536-4b7c-bef8-0dcef4cf8253" />
<img width="696" height="532" alt="Снимок экрана от 2025-12-08 11-39-43" src="https://github.com/user-attachments/assets/0ad6cdcf-d170-4be0-952a-368f70432207" />


# Задание 2
___
```
cat netology-web.yaml
```
<img width="669" height="481" alt="Снимок экрана от 2025-12-08 13-03-23" src="https://github.com/user-attachments/assets/646848b6-691e-420f-a49a-a701b2af3d93" />

```
kubectl apply -f netology-web.yaml
```
<img width="675" height="72" alt="Снимок экрана от 2025-12-08 13-05-15" src="https://github.com/user-attachments/assets/01c9a223-85f6-4ed4-a5ff-6ffe53f7c77b" />

```
kubectl get pods -n default
```
<img width="628" height="72" alt="Снимок экрана от 2025-12-08 13-05-49" src="https://github.com/user-attachments/assets/b13cafa9-f63c-44a2-8950-e8c990c7731b" />

```
kubectl get services netology-svc -n default
```
<img width="762" height="81" alt="Снимок экрана от 2025-12-08 13-07-17" src="https://github.com/user-attachments/assets/a72b6d9f-ceca-40b8-9bc6-3bfe017000f1" />

```
kubectl port-forward services/netology-svc 8888:81
```
<img width="792" height="74" alt="Снимок экрана от 2025-12-08 13-08-12" src="https://github.com/user-attachments/assets/60056e31-767e-4249-9193-c75d3e881449" />

```
curl http://127.0.0.1:8888
```
<img width="614" height="534" alt="Снимок экрана от 2025-12-08 13-09-07" src="https://github.com/user-attachments/assets/17c34bef-9cd2-4463-ba10-b22428ad4576" />




