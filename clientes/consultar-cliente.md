---
title: "Consultar Cliente"
api: "GET /api/v1/clientes/:cpf"
description: "Consulta dados de um cliente por CPF"
---

## Endpoint

<Endpoint method="get" url="/api/v1/clientes/:cpf" />

## Descrição

Consulta os dados completos de um cliente usando o CPF. O CPF pode ser enviado com ou sem formatação.

## Path Parameters

<ParamField path="cpf" type="string" required>
  CPF do cliente (com ou sem formatação: 12345678901 ou 123.456.789-01)
</ParamField>

## Headers

<ParamField header="X-Session-Id" type="string" required>
  ID da sessão obtido no login
</ParamField>

## Response

<ResponseField name="success" type="boolean">
  Indica se a operação foi bem-sucedida
</ResponseField>

<ResponseField name="data.cliente" type="object">
  Dados completos do cliente
</ResponseField>

<ResponseField name="data.cliente.cpf" type="string">
  CPF do cliente
</ResponseField>

<ResponseField name="data.cliente.nome" type="string">
  Nome completo do cliente
</ResponseField>

<ResponseField name="data.cliente.dataNascimento" type="string">
  Data de nascimento no formato ISO
</ResponseField>

<ResponseField name="data.cliente.renda" type="number">
  Renda do cliente
</ResponseField>

## Exemplo de Requisição

<CodeGroup>
```bash cURL
curl -X GET http://localhost:5000/api/v1/clientes/12345678901 \
  -H "X-Session-Id: seu_session_id"
```

```javascript JavaScript
const cpf = '12345678901';
const response = await fetch(`http://localhost:5000/api/v1/clientes/${cpf}`, {
  method: 'GET',
  headers: {
    'X-Session-Id': 'seu_session_id'
  }
});

const data = await response.json();
console.log(data);
```
</CodeGroup>

## Exemplo de Resposta

```json
{
  "success": true,
  "data": {
    "cliente": {
      "cpf": "12345678901",
      "nome": "João Silva",
      "dataNascimento": "1990-01-01T00:00:00-03:00",
      "renda": 5000.00,
      "matricula": "1234567890"
    }
  }
}
```

<Info>
  O CPF pode ser enviado com ou sem formatação. A API remove automaticamente pontos e traços.
</Info>
