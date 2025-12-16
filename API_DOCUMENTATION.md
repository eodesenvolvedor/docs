# 📚 Documentação Completa da API BMG Automação - FamTech

## 🚀 Informações Gerais

- **Base URL**: `http://localhost:5000`
- **Versão**: `2.0.0 - HARDCORE MODE 🚀`
- **Formato de Resposta**: JSON
- **Autenticação**: SessionId via Header `X-Session-Id` (obrigatório para a maioria dos endpoints)

---

## 🎯 Guia Rápido de Início

### Início Rápido em 3 Passos

#### 1️⃣ Fazer Login

Primeiro, você precisa fazer login para obter um `sessionId`:

```bash
curl -X POST http://localhost:5000/api/v1/autenticacao/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "seu_usuario",
    "password": "sua_senha"
  }'
```

**Resposta**:
```json
{
  "success": true,
  "data": {
    "sessionId": "abc123xyz..."
  }
}
```

**Guarde o `sessionId`! Você vai precisar dele para todas as outras requisições.**

#### 2️⃣ Consultar um Cliente

Use o `sessionId` obtido no passo 1:

```bash
curl -X GET http://localhost:5000/api/v1/clientes/12345678901 \
  -H "X-Session-Id: abc123xyz..."
```

#### 3️⃣ Consultar Ofertas por Entidade

Exemplo para entidade 1581:

```bash
curl -X GET "http://localhost:5000/api/v1/bmg/1581/ofertas/cpf/92899617753/matricula/1877250560" \
  -H "X-Session-Id: abc123xyz..."
```

Para entidade 164 (com orgao):

```bash
curl -X GET "http://localhost:5000/api/v1/bmg/164/ofertas/cpf/42895847134/matricula/1415407?orgao=145" \
  -H "X-Session-Id: abc123xyz..."
```

### 📋 Fluxo Completo de Uso

```
1. Login → Obter sessionId
   ↓
2. Usar sessionId em todas as requisições
   ↓
3. Consultar clientes ou ofertas
   ↓
4. Logout (opcional) → Remover sessão
```

### 🔑 Entidades Disponíveis

| Entidade | Descrição | Parâmetros Especiais |
|----------|-----------|---------------------|
| 1581 | Entidade 1581 | Nenhum |
| 4277 | Entidade 4277 | Nenhum |
| 3 | Entidade 3 | Nenhum |
| 164 | Entidade 164 | `?orgao=145` (obrigatório) |

---

## 📋 Índice Completo

