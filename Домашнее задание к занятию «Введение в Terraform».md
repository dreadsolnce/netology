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

6. 
