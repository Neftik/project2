### Сервис Калькулятор

Этот проект представляет собой простой HTTP-сервер, который вычисляет математические выражения, предоставленные в формате JSON. Сервер поддерживает базовые арифметические операции, включая сложение, вычитание, умножение и деление, а также обработку некорректных входных данных и ошибок.

### Функционал
- Поддержка базовых арифметических выражений: `+`, `-`, `*`, `/` и скобок.
- Понятные сообщения об ошибках для некорректных выражений или неподдерживаемых операций.
- Обработка краевых случаев, таких как деление на ноль или несоответствующие скобки.
- Удобный API с использованием формата JSON для запросов и ответов.

### Использование API
#### Эндпоинт
**POST** `/api/v1/calculate`

#### Тело запроса
Отправьте JSON-объект со следующей структурой:
```json
{
  "expression": "<арифметическое выражение>"
}
```

#### Ответ
##### Успешно
- **HTTP Код состояния**: `200 OK`
- **Тело ответа**:
```json
{
  "result": "<результат вычисления>"
}
```

##### Ошибки
- **Неверный метод**:
  - **HTTP Код состояния**: `405 Method Not Allowed`
  - **Тело ответа**:
    ```json
    {
      "error": "Method not allowed"
    }
    ```

- **Неверное тело запроса**:
  - **HTTP Код состояния**: `400 Bad Request`
  - **Тело ответа**:
    ```json
    {
      "error": "Invalid request body"
    }
    ```

- **Пустое выражение**:
  - **HTTP Код состояния**: `400 Bad Request`
  - **Тело ответа**:
    ```json
    {
      "error": "Expression cannot be empty"
    }
    ```

- **Ошибка обработки**:
  - **HTTP Код состояния**: `422 Unprocessable Entity`
  - **Тело ответа**:
    ```json
    {
      "error": "<детальное сообщение об ошибке>"
    }
    ```

### Примеры
#### Успешное вычисление
```bash
curl --location 'http://localhost:8080/api/v1/calculate' --header 'Content-Type: application/json' --data '{
  "expression": "2+2*2"
}'
```
Ответ:
```json
{
  "result": "6"
}
```

#### Деление на ноль
```bash
curl --location 'http://localhost:8080/api/v1/calculate' --header 'Content-Type: application/json' --data '{
  "expression": "1/0"
}'
```
Ответ:
```json
{
  "error": "division by zero"
}
```

#### Некорректное выражение
```bash
curl --location 'http://localhost:8080/api/v1/calculate' --header 'Content-Type: application/json' --data '{
  "expression": "2+*2"
}'
```
Ответ:
```json
{
  "error": "processing error: invalid token '*'"
}
```

#### Пустое выражение
```bash
curl --location 'http://localhost:8080/api/v1/calculate' --header 'Content-Type: application/json' --data '{
  "expression": ""
}'
```
Ответ:
```json
{
  "error": "Expression cannot be empty"
}
```

### Как запустить
#### Требования
- Установленный Go (версия 1.18 или выше).

#### Шаги
1. Клонируйте репозиторий:
   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. Запустите сервис:
   ```bash
   go run ./...
   ```

3. Сервер запустится на `http://localhost:8080`.

4. Используйте инструменты, такие как curl или Postman, для взаимодействия с API.

### Тестирование сервиса
#### Примеры сценариев
- **Корректное выражение**:
  ```bash
  curl --location 'http://localhost:8080/api/v1/calculate'   --header 'Content-Type: application/json'   --data '{
    "expression": "3+(4*5)-7"
  }'
  ```

- **Некорректный токен**:
  ```bash
  curl --location 'http://localhost:8080/api/v1/calculate'   --header 'Content-Type: application/json'   --data '{
    "expression": "3+abc"
  }'
  ```

- **Несоответствующие скобки**:
  ```bash
  curl --location 'http://localhost:8080/api/v1/calculate'   --header 'Content-Type: application/json'   --data '{
    "expression": "(3+5"
  }'
  ```


### Как запустить тесты
#### Требования
- Установленный Go.

#### Шаги
1. Перейдите в корневую директорию проекта:
   ```bash
   cd <repository-directory>
   ```

2. Запустите тесты с помощью команды:
   ```bash
   go test ./...
   ```

3. Результаты выполнения тестов отобразятся в консоли. В случае успешного прохождения будет выведено `ok`, а в случае ошибок — подробная информация о причинах их возникновения.
