---
title: "Guia Rápido"
description: "Comece a usar a API em 3 passos simples"
---

## 🚀 Início Rápido em 3 Passos

### 1️⃣ Fazer Login

Primeiro, você precisa fazer login para obter um `sessionId`:

```bash
curl -X POST http://localhost:5000/api/v1/autenticacao/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "seu_usuario",
    "password": "sua_senha"
  }'
```

<ResponseField name="sessionId" type="string">
  Guarde este valor! Você vai precisar dele para todas as outras requisições.
</ResponseField>

**Resposta**:
```json
{
  "success": true,
  "data": {
    "sessionId": "abc123xyz..."
  }
}
```

---

### 2️⃣ Consultar um Cliente

Use o `sessionId` obtido no passo 1:

```bash
curl -X GET http://localhost:5000/api/v1/clientes/12345678901 \
  -H "X-Session-Id: abc123xyz..."
```

---

### 3️⃣ Consultar Ofertas por Entidade

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

---

## 📋 Fluxo Completo de Uso

```
1. Login → Obter sessionId
   ↓
2. Usar sessionId em todas as requisições
   ↓
3. Consultar clientes ou ofertas
   ↓
4. Logout (opcional) → Remover sessão
```

---

## 🔑 Entidades Disponíveis

| Entidade | Descrição | Parâmetros Especiais |
|----------|-----------|---------------------|
| **1581** | Entidade 1581 | Nenhum |
| **4277** | Entidade 4277 | Nenhum |
| **3** | Entidade 3 | Nenhum |
| **164** | Entidade 164 | `?orgao=145` (obrigatório) |

---

## 💡 Próximos Passos

<CardGroup cols={2}>
  <Card
    title="Autenticação Completa"
    icon="key"
    href="/autenticacao/login"
  >
    Aprenda sobre login, refresh de sessão e logout
  </Card>
  <Card
    title="Consultar Clientes"
    icon="users"
    href="/clientes/consultar-cliente"
  >
    Veja como consultar dados de clientes
  </Card>
  <Card
    title="Consultar Ofertas"
    icon="gift"
    href="/ofertas/consultar-ofertas"
  >
    Aprenda a consultar ofertas de crédito
  </Card>
  <Card
    title="Exemplos Práticos"
    icon="code"
    href="/exemplos-praticos/fluxo-completo"
  >
    Veja exemplos completos de código
  </Card>
</CardGroup>
