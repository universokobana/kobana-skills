# Kobana Charge API - Complete Reference

Complete API documentation for managing Pix charges, accounts, commands, and payments.

## Base URLs

| Environment | URL |
|-------------|-----|
| Production | `https://api.kobana.com.br` |
| Sandbox | `https://api-sandbox.kobana.com.br` |

## Authentication

```
Authorization: Bearer {your_api_token}
```

## Common Headers

| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | Yes | Bearer token |
| `Content-Type` | Yes | `application/json` |
| `User-Agent` | No | Valid email for contact |
| `X-Idempotency-Key` | No | Prevents replay processing |

---

## Pix Accounts

### List Pix Accounts
```
GET /v2/charge/pix_accounts
```

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `page` | integer | Page number |
| `per_page` | integer | Items per page |

### Create Pix Account
```
POST /v2/charge/pix_accounts
```

**Request Body:**
```json
{
  "financial_provider_slug": "banco_do_brasil",
  "key": "12345678901",
  "key_type": "cpf",
  "beneficiary": {
    "document": "12345678901234",
    "name": "Company LTDA",
    "address": {
      "city": "São Paulo",
      "state": "SP"
    }
  }
}
```

### Get Pix Account
```
GET /v2/charge/pix_accounts/{uid}
```

### Update Pix Account
```
PUT /v2/charge/pix_accounts/{uid}
```

### Delete Pix Account
```
DELETE /v2/charge/pix_accounts/{uid}
```

---

## Pix Charges

### List Pix Charges
```
GET /v2/charge/pix
```

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `page` | integer | Page number |
| `per_page` | integer | Items per page (default 50) |
| `status` | string | opened, paid, overdue, canceled |
| `registration_status` | string | pending, confirmed, rejected, failed |
| `pix_account_uid` | string | Filter by Pix account |
| `created_from` | date | Created date from (YYYY-MM-DD) |
| `created_to` | date | Created date to |
| `expire_from` | date | Expire date from |
| `expire_to` | date | Expire date to |
| `paid_from` | date | Paid date from |
| `paid_to` | date | Paid date to |
| `txid` | string | Filter by TXID |
| `external_id` | string | Filter by external ID |
| `payer_document_number` | string | Filter by payer CPF/CNPJ |
| `tags` | string | Filter by tags (comma-separated) |

### Create Pix Charge
```
POST /v2/charge/pix
```

**Required Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `amount` | decimal | Value in BRL (minimum 0.01) |
| `pix_account_uid` | string | UUID of the Pix Account |
| `expire_at` | string | Expiration date (ISO 8601) |

**Optional Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `registration_kind` | string | `instant` (default) or `billing` |
| `payer` | object | Payer information |
| `fine_type` | string | `percentage`, `amount`, or `none` |
| `fine_percentage` | decimal | Fine percentage |
| `fine_amount` | decimal | Fine amount |
| `interest_type` | string | Interest type (see below) |
| `interest_percentage` | decimal | Interest percentage |
| `interest_amount` | decimal | Interest amount |
| `discount_type` | string | Discount type (see below) |
| `discount_first_percentage` | decimal | First discount percentage |
| `discount_first_amount` | decimal | First discount amount |
| `discount_first_days` | integer | Days for first discount |
| `discount_second_*` | - | Second discount (optional) |
| `discount_third_*` | - | Third discount (optional) |
| `reduction_type` | string | `percentage`, `amount`, or `none` |
| `reduction_percentage` | decimal | Reduction percentage |
| `reduction_amount` | decimal | Reduction amount |
| `revoke_days` | integer | Days active after due date (1-365) |
| `message` | string | Message to payer (max 140 chars) |
| `additional_info` | object | Up to 10 key-value pairs |
| `custom_data` | object | Custom metadata (JSON) |
| `external_id` | string | External ID for tracking |
| `tags` | array | Tags for organization |
| `txid` | string | Custom TXID (1-35 chars) |
| `ignore_whatsapp` | boolean | Never send via WhatsApp |
| `password_protected_mode` | integer | 0=disabled, 1=last 4, 2=first 5 |

**Payer Object:**
```json
{
  "payer": {
    "document_number": "111.321.322-09",
    "name": "João da Silva",
    "email": "joao@example.com",
    "address": {
      "street": "Rua das Flores",
      "number": "123",
      "complement": "Apt 101",
      "neighborhood": "Centro",
      "city_name": "São Paulo",
      "state": "SP",
      "zip_code": "01234-567"
    }
  }
}
```

