## 💸 4. Transfer Service — Transaction API (Методы управления и аудита транзакций)

Микросервис для работы с историей транзакций, переводами и аналитикой. Все запросы проходят через API Gateway на порту `8080`. В успешных ответах возвращаются данные на основе структуры `TransactionConvert`.

---

### 1) Получить полный список транзакций (Get Transaction List)
Возвращает глобальный список всех транзакций в системе. Обычно используется администраторами или сервисами аналитики.

* **Метод:** `GET`
* **Путь через Gateway:** `http://localhost:8080/list`
* **Тело запроса:** отсутствует
* **Успешный ответ (`200 OK — List<TransactionConvert>`):**
  ```json
  [
    {
      "transferId": "a1b2c3d4-e5f6-7a8b-9c0d-1e2f3a4b5c6d",
      "userId": "456e4567-e89b-12d3-a456-426614174111",
      "fromAccId": "111e4567-e89b-12d3-a456-426614174001",
      "toAccId": "222e4567-e89b-12d3-a456-426614174002",
      "amount": 5000.00,
      "currency": "RUB",
      "transactionType": "TRANSFER",
      "transactionStatus": "SUCCESS",
      "timestamp": "2026-06-07T12:00:00"
    },
    {
      "transferId": "f4g5h6i7-j8k9-0l1m-2n3o-4p5q6r7s8t9u",
      "userId": "456e4567-e89b-12d3-a456-426614174111",
      "fromAccId": "111e4567-e89b-12d3-a456-426614174001",
      "toAccId": null,
      "amount": 1000.00,
      "currency": "RUB",
      "transactionType": "WITHDRAW",
      "transactionStatus": "SUCCESS",
      "timestamp": "2026-06-07T13:15:30"
    }
  ]
  ```

### 2) Просмотр транзакции по её ID (View Transaction By ID)
Возвращает детальную информацию о конкретной транзакции по её уникальному идентификатору `tranId`.

* **Метод:** `GET`
* **Путь через Gateway:** `http://localhost:8080/viewTransactionById`
* **Request Parameters:** `tranId`
* **Тело запроса:** отсутствует
* **Успешный ответ (`200 OK — TransactionConvert`):**
  ```json
  {
    "transferId": "a1b2c3d4-e5f6-7a8b-9c0d-1e2f3a4b5c6d",
    "userId": "456e4567-e89b-12d3-a456-426614174111",
    "fromAccId": "111e4567-e89b-12d3-a456-426614174001",
    "toAccId": "222e4567-e89b-12d3-a456-426614174002",
    "amount": 5000.00,
    "currency": "RUB",
    "transactionType": "TRANSFER",
    "transactionStatus": "SUCCESS",
    "timestamp": "2026-06-07T12:00:00"
  }
  ```

### 3) Просмотр транзакций по ID пользователя (View Transactions By User ID)
Возвращает список всех транзакций, совершенных конкретным пользователем. *(Требует проверки прав доступа на уровне Gateway, чтобы пользователь мог запрашивать только свою историю).*

* **Метод:** `GET`
* **Путь через Gateway:** `http://localhost:8080/viewTransactionsByUserId`
* **Request Parameters:** `userId`
* **Тело запроса:** отсутствует
* **Успешный ответ (`200 OK — List<TransactionConvert>`):**
  ```json
  [
    {
      "transferId": "a1b2c3d4-e5f6-7a8b-9c0d-1e2f3a4b5c6d",
      "userId": "456e4567-e89b-12d3-a456-426614174111",
      "fromAccId": "111e4567-e89b-12d3-a456-426614174001",
      "toAccId": "222e4567-e89b-12d3-a456-426614174002",
      "amount": 5000.00,
      "currency": "RUB",
      "transactionType": "TRANSFER",
      "transactionStatus": "SUCCESS",
      "timestamp": "2026-06-07T12:00:00"
    }
  ]
  ```

### 4) Просмотр транзакций по ID аккаунта (View Transactions By Account ID)
Возвращает список всех транзакций (как входящих, так и исходящих), связанных с конкретным банковским счетом.

