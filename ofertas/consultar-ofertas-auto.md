---
title: "Consultar Ofertas (Auto)"
api: "GET /api/v1/ofertas/auto/cpf/:cpf/matricula/:matricula"
description: "Busca cliente primeiro para obter data de nascimento e depois consulta ofertas"
---

## Endpoint

<Endpoint method="get" url="/api/v1/ofertas/auto/cpf/:cpf/matricula/:matricula" />

## Descrição

Esta rota primeiro busca os dados do cliente para obter a data de nascimento real e depois consulta as ofertas. Recomendado para maior precisão.

## Path Parameters

<ParamField path="cpf" type="string" required>
  CPF do cliente
</ParamField>

<ParamField path="matricula" type="string" required>
  Matrícula do cliente na entidade
</ParamField>

## Headers

<ParamField header="X-Session-Id" type="string" required>
  ID da sessão obtido no login
</ParamField>

## Response

<ResponseField name="success" type="boolean">
  Indica se a operação foi bem-sucedida
</ResponseField>

<ResponseField name="data.ofertas" type="array">
  Array com as ofertas disponíveis
</ResponseField>

## Exemplo de Requisição

<CodeGroup>
```bash cURL
curl -X GET "http://localhost:5000/api/v1/ofertas/auto/cpf/12345678901/matricula/1234567890" \
  -H "X-Session-Id: seu_session_id"
```

```javascript JavaScript
const cpf = '12345678901';
const matricula = '1234567890';
const response = await fetch(`http://localhost:5000/api/v1/ofertas/auto/cpf/${cpf}/matricula/${matricula}`, {
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
    "ofertas": [
      {
        "codigo": "123",
        "nome": "Crédito Consignado",
        "valor": 50000.00,
        "taxa": 1.99
      }
    ]
  }
}
```

<Info>
  Esta rota faz duas requisições internas: primeiro busca o cliente e depois consulta as ofertas com a data de nascimento real.
</Info>

<Warning>
  Esta rota é mais lenta que a rota padrão, mas oferece maior precisão ao usar a data de nascimento real do cliente.
</Warning>
