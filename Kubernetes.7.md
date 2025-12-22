***Установка helm***

```
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4 | bash
```

```
helm version
```

***Автодополнение команд helm***

```
helm completion bash > ~/.helm_completion.sh
```

```
# add to .bashrc
source ~/.helm_completion.sh
```

```
source ~/.bashrc
```

### Задание 1. Подготовить Helm-чарт для приложения

***Создание нового чарта с именем app-test***

```
helm create app-test
```

***Проверка шаблона***

```
helm template template-app .
```

***Установить шаблон***

```
helm install app-test
```
<img width="1086" height="376" alt="Снимок экрана от 2025-12-22 10-43-48" src="https://github.com/user-attachments/assets/98e7ec52-b200-4e21-8932-542374d4ac3a" />


***Обновить (а при отсутствии установить) мой чарт***

```
helm upgrade -i app-test .
```

<img width="701" height="214" alt="Снимок экрана от 2025-12-22 11-04-19" src="https://github.com/user-attachments/assets/93a6fa82-ac9d-40c6-b648-08eb9e3985ff" />


***Удалить***

```
helm uninstall app-test
```

***Применение изменений***

> [!NOTE] Замечание
> При изменении Values на свои значения, желательно их изменять не в файле values.yml а создать новый файл и в нем переназначать новые значения

<img width="622" height="92" alt="Снимок экрана от 2025-12-22 11-49-10" src="https://github.com/user-attachments/assets/8f37947e-ed6d-403d-bed1-470a31f00b8b" />



> [!NOTE] Замечание
> И при запуске через helm ссылаться на этот файл

```
helm upgrade -i app-test . -f values.prod.yaml
```

<img width="1339" height="201" alt="Снимок экрана от 2025-12-22 11-55-30" src="https://github.com/user-attachments/assets/8815b9be-c446-44ba-b7f5-5760ed994047" />

<img width="1368" height="195" alt="Снимок экрана от 2025-12-22 11-57-09" src="https://github.com/user-attachments/assets/f314a439-cc2b-4f2f-b831-5690f5dd1e88" />

### Задание 2. Запустить две версии в разных неймспейсах

***Создаем namespace***

```
kubectl create namespace app1
```

```
kubectl create namespace app2
```

```
kubectl get namespaces
```

<img width="607" height="86" alt="Снимок экрана от 2025-12-22 12-01-21" src="https://github.com/user-attachments/assets/fde22b39-f423-44dd-bb14-48729e3136d1" />


```
helm upgrade -i app-test . -n default
```

```
helm upgrade -i app-test . -f values.dev.yaml -n app1
```

```
helm upgrade -i app-test . -f values.prod.yaml -n app2
```

```
helm list -n default
```

```
helm list -n app1
```

```
helm list -n app2
```

<img width="1100" height="188" alt="Снимок экрана от 2025-12-22 13-19-27" src="https://github.com/user-attachments/assets/57196359-5e3f-441a-aa96-d27313d83cf3" />

<img width="585" height="165" alt="Снимок экрана от 2025-12-22 13-20-32" src="https://github.com/user-attachments/assets/74013fb4-52b0-4fd5-8a17-62e6061f0fb2" />

<img width="589" height="167" alt="Снимок экрана от 2025-12-22 13-20-42" src="https://github.com/user-attachments/assets/1171c0ca-eec4-402b-ad6b-86748126b0dc" />

<img width="526" height="191" alt="Снимок экрана от 2025-12-22 13-20-53" src="https://github.com/user-attachments/assets/f62bf601-66fe-4a93-8d9a-2ebebc5371b6" />






