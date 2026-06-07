### 🔐 2. Auth Service
### 1. Admin API (Методы администратора авторизации)

Эндпоинты для управления криптографическими ключами и конфиденциальными данными системы аутентификации. Все запросы идут через API Gateway на порту `8080`.

---

### 1) Просмотр секретного ключа (View Secret Key)
Возвращает текущий секретный ключ системы после успешной проверки учетных данных администратора.

* **Метод:** `POST`
* **Путь через Gateway:** `http://localhost:8080/admins/view/secretKey`
* **Тело запроса (`ViewSecretKeyDto` JSON):**
  ```json
  {
    "email": "admin@bank.com",
    "password": "super_secure_password"
  }
  ```
* **Успешный ответ (`200 OK — SecretKeyDto`):**
  ```json
  {
    "value": "b9QJrK9R1Z5t8yF2uH3nA4sD7xL6vB1cE9mT2Wq8YzU=",
    "timestamp": "2026-06-07T14:55:00"
  }
  ```

### 2) Получение публичного ключа (Get Public Key)
Выгружает файл публичного RSA-ключа (`public.pem`) из ресурсов приложения для проверки подписей токенов.

* **Метод:** `GET`
* **Путь через Gateway:** `http://localhost:8080/admins/public-key`
* **Тело запроса:** отсутствует
* **Успешный ответ (`200 OK` — `text/plain` или `application/x-pem-file`):**
  ```text
  -----BEGIN PUBLIC KEY-----
  MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA0M3N6m...
  -----END PUBLIC KEY-----
  ```

### 3) Ручная генерация секретного ключа (Manual Generate Secret Key)
Принудительно запускает процесс генерации нового секретного ключа в системе через соответствующий сервис.

* **Метод:** `GET`
* **Путь через Gateway:** `http://localhost:8080/admins/generate/secretKey`
* **Тело запроса:** отсутствует
* **Успешный ответ (`200 OK`):** `Manual generate secretKey successfully`


### 2. Public API (Публичные методы авторизации)

Эндпоинты для регистрации и аутентификации различных ролей пользователей (Admin, Manager, User). 

---

### 1) Регистрация администратора (Register Admin)
Регистрирует новый аккаунт администратора в системе. Для успешной регистрации обязательно передать валидный секретный ключ системы (`secretKey`).

* **Метод:** `POST`
* **Путь через Gateway:** `http://localhost:8080/auth/register/admin`
* **Тело запроса (`AdminRegisterRequest` JSON):**
  ```json
  {
    "secretKey": "b9QJrK9R1Z5t8yF2uH3nA4sD7xL6vB1cE9mT2Wq8YzU=",
    "username": "chief_admin",
    "password": "strongpassword123",
    "email": "admin@bank.com"
  }
  ```
* **Успешный ответ (`200 OK — JWT Tokens Map`):**
  ```json
  {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
  ```

### 2) Аутентификация администратора (Login Admin)
Выполняет вход в систему под учетной записью администратора и выдает пару JWT-токенов. Для успешного входа необходимо передать валидный `secretKey`.

* **Метод:** `POST`
* **Путь через Gateway:** `http://localhost:8080/auth/login/admin`
* **Тело запроса (`AdminLoginRequest` JSON):**
  ```json
  {
    "secretKey": "b9QJrK9R1Z5t8yF2uH3nA4sD7xL6vB1cE9mT2Wq8YzU=",
    "email": "admin@bank.com",
    "password": "strongpassword123"
  }
  ```
* **Успешный ответ (`200 OK — JWT Tokens Map`):**
  ```json
  {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
  ```

### 3) Регистрация менеджера (Register Manager)
Регистрирует новый аккаунт менеджера в системе. Для успешной регистрации также требуется передать валидный секретный ключ системы (`secretKey`).

* **Метод:** `POST`
* **Путь через Gateway:** `http://localhost:8080/auth/register/manager`
* **Тело запроса (`ManagerRegisterRequest` JSON):**
  ```json
  {
    "secretKey": "b9QJrK9R1Z5t8yF2uH3nA4sD7xL6vB1cE9mT2Wq8YzU=",
    "username": "lead_manager",
    "password": "managerpassword123",
    "email": "manager@bank.com"
  }
  ```
* **Успешный ответ (`200 OK — JWT Tokens Map`):**
  ```json
  {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
  ```

### 4) Аутентификация манаджера (Login Manager)
Выполняет вход в систему под учетной записью менеджера и выдает пару JWT-токенов. Для успешного входа необходимо передать валидный `secretKey`.

* **Метод:** `POST`
* **Путь через Gateway:** `http://localhost:8080/auth/login/manager`
* **Тело запроса (`ManagerLoginRequest` JSON):**
  ```json
  {
    "secretKey": "b9QJrK9R1Z5t8yF2uH3nA4sD7xL6vB1cE9mT2Wq8YzU=",
    "email": "manager@bank.com",
    "password": "strongpassword123"
  }
  ```
* **Успешный ответ (`200 OK — JWT Tokens Map`):**
  ```json
  {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
  ```  

### 5) Регистрация пользователя (Register User)
Регистрирует новый аккаунт пользователя в системе.

* **Метод:** `POST`
* **Путь через Gateway:** `http://localhost:8080/auth/register/user`
* **Тело запроса (`UserRegisterRequest` JSON):**
  ```json
  {
    "username": "romanuser",
    "password": "strongpassword123",
    "email": "admin@bank.com"
  }
  ```
* **Успешный ответ (`200 OK — JWT Tokens Map`):**
  ```json
  {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
  ```

### 6) Аутентификация пользователя (Login User)
Выполняет вход в систему под учетной записью пользователя и выдает пару JWT-токенов. 

* **Метод:** `POST`
* **Путь через Gateway:** `http://localhost:8080/auth/login/user`
* **Тело запроса (`UserLoginRequest` JSON):**
  ```json
  {
    "email": "user@bank.com",
    "password": "strongpassword123"
  }
  ```
* **Успешный ответ (`200 OK — JWT Tokens Map`):**
  ```json
  {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
  ```

