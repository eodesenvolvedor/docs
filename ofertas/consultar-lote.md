---
title: "Consultar Ofertas em Lote"
api: "POST /api/v1/ofertas/consultar-lote"
description: "Consulta ofertas de até 200 CPFs/matrículas de uma vez"
---

## Endpoint

<Endpoint method="post" url="/api/v1/ofertas/consultar-lote" />

## Descrição

Consulta ofertas de múltiplos clientes em uma única requisição. Processa até 200 consultas por vez. Para cada cliente, primeiro busca os dados para obter a data de nascimento.

## Body Parameters

<ParamField body="consultas" type="array" required>
  Array de objetos com cpf e matricula (máximo 200)
</ParamField>

<ParamField body="consultas[].cpf" type="string" required>
  CPF do cliente
</ParamField>

<ParamField body="consultas[].matricula" type="string" required>
  Matrícula do cliente
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
  Total de consultas processadas
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
curl -X POST http://localhost:5000/api/v1/ofertas/consultar-lote \
  -H "Content-Type: application/json" \
  -H "X-Session-Id: seu_session_id" \
  -d '{
    "consultas": [
      {
        "cpf": "12345678901",
        "matricula": "1234567890"
      },
      {
        "cpf": "98765432100",
        "matricula": "0987654321"
      }
    ]
  }'
```

```javascript JavaScript
const response = await fetch('http://localhost:5000/api/v1/ofertas/consultar-lote', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-Session-Id': 'seu_session_id'
  },
  body: JSON.stringify({
    consultas: [
      {
        cpf: '12345678901',
        matricula: '1234567890'
      },
      {
        cpf: '98765432100',
        matricula: '0987654321'
      }
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
    "totalProcessado": 2,
    "totalSucesso": 2,
    "totalErros": 0,
    "resultados": [
      {
        "cpf": "12345678901",
        "matricula": "1234567890",
        "dataNascimento": "1990-01-01T00:00:00-03:00",
        "dados": { ... }
      }
    ],
    "erros": []
  }
}
```

<Warning>
  O limite máximo é de 200 consultas por requisição. Requisições maiores serão rejeitadas.
</Warning>

<Info>
  Para cada consulta, a API primeiro busca os dados do cliente para obter a data de nascimento real antes de consultar as ofertas.
</Info>
