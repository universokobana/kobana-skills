# Estrutura Padrão de Skills Kobana

Este documento define a estrutura padrão para desenvolvimento de skills Kobana, com dois tipos principais: **API-based** e **MCP-based**.

## Estrutura de Diretórios

```
skills/
├── api-{namespace}/          # Skill baseada em API REST
│   ├── SKILL.md              # Instruções principais (obrigatório)
│   ├── LICENSE.txt           # Licença (obrigatório)
│   └── references/
│       └── REFERENCE.md      # Referência técnica completa
│
└── mcp-{namespace}/          # Skill baseada em MCP Server
    ├── SKILL.md              # Instruções principais (obrigatório)
    ├── LICENSE.txt           # Licença (obrigatório)
    └── references/
        └── REFERENCE.md      # Referência técnica completa
```

## Convenção de Nomenclatura

| Tipo | Padrão | Exemplo |
|------|--------|---------|
| API-based | `api-{namespace}` | `api-charge-pix`, `api-payment-billet` |
| MCP-based | `mcp-{namespace}` | `mcp-charge-pix`, `mcp-payment-billet` |

O `{namespace}` deve seguir o padrão:
- Lowercase apenas
- Separado por hífens
- Formato: `{domínio}-{recurso}` (ex: `charge-pix`, `payment-billet`, `transfer-ted`)

---

## Skill Baseada em API REST

### Quando Usar

Use skills API-based quando:
- O usuário precisa fazer chamadas HTTP diretas
- Integrando com backend systems
- Necessita controle total sobre as requisições
- Não há MCP server configurado

### Estrutura do SKILL.md

```yaml
---
name: api-{namespace}
description: Guide for [descrição]. Use when the user wants to [casos de uso].
license: Complete terms in LICENSE.txt
metadata:
  author: Kobana
  version: "X.Y"
---
```

### Template Completo

```markdown
---
name: api-{namespace}
description: Guide for managing [recursos] using Kobana API. Use when the user wants to [lista de ações].
license: Complete terms in LICENSE.txt
metadata:
  author: Kobana
  version: "1.0"
---

# Kobana {Domínio} API Skill

[Descrição curta do propósito da skill]

## Base URLs

```
Production: https://api.kobana.com.br
Sandbox:    https://api-sandbox.kobana.com.br
```

## Authentication

```
Authorization: Bearer {your_api_token}
```

## API Endpoints Overview

### {Recurso Principal}
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/v2/{path}` | List all |
| POST | `/v2/{path}` | Create new |
| GET | `/v2/{path}/{uid}` | Get specific |
| PUT | `/v2/{path}/{uid}` | Update |
| DELETE | `/v2/{path}/{uid}` | Delete |

## {Ação Principal}

### Minimum Required Parameters
```json
POST /v2/{path}

{
  "param1": "value1",
  "param2": "value2"
}
```

### Complete Example
```json
POST /v2/{path}

{
  "param1": "value1",
  "param2": "value2",
  "optional_param": "value"
}
```

## Important: Asynchronous Behavior

[Explicar comportamento assíncrono se aplicável]

## Listing and Filtering

### List with Filters
```
GET /v2/{path}?status=active&created_from=2026-01-01&per_page=50
```

| Parameter | Description |
|-----------|-------------|
| `status` | Filter by status |
| `created_from` / `created_to` | Filter by date |
| `page` / `per_page` | Pagination |

## Status Reference

### {Recurso} Status
- `status1` - Description
- `status2` - Description

## Best Practices

1. **Item 1** - Explicação
2. **Item 2** - Explicação
3. **Use X-Idempotency-Key header** - Prevent duplicates
4. **Test in sandbox** - Before production

## Reference Documentation

See [references/REFERENCE.md](references/REFERENCE.md) for complete API documentation.
```

### Estrutura do REFERENCE.md (API)

```markdown
# Kobana {Domínio} API - Complete Reference

Complete API documentation for [recursos].

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

## {Recurso}

### List
```
GET /v2/{path}
```

**Query Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `page` | integer | Page number |
| `per_page` | integer | Items per page |

### Create
```
POST /v2/{path}
```

**Required Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|

**Optional Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|

### Get
```
GET /v2/{path}/{uid}
```

### Update
```
PUT /v2/{path}/{uid}
```

### Delete
```
DELETE /v2/{path}/{uid}
```

---

## Response Structure

### Success Response (201 Created)
```json
{
  "status": 201,
  "data": {
    "uid": "...",
    ...
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
    "next": "/v2/{path}?page=2",
    "prev": null
  }
}
```

---

## Status Reference