1. [Autenticação](#-autenticação)
   - [Login](#1-login)
   - [Refresh Session](#2-refresh-session)
   - [Logout](#3-logout)
   - [Sessões Ativas](#4-sessões-ativas)

2. [Clientes](#-clientes)
   - [Consultar Cliente](#1-consultar-cliente)
   - [Consultar Lote](#2-consultar-lote)

3. [Ofertas](#-ofertas)
   - [Consultar Ofertas](#1-consultar-ofertas)
   - [Consultar Ofertas (Auto)](#2-consultar-ofertas-auto)
   - [Consultar Ofertas em Lote](#3-consultar-ofertas-em-lote)

4. [Ofertas por Entidade](#-ofertas-por-entidade)
   - [Consultar Ofertas por Entidade](#1-consultar-ofertas-por-entidade)

5. [Automação BMG Consig](#-automação-bmg-consig)
   - [Login Automatizado](#1-login-automatizado)
   - [Testar 2Captcha](#2-testar-2captcha)

6. [Rotas Legadas](#-rotas-legadas)

7. [Exemplos Práticos Avançados](#-exemplos-práticos-avançados)

8. [Tratamento de Erros](#-tratamento-de-erros)

9. [Dicas e Problemas Comuns](#-dicas-e-problemas-comuns)

---

## 🔐 Autenticação

### 1. Login

Realiza login automático e captura os tokens de autenticação.

**Endpoint**: `POST /api/v1/autenticacao/login`

**Body**:
```json
{
  "username": "seu_usuario",
  "password": "sua_senha",
  "reuseSession": true
}
```

**Resposta de Sucesso**:
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

#### Exemplo cURL:
```bash
curl -X POST http://localhost:5000/api/v1/autenticacao/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "seu_usuario",
    "password": "sua_senha",
    "reuseSession": true
  }'
```

#### Exemplo Postman:
1. **Method**: `POST`
2. **URL**: `http://localhost:5000/api/v1/autenticacao/login`
3. **Headers**:
   - `Content-Type`: `application/json`
4. **Body** (raw JSON):
```json
{
  "username": "seu_usuario",
  "password": "sua_senha",
  "reuseSession": true
}
```

#### Exemplo JavaScript:
```javascript
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

---

### 2. Refresh Session

Renova os tokens de uma sessão existente mantendo o mesmo sessionId.

**Endpoint**: `POST /api/v1/autenticacao/refresh-session`

**Body**:
```json
{
  "sessionId": "abc123...",
  "username": "seu_usuario",
  "password": "sua_senha"
}
```

**Resposta de Sucesso**:
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

#### Exemplo cURL:
```bash
curl -X POST http://localhost:5000/api/v1/autenticacao/refresh-session \
  -H "Content-Type: application/json" \
  -d '{
    "sessionId": "abc123...",
    "username": "seu_usuario",
    "password": "sua_senha"
  }'
```

#### Exemplo Postman:
1. **Method**: `POST`
2. **URL**: `http://localhost:5000/api/v1/autenticacao/refresh-session`
3. **Headers**:
   - `Content-Type`: `application/json`
4. **Body** (raw JSON):
```json
{
  "sessionId": "abc123...",
  "username": "seu_usuario",
  "password": "sua_senha"
}
```

#### Exemplo JavaScript:
```javascript
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

---

### 3. Logout

Remove a sessão do usuário.

**Endpoint**: `POST /api/v1/autenticacao/logout`

**Body** (opcional se enviar no header):
```json
{
  "sessionId": "seu_session_id"
}
```

**OU**

**Header**: `X-Session-Id: seu_session_id`

**Resposta de Sucesso**:
```json
{
  "success": true,
  "message": "Sessão removida com sucesso"
}
```

#### Exemplo cURL:
```bash
curl -X POST http://localhost:5000/api/v1/autenticacao/logout \
  -H "Content-Type: application/json" \
  -H "X-Session-Id: seu_session_id" \
  -d '{
    "sessionId": "seu_session_id"
  }'
```

#### Exemplo Postman:
1. **Method**: `POST`
2. **URL**: `http://localhost:5000/api/v1/autenticacao/logout`
3. **Headers**:
   - `Content-Type`: `application/json`
   - `X-Session-Id`: `seu_session_id`
4. **Body** (raw JSON):
```json
{
  "sessionId": "seu_session_id"
}
```

#### Exemplo JavaScript:
```javascript
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

---

### 4. Sessões Ativas

Lista todas as sessões ativas (debug/admin).

**Endpoint**: `GET /api/v1/autenticacao/sessoes-ativas`

**Resposta de Sucesso**:
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
      }
    ]
  }
}
```

#### Exemplo cURL:
```bash
curl -X GET http://localhost:5000/api/v1/autenticacao/sessoes-ativas
```

#### Exemplo Postman:
1. **Method**: `GET`
2. **URL**: `http://localhost:5000/api/v1/autenticacao/sessoes-ativas`
3. **Headers**: Nenhum necessário

#### Exemplo JavaScript:
```javascript
const response = await fetch('http://localhost:5000/api/v1/autenticacao/sessoes-ativas', {
  method: 'GET'
});

const data = await response.json();
console.log(data);
```

---

## 👤 Clientes

### 1. Consultar Cliente

Consulta dados de um cliente por CPF.

**Endpoint**: `GET /api/v1/clientes/:cpf`

**Parâmetros**:
- `cpf` (path): CPF do cliente (com ou sem formatação)

**Headers**:
- `X-Session-Id`: `seu_session_id` (obrigatório)

**Resposta de Sucesso**:
```json
{
  "success": true,
  "data": {
    "cliente": {
      "cpf": "12345678901",
      "nome": "João Silva",
      "dataNascimento": "1990-01-01T00:00:00-03:00",
      "renda": 5000.00
    }
  }
}
```

#### Exemplo cURL:
```bash
curl -X GET http://localhost:5000/api/v1/clientes/12345678901 \
  -H "X-Session-Id: seu_session_id"
```

#### Exemplo Postman:
1. **Method**: `GET`
2. **URL**: `http://localhost:5000/api/v1/clientes/12345678901`
3. **Headers**:
   - `X-Session-Id`: `seu_session_id`

#### Exemplo JavaScript:
```javascript
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

---

### 2. Consultar Lote

Consulta até 200 CPFs de uma vez.

**Endpoint**: `POST /api/v1/clientes/consultar-lote`

**Body**:
```json
{
  "cpfs": [
    "12345678901",
    "98765432100",
    "11122233344"
  ],
  "sessionId": "seu_session_id"
}
```

**OU**

**Headers**:
- `X-Session-Id`: `seu_session_id` (obrigatório se não enviar no body)

**Resposta de Sucesso**:
```json
{
  "success": true,
  "data": {
    "totalProcessado": 3,
    "totalSucesso": 3,
    "totalErros": 0,
    "resultados": [
      {
        "cpf": "12345678901",
        "dados": { ... }
      }
    ],
    "erros": []
  }
}
```

#### Exemplo cURL:
```bash
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

#### Exemplo Postman:
1. **Method**: `POST`
2. **URL**: `http://localhost:5000/api/v1/clientes/consultar-lote`
3. **Headers**:
   - `Content-Type`: `application/json`
   - `X-Session-Id`: `seu_session_id`
4. **Body** (raw JSON):
```json
{
  "cpfs": [
    "12345678901",
    "98765432100",
    "11122233344"
  ]
}
```

#### Exemplo JavaScript:
```javascript
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

---

## 🎯 Ofertas

### 1. Consultar Ofertas

Consulta ofertas de um cliente.

**Endpoint**: `GET /api/v1/ofertas/cpf/:cpf/matricula/:matricula`

**Parâmetros**:
- `cpf` (path): CPF do cliente
- `matricula` (path): Matrícula do cliente

**Headers**:
- `X-Session-Id`: `seu_session_id` (obrigatório)

**Resposta de Sucesso**:
```json
{
  "success": true,
  "data": {
    "ofertas": [ ... ]
  }
}
```

#### Exemplo cURL:
```bash
curl -X GET "http://localhost:5000/api/v1/ofertas/cpf/12345678901/matricula/1234567890" \
  -H "X-Session-Id: seu_session_id"
```

#### Exemplo Postman:
1. **Method**: `GET`
2. **URL**: `http://localhost:5000/api/v1/ofertas/cpf/12345678901/matricula/1234567890`
3. **Headers**:
   - `X-Session-Id`: `seu_session_id`

#### Exemplo JavaScript:
```javascript
const cpf = '12345678901';
const matricula = '1234567890';
const response = await fetch(`http://localhost:5000/api/v1/ofertas/cpf/${cpf}/matricula/${matricula}`, {
  method: 'GET',
  headers: {
    'X-Session-Id': 'seu_session_id'
  }
});

const data = await response.json();
console.log(data);
```

---

### 2. Consultar Ofertas (Auto)

Busca cliente primeiro para obter data de nascimento e depois consulta ofertas.

**Endpoint**: `GET /api/v1/ofertas/auto/cpf/:cpf/matricula/:matricula`

**Parâmetros**:
- `cpf` (path): CPF do cliente
- `matricula` (path): Matrícula do cliente

**Headers**:
- `X-Session-Id`: `seu_session_id` (obrigatório)

**Resposta de Sucesso**:
```json
{
  "success": true,
  "data": {
    "ofertas": [ ... ]
  }
}
```

#### Exemplo cURL:
```bash
curl -X GET "http://localhost:5000/api/v1/ofertas/auto/cpf/12345678901/matricula/1234567890" \
  -H "X-Session-Id: seu_session_id"
```

#### Exemplo Postman:
1. **Method**: `GET`
2. **URL**: `http://localhost:5000/api/v1/ofertas/auto/cpf/12345678901/matricula/1234567890`
3. **Headers**:
   - `X-Session-Id`: `seu_session_id`

#### Exemplo JavaScript:
```javascript
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

---

### 3. Consultar Ofertas em Lote

Consulta ofertas de até 200 CPFs/matrículas de uma vez.

**Endpoint**: `POST /api/v1/ofertas/consultar-lote`

**Body**:
```json
{
  "consultas": [
    {
      "cpf": "12345678901",
      "matricula": "1234567890"
    },
    {
      "cpf": "98765432100",
      "matricula": "0987654321"
    }
  ],
  "sessionId": "seu_session_id"
}
```

**OU**

**Headers**:
- `X-Session-Id`: `seu_session_id` (obrigatório se não enviar no body)

**Resposta de Sucesso**:
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
        "dados": { ... }
      }
    ],
    "erros": []
  }
}
```

#### Exemplo cURL:
```bash
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

#### Exemplo Postman:
1. **Method**: `POST`
2. **URL**: `http://localhost:5000/api/v1/ofertas/consultar-lote`
3. **Headers**:
   - `Content-Type`: `application/json`
   - `X-Session-Id`: `seu_session_id`
4. **Body** (raw JSON):
```json
{
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
}
```

#### Exemplo JavaScript:
```javascript
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

---

## 🏢 Ofertas por Entidade

### 1. Consultar Ofertas por Entidade

Consulta ofertas de um cliente para uma entidade específica. Busca automaticamente os dados do cliente para obter a data de nascimento.

**Endpoint**: `GET /api/v1/bmg/:entidade/ofertas/cpf/:cpf/matricula/:matricula`

**Parâmetros**:
- `entidade` (path): Código da entidade (1581, 4277, 3, 164, etc.)
- `cpf` (path): CPF do cliente
- `matricula` (path): Matrícula do cliente
- `orgao` (query, opcional): Código do órgão (necessário para entidade 164)

**Headers**:
- `X-Session-Id`: `seu_session_id` (obrigatório)

**Entidades Disponíveis**:
- `1581` - Entidade 1581
- `4277` - Entidade 4277
- `3` - Entidade 3
- `164` - Entidade 164 (requer parâmetro `orgao`)

**Resposta de Sucesso**:
```json
{
  "success": true,
  "data": {
    "ofertas": [ ... ]
  }
}
```

#### Exemplo cURL - Entidade 1581:
```bash
curl -X GET "http://localhost:5000/api/v1/bmg/1581/ofertas/cpf/92899617753/matricula/1877250560" \
  -H "X-Session-Id: seu_session_id"
```

#### Exemplo cURL - Entidade 4277:
```bash
curl -X GET "http://localhost:5000/api/v1/bmg/4277/ofertas/cpf/92899617753/matricula/1877250560" \
  -H "X-Session-Id: seu_session_id"
```

#### Exemplo cURL - Entidade 3:
```bash
curl -X GET "http://localhost:5000/api/v1/bmg/3/ofertas/cpf/92899617753/matricula/1877250560" \
  -H "X-Session-Id: seu_session_id"
```

#### Exemplo cURL - Entidade 164 (com orgao):
```bash
curl -X GET "http://localhost:5000/api/v1/bmg/164/ofertas/cpf/42895847134/matricula/1415407?orgao=145" \
  -H "X-Session-Id: seu_session_id"
```

#### Exemplo Postman - Entidade 1581:
1. **Method**: `GET`
2. **URL**: `http://localhost:5000/api/v1/bmg/1581/ofertas/cpf/92899617753/matricula/1877250560`
3. **Headers**:
   - `X-Session-Id`: `seu_session_id`

#### Exemplo Postman - Entidade 164 (com orgao):
1. **Method**: `GET`
2. **URL**: `http://localhost:5000/api/v1/bmg/164/ofertas/cpf/42895847134/matricula/1415407?orgao=145`
3. **Headers**:
   - `X-Session-Id`: `seu_session_id`

#### Exemplo JavaScript - Entidade 1581:
```javascript
const entidade = '1581';
const cpf = '92899617753';
const matricula = '1877250560';
const response = await fetch(`http://localhost:5000/api/v1/bmg/${entidade}/ofertas/cpf/${cpf}/matricula/${matricula}`, {
  method: 'GET',
  headers: {
    'X-Session-Id': 'seu_session_id'
  }
});

const data = await response.json();
console.log(data);
```

#### Exemplo JavaScript - Entidade 164 (com orgao):
```javascript
const entidade = '164';
const cpf = '42895847134';
const matricula = '1415407';
const orgao = '145';
const response = await fetch(`http://localhost:5000/api/v1/bmg/${entidade}/ofertas/cpf/${cpf}/matricula/${matricula}?orgao=${orgao}`, {
  method: 'GET',
  headers: {
    'X-Session-Id': 'seu_session_id'
  }
});

const data = await response.json();
console.log(data);
```

---

## 🤖 Automação BMG Consig

### 1. Login Automatizado

Realiza login automatizado no BMG Consig usando Puppeteer + 2Captcha.

**Endpoint**: `POST /api/v1/bmg-consig/login-automation`

**Body**:
```json
{
  "username": "paula.famdin",
  "password": "Hoje1711*",
  "headless": false,
  "keepBrowserOpen": true
}
```

**Parâmetros**:
- `username` (obrigatório): Usuário do BMG Consig
- `password` (obrigatório): Senha do BMG Consig
- `headless` (opcional): Se o navegador deve rodar em modo headless (padrão: `false`)
- `keepBrowserOpen` (opcional): Se o navegador deve permanecer aberto (padrão: `true`)

**Resposta de Sucesso**:
```json
{
  "success": true,
  "message": "Login realizado com sucesso!",
  "data": {
    "cookies": [ ... ],
    "sessionInfo": {
      "sessionId": "...",
      "ip": "...",
      "sessionInterval": "..."
    },
    "url": "https://www.bmgconsig.com.br/principal/...",
    "totalCookies": 10
  }
}
```

#### Exemplo cURL:
```bash
curl -X POST http://localhost:5000/api/v1/bmg-consig/login-automation \
  -H "Content-Type: application/json" \
  -d '{
    "username": "paula.famdin",
    "password": "Hoje1711*",
    "headless": false,
    "keepBrowserOpen": true
  }'
```

#### Exemplo Postman:
1. **Method**: `POST`
2. **URL**: `http://localhost:5000/api/v1/bmg-consig/login-automation`
3. **Headers**:
   - `Content-Type`: `application/json`
4. **Body** (raw JSON):
```json
{
  "username": "paula.famdin",
  "password": "Hoje1711*",
  "headless": false,
  "keepBrowserOpen": true
}
```

#### Exemplo JavaScript:
```javascript
const response = await fetch('http://localhost:5000/api/v1/bmg-consig/login-automation', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    username: 'paula.famdin',
    password: 'Hoje1711*',
    headless: false,
    keepBrowserOpen: true
  })
});

const data = await response.json();
console.log(data);
```

---

### 2. Testar 2Captcha

Testa se a API key do 2Captcha está configurada corretamente.

**Endpoint**: `GET /api/v1/bmg-consig/test-2captcha`

**Resposta de Sucesso**:
```json
{
  "success": true,
  "message": "API key do 2Captcha está configurada",
  "apiKeyPreview": "b09b1e93...8b4b"
}
```

#### Exemplo cURL:
```bash
curl -X GET http://localhost:5000/api/v1/bmg-consig/test-2captcha
```

#### Exemplo Postman:
1. **Method**: `GET`
2. **URL**: `http://localhost:5000/api/v1/bmg-consig/test-2captcha`
3. **Headers**: Nenhum necessário

#### Exemplo JavaScript:
```javascript
const response = await fetch('http://localhost:5000/api/v1/bmg-consig/test-2captcha', {
  method: 'GET'
});

const data = await response.json();
console.log(data);
```

---

## 🔄 Rotas Legadas

As rotas abaixo são mantidas para compatibilidade, mas recomenda-se usar as novas rotas separadas por entidade.

### 1. Login (Legado)

**Endpoint**: `POST /api/v1/bmg/login`

Funciona igual à rota `/api/v1/autenticacao/login`.

#### Exemplo cURL:
```bash
curl -X POST http://localhost:5000/api/v1/bmg/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "seu_usuario",
    "password": "sua_senha",
    "reuseSession": true
  }'
```

#### Exemplo Postman:
1. **Method**: `POST`
2. **URL**: `http://localhost:5000/api/v1/bmg/login`
3. **Headers**:
   - `Content-Type`: `application/json`
4. **Body** (raw JSON):
```json
{
  "username": "seu_usuario",
  "password": "sua_senha",
  "reuseSession": true
}
```

#### Exemplo JavaScript:
```javascript
const response = await fetch('http://localhost:5000/api/v1/bmg/login', {
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

---

### 2. Refresh Session (Legado)

**Endpoint**: `POST /api/v1/bmg/refresh-session`

Funciona igual à rota `/api/v1/autenticacao/refresh-session`.

#### Exemplo cURL:
```bash
curl -X POST http://localhost:5000/api/v1/bmg/refresh-session \
  -H "Content-Type: application/json" \
  -d '{
    "sessionId": "abc123...",
    "username": "seu_usuario",
    "password": "sua_senha"
  }'
```

#### Exemplo Postman:
1. **Method**: `POST`
2. **URL**: `http://localhost:5000/api/v1/bmg/refresh-session`
3. **Headers**:
   - `Content-Type`: `application/json`
4. **Body** (raw JSON):
```json
{
  "sessionId": "abc123...",
  "username": "seu_usuario",
  "password": "sua_senha"
}
```

#### Exemplo JavaScript:
```javascript
const response = await fetch('http://localhost:5000/api/v1/bmg/refresh-session', {
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

---

### 3. Logout (Legado)

**Endpoint**: `POST /api/v1/bmg/logout`

Funciona igual à rota `/api/v1/autenticacao/logout`.

#### Exemplo cURL:
```bash
curl -X POST http://localhost:5000/api/v1/bmg/logout \
  -H "Content-Type: application/json" \
  -H "X-Session-Id: seu_session_id" \
  -d '{
    "sessionId": "seu_session_id"
  }'
```

#### Exemplo Postman:
1. **Method**: `POST`
2. **URL**: `http://localhost:5000/api/v1/bmg/logout`
3. **Headers**:
   - `Content-Type`: `application/json`
   - `X-Session-Id`: `seu_session_id`
4. **Body** (raw JSON):
```json
{
  "sessionId": "seu_session_id"
}
```

#### Exemplo JavaScript:
```javascript
const response = await fetch('http://localhost:5000/api/v1/bmg/logout', {
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

---

### 4. Sessões Ativas (Legado)

**Endpoint**: `GET /api/v1/bmg/sessoes-ativas`

Funciona igual à rota `/api/v1/autenticacao/sessoes-ativas`.

#### Exemplo cURL:
```bash
curl -X GET http://localhost:5000/api/v1/bmg/sessoes-ativas
```

#### Exemplo Postman:
1. **Method**: `GET`
2. **URL**: `http://localhost:5000/api/v1/bmg/sessoes-ativas`
3. **Headers**: Nenhum necessário

#### Exemplo JavaScript:
```javascript
const response = await fetch('http://localhost:5000/api/v1/bmg/sessoes-ativas', {
  method: 'GET'
});

const data = await response.json();
console.log(data);
```

---

### 5. Consultar Cliente (Legado)

**Endpoint**: `GET /api/v1/bmg/Cliente/cpf/:cpf`

Funciona igual à rota `/api/v1/clientes/:cpf`.

#### Exemplo cURL:
```bash
curl -X GET http://localhost:5000/api/v1/bmg/Cliente/cpf/12345678901 \
  -H "X-Session-Id: seu_session_id"
```

#### Exemplo Postman:
1. **Method**: `GET`
2. **URL**: `http://localhost:5000/api/v1/bmg/Cliente/cpf/12345678901`
3. **Headers**:
   - `X-Session-Id`: `seu_session_id`

#### Exemplo JavaScript:
```javascript
const cpf = '12345678901';
const response = await fetch(`http://localhost:5000/api/v1/bmg/Cliente/cpf/${cpf}`, {
  method: 'GET',
  headers: {
    'X-Session-Id': 'seu_session_id'
  }
});

const data = await response.json();
console.log(data);
```

---

### 6. Consultar Ofertas (Legado)

**Endpoint**: `GET /api/v1/bmg/ofertas/cpf/:cpf/matricula/:matricula`

Funciona igual à rota `/api/v1/ofertas/cpf/:cpf/matricula/:matricula`.

#### Exemplo cURL:
```bash
curl -X GET "http://localhost:5000/api/v1/bmg/ofertas/cpf/12345678901/matricula/1234567890" \
  -H "X-Session-Id: seu_session_id"
```

#### Exemplo Postman:
1. **Method**: `GET`
2. **URL**: `http://localhost:5000/api/v1/bmg/ofertas/cpf/12345678901/matricula/1234567890`
3. **Headers**:
   - `X-Session-Id`: `seu_session_id`

#### Exemplo JavaScript:
```javascript
const cpf = '12345678901';
const matricula = '1234567890';
const response = await fetch(`http://localhost:5000/api/v1/bmg/ofertas/cpf/${cpf}/matricula/${matricula}`, {
  method: 'GET',
  headers: {
    'X-Session-Id': 'seu_session_id'
  }
});

const data = await response.json();
console.log(data);
```

---

### 7. Consultar Ofertas Auto (Legado)

**Endpoint**: `GET /api/v1/bmg/ofertas-auto/cpf/:cpf/matricula/:matricula`

Funciona igual à rota `/api/v1/ofertas/auto/cpf/:cpf/matricula/:matricula`.

#### Exemplo cURL:
```bash
curl -X GET "http://localhost:5000/api/v1/bmg/ofertas-auto/cpf/12345678901/matricula/1234567890" \
  -H "X-Session-Id: seu_session_id"
```

#### Exemplo Postman:
1. **Method**: `GET`
2. **URL**: `http://localhost:5000/api/v1/bmg/ofertas-auto/cpf/12345678901/matricula/1234567890`
3. **Headers**:
   - `X-Session-Id`: `seu_session_id`

#### Exemplo JavaScript:
```javascript
const cpf = '12345678901';
const matricula = '1234567890';
const response = await fetch(`http://localhost:5000/api/v1/bmg/ofertas-auto/cpf/${cpf}/matricula/${matricula}`, {
  method: 'GET',
  headers: {
    'X-Session-Id': 'seu_session_id'
  }
});

const data = await response.json();
console.log(data);
```

---

### 8. Consultar Lote (Legado)

**Endpoint**: `POST /api/v1/bmg/consultar-lote`

Consulta até 200 CPFs de uma vez.

**Body**:
```json
{
  "cpfs": [
    "12345678901",
    "98765432100"
  ]
}
```

**Headers**:
- `X-Session-Id`: `seu_session_id` (obrigatório)

#### Exemplo cURL:
```bash
curl -X POST http://localhost:5000/api/v1/bmg/consultar-lote \
  -H "Content-Type: application/json" \
  -H "X-Session-Id: seu_session_id" \
  -d '{
    "cpfs": [
      "12345678901",
      "98765432100"
    ]
  }'
```

#### Exemplo Postman:
1. **Method**: `POST`
2. **URL**: `http://localhost:5000/api/v1/bmg/consultar-lote`
3. **Headers**:
   - `Content-Type`: `application/json`
   - `X-Session-Id`: `seu_session_id`
4. **Body** (raw JSON):
```json
{
  "cpfs": [
    "12345678901",
    "98765432100"
  ]
}
```

#### Exemplo JavaScript:
```javascript
const response = await fetch('http://localhost:5000/api/v1/bmg/consultar-lote', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-Session-Id': 'seu_session_id'
  },
  body: JSON.stringify({
    cpfs: [
      '12345678901',
      '98765432100'
    ]
  })
});

const data = await response.json();
console.log(data);
```

---

### 9. Consultar Ofertas Lote (Legado)

**Endpoint**: `POST /api/v1/bmg/consultar-ofertas-lote`

Consulta ofertas de até 200 CPFs/matrículas de uma vez.

**Body**:
```json
{
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
}
```

**Headers**:
- `X-Session-Id`: `seu_session_id` (obrigatório)

#### Exemplo cURL:
```bash
curl -X POST http://localhost:5000/api/v1/bmg/consultar-ofertas-lote \
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

#### Exemplo Postman:
1. **Method**: `POST`
2. **URL**: `http://localhost:5000/api/v1/bmg/consultar-ofertas-lote`
3. **Headers**:
   - `Content-Type`: `application/json`
   - `X-Session-Id`: `seu_session_id`
4. **Body** (raw JSON):
```json
{
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
}
```

#### Exemplo JavaScript:
```javascript
const response = await fetch('http://localhost:5000/api/v1/bmg/consultar-ofertas-lote', {
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

---

## 💻 Exemplos Práticos Avançados

### Exemplo 1: Fluxo Completo - Consultar Ofertas

```javascript
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

---

### Exemplo 2: Consultar Múltiplas Entidades

```javascript
const sessionId = 'seu_session_id'; // Obtenha através do login
const cpf = '92899617753';
const matricula = '1877250560';

const entidades = ['1581', '4277', '3'];

// Consultar ofertas para cada entidade
for (const entidade of entidades) {
  try {
    const response = await fetch(
      `http://localhost:5000/api/v1/bmg/${entidade}/ofertas/cpf/${cpf}/matricula/${matricula}`,
      {
        method: 'GET',
        headers: {
          'X-Session-Id': sessionId
        }
      }
    );

    const data = await response.json();
    console.log(`✅ Ofertas para entidade ${entidade}:`, data);
  } catch (error) {
    console.error(`❌ Erro ao consultar entidade ${entidade}:`, error);
  }
}
```

---

### Exemplo 3: Consultar Lote de Clientes

```javascript
const sessionId = 'seu_session_id';

const cpfs = [
  '12345678901',
  '98765432100',
  '11122233344',
  '55566677788'
];

const response = await fetch('http://localhost:5000/api/v1/clientes/consultar-lote', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-Session-Id': sessionId
  },
  body: JSON.stringify({
    cpfs: cpfs
  })
});

