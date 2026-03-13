# Пояснения к дипломной работе

## Предустановленные компоненты проекта

#### Предварительно зарегестрированно доменное имя и созданы записи

<img width="1361" height="138" alt="Снимок экрана от 2026-03-13 15-22-00" src="https://github.com/user-attachments/assets/e3ed2ecf-5116-4afb-a1c3-49e59871fe0e" />

___
Замечание: ip адрес резервируется и защищается от удаления при первом запуске проекта. Можно назначить и вручную.

#### Для dns записи выпущены сертификаты от Let`s Encrypt

<img width="795" height="1062" alt="Снимок экрана от 2026-03-13 15-25-54" src="https://github.com/user-attachments/assets/284dc788-d570-4534-a945-5c77dacc283d" />

## Запуск проекта

```
./start.sh apply
```
___
Процесс разворачивания проекта в среднем занимает около 30 минут.

<img width="756" height="663" alt="Снимок экрана от 2026-03-13 15-53-27" src="https://github.com/user-attachments/assets/1fc695da-d628-4f41-8acd-d7bbe21f2ab4" />

## В результате выполнения команды будут созданы следующие ресурсы:

<img width="1536" height="298" alt="Снимок экрана от 2026-03-13 16-03-45" src="https://github.com/user-attachments/assets/47ccf846-c0fd-4bdb-bd61-19e603154376" />

#### 1. Вирутальная машнина бастион с публичныи айпи адресом и установленным nginx для проксирования трафика на внутренние vm 
#### 2. Развернут кластер k8s на трех мастер нодах (3-ех VM) без внешнего ip с возможностью размещения на них подов.
**Примечание:** Данный шаг обусловлен только экономией ресурсов
В клпастере выполнен деплой следующих приложений: тестовое приложение с базой данных mysql, prometheus + grafana, а также atlantis.
Все приложения доступны по доменному имени и протоколу https

## Проверка работы grafana: 

<img width="1465" height="926" alt="Снимок экрана от 2026-03-13 16-05-14" src="https://github.com/user-attachments/assets/1cc12730-c33e-4b79-8bbb-306fc5a7af0e" />

<img width="2331" height="1069" alt="Снимок экрана от 2026-03-13 16-06-34" src="https://github.com/user-attachments/assets/935139e5-1a05-4b1a-bae9-a9f6fd3b6f1d" />

<img width="2308" height="1204" alt="Снимок экрана от 2026-03-13 16-06-57" src="https://github.com/user-attachments/assets/eaa841a2-69d4-4a5f-b8d8-78ca80fb340c" />

<img width="2293" height="1306" alt="Снимок экрана от 2026-03-13 16-07-17" src="https://github.com/user-attachments/assets/9dd35d0d-5bd5-4ab0-b795-3c025ac03c85" />

## Проверка работы тестового приложения

<img width="2227" height="1240" alt="Снимок экрана от 2026-03-13 16-08-45" src="https://github.com/user-attachments/assets/99b46bd4-ef7a-4b29-9980-0b2f0f4df3ba" />

## Проверка работы atlantis

<img width="1559" height="714" alt="Снимок экрана от 2026-03-13 16-09-29" src="https://github.com/user-attachments/assets/194784a3-7577-4f58-8f6e-d0d376dc7c63" />

### Тестирование работы atlantis

Добавим в конфигурацию проекта terraform создание тестовой сети и деплой ее с помощью создания pull request

<img width="844" height="486" alt="Снимок экрана от 2026-03-13 16-17-52" src="https://github.com/user-attachments/assets/424d715c-6f3f-4007-9768-112426e34022" />

<img width="1048" height="708" alt="Снимок экрана от 2026-03-13 16-18-52" src="https://github.com/user-attachments/assets/3a5a6e68-1a26-4d34-8255-6bd3ecf55a85" />

<img width="1102" height="423" alt="Снимок экрана от 2026-03-13 16-19-30" src="https://github.com/user-attachments/assets/408e5116-ba42-4752-819b-a6738f2e48a8" />

<img width="1040" height="733" alt="Снимок экрана от 2026-03-13 16-20-09" src="https://github.com/user-attachments/assets/924b0881-0275-45b4-82b6-27d7497c9eab" />

<img width="1480" height="720" alt="Снимок экрана от 2026-03-13 16-20-16" src="https://github.com/user-attachments/assets/083e6cb2-3715-493f-afb4-47568499cd48" />

<img width="1100" height="1035" alt="Снимок экрана от 2026-03-13 16-22-44" src="https://github.com/user-attachments/assets/ae6eaf47-6a94-4365-a077-16217128aaa1" />

<img width="1085" height="400" alt="Снимок экрана от 2026-03-13 16-23-21" src="https://github.com/user-attachments/assets/bda2bfc6-0fc1-48c0-a6b1-14f72ca57fb0" />

<img width="1041" height="709" alt="Снимок экрана от 2026-03-13 16-23-53" src="https://github.com/user-attachments/assets/b3434e58-26d0-4ccf-bd1e-7b60e76d16b5" />








































