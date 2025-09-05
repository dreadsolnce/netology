# Задание 1

TFLinit:<br>
  тип ошибок 1: terraform_required_providers  - отсутствие версии провайдера<br>
  тип ошибок 2: terraform_unused_declarations - неиспользуемы переменные в проекте<br>
  тип ошибок 3: terraform_module_pinned_source - использование дефолтной ветки в источнике для модуля<br>

checkov:
  тип ошибок 1: Ensure Terraform module sources use a commit hash
  тип ошибок 2: Ensure Terraform module sources use a tag with a version number
  