const data = await response.json();

console.log(`✅ Total processado: ${data.data.totalProcessado}`);
console.log(`✅ Sucessos: ${data.data.totalSucesso}`);
console.log(`❌ Erros: ${data.data.totalErros}`);

// Processar resultados
data.data.resultados.forEach(resultado => {
  console.log(`CPF ${resultado.cpf}:`, resultado.dados);
});

// Processar erros
if (data.data.erros.length > 0) {
  console.log('Erros encontrados:');
  data.data.erros.forEach(erro => {
    console.log(`CPF ${erro.cpf}: ${erro.erro}`);
  });
}
```

---

### Exemplo 4: Gerenciamento de Sessão com Classe

```javascript
class BMGAPIClient {
  constructor(baseURL = 'http://localhost:5000') {
    this.baseURL = baseURL;
    this.sessionId = null;
  }

  async login(username, password) {
    const response = await fetch(`${this.baseURL}/api/v1/autenticacao/login`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        username,
        password,
        reuseSession: true
      })
    });

    const data = await response.json();
    
    if (data.success) {
      this.sessionId = data.data.sessionId;
      return data;
    } else {
      throw new Error('Falha no login');
    }
  }

  async consultarCliente(cpf) {
    if (!this.sessionId) {
      throw new Error('Faça login primeiro');
    }

    const response = await fetch(`${this.baseURL}/api/v1/clientes/${cpf}`, {
      method: 'GET',
      headers: {
        'X-Session-Id': this.sessionId
      }
    });

    return await response.json();
  }

  async consultarOfertasPorEntidade(entidade, cpf, matricula, orgao = null) {
    if (!this.sessionId) {
      throw new Error('Faça login primeiro');
    }

    let url = `${this.baseURL}/api/v1/bmg/${entidade}/ofertas/cpf/${cpf}/matricula/${matricula}`;
    if (orgao) {
      url += `?orgao=${orgao}`;
    }

    const response = await fetch(url, {
      method: 'GET',
      headers: {
        'X-Session-Id': this.sessionId
      }
    });

    return await response.json();
  }

  async logout() {
    if (!this.sessionId) {
      return;
    }

    await fetch(`${this.baseURL}/api/v1/autenticacao/logout`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'X-Session-Id': this.sessionId
      },
      body: JSON.stringify({
        sessionId: this.sessionId
      })
    });

    this.sessionId = null;
  }
}

