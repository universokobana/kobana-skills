# Claude Instructions for Kobana Skills

This repository contains Kobana's official skills for financial automation with Claude.

## Repository Structure

```
kobana-skills/
├── skills/
│   ├── api-charge-pix/       # Pix charges via REST API
│   └── mcp-charge-pix/       # Pix charges via MCP Server
├── spec/                     # Agent Skills specification
└── template/                 # Skill template
```

## Available Skills

### api-charge-pix
- **Purpose**: Create and manage Pix charges using Kobana REST API
- **Documentation**: `skills/api-charge-pix/SKILL.md`
- **Full Reference**: `skills/api-charge-pix/references/REFERENCE.md`

### mcp-charge-pix
- **Purpose**: Create and manage Pix charges using kobana-mcp-charge MCP server
- **Documentation**: `skills/mcp-charge-pix/SKILL.md`
- **Full Reference**: `skills/mcp-charge-pix/references/REFERENCE.md`

## External Documentation

### Kobana API
- **API Documentation**: https://developers.kobana.com.br
- **API Specifications (OpenAPI)**: https://github.com/universokobana/kobana-api-specs
- **Base URLs**:
  - Production: `https://api.kobana.com.br`
  - Sandbox: `https://api-sandbox.kobana.com.br`

### Kobana MCP Servers
- **Repository**: https://github.com/universokobana/kobana-mcp-servers
- **npm packages**:
  - `kobana-mcp-charge` - Pix charges, accounts, payments
  - `kobana-mcp-admin` - Certificates, connections, users
  - `kobana-mcp-financial` - Financial accounts, balances
  - `kobana-mcp-payment` - Bank billets, Pix, taxes
  - `kobana-mcp-transfer` - Pix, TED, internal transfers
- **Remote MCP endpoints**:
  - Production: `https://mcp.kobana.com.br/{namespace}/mcp`
  - Sandbox: `https://mcp-sandbox.kobana.com.br/{namespace}/mcp`

## When to Use Each Skill

### Use api-charge-pix when:
- Making direct HTTP calls to Kobana API
- Integrating with backend systems
- Need full control over API requests
- Working without MCP server configured

### Use mcp-charge-pix when:
- MCP server is already configured
- Using Claude Desktop or Claude Code with MCP
- Want simplified tool-based interaction
- Prefer not to handle HTTP requests directly

## Authentication

Both skills require a Kobana access token:
- **API**: Pass via `Authorization: Bearer {token}` header
- **MCP**: Configure via `KOBANA_ACCESS_TOKEN` environment variable

## Quick Commands

### Create Pix Charge (API)
```bash
curl -X POST https://api.kobana.com.br/v2/charge/pix \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"amount": 100.00, "pix_account_uid": "...", "expire_at": "2026-02-01T12:00:00-03:00"}'
```

### Create Pix Charge (MCP)
```
Use tool: create_charge_pix
Parameters: {"amount": 100.00, "pix_account_uid": "...", "expire_at": "2026-02-01T12:00:00-03:00"}
```

## About Kobana

Kobana is a Brazilian financial automation platform connecting businesses to 40+ banks through a unified API. More info at https://www.kobana.com.br
