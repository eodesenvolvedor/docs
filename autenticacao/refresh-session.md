---
title: "Refresh Session"
api: "POST /api/v1/autenticacao/refresh-session"
description: "Renova os tokens de uma sessão existente mantendo o mesmo sessionId"
---

## Endpoint

<Endpoint method="post" url="/api/v1/autenticacao/refresh-session" />

## Descrição

Renova os tokens de autenticação de uma sessão existente, mantendo o mesmo `sessionId`. Útil quando os tokens estão próximos de expirar.

## Body Parameters

<ParamField body="sessionId" type="string" required>
  ID da sessão que será renovada
</ParamField>

<ParamField body="username" type="string" required>
  Nome de usuário
</ParamField>

<ParamField body="password" type="string" required>
  Senha do usuário
</ParamField>

## Response

<ResponseField name="success" type="boolean">
  Indica se a operação foi bem-sucedida
</ResponseField>

<ResponseField name="data.sessionId" type="string">
  O mesmo sessionId fornecido (mantido)
</ResponseField>

<ResponseField name="data.authorization" type="string">
  Novo token de autorização Bearer
</ResponseField>

<ResponseField name="data.authorizationCorban" type="string">
  Novo token de autorização Corban Bearer
</ResponseField>

<ResponseField name="data.refreshToken" type="string">
  Novo token para renovação
</ResponseField>

## Exemplo de Requisição

<CodeGroup>
```bash cURL
curl -X POST http://localhost:5000/api/v1/autenticacao/refresh-session \
  -H "Content-Type: application/json" \
  -d '{
    "sessionId": "abc123...",
    "username": "seu_usuario",
    "password": "sua_senha"
  }'
```

```javascript JavaScript
const response = await fetch('http://localhost:5000/api/v1/autenticacao/refresh-session', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    sessionId: 'abc123...',
    username: 'seu_usuario',
    password: 'sua_senha'
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
    "sessionId": "abc123...",
    "authorization": "Bearer ...",
    "authorizationCorban": "Bearer ...",
    "refreshToken": "Bearer ..."
  }
}
```

<Info>
  O `sessionId` permanece o mesmo após o refresh, facilitando o gerenciamento de sessões.
</Info>