// Uso:
const client = new BMGAPIClient();

try {
  // Login
  await client.login('seu_usuario', 'sua_senha');
  console.log('✅ Login realizado!');

  // Consultar cliente
  const cliente = await client.consultarCliente('12345678901');
  console.log('✅ Cliente:', cliente);

  // Consultar ofertas
  const ofertas = await client.consultarOfertasPorEntidade('1581', '92899617753', '1877250560');
  console.log('✅ Ofertas:', ofertas);

  // Logout
  await client.logout();
  console.log('✅ Logout realizado!');
} catch (error) {
  console.error('❌ Erro:', error);
}
```

---

## ⚠️ Tratamento de Erros

### Exemplo de Tratamento Completo

```javascript
async function consultarOfertasComTratamentoErros(entidade, cpf, matricula, sessionId, orgao = null) {
  try {
    let url = `http://localhost:5000/api/v1/bmg/${entidade}/ofertas/cpf/${cpf}/matricula/${matricula}`;
    if (orgao) {
      url += `?orgao=${orgao}`;
    }

    const response = await fetch(url, {
      method: 'GET',
      headers: {
        'X-Session-Id': sessionId
      }
    });

    const data = await response.json();

    if (!response.ok) {
      // Tratar diferentes códigos de erro
      switch (response.status) {
        case 400:
          throw new Error(`Requisição inválida: ${data.error || 'Verifique os parâmetros'}`);
        case 401:
          throw new Error('SessionId inválido. Faça login novamente.');
        case 404:
          throw new Error('Rota não encontrada.');
        case 500:
          throw new Error('Erro interno do servidor.');
        default:
          throw new Error(`Erro HTTP ${response.status}: ${data.error || 'Erro desconhecido'}`);
      }
    }

    return data;
  } catch (error) {
    if (error instanceof TypeError) {
      // Erro de rede
      throw new Error('Erro de conexão. Verifique se o servidor está rodando.');
    }
    throw error;
  }
}

