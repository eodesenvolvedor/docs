---
title: "Consultar Lote"
api: "POST /api/v1/clientes/consultar-lote"
description: "Consulta até 200 CPFs de uma vez"
---

## Endpoint

<Endpoint method="post" url="/api/v1/clientes/consultar-lote" />

## Descrição

Consulta dados de múltiplos clientes em uma única requisição. Processa até 200 CPFs por vez de forma sequencial.

## Body Parameters

<ParamField body="cpfs" type="array" required>
  Array de CPFs para consulta (máximo 200)
</ParamField>

<ParamField body="sessionId" type="string">
  ID da sessão (opcional se enviar no header)
</ParamField>

## Headers

<ParamField header="X-Session-Id" type="string" required>
  ID da sessão (obrigatório se não enviar no body)
</ParamField>

## Response

<ResponseField name="success" type="boolean">
  Indica se a operação foi bem-sucedida
</ResponseField>

<ResponseField name="data.totalProcessado" type="number">
  Total de CPFs processados
</ResponseField>

<ResponseField name="data.totalSucesso" type="number">
  Total de consultas bem-sucedidas
</ResponseField>

<ResponseField name="data.totalErros" type="number">
  Total de erros encontrados
</ResponseField>

<ResponseField name="data.resultados" type="array">
  Array com os resultados das consultas bem-sucedidas
</ResponseField>

<ResponseField name="data.erros" type="array">
  Array com os erros encontrados
</ResponseField>

## Exemplo de Requisição

<CodeGroup>
```bash cURL
curl -X POST http://localhost:5000/api/v1/clientes/consultar-lote \
  -H "Content-Type: application/json" \
  -H "X-Session-Id: seu_session_id" \
  -d '{
    "cpfs": [
      "12345678901",
      "98765432100",
      "11122233344"
    ]
  }'
```

```javascript JavaScript
const response = await fetch('http://localhost:5000/api/v1/clientes/consultar-lote', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-Session-Id': 'seu_session_id'
  },
  body: JSON.stringify({
    cpfs: [
      '12345678901',
      '98765432100',
      '11122233344'
    ]
  })
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
    "totalProcessado": 3,
    "totalSucesso": 2,
    "totalErros": 1,
    "resultados": [
      {
        "cpf": "12345678901",
        "dados": { ... }
      },
      {
        "cpf": "98765432100",
        "dados": { ... }
      }
    ],
    "erros": [
      {
        "cpf": "11122233344",
        "erro": "Cliente não encontrado"
      }
    ]
  }
}
```

<Warning>
  O limite máximo é de 200 CPFs por requisição. Requisições maiores serão rejeitadas.
</Warning>

<Info>
  O processamento é sequencial com pequenos delays entre requisições para não sobrecarregar a API BMG.
</Info>
