### Задание. При деплое приложение web-consumer не может подключиться к auth-db. Необходимо это исправить

[ссылка на домашнее задание](https://github.com/netology-code/kuber-homeworks/blob/main/3.5/3.5.md#задание-при-деплое-приложение-web-consumer-не-может-подключиться-к-auth-db-необходимо-это-исправить)

1. Установить приложение по команде:

```shell
kubectl apply -f https://raw.githubusercontent.com/netology-code/kuber-homeworks/main/3.5/files/task.yaml
```
<img width="1383" height="72" alt="Снимок экрана от 2026-01-19 10-24-48" src="https://github.com/user-attachments/assets/b9605f65-9d44-47f8-859c-79a74e5efd8e" />



**Создаем namespace:**

```
kubectl create namespace web
```

```
kubectl create namespace data
```


```shell
kubectl apply -f https://raw.githubusercontent.com/netology-code/kuber-homeworks/main/3.5/files/task.yaml
```

<img width="967" height="99" alt="Снимок экрана от 2026-01-19 10-26-26" src="https://github.com/user-attachments/assets/d4cb9fa7-f41d-42d1-a27c-121fcd2faad8" />


2. Выявить проблему и описать.

```
kubectl -n web get deployments.apps -o wide
```

<img width="913" height="61" alt="Снимок экрана от 2026-01-19 10-29-56" src="https://github.com/user-attachments/assets/9c642ac8-ba4a-480b-abdc-2f8a14bf1306" />


```
kubectl -n web get pods -o wide
```

<img width="1109" height="97" alt="Снимок экрана от 2026-01-19 10-30-23" src="https://github.com/user-attachments/assets/6f42c5c3-67a7-42e8-af38-426a08a6f9f6" />



> [!NOTE]
> Видим, что контейнеры не запустились

**Смотрим логи пода:**

```
kubectl -n web logs web-consumer-64fc486bdf-dnnpd
```

<img width="1205" height="71" alt="Снимок экрана от 2026-01-19 10-37-14" src="https://github.com/user-attachments/assets/62baa0bd-758d-46f7-9667-6a69f9ceae6d" />



> [!NOTE] 
> Из логов видно, что образ не может быть загружен. Контейнер ожидает запуска.

**Проверим правильно ли указано название образа.**

```
cat task.yml
```

<img width="397" height="129" alt="Снимок экрана от 2026-01-19 10-43-26" src="https://github.com/user-attachments/assets/44067674-a917-4990-85f8-9be18478282f" />


***Ищем image на Docker Hub и находим его. Соответственно проблема не в названии образа. ***

<img width="800" height="120" alt="Снимок экрана от 2026-01-19 10-44-59" src="https://github.com/user-attachments/assets/b2e17e53-6719-4935-8fad-edc557661e9d" />

**Проверим вывод информации через describe**

```
kubectl -n web describe pods web-consumer-64fc486bdf-mwc7b
```

<img width="2554" height="207" alt="Снимок экрана от 2026-01-19 11-45-12" src="https://github.com/user-attachments/assets/75a47af6-30fe-4b71-b140-26ac6eb25fc1" />




> [!NOTE]
> В выводе обращаем на ошибку, которая означает что ==образ был собран с использованием устаревшего формата манифеста **Docker Image Manifest V1**, который больше не поддерживается в современных версиях== containerd. Если посмотреть информацию об образе на сайте docker hub, то действительно можно увидеть что последняя версия образа была собрана 10 лет назад.

Есть два пути решения данной проблемы:

- **Использовать другой, более актуальный образ**
- **Пересобрать устаревший образ**

**Для решения проблемы выберем первый вариант, использовать другой образ например curlimages/curl:latest***

<img width="388" height="47" alt="Снимок экрана от 2026-01-19 12-09-30" src="https://github.com/user-attachments/assets/14177ddc-0658-4072-b0ca-92b0b97b67d3" />

```
kubectl apply -f task.yaml
```

```
kubectl -n web get deployments.apps web-consumer -o wide
```

<img width="896" height="79" alt="Снимок экрана от 2026-01-19 12-10-31" src="https://github.com/user-attachments/assets/4553d549-59d3-4f0a-910e-0b8464296bbf" />


Видим что контейнеры запустились. Проверим логи подов.

```
kubectl -n web logs pods/web-consumer-86bf747999-9ggzn
```

<img width="987" height="173" alt="Снимок экрана от 2026-01-19 12-12-13" src="https://github.com/user-attachments/assets/358d4fa0-77f6-48a2-8a63-463831ac82ab" />


> [!NOTE]
> Видим ошибку: curl: (6) Не удалось разрешить хост: auth-db

Данная ошибка вызвана тем, что deployment: web-consumer находится в другом namespace в отличии от deployment: auth-db и service: auth-db. Изменим namespace для deployment: web-consumer на data


<img width="195" height="75" alt="Снимок экрана от 2026-01-19 12-18-03" src="https://github.com/user-attachments/assets/191f7186-2b3b-40e5-9386-49d0e89d24ce" />


```
kubectl -n data logs web-consumer-86bf747999-m8mzx
```

<img width="737" height="555" alt="Снимок экрана от 2026-01-19 12-19-14" src="https://github.com/user-attachments/assets/2439870d-cab1-492c-8388-a46f11baa604" />


В итоге получаем ответ от auth-db.