// Uso:
try {
  const ofertas = await consultarOfertasComTratamentoErros(
    '1581',
    '92899617753',
    '1877250560',
    'seu_session_id'
  );
  console.log('✅ Sucesso:', ofertas);
} catch (error) {
  console.error('❌ Erro:', error.message);
}
```

### Códigos de Erro

#### 400 - Bad Request
Requisição inválida. Verifique os parâmetros enviados.

#### 401 - Unauthorized
SessionId não fornecido ou inválido. Faça login novamente.

#### 404 - Not Found
Rota não encontrada.

#### 500 - Internal Server Error
Erro interno do servidor. Verifique os logs.

---

## 💡 Dicas e Problemas Comuns

### Dicas Importantes

1. **Sempre use o header `X-Session-Id`** nas requisições que precisam de autenticação
2. **CPF pode ser com ou sem formatação** - a API remove automaticamente
3. **Máximo de 200 itens** em consultas em lote
4. **Para entidade 164**, sempre adicione `?orgao=145` na URL

### Problemas Comuns

#### Erro 401 - Unauthorized
**Causa**: SessionId inválido ou não fornecido  
**Solução**: Faça login novamente e use o sessionId retornado

#### Erro 400 - Bad Request
**Causa**: Parâmetros inválidos ou faltando  
**Solução**: Verifique se todos os parâmetros obrigatórios foram enviados

#### Erro 500 - Internal Server Error
**Causa**: Erro no servidor  
**Solução**: Verifique os logs do servidor ou entre em contato com suporte

---

## 📝 Notas Importantes

1. **SessionId**: A maioria dos endpoints requer o header `X-Session-Id` com um sessionId válido obtido através do login.

2. **CPF**: Os CPFs podem ser enviados com ou sem formatação (pontos e traços). A API remove automaticamente a formatação.

3. **Limites**: 
   - Consultas em lote: máximo de 200 itens por requisição
   - Ofertas em lote: máximo de 200 consultas por requisição

4. **Entidades**: As entidades disponíveis são: 1581, 4277, 3, 164. A entidade 164 requer o parâmetro `orgao` como query param.

5. **Rotas Legadas**: As rotas em `/api/v1/bmg/*` são mantidas para compatibilidade, mas recomenda-se usar as novas rotas organizadas por entidade.

---

## 🆘 Suporte

Para dúvidas ou problemas, consulte os logs do servidor ou entre em contato com a equipe de desenvolvimento.

---

**Última atualização**: 2024  
**Versão da API**: 2.0.0 - HARDCORE MODE 🚀
