# my-api

# API Документация

## Описание
Этот проект представляет собой простое REST API для работы с пользователями, реализованное на чистом PHP. 

## Установка
1. Клонируйте репозиторий:
   ```bash
   https://github.com/tuychiyev31/symfony-user-api.git
   cd symfony-user-api


Настройте подключение к базе данных. Откройте файл index.php и измените следующие параметры в функции getDbConnection в соответствии с вашими данными:
php
Copy code
$host = 'localhost'; // Хост базы данных
$db = 'your_database'; // Имя базы данных
$user = 'your_user'; // Имя пользователя базы данных
$pass = 'your_password'; // Пароль пользователя базы данных

Создание пользователя
{
  "name": "string",
  "email": "string",
  "password": "string"
}
Обновление информации пользователя
Endpoint: /users
Method: PUT
Request Body:
json

{
  "id": "int",
  "name": "string",
  "email": "string"
}
Response:
json
{
  "message": "Информация о пользователе обновлена"
}
Удаление пользователя
Endpoint: /users
Method: DELETE
Request Body:
json

{
  "id": "int"
}
Response:
json
{
  "message": "Пользователь удален"
}

Получение информации о пользователях
Endpoint: /users
Method: GET
Response:
json
[
  {
    "id": "int",
    "name": "string",
    "email": "string"
  }
]

Ответы сервера: Все ответы сервера возвращаются в формате JSON с соответствующим HTTP статусом.
Безопасность паролей: Пароли хранятся в зашифрованном виде с использованием функции password_hash.
Контакты
Для вопросов и предложений, пожалуйста, обращайтесь по адресу электронной почты: tuuychiyevabdulla5@gmail.com.
