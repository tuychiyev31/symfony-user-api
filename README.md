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


Albatta, mana to'liq kod va README.md ni bitta faylda ketma-ket qilib yozaman:

index.php fayli:
php
Copy code
<?php
header("Content-Type: application/json");

// Получение метода
$method = $_SERVER['REQUEST_METHOD'];
$endpoint = strtok($_SERVER['REQUEST_URI'], '?');

// CRUD операции для пользователя
switch ($endpoint) {
    case '/users':
        handleUserRequests($method);
        break;
    default:
        http_response_code(404);
        echo json_encode(['message' => 'Not Found']);
}

function getDbConnection() {
    $host = 'localhost';
    $db = 'your_database';
    $user = 'your_user';
    $pass = 'your_password';
    $dsn = "mysql:host=$host;dbname=$db;charset=utf8mb4";
    try {
        $pdo = new PDO($dsn, $user, $pass, [
            PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
            PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
        ]);
        return $pdo;
    } catch (PDOException $e) {
        echo json_encode(['error' => 'Не удалось подключиться к базе данных']);
        exit;
    }
}

function handleUserRequests($method) {
    switch ($method) {
        case 'POST':
            createUser();
            break;
        case 'PUT':
            updateUser();
            break;
        case 'DELETE':
            deleteUser();
            break;
        case 'GET':
            getUsers();
            break;
        default:
            http_response_code(405);
            echo json_encode(['message' => 'Метод не разрешен']);
    }
}

function createUser() {
    // Создание нового пользователя
    $data = json_decode(file_get_contents('php://input'), true);
    if (!isset($data['name'], $data['email'], $data['password'])) {
        http_response_code(400);
        echo json_encode(['message' => 'Неверный ввод']);
        return;
    }

    $conn = getDbConnection();
    $stmt = $conn->prepare("INSERT INTO users (name, email, password) VALUES (:name, :email, :password)");
    $stmt->execute([
        ':name' => $data['name'],
        ':email' => $data['email'],
        ':password' => password_hash($data['password'], PASSWORD_DEFAULT),
    ]);

    http_response_code(201);
    echo json_encode(['message' => 'Пользователь создан']);
}

function updateUser() {
    // Обновление информации пользователя
    $data = json_decode(file_get_contents('php://input'), true);
    if (!isset($data['id'], $data['name'], $data['email'])) {
        http_response_code(400);
        echo json_encode(['message' => 'Неверный ввод']);
        return;
    }

    $conn = getDbConnection();
    $stmt = $conn->prepare("UPDATE users SET name = :name, email = :email WHERE id = :id");
    $stmt->execute([
        ':id' => $data['id'],
        ':name' => $data['name'],
        ':email' => $data['email'],
    ]);

    echo json_encode(['message' => 'Информация о пользователе обновлена']);
}

function deleteUser() {
    // Удаление пользователя
    parse_str(file_get_contents('php://input'), $data);
    if (!isset($data['id'])) {
        http_response_code(400);
        echo json_encode(['message' => 'Неверный ввод']);
        return;
    }

    $conn = getDbConnection();
    $stmt = $conn->prepare("DELETE FROM users WHERE id = :id");
    $stmt->execute([':id' => $data['id']]);

    echo json_encode(['message' => 'Пользователь удален']);
}

function getUsers() {
    // Получение информации о пользователях
    $conn = getDbConnection();
    $stmt = $conn->query("SELECT id, name, email FROM users");
    $users = $stmt->fetchAll();

    echo json_encode($users);
}
README.md fayli:
markdown
Copy code
# API Документация

## Описание
Этот проект представляет собой простое REST API для работы с пользователями, реализованное на чистом PHP. 

## Установка
1. Клонируйте репозиторий:
   ```bash
   git clone https://github.com/ваш_пользователь/ваш_репозиторий.git
   cd ваш_репозиторий
Настройте подключение к базе данных. Откройте файл index.php и измените следующие параметры в функции getDbConnection в соответствии с вашими данными:
php
Copy code
$host = 'localhost'; // Хост базы данных
$db = 'your_database'; // Имя базы данных
$user = 'your_user'; // Имя пользователя базы данных
$pass = 'your_password'; // Пароль пользователя базы данных
Методы API
Создание пользователя
Endpoint: /users
Method: POST
Request Body:
json
Copy code
{
  "name": "string",
  "email": "string",
  "password": "string"
}
Response:
json
Copy code
{
  "message": "Пользователь создан"
}
Обновление информации пользователя
Endpoint: /users
Method: PUT
Request Body:
json
Copy code
{
  "id": "int",
  "name": "string",
  "email": "string"
}
Response:
json
Copy code
{
  "message": "Информация о пользователе обновлена"
}
Удаление пользователя
Endpoint: /users
Method: DELETE
Request Body:
json
Copy code
{
  "id": "int"
}
Response:
json
Copy code
{
  "message": "Пользователь удален"
}
Получение информации о пользователях
Endpoint: /users
Method: GET
Response:
json
Copy code
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
