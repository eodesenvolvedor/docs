---
title: "Múltiplas Entidades"
description: "Como consultar ofertas para várias entidades"
---

## Exemplo: Consultar Múltiplas Entidades

Este exemplo mostra como consultar ofertas para diferentes entidades em sequência.

<CodeGroup>
```javascript JavaScript
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
</CodeGroup>

## Usando Promise.all para Paralelismo

<CodeGroup>
```javascript JavaScript Paralelo
const sessionId = 'seu_session_id';
const cpf = '92899617753';
const matricula = '1877250560';

const entidades = ['1581', '4277', '3'];

// Consultar todas as entidades em paralelo
const promises = entidades.map(entidade =>
  fetch(
    `http://localhost:5000/api/v1/bmg/${entidade}/ofertas/cpf/${cpf}/matricula/${matricula}`,
    {
      method: 'GET',
      headers: {
        'X-Session-Id': sessionId
      }
    }
  ).then(res => res.json())
);

const resultados = await Promise.all(promises);
console.log('Resultados:', resultados);
```
</CodeGroup>

<Warning>
  Consultas paralelas podem sobrecarregar a API. Use com moderação e considere adicionar delays entre requisições.
</Warning>
