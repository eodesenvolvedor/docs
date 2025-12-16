---
title: "Clientes"
description: "Consulte dados de clientes individualmente ou em lote"
---

## Visão Geral

A API permite consultar dados completos de clientes usando o CPF. Você pode consultar um cliente por vez ou processar até 200 CPFs em uma única requisição.

## Endpoints Disponíveis

<CardGroup cols={2}>
  <Card
    title="Consultar Cliente"
    icon="user"
    href="/clientes/consultar-cliente"
  >
    Consulta dados de um cliente por CPF
  </Card>
  <Card
    title="Consultar Lote"
    icon="users"
    href="/clientes/consultar-lote"
  >
    Consulta até 200 CPFs de uma vez
  </Card>
</CardGroup>

## Exemplo Rápido

```javascript
const sessionId = 'seu_session_id';

// Consultar um cliente
const cliente = await fetch('http://localhost:5000/api/v1/clientes/12345678901', {
  headers: { 'X-Session-Id': sessionId }
});

// Consultar múltiplos clientes
const lote = await fetch('http://localhost:5000/api/v1/clientes/consultar-lote', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-Session-Id': sessionId
  },
  body: JSON.stringify({
    cpfs: ['12345678901', '98765432100']
  })
});
```

<Info>
  O CPF pode ser enviado com ou sem formatação. A API remove automaticamente pontos e traços.
</Info>

<Warning>
  O limite máximo é de 200 CPFs por requisição em consultas em lote.
</Warning>
