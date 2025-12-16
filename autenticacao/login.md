---
title: "Login"
api: "POST /api/v1/autenticacao/login"
description: "Realiza login automático e captura os tokens de autenticação"
---

## Endpoint

<Endpoint method="post" url="/api/v1/autenticacao/login" />

## Descrição

Realiza login automático e captura os tokens de autenticação necessários para usar a API.

## Body Parameters

<ParamField body="username" type="string" required>
  Nome de usuário para autenticação
</ParamField>

<ParamField body="password" type="string" required>
  Senha do usuário
</ParamField>

<ParamField body="reuseSession" type="boolean" default="true">
  Se deve reutilizar uma sessão existente (opcional)
</ParamField>

## Response

<ResponseField name="success" type="boolean">
  Indica se a operação foi bem-sucedida
</ResponseField>

<ResponseField name="data.sessionId" type="string">
  ID da sessão criada. Use este valor no header X-Session-Id nas próximas requisições
</ResponseField>

<ResponseField name="data.authorization" type="string">
  Token de autorização Bearer
</ResponseField>

<ResponseField name="data.authorizationCorban" type="string">
  Token de autorização Corban Bearer
</ResponseField>

<ResponseField name="data.refreshToken" type="string">
  Token para renovação da sessão
</ResponseField>

## Exemplo de Requisição

<CodeGroup>
```bash cURL
curl -X POST http://localhost:5000/api/v1/autenticacao/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "seu_usuario",
    "password": "sua_senha",
    "reuseSession": true
  }'
```

```javascript JavaScript
const response = await fetch('http://localhost:5000/api/v1/autenticacao/login', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    username: 'seu_usuario',
    password: 'sua_senha',
    reuseSession: true
  })
});

const data = await response.json();
console.log(data);
```

```json JSON
{
  "username": "seu_usuario",
  "password": "sua_senha",
  "reuseSession": true
}
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

## Notas Importantes

<Warning>
  Guarde o `sessionId` retornado! Você precisará dele para todas as requisições autenticadas usando o header `X-Session-Id`.
</Warning>

<Info>
  O parâmetro `reuseSession` permite reutilizar uma sessão existente se o usuário já estiver autenticado, evitando criar múltiplas sessões.
</Info>
