---
title: "Consultas em Lote"
description: "Como processar múltiplas consultas de forma eficiente"
---

## Exemplo: Consultar Lote de Clientes

<CodeGroup>
```javascript JavaScript
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
</CodeGroup>

## Exemplo: Consultar Ofertas em Lote

<CodeGroup>
```javascript JavaScript
const sessionId = 'seu_session_id';

const consultas = [
  {
    cpf: '12345678901',
    matricula: '1234567890'
  },
  {
    cpf: '98765432100',
    matricula: '0987654321'
  },
  {
    cpf: '11122233344',
    matricula: '1112223334'
  }
];

const response = await fetch('http://localhost:5000/api/v1/ofertas/consultar-lote', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-Session-Id': sessionId
  },
  body: JSON.stringify({
    consultas: consultas
  })
});

const data = await response.json();

console.log(`✅ Total processado: ${data.data.totalProcessado}`);
console.log(`✅ Sucessos: ${data.data.totalSucesso}`);
console.log(`❌ Erros: ${data.data.totalErros}`);

// Processar resultados
data.data.resultados.forEach(resultado => {
  console.log(`CPF ${resultado.cpf} - Matrícula ${resultado.matricula}:`, resultado.dados);
});
```
</CodeGroup>

<Warning>
  O limite máximo é de 200 itens por requisição. Para processar mais, divida em múltiplas requisições.
</Warning>