* **Метод:** `GET`
* **Путь через Gateway:** `http://localhost:8080/viewTransactionsByAccountId`
* **Request Parameters:** `accountId`
* **Тело запроса:** отсутствует
* **Успешный ответ (`200 OK — List<TransactionConvert>`):**
  ```json
  [
    {
      "transferId": "a1b2c3d4-e5f6-7a8b-9c0d-1e2f3a4b5c6d",
      "userId": "456e4567-e89b-12d3-a456-426614174111",
      "fromAccId": "111e4567-e89b-12d3-a456-426614174001",
      "toAccId": "222e4567-e89b-12d3-a456-426614174002",
      "amount": 5000.00,
      "currency": "RUB",
      "transactionType": "TRANSFER",
      "transactionStatus": "SUCCESS",
      "timestamp": "2026-06-07T12:00:00"
    }
  ]
  ```

### 5) Сортировка транзакций от новых к старым (View Transactions From New To Old)
Возвращает глобальный список всех транзакций, отсортированных по времени создания в порядке убывания (сначала самые свежие).

* **Метод:** `GET`
* **Путь через Gateway:** `http://localhost:8080/viewTransactionsFromNewToOld`
* **Тело запроса:** отсутствует
* **Успешный ответ (`200 OK — List<TransactionConvert>`):**
  ```json
  [
    {
      "transferId": "f4g5h6i7-j8k9-0l1m-2n3o-4p5q6r7s8t9u",
      "userId": "456e4567-e89b-12d3-a456-426614174111",
      "fromAccId": "111e4567-e89b-12d3-a456-426614174001",
      "toAccId": null,
      "amount": 1000.00,
      "currency": "RUB",
      "transactionType": "WITHDRAW",
      "transactionStatus": "SUCCESS",
      "timestamp": "2026-06-07T13:15:30"
    },
    {
      "transferId": "a1b2c3d4-e5f6-7a8b-9c0d-1e2f3a4b5c6d",
      "userId": "456e4567-e89b-12d3-a456-426614174111",
      "fromAccId": "111e4567-e89b-12d3-a456-426614174001",
      "toAccId": "222e4567-e89b-12d3-a456-426614174002",
      "amount": 5000.00,
      "currency": "RUB",
      "transactionType": "TRANSFER",
      "transactionStatus": "SUCCESS",
      "timestamp": "2026-06-07T12:00:00"
    }
  ]
  ```

### 6) Сортировка транзакций от старых к новым (View Transactions From Old To New)
Возвращает глобальный список всех транзакций, отсортированных по времени создания в хронологическом порядке (сначала самые ранние).

* **Метод:** `GET`
* **Путь через Gateway:** `http://localhost:8080/viewTransactionsFromOldToNew`
* **Тело запроса:** отсутствует
* **Успешный ответ (`200 OK — List<TransactionConvert>`):**
  ```json
  [
    {
      "transferId": "a1b2c3d4-e5f6-7a8b-9c0d-1e2f3a4b5c6d",
      "userId": "456e4567-e89b-12d3-a456-426614174111",
      "fromAccId": "111e4567-e89b-12d3-a456-426614174001",
      "toAccId": "222e4567-e89b-12d3-a456-426614174002",
      "amount": 5000.00,
      "currency": "RUB",
      "transactionType": "TRANSFER",
      "transactionStatus": "SUCCESS",
      "timestamp": "2026-06-07T12:00:00"
    },
    {
      "transferId": "f4g5h6i7-j8k9-0l1m-2n3o-4p5q6r7s8t9u",
      "userId": "456e4567-e89b-12d3-a456-426614174111",
      "fromAccId": "111e4567-e89b-12d3-a456-426614174001",
      "toAccId": null,
      "amount": 1000.00,
      "currency": "RUB",
      "transactionType": "WITHDRAW",
      "transactionStatus": "SUCCESS",
      "timestamp": "2026-06-07T13:15:30"
    }
  ]
  ```

### 7) Получить сумму всех переводов пользователя (Get Amount of User Transfers)
Возвращает суммарный объем (оборот) всех денежных переводов конкретного пользователя, переданного через Query-параметр `userId`. *(Требует проверки прав доступа на уровне Gateway, чтобы пользователь мог запрашивать аналитику только по своему аккаунту).*

