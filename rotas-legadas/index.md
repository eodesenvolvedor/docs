---
title: "Rotas Legadas"
description: "Rotas mantidas para compatibilidade com versões anteriores"
---

## Visão Geral

As rotas em `/api/v1/bmg/*` são mantidas para compatibilidade com versões anteriores da API. **Recomendamos usar as novas rotas organizadas por entidade.**

## Por Que Migrar?

As novas rotas oferecem:
- ✅ Melhor organização por funcionalidade
- ✅ Suporte para múltiplas entidades
- ✅ Estrutura mais clara e intuitiva
- ✅ Manutenção mais fácil

## Rotas Disponíveis

### Autenticação
- `POST /api/v1/bmg/login` → Use `/api/v1/autenticacao/login`
- `POST /api/v1/bmg/refresh-session` → Use `/api/v1/autenticacao/refresh-session`
- `POST /api/v1/bmg/logout` → Use `/api/v1/autenticacao/logout`
- `GET /api/v1/bmg/sessoes-ativas` → Use `/api/v1/autenticacao/sessoes-ativas`

### Clientes
- `GET /api/v1/bmg/Cliente/cpf/:cpf` → Use `/api/v1/clientes/:cpf`

### Ofertas
- `GET /api/v1/bmg/ofertas/cpf/:cpf/matricula/:matricula` → Use `/api/v1/ofertas/cpf/:cpf/matricula/:matricula`
- `GET /api/v1/bmg/ofertas-auto/cpf/:cpf/matricula/:matricula` → Use `/api/v1/ofertas/auto/cpf/:cpf/matricula/:matricula`
- `POST /api/v1/bmg/consultar-lote` → Use `/api/v1/clientes/consultar-lote`
- `POST /api/v1/bmg/consultar-ofertas-lote` → Use `/api/v1/ofertas/consultar-lote`

## Exemplo de Migração

**Antigo**:
```bash
GET /api/v1/bmg/ofertas/cpf/123/matricula/456
```

**Novo**:
```bash
GET /api/v1/ofertas/cpf/123/matricula/456
# ou para entidade específica:
GET /api/v1/bmg/1581/ofertas/cpf/123/matricula/456
```

<Warning>
  As rotas legadas continuam funcionando, mas podem ser descontinuadas em versões futuras. Migre para as novas rotas quando possível.
</Warning>

<Info>
  As novas rotas oferecem melhor organização e suporte para múltiplas entidades.
</Info>
