# Задача 2

<img width="1245" height="1101" alt="Screenshot_13" src="https://github.com/user-attachments/assets/745c5ff5-32b3-4b58-bd74-455ab92a1c89" />

# Задача 3

<img width="1561" height="994" alt="Снимок экрана от 2025-08-18 12-12-35" src="https://github.com/user-attachments/assets/77ad396b-e38a-4996-9d94-7424675edc87" />

# Задача 4

<img width="1968" height="394" alt="Снимок экрана от 2025-08-18 12-59-20" src="https://github.com/user-attachments/assets/3d56cc9d-bd0d-4834-bfff-c5b231432b7a" />

<img width="952" height="1063" alt="Снимок экрана от 2025-08-18 13-26-12" src="https://github.com/user-attachments/assets/41bdee3b-0b54-49ca-8beb-be534c9f708d" />

Ссылка на fork: https://github.com/dreadsolnce/shvirtd-example-python

# Задача 5

***Скрипт backup.sh***
```
#!/bin/bash

MYSQL_ROOT_PASSWORD=$(cat /opt/project/.env | grep MYSQL_ROOT_PASSWORD | awk -F= '{print $2}')
MYSQL_DATABASE=$(cat /opt/project/.env | grep  MYSQL_DATABASE| awk -F= '{print $2}')

docker run --rm --env-file /opt/project/.env  --entrypoint "" -v /opt/backup:/backup --link=mysql:alias --network=project_backend my-mysqldump:latest mysqldump --opt -h alias -u root -p"${MYSQL_ROOT_PASSWORD}" "--result-file=/backup/dumps.sql_$(date +%F_%T)" ${MYSQL_DATABASE}
```

***Cron-task:***

```
*/1 * * * * /opt/project/backup.sh
```

***Копии базы данных:***

<img width="753" height="145" alt="Снимок экрана от 2025-08-18 14-51-32" src="https://github.com/user-attachments/assets/68e16dea-3806-4e81-a5ac-e8f84714c01d" />

# Задача 6

<img width="1181" height="427" alt="Снимок экрана от 2025-08-19 10-22-40" src="https://github.com/user-attachments/assets/62606b5d-acb7-41ac-841d-4f4d4c505034" />

<img width="819" height="411" alt="Снимок экрана от 2025-08-19 10-23-54" src="https://github.com/user-attachments/assets/f3bd7e67-3b9f-4860-ae38-ede2848f9914" />

<img width="1105" height="398" alt="Снимок экрана от 2025-08-19 10-25-52" src="https://github.com/user-attachments/assets/dae6ac9d-1af3-438c-a22c-2ae4cb520b5c" />

<img width="809" height="485" alt="Снимок экрана от 2025-08-19 10-27-05" src="https://github.com/user-attachments/assets/6a3b76b6-56af-4f8e-a7c9-74d7dd3f1ec7" />


# Задача 6.1

<img width="1433" height="420" alt="Снимок экрана от 2025-08-19 10-34-54" src="https://github.com/user-attachments/assets/f0b6ccd9-052a-400e-b447-8e45c6b8e2f3" />

# Задача 6.2

<img width="1438" height="996" alt="Снимок экрана от 2025-08-19 10-39-43" src="https://github.com/user-attachments/assets/9a067602-8a43-4b01-8835-fa5acc981279" />

<img width="1311" height="185" alt="Снимок экрана от 2025-08-19 10-40-34" src="https://github.com/user-attachments/assets/6fc4dce9-e37c-4d64-be7c-166088aa433c" />

# Задача 7

<img width="1015" height="231" alt="Снимок экрана от 2025-08-19 11-32-09" src="https://github.com/user-attachments/assets/90a65e47-5fb2-4ed8-9ed8-cac0491252bb" />

<img width="634" height="237" alt="Снимок экрана от 2025-08-19 11-33-56" src="https://github.com/user-attachments/assets/aa9dcf94-67eb-4e1e-837f-ac8c7f8d3b0a" />

<img width="348" height="66" alt="Снимок экрана от 2025-08-19 11-27-44" src="https://github.com/user-attachments/assets/994f608e-25d4-465a-8547-c5c6702ec018" />

