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
kubectl describe services service-web 
```

```
kubectl get pods
```

```
kubectl exec -it web-b94ff486-b7brb -- bash
```

```
curl http://service-web:80
```

```
curl http://service-web:8080
```

```
kubectl exec -it web-b94ff486-b7brb -c multitool-container -- bash
```

```
curl http://service-web:80
```

```
curl http://service-web:8080
```

<img width="881" height="956" alt="Снимок экрана от 2025-12-10 11-24-10" src="https://github.com/user-attachments/assets/8847fa66-bc7d-47fe-9178-63af230e6976" />

<img width="890" height="479" alt="Снимок экрана от 2025-12-10 11-24-24" src="https://github.com/user-attachments/assets/6ee57816-8c57-4c33-955e-43f617c080e1" />

```
cat pod-multitool.yaml
```

<img width="580" height="241" alt="Снимок экрана от 2025-12-10 11-28-14" src="https://github.com/user-attachments/assets/1d7b4938-3c69-4b52-b1ca-53389b8e5ec2" />

```
kubectl exec -it pod-multitool -- bash
```

<img width="695" height="509" alt="Снимок экрана от 2025-12-10 11-29-23" src="https://github.com/user-attachments/assets/d0a604b2-d2a7-4650-8554-03d267c35063" />

# Задание 2
___

```
cat deployment-web-nginx-busybox.yaml
```

<img width="1588" height="582" alt="Снимок экрана от 2025-12-10 17-00-03" src="https://github.com/user-attachments/assets/b4559c88-48c6-459a-8417-d3f23b48b86b" />

```
cat service-web-nginx-busybox.yaml 

```

<img width="657" height="276" alt="Снимок экрана от 2025-12-10 17-00-23" src="https://github.com/user-attachments/assets/f8829098-05d3-4592-95fd-5089593ba520" />

```
kubectl get pods
```
```
kubectl get services
```
```
kubectl get deployments.apps
```
```
kubectl get pods
```
```
kubectl apply -f service-web-nginx-busybox.yaml
```
```
kubectl get pods
```
<img width="857" height="434" alt="Снимок экрана от 2025-12-10 16-59-14" src="https://github.com/user-attachments/assets/01b4291c-cbbe-45a1-8d0e-efeb6c3a8129" />




