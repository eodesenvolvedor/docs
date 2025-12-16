---
title: "Logout"
api: "POST /api/v1/autenticacao/logout"
description: "Remove a sessão do usuário"
---

## Endpoint

<Endpoint method="post" url="/api/v1/autenticacao/logout" />

## Descrição

Remove uma sessão ativa do sistema. Após o logout, o `sessionId` não poderá mais ser usado.

## Body Parameters

<ParamField body="sessionId" type="string" required>
  ID da sessão a ser removida
</ParamField>

## Headers

<ParamField header="X-Session-Id" type="string" required>
  ID da sessão (alternativa ao body)
</ParamField>

## Response

<ResponseField name="success" type="boolean">
  Indica se a operação foi bem-sucedida
</ResponseField>

<ResponseField name="message" type="string">
  Mensagem de confirmação
</ResponseField>

## Exemplo de Requisição

<CodeGroup>
```bash cURL
curl -X POST http://localhost:5000/api/v1/autenticacao/logout \
  -H "Content-Type: application/json" \
  -H "X-Session-Id: seu_session_id" \
  -d '{
    "sessionId": "seu_session_id"
  }'
```

```javascript JavaScript
const response = await fetch('http://localhost:5000/api/v1/autenticacao/logout', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-Session-Id': 'seu_session_id'
  },
  body: JSON.stringify({
    sessionId: 'seu_session_id'
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
  "message": "Sessão removida com sucesso"
}
```

<Info>
  Você pode enviar o `sessionId` tanto no body quanto no header `X-Session-Id`. Ambos funcionam.
</Info>
