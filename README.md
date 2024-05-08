
# Документация
## 1. CreateAddress - создание кошелька для пополнений.
## Путь
### POST "/create/in"
### Headers запроса
####  -- ApiPublic: string
####  -- Signature: string
### Тело запроса
```json
{
    "externalID": "string",                // Внешний уникальный идентификатор кошелька, номер счета
    "callbackURL": "string",               // Cсылка, по которой будет произведен get запрос при финальном статусе транзакции
    "chainType": "ChainType(string)"       // Тип сети
}
```
### Headers ответа
* Warning: string(опциональный хедер, приходит, если произошла ошибка)
### Тело ответа
```json
{
    "address": "string"     // Адрес кошелька
}
```

## 2. Withdraw - вывод средств.
## Путь
### POST "/create/out"
### Headers запроса
####  -- ApiPublic: string
####  -- Signature: string
### Тело запроса
```json
{
    "receiver": "string",               // Внутренний уникальный идентификатор кошелька, номер счета
    "amount": "float64",                // Cумма перевода
    "token": "string",                  // Название токена
    "externalID": "string",             // Ваш уникальный индетификатор транзакции
    "callbackURL": "string",            // Опциональное поле. Cсылка, по которой будет произведен get запрос при финальном статусе транзакции
    "chainType": "ChainType(string)"    // Название сети
}
```
### Headers ответа
* Warning: string(опциональный хедер, приходит, если произошла ошибка)
### Тело ответа
```json
{
    "trackerID": "string"   // Внутренний идентификатор транзакции
}
```

## 3. SelectTransactionsByLimit - постраничное отображение списка транзакций.
## Путь
### POST "/transaction/limit"
### Headers запроса
####  -- ApiPublic: string
####  -- Signature: string
### Тело запроса
```json
{
    "limit": 10,    // uint64, кол-во отображаемых транзакций
    "offset": 0     // uint64, номер страницы
}
```
### Headers ответа
* Warning: string(опциональный хедер, приходит, если произошла ошибка)
### Тело ответа
```json
[
    {
        "trackerID": "string",          // Внутренний идентификатор транзакции
        "externalID": "string",         // Ваш уникальный идентификатор транзакции
        "amount": "1.00",               // float64, сумма транзакции. У выводов отрицательная, а у пополнений - положительная
        "commission": "-0.15",          // float64, комиссия. Всегда отрицательная
        "chain": "string",              // Название сети, в которой была совершена транзакция
        "token": "string",              // Название токена
        "status": "string",             // Статус: SUCCESS, ACCEPTED, ERROR
        "hash": "string",               // Хэш транзакции. Уникальный идентификатор транзакции в блокчейне
        "receiver": "string"            // Кошелек получателя
    }
]
```

## 4. GetTransactionByID - получение транзакции по Вашему уникальному идентификатору.
## Путь
### POST "/transaction/id"
### Headers запроса
####  -- ApiPublic: string
####  -- Signature: string
### Тело запроса
```json
{
    "externalID": "string"   // Внешний идентификатор транзакции
}
```
### Headers ответа
* Warning: string(опциональный хедер, приходит, если произошла ошибка)
### Тело ответа
```json
{
[
    {
    "trackerID": "string",          // Внутренний идентификатор транзакции
    "externalID": "string",         // Ваш уникальный идентификатор транзакции
    "amount": "1.00",               // float64, сумма транзакции. У выводов отрицательная, а у пополнений - положительная
    "commission": "-0.15",          // float64, комиссия. Всегда отрицательная
    "chain": "string",              // Название сети, в которой была совершена транзакция
    "token": "string",              // Название токена
    "status": "string",             // Статус: SUCCESS, ACCEPTED, ERROR
    "hash": "string",               // Хэш транзакции. Уникальный идентификатор транзакции в блокчейне
    "receiver": "string"            // Кошелек получателя
    }
]
}
```
## Формирование сигнатуры
```
- func calcSignature(secret string, message string) string {
    mac := hmac.New(sha512.New, []byte(secret))
    mac.Write([]byte(message))
    return hex.EncodeToString(mac.Sum(nil))
}
```
Принимает на вход privateKey (он же secret), берет тело запроса, производит шифрование по формату sha512, возвращает готовый энкодер hmac, где ключем является privateKey.
После чего энкодит переведенное в байты тело запроса.


# Connect to payform

## 1. CreateOrder

## Endpoint

### POST "/api/order"

### Request header

#### -- ApiPublic: string

#### -- Signature: string

### Request body

```json
{
  "amount": "number",
  "redirectURL": "string",
  "header": "string",
  "callbackURL": "string",
  "externalID": "string"
}
```

### Response header

- Warning: string(optional header, appears if an error occurs)

### Response body

```json
{
  "redirectURL": "string"
}
```

## 2. Get order status

## Endpoint

### POST "/api/order/status"

### Request header

#### -- ApiPublic: string

#### -- Signature: string

### Response body

```json
{
  "externalID": "string"
}
```

### Response header

- Warning: string(optional header, appears if an error occurs)

### Response body

```json
{
  "externalID": "string",
  "amount": "number",
  "redirectURL": "string",
  "header": "string",
  "callbackURL": "string",
  "status": "string",
  "created": "string"
}
```