| Status | Description |
|--------|-------------|

---

## Error Responses

### 401 Unauthorized
```json
{
  "status": 401,
  "errors": [{"title": "Invalid API Token", "code": "unauthorized"}]
}
```

### 422 Validation Error
```json
{
  "status": 422,
  "errors": [{"code": "validation_error", "param": "field", "detail": "message"}]
}
```

---

## Webhooks

| Event | Description |
|-------|-------------|

---

## Additional Resources

- **API Documentation**: https://developers.kobana.com.br
- **API Specification**: https://github.com/universokobana/kobana-api-specs
```

---

## Skill Baseada em MCP Server

### Quando Usar

Use skills MCP-based quando:
- MCP server já está configurado
- Usando Claude Desktop ou Claude Code com MCP
- Prefere interação simplificada via tools
- Não quer lidar com requisições HTTP diretamente

### Estrutura do SKILL.md

```yaml
---
name: mcp-{namespace}
description: Guide for [descrição] using the kobana-mcp-{package} MCP server. Use when the user wants to [casos de uso] using MCP tools.
license: Complete terms in LICENSE.txt
metadata:
  author: Kobana
  version: "X.Y"
compatibility: Requires kobana-mcp-{package} MCP server configured
---
```

### Template Completo

```markdown
---
name: mcp-{namespace}
description: Guide for [ação] using the kobana-mcp-{package} MCP server. Use when the user wants to [casos de uso] using MCP tools instead of direct API calls.
license: Complete terms in LICENSE.txt
metadata:
  author: Kobana
  version: "1.0"
compatibility: Requires kobana-mcp-{package} MCP server configured
---

# Kobana {Recurso} via MCP

[Descrição curta] using the `kobana-mcp-{package}` MCP server tools.

## Prerequisites

