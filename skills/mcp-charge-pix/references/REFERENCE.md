# kobana-mcp-charge - Complete Reference

MCP (Model Context Protocol) server for the Kobana Charge API v2. This server provides tools for managing Pix payments and accounts through the Kobana platform.

**npm package**: [kobana-mcp-charge](https://www.npmjs.com/package/kobana-mcp-charge)

## Quick Start

```bash
# Local mode (stdio)
KOBANA_ACCESS_TOKEN=your_token npx kobana-mcp-charge

# HTTP mode (hosted)
KOBANA_ACCESS_TOKEN=your_token npx kobana-mcp-charge-http
```

## Configuration

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `KOBANA_ACCESS_TOKEN` | Yes | - | Bearer access token for Kobana API |
| `KOBANA_API_URL` | No | `https://api.kobana.com.br` | Base URL for Kobana API |

### Sandbox Environment

```bash
export KOBANA_API_URL=https://api-sandbox.kobana.com.br
export KOBANA_ACCESS_TOKEN=your_sandbox_token
npx kobana-mcp-charge
```

## All Available Tools (35 tools)

### Pix Accounts (5 tools)

| Tool | Description |
|------|-------------|
| `list_charge_pix_accounts` | List all Pix accounts |
| `create_charge_pix_account` | Create a new Pix account |
| `get_charge_pix_account` | Get a specific Pix account |
| `update_charge_pix_account` | Update a Pix account |
| `delete_charge_pix_account` | Delete a Pix account |

### Pix Charges (6 tools)

| Tool | Description |
|------|-------------|
| `list_charge_pix` | List all Pix charges with filters |
| `create_charge_pix` | Create a new Pix charge |
| `get_charge_pix` | Get a specific Pix charge |
| `update_charge_pix` | Update a Pix charge |
| `delete_charge_pix` | Delete a canceled Pix charge |
| `cancel_charge_pix` | Cancel a Pix charge |

### Pix Commands (2 tools)

| Tool | Description |
|------|-------------|
| `list_charge_pix_commands` | List commands for a Pix charge |
| `get_charge_pix_command` | Get a specific command |

### Automatic Pix (18 tools)

| Tool | Description |
|------|-------------|
| `list_charge_automatic_pix` | List automatic Pix configurations |
| `get_charge_automatic_pix` | Get an automatic Pix configuration |
| `update_charge_automatic_pix` | Update automatic Pix |
| `patch_charge_automatic_pix` | Patch automatic Pix |
| `cancel_charge_automatic_pix` | Cancel automatic Pix |
| `retry_charge_automatic_pix` | Retry automatic Pix |
| `list_charge_automatic_pix_recurrences` | List recurrences |
| `create_charge_automatic_pix_recurrence` | Create a recurrence |
| `get_charge_automatic_pix_recurrence` | Get a recurrence |
| `update_charge_automatic_pix_recurrence` | Update a recurrence |
| `patch_charge_automatic_pix_recurrence` | Patch a recurrence |
| `cancel_charge_automatic_pix_recurrence` | Cancel a recurrence |
| `create_charge_automatic_pix_recurrence_pix` | Create Pix for recurrence |
| `list_charge_automatic_pix_requests` | List requests |
| `create_charge_automatic_pix_recurrence_request` | Create a request |
| `get_charge_automatic_pix_request` | Get a request |
| `patch_charge_automatic_pix_request` | Patch a request |
| `cancel_charge_automatic_pix_request` | Cancel a request |

### Payments (4 tools)

| Tool | Description |
|------|-------------|
| `list_charge_payments` | List all payments |
| `create_charge_payment` | Create a new payment |
| `get_charge_payment` | Get a specific payment |
| `delete_charge_payment` | Delete a payment |

## Tool Parameters

### create_charge_pix

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
| `payer.document_number` | string | CPF or CNPJ |
| `payer.name` | string | Full name or company name |
| `payer.email` | string | Email address |
| `payer.address` | object | Address (required for Sicoob) |
| `fine_type` | string | `percentage`, `amount`, or `none` |
| `fine_percentage` | decimal | Fine percentage |
| `fine_amount` | decimal | Fine amount |
| `interest_type` | string | See interest types below |
| `interest_percentage` | decimal | Interest percentage |
| `interest_amount` | decimal | Interest amount |
| `discount_type` | string | See discount types below |
| `discount_first_percentage` | decimal | First discount percentage |
| `discount_first_days` | integer | Days for first discount |
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

### list_charge_pix

| Parameter | Type | Description |
|-----------|------|-------------|
| `status` | string | Filter by status |
| `registration_status` | string | Filter by registration status |
| `pix_account_uid` | string | Filter by Pix account |
| `created_from` | string | Created date from (YYYY-MM-DD) |
| `created_to` | string | Created date to (YYYY-MM-DD) |
| `expire_from` | string | Expire date from (YYYY-MM-DD) |
| `expire_to` | string | Expire date to (YYYY-MM-DD) |
| `per_page` | integer | Results per page (default 50) |
| `page` | integer | Page number |

### get_charge_pix

| Parameter | Type | Description |
|-----------|------|-------------|
| `uid` | string | Pix charge UID (required) |

### cancel_charge_pix

| Parameter | Type | Description |
|-----------|------|-------------|
| `uid` | string | Pix charge UID (required) |

## Interest Types

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

## Discount Types

| Type | Description |
|------|-------------|
| `fixed_amount` | Fixed value |
| `fixed_percentage` | Fixed percentage |
| `advance_amount_calendar` | Value per advance (calendar days) |
| `advance_amount_business` | Value per advance (business days) |
| `advance_percentage_calendar` | Percentage per advance (calendar days) |
| `advance_percentage_business` | Percentage per advance (business days) |

## Response Structure

### Pix Charge Response

```json
{
  "uid": "019c0cbe-f018-717e-be6e-...",
  "txid": "E12345678202301011200...",
  "status": "opened",
  "registration_status": "confirmed",
  "kind": "instant",

  "amount": 100.50,
  "paid_amount": null,

  "created_at": "2026-01-30T11:32:45-03:00",
  "expire_at": "2026-01-30T12:32:45-03:00",
  "paid_at": null,

  "qrcode": {
    "emv": "00020126580014br.gov.bcb.pix...",
    "png": "data:image/png;base64,..."
  },

  "url": "https://kdoc.to/1/WYOuEJbUjXLk",

  "payer": {
    "document_number": "111.321.322-09",
    "name": "João da Silva"
  },

  "formats": {
    "default": {
      "html": "https://kdoc.to/1/WYOuEJbUjXLk",
      "png": "https://kdoc.to/1/WYOuEJbUjXLk.png",
      "pdf": "https://kdoc.to/1/WYOuEJbUjXLk.pdf"
    }
  }
}
```

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

## Client Configurations

### Claude Desktop (macOS)

`~/Library/Application Support/Claude/claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "kobana-charge": {
      "command": "npx",
      "args": ["-y", "kobana-mcp-charge"],
      "env": {
        "KOBANA_ACCESS_TOKEN": "your_access_token"
      }
    }
  }
}
```

### Claude Desktop (Windows)

`%APPDATA%\Claude\claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "kobana-charge": {
      "command": "npx",
      "args": ["-y", "kobana-mcp-charge"],
      "env": {
        "KOBANA_ACCESS_TOKEN": "your_access_token"
      }
    }
  }
}
```

### Claude Code

`.mcp.json` in project root:

```json
{
  "mcpServers": {
    "kobana-charge": {
      "command": "npx",
      "args": ["-y", "kobana-mcp-charge"],
      "env": {
        "KOBANA_ACCESS_TOKEN": "your_access_token"
      }
    }
  }
}
```

### Remote MCP (Hosted at mcp.kobana.com.br)

```json
{
  "mcpServers": {
    "kobana-charge": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp.kobana.com.br/charge/mcp",
        "--header",
        "Authorization: Bearer your_access_token"
      ]
    }
  }
}
```

### Remote MCP Sandbox

```json
{
  "mcpServers": {
    "kobana-charge": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp-sandbox.kobana.com.br/charge/mcp",
        "--header",
        "Authorization: Bearer your_sandbox_token"
      ]
    }
  }
}
```

## OAuth Scopes

| Resource | Scope |
|----------|-------|
| Pix Accounts | `charge.pix_accounts` |
| Pix Charges | `charge.pix` |
| Pix Commands | `charge.pix` |
| Automatic Pix - Charges | `charge.automatic_pix.pix` |
| Automatic Pix - Recurrences | `charge.automatic_pix.recurrences` |
| Automatic Pix - Requests | `charge.automatic_pix.requests` |
| Payments | `charge.payments` |

## Additional Resources

- **GitHub Repository**: https://github.com/universokobana/kobana-mcp-servers
- **npm Package**: https://www.npmjs.com/package/kobana-mcp-charge
- **Kobana Documentation**: https://developers.kobana.com.br
- **API Specification**: https://github.com/universokobana/kobana-api-specs