**Interest Types:**
| Type | Description |
|------|-------------|
| `daily_amount_calendar` | Daily value (calendar days) |
| `daily_percentage_calendar` | Daily percentage (calendar days) |
| `monthly_percentage_calendar` | Monthly percentage (calendar days) |
| `yearly_percentage_calendar` | Yearly percentage (calendar days) |
| `daily_amount_business` | Daily value (business days) |
| `daily_percentage_business` | Daily percentage (business days) |
| `monthly_percentage_business` | Monthly percentage (business days) |
| `yearly_percentage_business` | Yearly percentage (business days) |

**Discount Types:**
| Type | Description |
|------|-------------|
| `fixed_amount` | Fixed value |
| `fixed_percentage` | Fixed percentage |
| `advance_amount_calendar` | Value per advance (calendar days) |
| `advance_amount_business` | Value per advance (business days) |
| `advance_percentage_calendar` | Percentage per advance (calendar days) |
| `advance_percentage_business` | Percentage per advance (business days) |

### Get Pix Charge
```
GET /v2/charge/pix/{uid}
```

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `field` | string | Alternative field: `id`, `external_id`, `txid` |

### Update Pix Charge
```
PUT /v2/charge/pix/{uid}/update
```

**Request Body:**
```json
{
  "amount": 550.00,
  "expire_at": "2026-02-15T23:59:59-03:00",
  "payer": {
    "name": "New Name"
  }
}
```

### Delete Pix Charge
```
DELETE /v2/charge/pix/{uid}
```

Only canceled charges can be deleted.

### Cancel Pix Charge
```
POST /v2/charge/pix/{uid}/cancel
```

---

## Pix Commands

Commands represent operations executed on Pix charges (registration, updates, etc.).

### List Commands
```
GET /v2/charge/pix/{pix_uid}/commands
```

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `page` | integer | Page number |
| `per_page` | integer | Items per page |

### Get Command
```
GET /v2/charge/pix/{pix_uid}/commands/{id}
```

---

## Charge Payments

Payments received for charges.

### List Payments
```
GET /v2/charge/payments
```

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `page` | integer | Page number |
| `per_page` | integer | Items per page |
| `pix_uid` | string | Filter by Pix charge |

### Create Payment
```
POST /v2/charge/payments
```

### Get Payment
```
GET /v2/charge/payments/{uid}
```

### Delete Payment
```
DELETE /v2/charge/payments/{uid}
```

---

## Response Structure

### Success Response (201 Created)
```json
{
  "status": 201,
  "data": {
    "uid": "019c0cbe-f018-717e-be6e-...",
    "txid": null,
    "status": "opened",
    "registration_status": "pending",
    "pix_account_uid": "019c0cbe-eff8-7a25-...",
    "kind": "instant",

    "created_at": "2026-01-30T11:32:45-03:00",
    "updated_at": "2026-01-30T11:32:45-03:00",
    "expire_at": "2026-02-09T11:32:45-03:00",
    "canceled_at": null,
    "registered_at": null,
    "paid_at": null,

    "amount": 100.5,
    "paid_amount": null,

    "qrcode": {
      "emv": null,
      "png": null
    },

    "payer": {
      "document_number": "111.321.322-09",
      "name": "João da Silva",
      "emails": [{"address": "joao@example.com"}]
    },

    "url": "https://kdoc.to/1/WYOuEJbUjXLk",

    "formats": {
      "default": {
        "html": "https://kdoc.to/1/WYOuEJbUjXLk",
        "png": "https://kdoc.to/1/WYOuEJbUjXLk.png",
        "pdf": "https://kdoc.to/1/WYOuEJbUjXLk.pdf"
      },
      "qrcode": {
        "html": "https://kdoc.to/1/WYOuEJbUjXLk/qrcode",
        "png": "https://kdoc.to/1/WYOuEJbUjXLk/qrcode.png",
        "pdf": "https://kdoc.to/1/WYOuEJbUjXLk/qrcode.pdf"
      }
    },

    "interest": {
      "type": null,
      "amount": null,
      "percentage": null
    },

    "discounts": {
      "type": null,
      "discount_first": {"amount": null, "percentage": null, "days": null},
      "discount_second": {"amount": null, "percentage": null, "days": null},
      "discount_third": {"amount": null, "percentage": null, "days": null}
    },

    "reduction": {
      "type": "none",
      "amount": null,
      "percentage": null
    },

    "fine": {
      "type": null,
      "amount": null,
      "percentage": null
    },

    "revoke_days": null,
    "tags": ["tag1", "tag2"],
    "custom_data": null,
    "external_id": null,
    "payments": []
  }
}
```

