---
title: "Ofertas por Entidade - Introdução"
description: "Consulte ofertas específicas por entidade"
---

## Visão Geral

A API permite consultar ofertas de crédito consignado para entidades específicas. Cada entidade pode ter configurações e parâmetros diferentes.

## Entidades Disponíveis

| Entidade | Descrição | Parâmetros Especiais |
|----------|-----------|---------------------|
| **1581** | Entidade 1581 | Nenhum |
| **4277** | Entidade 4277 | Nenhum |
| **3** | Entidade 3 | Nenhum |
| **164** | Entidade 164 | `?orgao=145` (obrigatório) |

## Endpoint Base

<Endpoint method="get" url="/api/v1/bmg/:entidade/ofertas/cpf/:cpf/matricula/:matricula" />

## Como Funciona

Esta rota automaticamente:
1. Busca os dados do cliente para obter a data de nascimento
2. Consulta as ofertas usando a entidade especificada
3. Retorna as ofertas disponíveis

## Exemplo Rápido

```bash
# Entidade 1581
curl -X GET "http://localhost:5000/api/v1/bmg/1581/ofertas/cpf/92899617753/matricula/1877250560" \
  -H "X-Session-Id: seu_session_id"

# Entidade 164 (com orgao)
curl -X GET "http://localhost:5000/api/v1/bmg/164/ofertas/cpf/42895847134/matricula/1415407?orgao=145" \
  -H "X-Session-Id: seu_session_id"
```

## Próximos Passos

<CardGroup cols={2}>
  <Card
    title="Entidade 1581"
    icon="building"
    href="/ofertas-por-entidade/entidade-1581"
  />
  <Card
    title="Entidade 4277"
    icon="building"
    href="/ofertas-por-entidade/entidade-4277"
  />
  <Card
    title="Entidade 3"
    icon="building"
    href="/ofertas-por-entidade/entidade-3"
  />
  <Card
    title="Entidade 164"
    icon="building"
    href="/ofertas-por-entidade/entidade-164"
  />
</CardGroup>
