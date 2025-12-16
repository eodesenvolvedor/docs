---
title: "Exemplos de Tratamento de Erros"
description: "Exemplos práticos de como tratar erros"
---

## Função com Tratamento Completo

<CodeGroup>

```javascript JavaScript
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

</CodeGroup>

## Tratamento com Retry

<CodeGroup>

```javascript JavaScript
async function consultarComRetry(url, options, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      const response = await fetch(url, options);
      
      if (response.ok) {
        return await response.json();
      }
      
      // Se for erro 401, não tente novamente
      if (response.status === 401) {
        throw new Error('Sessão expirada. Faça login novamente.');
      }
      
      // Para outros erros, tente novamente
      if (i < maxRetries - 1) {
        await new Promise(resolve => setTimeout(resolve, 1000 * (i + 1)));
        continue;
      }
      
      throw new Error(`Erro HTTP ${response.status}`);
    } catch (error) {
      if (i === maxRetries - 1) {
        throw error;
      }
      await new Promise(resolve => setTimeout(resolve, 1000 * (i + 1)));
    }
  }
}
```

</CodeGroup>

<Info>
  O retry automático pode ser útil para erros temporários, mas evite para erros 401 (não autorizado).
</Info>