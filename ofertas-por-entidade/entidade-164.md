---
title: "Entidade 164"
api: "GET /api/v1/bmg/164/ofertas/cpf/:cpf/matricula/:matricula?orgao=145"
description: "Consulta ofertas para a entidade 164 (requer parâmetro orgao)"
---

## Endpoint

<Endpoint method="get" url="/api/v1/bmg/164/ofertas/cpf/:cpf/matricula/:matricula?orgao=145" />

## Path Parameters

<ParamField path="cpf" type="string" required>
  CPF do cliente
</ParamField>

<ParamField path="matricula" type="string" required>
  Matrícula do cliente
</ParamField>

## Query Parameters

<ParamField query="orgao" type="string" required>
  Código do órgão (geralmente "145")
</ParamField>

## Headers

<ParamField header="X-Session-Id" type="string" required>
  ID da sessão obtido no login
</ParamField>

## Exemplo de Requisição

<CodeGroup>
```bash cURL
curl -X GET "http://localhost:5000/api/v1/bmg/164/ofertas/cpf/42895847134/matricula/1415407?orgao=145" \
  -H "X-Session-Id: seu_session_id"
```

```javascript JavaScript
const entidade = '164';
const cpf = '42895847134';
const matricula = '1415407';
const orgao = '145';
const response = await fetch(`http://localhost:5000/api/v1/bmg/${entidade}/ofertas/cpf/${cpf}/matricula/${matricula}?orgao=${orgao}`, {
  method: 'GET',
  headers: {
    'X-Session-Id': 'seu_session_id'
  }
});

const data = await response.json();
console.log(data);
```
</CodeGroup>

<Warning>
  A entidade 164 **requer** o parâmetro `orgao` na query string. Sem ele, a requisição falhará.
</Warning>
