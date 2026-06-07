### 💳 1. Account Service (Управление счетами)

Микросервис отвечает за создание банковских аккаунтов/счетов пользователей и предоставление информации по ним. Все запросы проходят через API Gateway.

#### 1) Создать новый аккаунт
* **Метод:** `POST`
* **Путь:** `/account/create`
* **Полный путь через Gateway:** `http://localhost:8765/account/create`
* **Тело запроса (`CreateAcc` JSON):**
  ```json
  {
    "userId": "456e4567-e89b-12d3-a456-426614174111",
    "balance": 0.00,
    "currency": "RUB" 
  }
  ```
#### 2) Посмотреть список моих аккаунтов
* **Метод:** `POST` 
* **Путь:** `/account/myAccounts`
* **Полный путь (прямой):** `http://localhost:8080/account/myAccounts`
* **Заголовки (Headers):**
  * `X-User-Id: <UUID>` (Идентификатор пользователя, автоматически прокидываемый шлюзом Gateway)
* **Тело запроса:** отсутствует
* **Успешный ответ (`200 OK` — `List<AccountMap>`):**
  ```json
  [
    {
      "accountId": "111e4567-e89b-12d3-a456-426614174001",
      "userId": "456e4567-e89b-12d3-a456-426614174111",
      "balance": 0.00,
      "currency": "RUB",
      "status": "PENDING_VERIFICATION",
      "timestamp": "2026-06-05T13:30:00"
    },
    {
      "accountId": "222e4567-e89b-12d3-a456-426614174002",
      "userId": "456e4567-e89b-12d3-a456-426614174111",
      "balance": 25000.50,
      "currency": "USD",
      "status": "ACTIVE",
      "timestamp": "2026-06-06T10:15:23"
    }
  ]
  ```

### 🔑 2. Account Service — Admin API (Административные методы)

Эндпоинты для управления и аудита аккаунтов пользователей. Доступны только пользователям с правами администратора. Все запросы идут через API Gateway на порту `8080`.

---

#### 1) Посмотреть баланс аккаунта пользователя
* **Метод:** `POST`
* **Путь через Gateway:** `http://localhost:8080/account/admin/balance`
* **Тело запроса (`UuidIdRequest` JSON):**
  ```json
  {
    "id": "111e4567-e89b-12d3-a456-426614174001"
  }
  ```
* **Успешный ответ (`200 OK — AccountBalanceDto`):**
  ```json
  {
    "balance": 150050.75,
    "currency": "RUB"
  }
  ```

#### 2) Просмотр полных данных аккаунта пользователя
* **Метод:** `POST`
* **Путь через Gateway:** `http://localhost:8080/account/admin/data`
* **Тело запроса (`UuidIdRequest` JSON):**
  ```json
  {
    "id": "111e4567-e89b-12d3-a456-426614174001"
  }
  ```
* **Успешный ответ (`200 OK — AccountMap`):**
  ```json
  {
    "accountId": "111e4567-e89b-12d3-a456-426614174001",
    "userId": "456e4567-e89b-12d3-a456-426614174111",
    "balance": 150050.75,
    "currency": "RUB",
    "status": "ACTIVE",
    "timestamp": "2026-06-05T13:30:00"
  }
  ```
