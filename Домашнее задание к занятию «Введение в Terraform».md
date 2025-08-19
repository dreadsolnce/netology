# Чек-лист готовности к домашнему заданию

<img width="331" height="102" alt="Снимок экрана от 2025-08-19 12-40-38" src="https://github.com/user-attachments/assets/ac090a51-e02c-4b30-bdf6-ea7c8c5cc8a6" />

<hr>

# Задание 1

2. Допустимо хранить секретную информацию в файле: personal.auto.tfvars
3. "result": "jnOOKq5DKq12WKlk"
4. 
   ***первая ошибка:*** блок resource должен содержать обязательно два параметра, а именно тип объекта и уникальное имя в текущем проекте. Отсутствует имя.<br>
   ***вторая ошибка:*** уникальное имя должно начинаться с буквы и содержать буквы,символы подчёркивания, цифры, и символ тире. Начинается с цифры.<br>
   ***третья ошибка:*** переменная random_string_FAKE не объявлена. Вместо нее надо использовать random_string. А также также аргумент result 
5.  
  ```
resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "example_${random_password.random_string.result}"

  ports {
    internal = 80
    external = 9090
  }
}
  ```

<img width="1346" height="80" alt="Снимок экрана от 2025-08-19 14-33-02" src="https://github.com/user-attachments/assets/a2974bf2-205e-4e07-995d-7af1b8704f71" />

6. Опасность применения ключа заключается в том что изменения применяются без дополнительного одобрения (т.е. без ввода YES). Отсутствует дополнительный контроль при внесении изменений.
   Использование данного ключа -auto-approve может быть полезно в пайплайнах CI/CD при автоматизации процесса сборки, тестирования и развёртывания.

   <img width="1320" height="113" alt="Снимок экрана от 2025-08-19 15-27-07" src="https://github.com/user-attachments/assets/9c5064f5-11e8-4529-bec8-095ab78166fa" />

7.
```
cat terraform.tfstate
{
  "version": 4,
  "terraform_version": "1.9.8",
  "serial": 11,
  "lineage": "8f6e94f8-3c4d-c47e-c1d1-ff3ede566ffb",
  "outputs": {},
  "resources": [],
  "check_results": null
```

8. <br><b>Ответ: keep_locally = true</b> <br>
Атрибут keep_locally в ресурсе Terraform docker_image определяет, будет ли образ Docker, управляемый этим ресурсом, удален с локального хоста Docker при уничтожении ресурса Terraform.
Соответственно значение true атрибута keep_locally говорит нам, что образ nginx:latest останется на локальном хосте при уничтожении ресурсов с помощью terraform. Это может быть полезно
если от данного образа зависят другие процессы. 

Строка из документации:
keep_locally (Boolean) If true, then the Docker image won't be deleted on destroy operation. If this is false, it will delete the image from the docker local storage on destroy operation.
 
