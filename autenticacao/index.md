---
title: "Autenticação"
description: "Gerencie sessões, faça login e renove tokens de autenticação"
---

## Visão Geral

A API utiliza um sistema de autenticação baseado em sessões. Você precisa fazer login para obter um `sessionId` que será usado em todas as requisições autenticadas.

## Fluxo de Autenticação

```
1. Login → Obter sessionId
   ↓
2. Usar sessionId no header X-Session-Id
   ↓
3. Refresh Session (opcional) → Renovar tokens
   ↓
4. Logout (opcional) → Remover sessão
```

## Endpoints Disponíveis

<CardGroup cols={2}>
  <Card
    title="Login"
    icon="sign-in"
    href="/autenticacao/login"
  >
    Realiza login e obtém sessionId
  </Card>
  <Card
    title="Refresh Session"
    icon="refresh"
    href="/autenticacao/refresh-session"
  >
    Renova tokens mantendo o sessionId
  </Card>
  <Card
    title="Logout"
    icon="sign-out"
    href="/autenticacao/logout"
  >
    Remove a sessão do usuário
  </Card>
  <Card
    title="Sessões Ativas"
    icon="list"
    href="/autenticacao/sessoes-ativas"
  >
    Lista todas as sessões ativas
  </Card>
</CardGroup>

## Exemplo Rápido

```javascript
// 1. Login
const loginResponse = await fetch('http://localhost:5000/api/v1/autenticacao/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    username: 'seu_usuario',
    password: 'sua_senha'
  })
});

const { data } = await loginResponse.json();
const sessionId = data.sessionId;

// 2. Usar sessionId nas próximas requisições
const response = await fetch('http://localhost:5000/api/v1/clientes/12345678901', {
  headers: { 'X-Session-Id': sessionId }
});
```

<Warning>
  Guarde o `sessionId` após o login! Você precisará dele para todas as requisições autenticadas.
</Warning>
