[README.md](https://github.com/user-attachments/files/22259753/README.md)
# Qiwi Payout API — Тестовое задание

## 📌 Описание
Цель задания — подготовить тесты для проверки работы Qiwi Payout API по документации:  
https://developer.qiwi.com/ru/payout/v1/#about  

⚠️ Важно: сервис **нерабочий**. Все запросы в реальности будут возвращать ошибки (`401` или `403`).  
Задача — показать умение работать с документацией API и формировать корректные запросы.  

---

## 🔹 Проверяемые сценарии

### 1. Доступность сервиса
- **Цель**: убедиться, что сервис отвечает и возвращает JSON-ответ.  
- **Метод**: `GET`  
- **URL**: 'https://api-test.qiwi.com/partner/payout/v1/agents/{{agentId}}/points/{{pointId}}/payments' 
- **Заголовки**:  
  - `Authorization: Bearer {{token}}`  
  - `Accept: application/json`  
- **Ожидаемый результат**: сервис отвечает (код < 500), ответ в формате JSON.

---

### 2. Запрос баланса
- **Цель**: проверить, что баланс всегда больше 0.  
- **Метод**: `GET`  
- **URL**: 'https://api-test.qiwi.com/partner/payout/v1/agents/{{agentId}}/points/{{pointId}}/balance'
- **Заголовки**:  
  - `Authorization: Bearer {{token}}`  
  - `Accept: application/json`  
- **Что проверять**:  
  - В ответе есть объект `accounts`.  
  - У первого аккаунта `balance.amount > 0`.  

---

### 3. Создание платежа (1 RUB)
- **Цель**: проверить создание платежа.  
- **Метод**: `POST`  
- **URL**: 'https://api-test.qiwi.com/partner/payout/v1/agents/{{agentId}}/points/{{pointId}}/payments/{{paymentId}}'
- **Заголовки**:  
  - `Authorization: Bearer {{token}}`  
  - `Content-Type: application/json`  
- **Тело запроса**:
  ```json
  {
    "sum": { "amount": 1, "currency": "RUB" },
    "paymentMethod": { "type": "Account", "accountId": "643" },
    "comment": "Test payment"
  }
 - **Что проверять**:
 -  Код ответа 200 или 201.
 -  В теле ответа есть уникальный id платежа.
 
 ---
 
 ### 4. Исполнение платежа
 - Цель: перевести созданный платёж в статус «исполнен».
 - Метод: POST
 - URL: {{base_url}}/payments/{{paymentId}}/execute (где {{paymentId}} — значение id, полученное в шаге 3)
 - **Заголовки**:
 - 'Authorization: Bearer {{api_token}}'
 - 'Content-Type: application/json'
 - Тело: {} (или пустое — см. документацию)
 - **Что проверять**:
 - В ответе есть поле status.
 - Значение status = SUCCESS (или аналогичное по документации).
