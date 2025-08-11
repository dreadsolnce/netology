# Задача 1

https://hub.docker.com/repository/docker/kolchinvladimir/custom-nginx/general

# Задача 2

<img width="1588" height="401" alt="Снимок экрана от 2025-08-11 13-11-15" src="https://github.com/user-attachments/assets/1d7636ea-29b6-4417-be87-17f77ae4e741" />

# Задача 3

Потому что мы подключились к контейнеру (как бы к процессу docker-а на хостовой машине) к его потоком ввода вывода (как бы находимся внутри контейнера) и соответственно при нажатии комбинаций клавиш Ctrl + C мы прерываем именно процесс.

Через локальный порт хоста 8080 мы не можем получить доступ т.к. у нас он связан с портом 80 контейнера, а в контейнере у нас открыт 81 порт, а 80 нет.

<img width="1246" height="569" alt="Снимок экрана от 2025-08-11 17-03-22" src="https://github.com/user-attachments/assets/80364e58-b37b-4efc-ac4b-aef76a8581e0" />

Замечание по задаче 3: т.к. образ nginx:1.21.1 основан на debian 10 (он снят с поддержки и соответственно не доступны репозитори), то apt update, apt-get install не работает и поэтому не возможно установить текстовый редактор. Я нашел и скачал отдельные пакеты vim в виде deb пакетов, добавил их в Dockerfile и их установку через мкенеджер пакетов dpkg. После чего я в контейнере уже смог изменить файл sources.list на путь [](http://archive.debian.org) а также изменить файл /etc/nginx/conf.d/default.conf. Ссылка на новый образ с установленным vim я также залил на docker hub c тегом 1.0.6: https://hub.docker.com/repository/docker/kolchinvladimir/custom-nginx/general
# Задача 4

<img width="1035" height="1166" alt="Снимок экрана от 2025-08-11 17-30-03" src="https://github.com/user-attachments/assets/eeebd073-5475-4a43-93c0-d25354c67c60" />

# Задача 5

Будет запущен файл compose.yaml т.к. он предпочтительней файла docker-compose.yaml.  Файл docker-compose.yaml нужен для обеспечения обратной совместимости с более ранними версиями docker-compose.

<img width="845" height="1062" alt="Снимок экрана от 2025-08-11 18-23-32" src="https://github.com/user-attachments/assets/de3bad91-86f2-4cc3-b568-7861049e039a" />

Суть предупреждения: говорит, что обнаружены потерянные контейнеры. Если вы удалили службу (в нашем примере удалили файл compose.yaml - служба portainer) необходимо выполнить комманду docker compose up -d с флагом --remove-orphans чтобы очистить ее!

<img width="1509" height="556" alt="Снимок экрана от 2025-08-11 18-32-44" src="https://github.com/user-attachments/assets/edf56c1e-debf-43dd-8874-5cee32bab0b7" />

<img width="1509" height="397" alt="Снимок экрана от 2025-08-11 18-37-00" src="https://github.com/user-attachments/assets/ea544b09-a48c-4ca3-8329-c156e6904c44" />


