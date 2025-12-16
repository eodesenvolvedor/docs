---
title: "Entidade 4277"
api: "GET /api/v1/bmg/4277/ofertas/cpf/:cpf/matricula/:matricula"
description: "Consulta ofertas para a entidade 4277"
---

## Endpoint

<Endpoint method="get" url="/api/v1/bmg/4277/ofertas/cpf/:cpf/matricula/:matricula" />

## Path Parameters

<ParamField path="cpf" type="string" required>
  CPF do cliente
</ParamField>

<ParamField path="matricula" type="string" required>
  Matrícula do cliente
</ParamField>

## Headers

<ParamField header="X-Session-Id" type="string" required>
  ID da sessão obtido no login
</ParamField>

## Exemplo de Requisição

<CodeGroup>
```bash cURL
curl -X GET "http://localhost:5000/api/v1/bmg/4277/ofertas/cpf/92899617753/matricula/1877250560" \
  -H "X-Session-Id: seu_session_id"
```

```javascript JavaScript
const entidade = '4277';
const cpf = '92899617753';
const matricula = '1877250560';
const response = await fetch(`http://localhost:5000/api/v1/bmg/${entidade}/ofertas/cpf/${cpf}/matricula/${matricula}`, {
  method: 'GET',
  headers: {
    'X-Session-Id': 'seu_session_id'
  }
});

const data = await response.json();
console.log(data);
```
</CodeGroup>
