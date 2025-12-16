---
title: "Códigos de Erro"
description: "Detalhes sobre os códigos de erro da API"
---

## 400 - Bad Request

**Causa**: Requisição inválida. Parâmetros faltando ou inválidos.

**Exemplo de Resposta**:
```json
{
  "success": false,
  "error": "CPF e matrícula são obrigatórios"
}
```

**Solução**: Verifique se todos os parâmetros obrigatórios foram enviados e estão no formato correto.

---

## 401 - Unauthorized

**Causa**: SessionId inválido, expirado ou não fornecido.

**Exemplo de Resposta**:
```json
{
  "success": false,
  "error": "SessionId não fornecido. Faça login primeiro."
}
```

**Solução**: 
1. Faça login novamente para obter um novo `sessionId`
2. Verifique se o header `X-Session-Id` está sendo enviado corretamente
3. Use o `refresh-session` se os tokens expiraram

---

## 404 - Not Found

**Causa**: Rota não encontrada.

**Exemplo de Resposta**:
```json
{
  "success": false,
  "error": "Rota não encontrada"
}
```

**Solução**: Verifique se a URL está correta e se o método HTTP está adequado (GET, POST, etc).

---

## 500 - Internal Server Error

**Causa**: Erro interno do servidor.

**Exemplo de Resposta**:
```json
{
  "success": false,
  "error": "Erro desconhecido"
}
```

**Solução**: 
1. Verifique os logs do servidor
2. Tente novamente após alguns instantes
3. Entre em contato com suporte se o problema persistir

---

## Tratamento Recomendado

Sempre trate erros em suas requisições:

```javascript
try {
  const response = await fetch(url, options);
  const data = await response.json();
  
  if (!response.ok) {
    // Tratar erro baseado no status
    switch (response.status) {
      case 400:
        console.error('Erro de validação:', data.error);
        break;
      case 401:
        console.error('Sessão expirada, faça login novamente');
        break;
      case 404:
        console.error('Rota não encontrada');
        break;
      case 500:
        console.error('Erro no servidor');
        break;
    }
  }
} catch (error) {
  console.error('Erro de rede:', error);
}
```
