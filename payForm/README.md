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