### List Response (200 OK)
```json
{
  "status": 200,
  "data": [...],
  "pagination": {
    "count": 100,
    "page": 1,
    "items": 50,
    "pages": 2,
    "last": false,
    "next": "/v2/charge/pix?page=2",
    "prev": null
  }
}
```

---

## Status Reference

### Charge Status
| Status | Description |
|--------|-------------|
| `opened` | Open (not paid) - initial state |
| `paid` | Paid |
| `overdue` | Overdue |
| `canceled` | Canceled |
| `generation_failed` | Created but not registered at bank |

### Registration Status
| Status | Description |
|--------|-------------|
| `pending` | Waiting for registration |
| `skipped` | Registration skipped |
| `requested` | Registration requested |
| `confirmed` | Registration confirmed (has QR Code) |
| `rejected` | Registration rejected |
| `failed` | Registration failed |

---

## Error Responses

### 401 Unauthorized
```json
{
  "status": 401,
  "errors": [
    {
      "title": "Invalid API Token",
      "code": "unauthorized",
      "detail": "The API Token is different for each Server/URL"
    }
  ]
}
```

### 403 Forbidden
```json
{
  "status": 403,
  "errors": [
    {
      "title": "Scope required: read write",
      "code": "forbidden"
    }
  ]
}
```

### 404 Not Found
```json
{
  "status": 404,
  "errors": [
    {
      "title": "Record not found",
      "code": "not_found",
      "detail": "This record does not exist or was deleted"
    }
  ]
}
```

### 422 Validation Error
```json
{
  "status": 422,
  "errors": [
    {
      "code": "validation_error",
      "param": "amount",
      "detail": "Amount cannot be blank"
    },
    {
      "code": "validation_error",
      "param": "payer",
      "detail": "Payer > CPF/CNPJ is not valid"
    }
  ]
}
```

---

## Webhooks

Configure webhooks to receive real-time notifications.

### Pix Events
| Event | Description |
|-------|-------------|
| `pix.db.created` | Pix created in database |
| `pix.register.requested` | Registration sent to bank |
| `pix.register.confirmed` | Registration confirmed (has QR Code) |
| `pix.register.rejected` | Registration rejected |
| `pix.register.failed` | Registration failed |
| `pix.paid` | Pix was paid |
| `pix.canceled` | Pix was canceled |

### Webhook Payload
```json
{
  "event": "pix.register.confirmed",
  "data": {
    "uid": "019c0cbe-f018-717e-...",
    "status": "opened",
    "registration_status": "confirmed",
    "qrcode": {
      "emv": "00020126580014br.gov.bcb.pix...",
      "png": "data:image/png;base64,..."
    },
    "url": "https://kdoc.to/1/WYOuEJbUjXLk"
  }
}
```

---

## About Pix

Pix is Brazil's instant payment system, created and managed by the Central Bank of Brazil. Launched in November 2020, it revolutionized financial transactions in Brazil.

### Key Features
- **Instant**: Transfers completed in up to 10 seconds
- **24/7 Availability**: Works on holidays and weekends
- **Free for individuals**: No fees for personal use
- **Simple**: Uses addressing keys (CPF, email, phone, random key)

### Pix Types

**Instant Pix (`registration_kind: "instant"`)**
- Short expiration (minutes to hours)
- No interest, fines, or discounts
- Use for: e-commerce, delivery, point-of-sale

**Billing Pix (`registration_kind: "billing"`)**
- Formal due date
- Supports interest, fines, discounts
- Stays active after due date (if `revoke_days` set)
- Use for: monthly invoices, subscriptions

---

## Additional Resources

- **API Documentation**: https://developers.kobana.com.br
- **API Specification**: https://github.com/universokobana/kobana-api-specs
- **Central Bank Pix Documentation**: https://www.bcb.gov.br/estabilidadefinanceira/pix
