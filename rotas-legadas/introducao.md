---
title: "Rotas Legadas"
description: "Rotas mantidas para compatibilidade"
---

## Visão Geral

As rotas em `/api/v1/bmg/*` são mantidas para compatibilidade com versões anteriores da API. Recomendamos usar as novas rotas organizadas por entidade.

## Rotas Disponíveis

### Autenticação

- `POST /api/v1/bmg/login` - Login (igual a `/api/v1/autenticacao/login`)
- `POST /api/v1/bmg/refresh-session` - Refresh session
- `POST /api/v1/bmg/logout` - Logout
- `GET /api/v1/bmg/sessoes-ativas` - Sessões ativas

### Clientes

- `GET /api/v1/bmg/Cliente/cpf/:cpf` - Consultar cliente

### Ofertas

- `GET /api/v1/bmg/ofertas/cpf/:cpf/matricula/:matricula` - Consultar ofertas
- `GET /api/v1/bmg/ofertas-auto/cpf/:cpf/matricula/:matricula` - Consultar ofertas (auto)
- `POST /api/v1/bmg/consultar-lote` - Consultar lote de clientes
- `POST /api/v1/bmg/consultar-ofertas-lote` - Consultar lote de ofertas

## Migração

<Warning>
  As rotas legadas continuam funcionando, mas podem ser descontinuadas em versões futuras. Migre para as novas rotas quando possível.
</Warning>

### Exemplo de Migração

**Antigo**:
```bash
GET /api/v1/bmg/ofertas/cpf/123/matricula/456
```

**Novo**:
```bash
GET /api/v1/ofertas/cpf/123/matricula/456
# ou
GET /api/v1/bmg/1581/ofertas/cpf/123/matricula/456
```

<Info>
  As novas rotas oferecem melhor organização e suporte para múltiplas entidades.
</Info>
