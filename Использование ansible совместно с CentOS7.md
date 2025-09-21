# ansible для работы с CentOS 7

### Т.к. на версии ОС CentOS7 установлен только python версии 2.7, то исходя из таблицы версий (см. ниже), на управляющем узле (компьютер на которм запускается playbook) можно использовать весрию ansible не выше 2.16 совместно с версией python не младше 3.10

https://docs.ansible.com/ansible/latest/reference_appendices/release_and_maintenance.html

<img width="731" height="1174" alt="Снимок экрана от 2025-09-21 12-15-23" src="https://github.com/user-attachments/assets/a1c814c9-47b9-4f2c-a24b-16ea57831bd6" />

### Для установки необходимой версии ansible можно воспользоваться программой pip.

Что бы установить ansible версии 2.16, необходимо сравнить версии с таблицей приведённой ниже:

<img width="978" height="506" alt="Снимок экрана от 2025-09-21 12-33-58" src="https://github.com/user-attachments/assets/ed6a8f76-b0ae-49a9-895a-dc1fa9097504" />

Для версии ansible 2.16 соответствует выпуск пакета Ansible Community 9.x

Таким образом выполнив комманду `pip3 install ansible==9.3.0` можно установить необходимую версию ansible

<img width="1610" height="613" alt="Снимок экрана от 2025-09-21 12-37-11" src="https://github.com/user-attachments/assets/a5402fcb-4f48-459c-97de-4a916f2f62ec" />

## Результат выполнения playbook на целевом хосте с ОС CentOS 7

<img width="570" height="771" alt="Снимок экрана от 2025-09-21 12-49-00" src="https://github.com/user-attachments/assets/597fb55d-6738-43de-a3aa-8dbe72c743cd" />

<img width="634" height="330" alt="Снимок экрана от 2025-09-21 12-48-23" src="https://github.com/user-attachments/assets/7567c261-321b-4c08-9833-fe957ad47fa4" />

<img width="923" height="1293" alt="Снимок экрана от 2025-09-21 12-47-58" src="https://github.com/user-attachments/assets/9616e3d6-8c85-4940-8de4-3f216f5bd78b" />

## Примечание:

Для установки различных версий python и ansible можно использовать переменные окружения python. Этот способ более предпочтителен, т.к. он подразумевает, что в систему не ствавятся ни какие пакеты, а вся работа осуществляется в отдельном виртуальном окружении. Таким образом на одном компьютере можно использовать различные версии python и ansible одновременно. Я лично предпочитаю использовать данный способ. Если будет кому интересно могу выложить процесс создания и использования виртуальных окружений.
