# Задание 1
___
```
cat deployment-web.yaml
```
<img width="635" height="549" alt="Снимок экрана от 2025-12-09 18-37-26" src="https://github.com/user-attachments/assets/4b2b7912-82c5-49bc-b898-b7e9a3dda8ec" />

```
kubectl apply -f deployment-web.yaml
```

```
kubectl get pods
```
<img width="712" height="173" alt="Снимок экрана от 2025-12-09 18-38-14" src="https://github.com/user-attachments/assets/4fe1a461-a552-4da1-a679-bd328bb329be" />

```
kubectl scale deployment web --replicas=2
```

```
kubectl get pods
```
<img width="784" height="133" alt="Снимок экрана от 2025-12-09 18-39-39" src="https://github.com/user-attachments/assets/4d260f28-1b44-4916-ba00-02e20b7024d1" />

```
cat service-web.yaml
```
<img width="573" height="340" alt="Снимок экрана от 2025-12-10 10-23-54" src="https://github.com/user-attachments/assets/3ddfdbb2-86f7-4881-9cd2-ed3dbda69531" />

```
kubectl get pods
```
```
kubectl exec -it services/service-web -- bash
```
```
curl http://web-b94ff486-b7brb:80
```
```
curl http://web-b94ff486-b7brb:8080
```

