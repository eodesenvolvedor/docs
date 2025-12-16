---
title: "Ofertas"
description: "Consulte ofertas de crédito consignado para seus clientes"
---

## Visão Geral

A API permite consultar ofertas de crédito consignado disponíveis para seus clientes. Você pode consultar ofertas individuais ou processar múltiplas consultas em lote.

## Endpoints Disponíveis

<CardGroup cols={2}>
  <Card
    title="Consultar Ofertas"
    icon="gift"
    href="/ofertas/consultar-ofertas"
  >
    Consulta ofertas usando CPF e matrícula
  </Card>
  <Card
    title="Consultar Ofertas (Auto)"
    icon="wand-magic"
    href="/ofertas/consultar-ofertas-auto"
  >
    Busca cliente primeiro para obter data de nascimento
  </Card>
  <Card
    title="Consultar Lote"
    icon="layer-group"
    href="/ofertas/consultar-lote"
  >
    Consulta até 200 ofertas de uma vez
  </Card>
</CardGroup>

## Exemplo Rápido

```javascript
const sessionId = 'seu_session_id';

// Consultar ofertas
const ofertas = await fetch(
  'http://localhost:5000/api/v1/ofertas/cpf/12345678901/matricula/1234567890',
  {
    headers: { 'X-Session-Id': sessionId }
  }
);

// Consultar ofertas (auto - busca data de nascimento primeiro)
const ofertasAuto = await fetch(
  'http://localhost:5000/api/v1/ofertas/auto/cpf/12345678901/matricula/1234567890',
  {
    headers: { 'X-Session-Id': sessionId }
  }
);
```

<Info>
  A rota "auto" busca automaticamente os dados do cliente para obter a data de nascimento real, oferecendo maior precisão.
</Info>

<Warning>
  O limite máximo é de 200 consultas por requisição em consultas em lote.
</Warning>
