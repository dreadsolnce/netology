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