* **Метод:** `GET`
* **Путь через Gateway:** `http://localhost:8080/viewAmountUserTransfers`
* **Request Parameters:** `userId`
* **Пример полного пути:** `http://localhost:8080/viewAmountUserTransfers?userId=456e4567-e89b-12d3-a456-426614174111`
* **Тело запроса:** отсутствует
* **Успешный ответ (`200 OK — AmountUserRequest`):**
  ```json
  {
    "amount": 154300.50,
    "currency": "RUB"
  }
  ```

### 8) Получить транзакции по их статусу (Get Transactions By Status)
Возвращает список всех транзакций в системе, отфильтрованных по переданному статусу (например, `SUCCESS`, `FAILED`, `PENDING`).

* **Метод:** `POST`
* **Путь через Gateway:** `http://localhost:8080/viewTransactionsByTranStatus`
* **Тело запроса (`TransactionStatusRequest` JSON):**
  ```json
  {
    "transactionStatusType": "SUCCESS"
  }
  ```
* **Успешный ответ (`200 OK — List<TransactionConvert>`):**
  ```json
  {
    "transferId": "a1b2c3d4-e5f6-7a8b-9c0d-1e2f3a4b5c6d",
    "userId": "456e4567-e89b-12d3-a456-426614174111",
    "fromAccId": "111e4567-e89b-12d3-a456-426614174001",
    "toAccId": "222e4567-e89b-12d3-a456-426614174002",
    "amount": 5000.00,
    "currency": "RUB",
    "transactionType": "TRANSFER",
    "transactionStatus": "SUCCESS",
    "timestamp": "2026-06-07T12:00:00"
  }
  ```

### 9) Фильтрация транзакций по виду операции (Get Transactions By Operation Type)
Возвращает список всех транзакций в системе, отфильтрованных по типу финансовой операции (например, `DEPOSIT`, `TRANSFER`, `WITHDRAW`).

* **Метод:** `POST`
* **Путь через Gateway:** `http://localhost:8080/viewTransactionsByOperationType`
* **Тело запроса (`TransactionOperationTypeRequest` JSON):**
  ```json
  {
    "transactionType": "TRANSFER"
  }
  ```
* **Успешный ответ (`200 OK — List<TransactionConvert>`):**
```json
  [
    {
      "transferId": "a1b2c3d4-e5f6-7a8b-9c0d-1e2f3a4b5c6d",
      "userId": "456e4567-e89b-12d3-a456-426614174111",
      "fromAccId": "111e4567-e89b-12d3-a456-426614174001",
      "toAccId": "222e4567-e89b-12d3-a456-426614174002",
      "amount": 5000.00,
      "currency": "RUB",
      "transactionType": "TRANSFER",
      "transactionStatus": "SUCCESS",
      "timestamp": "2026-06-07T12:00:00"
    }
  ]
  ```

### 10) Просмотр транзакций за определенный период (View Transactions Over Time)
Возвращает список транзакций, совершенных за выбранный промежуток времени (например, за последний час, день, неделю, месяц или год).

* **Метод:** `POST`
* **Путь через Gateway:** `http://localhost:8080/viewTransactionOverTime`
* **Тело запроса (`TransactionPeriod` JSON Enum):**
  ```json
    "MONTH"
  ```
* **Успешный ответ (`200 OK — List<TransactionConvert>`):**
```json
  [
    {
      "transferId": "a1b2c3d4-e5f6-7a8b-9c0d-1e2f3a4b5c6d",
      "userId": "456e4567-e89b-12d3-a456-426614174111",
      "fromAccId": "111e4567-e89b-12d3-a456-426614174001",
      "toAccId": "222e4567-e89b-12d3-a456-426614174002",
      "amount": 5000.00,
      "currency": "RUB",
      "transactionType": "TRANSFER",
      "transactionStatus": "SUCCESS",
      "timestamp": "2026-06-07T12:00:00"
    }
  ]
  ```
