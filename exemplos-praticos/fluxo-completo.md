---
title: "Fluxo Completo"
description: "Exemplo completo de uso da API do início ao fim"
---

## Exemplo: Fluxo Completo

Este exemplo mostra como fazer login, consultar ofertas e fazer logout.

<CodeGroup>
```javascript JavaScript Completo
// 1. Fazer Login
const loginResponse = await fetch('http://localhost:5000/api/v1/autenticacao/login', {
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

const loginData = await loginResponse.json();
const sessionId = loginData.data.sessionId;

console.log('✅ Login realizado! SessionId:', sessionId);

// 2. Consultar Ofertas para Entidade 1581
const ofertasResponse = await fetch(
  'http://localhost:5000/api/v1/bmg/1581/ofertas/cpf/92899617753/matricula/1877250560',
  {
    method: 'GET',
    headers: {
      'X-Session-Id': sessionId
    }
  }
);

const ofertasData = await ofertasResponse.json();
console.log('✅ Ofertas obtidas:', ofertasData);

// 3. Logout (opcional)
const logoutResponse = await fetch('http://localhost:5000/api/v1/autenticacao/logout', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-Session-Id': sessionId
  },
  body: JSON.stringify({
    sessionId: sessionId
  })
});

console.log('✅ Logout realizado!');
```
</CodeGroup>

## Passo a Passo

1. **Login**: Obtenha o `sessionId` fazendo login
2. **Consultar**: Use o `sessionId` para fazer requisições autenticadas
3. **Logout**: Remova a sessão quando terminar (opcional)

<Info>
  O `sessionId` é necessário para todas as requisições autenticadas. Guarde-o após o login.
</Info>
