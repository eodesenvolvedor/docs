---
title: "Sessões Ativas"
api: "GET /api/v1/autenticacao/sessoes-ativas"
description: "Lista todas as sessões ativas (debug/admin)"
---

## Endpoint

<Endpoint method="get" url="/api/v1/autenticacao/sessoes-ativas" />

## Descrição

Retorna uma lista de todas as sessões ativas no sistema. Útil para debug e administração.

## Response

<ResponseField name="success" type="boolean">
  Indica se a operação foi bem-sucedida
</ResponseField>

<ResponseField name="data.totalSessoes" type="number">
  Total de sessões ativas
</ResponseField>

<ResponseField name="data.sessoes" type="array">
  Array com informações de cada sessão
</ResponseField>

<ResponseField name="data.sessoes[].sessionId" type="string">
  ID da sessão
</ResponseField>

<ResponseField name="data.sessoes[].username" type="string">
  Nome de usuário da sessão
</ResponseField>

<ResponseField name="data.sessoes[].createdAt" type="string">
  Data de criação da sessão
</ResponseField>

## Exemplo de Requisição

<CodeGroup>
```bash cURL
curl -X GET http://localhost:5000/api/v1/autenticacao/sessoes-ativas
```

```javascript JavaScript
const response = await fetch('http://localhost:5000/api/v1/autenticacao/sessoes-ativas', {
  method: 'GET'
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
    "totalSessoes": 2,
    "sessoes": [
      {
        "sessionId": "abc123...",
        "username": "usuario1",
        "createdAt": "2024-01-01T00:00:00.000Z"
      },
      {
        "sessionId": "def456...",
        "username": "usuario2",
        "createdAt": "2024-01-01T01:00:00.000Z"
      }
    ]
  }
}
```

<Warning>
  Este endpoint é principalmente para uso administrativo e debug. Em produção, considere adicionar autenticação adicional.
</Warning>