The `kobana-mcp-{package}` MCP server must be configured. See [MCP Server Setup](#mcp-server-setup) below.

## Available MCP Tools

### {Recurso}
| Tool | Description |
|------|-------------|
| `list_{resource}` | List all |
| `create_{resource}` | Create new |
| `get_{resource}` | Get specific |
| `update_{resource}` | Update |
| `delete_{resource}` | Delete |

## {Ação Principal}

### Step 1: [Pré-requisito]

```
Use tool: list_{prerequisite}
```

### Step 2: [Ação Principal]

Use the `create_{resource}` tool with the required parameters:

**Example:**
```json
{
  "param1": "value1",
  "param2": "value2"
}
```

### Step 3: [Verificação/Pós-processamento]

```
Use tool: get_{resource}
Parameters: { "uid": "..." }
```

## Common Parameters

### Required
| Parameter | Type | Description |
|-----------|------|-------------|

### Optional
| Parameter | Type | Description |
|-----------|------|-------------|

## Status Reference

| Status | Description |
|--------|-------------|

## MCP Server Setup

### Claude Desktop Configuration

Add to `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "kobana-{package}": {
      "command": "npx",
      "args": ["-y", "kobana-mcp-{package}"],
      "env": {
        "KOBANA_ACCESS_TOKEN": "your_access_token"
      }
    }
  }
}
```

### Sandbox Environment

```json
{
  "mcpServers": {
    "kobana-{package}": {
      "command": "npx",
      "args": ["-y", "kobana-mcp-{package}"],
      "env": {
        "KOBANA_ACCESS_TOKEN": "your_sandbox_token",
        "KOBANA_API_URL": "https://api-sandbox.kobana.com.br"
      }
    }
  }
}
```

### Claude Code Configuration

Add to `.mcp.json` in your project root:

```json
{
  "mcpServers": {
    "kobana-{package}": {
      "command": "npx",
      "args": ["-y", "kobana-mcp-{package}"],
      "env": {
        "KOBANA_ACCESS_TOKEN": "your_access_token"
      }
    }
  }
}
```

### Remote MCP (Hosted)

```json
{
  "mcpServers": {
    "kobana-{package}": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp.kobana.com.br/{namespace}/mcp",
        "--header",
        "Authorization: Bearer your_access_token"
      ]
    }
  }
}
```

## Best Practices

1. **Item 1** - Explicação
2. **Item 2** - Explicação
3. **Test in sandbox** - Configure sandbox environment first

## Reference Documentation

See [references/REFERENCE.md](references/REFERENCE.md) for complete MCP server documentation.
```

### Estrutura do REFERENCE.md (MCP)

```markdown
# kobana-mcp-{package} - Complete Reference

MCP server for the Kobana {Domínio} API. This server provides tools for [recursos].

**npm package**: [kobana-mcp-{package}](https://www.npmjs.com/package/kobana-mcp-{package})

## Quick Start

```bash
# Local mode (stdio)
KOBANA_ACCESS_TOKEN=your_token npx kobana-mcp-{package}

# HTTP mode (hosted)
KOBANA_ACCESS_TOKEN=your_token npx kobana-mcp-{package}-http
```

## Configuration

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `KOBANA_ACCESS_TOKEN` | Yes | - | Bearer access token |
| `KOBANA_API_URL` | No | `https://api.kobana.com.br` | Base URL |

## All Available Tools (N tools)

### {Recurso} (N tools)

| Tool | Description |
|------|-------------|

## Tool Parameters

### create_{resource}

**Required Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|

**Optional Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|

## Response Structure

```json
{
  "uid": "...",
  ...
}
```

## Status Reference

| Status | Description |
|--------|-------------|

## Client Configurations

### Claude Desktop (macOS)
### Claude Desktop (Windows)
### Claude Code
### Remote MCP (Hosted)
### Remote MCP Sandbox

## OAuth Scopes

| Resource | Scope |
|----------|-------|

## Additional Resources

- **GitHub Repository**: https://github.com/universokobana/kobana-mcp-servers
- **npm Package**: https://www.npmjs.com/package/kobana-mcp-{package}
- **Kobana Documentation**: https://developers.kobana.com.br
```

---

## Checklist para Nova Skill

### Antes de Começar

- [ ] Definir o namespace: `{domínio}-{recurso}`
- [ ] Decidir o tipo: API-based ou MCP-based (ou ambos)
- [ ] Identificar os endpoints/tools principais

### Arquivos Obrigatórios

- [ ] `SKILL.md` com frontmatter válido
- [ ] `LICENSE.txt` com termos de licença
- [ ] `references/REFERENCE.md` com documentação completa

### Conteúdo do SKILL.md

- [ ] Frontmatter com `name`, `description`, `license`, `metadata`
- [ ] (MCP) Campo `compatibility` indicando requisito do MCP server
- [ ] Seção de Base URLs ou Prerequisites
- [ ] Seção de Authentication ou Available Tools
- [ ] Tabela de Endpoints/Tools overview
- [ ] Exemplo de criação com parâmetros mínimos
- [ ] Exemplo completo com todos os parâmetros
- [ ] Seção de Status Reference
- [ ] (MCP) Seção de MCP Server Setup
- [ ] Seção de Best Practices
- [ ] Link para REFERENCE.md

### Conteúdo do REFERENCE.md

- [ ] Documentação completa de todos os endpoints/tools
- [ ] Todos os parâmetros documentados (required e optional)
- [ ] Estrutura de resposta com exemplos JSON
- [ ] Tabela de status
- [ ] Códigos de erro
- [ ] (API) Webhooks disponíveis
- [ ] (MCP) Configurações de cliente
- [ ] Links para recursos externos

### Validação Final

- [ ] Nome do diretório = campo `name` do frontmatter
- [ ] `description` clara sobre O QUE faz e QUANDO usar
- [ ] Exemplos testados no ambiente sandbox
- [ ] Links funcionando

---

## Mapeamento Kobana Skills

| Domínio | API Skill | MCP Skill | MCP Package |
|---------|-----------|-----------|-------------|
| Charge - Pix | `api-charge-pix` | `mcp-charge-pix` | `kobana-mcp-charge` |
| Charge - Billet | `api-charge-billet` | `mcp-charge-billet` | `kobana-mcp-charge` |
| Payment - Billet | `api-payment-billet` | `mcp-payment-billet` | `kobana-mcp-payment` |
| Payment - Pix | `api-payment-pix` | `mcp-payment-pix` | `kobana-mcp-payment` |
| Payment - Tax | `api-payment-tax` | `mcp-payment-tax` | `kobana-mcp-payment` |
| Transfer - Pix | `api-transfer-pix` | `mcp-transfer-pix` | `kobana-mcp-transfer` |
| Transfer - TED | `api-transfer-ted` | `mcp-transfer-ted` | `kobana-mcp-transfer` |
| Admin | `api-admin` | `mcp-admin` | `kobana-mcp-admin` |
| Financial | `api-financial` | `mcp-financial` | `kobana-mcp-financial` |

---

## Recursos Externos

- **Agent Skills Spec**: [spec/agent-skills-spec.md](agent-skills-spec.md)
- **Kobana API Docs**: https://developers.kobana.com.br
- **Kobana API Specs**: https://github.com/universokobana/kobana-api-specs
- **Kobana MCP Servers**: https://github.com/universokobana/kobana-mcp-servers
