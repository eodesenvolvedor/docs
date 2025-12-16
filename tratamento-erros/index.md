---
title: "Tratamento de Erros"
description: "Aprenda a tratar erros e lidar com diferentes cenários"
---

## Visão Geral

A API retorna códigos de status HTTP padrão para indicar o resultado das requisições. É importante tratar esses erros adequadamente em sua aplicação.

## Códigos de Status

| Código | Significado | Descrição |
|--------|-------------|-----------|
| **200** | OK | Requisição bem-sucedida |
| **400** | Bad Request | Parâmetros inválidos ou faltando |
| **401** | Unauthorized | SessionId inválido ou não fornecido |
| **404** | Not Found | Rota não encontrada |
| **500** | Internal Server Error | Erro interno do servidor |

## Estrutura de Erro

Quando ocorre um erro, a resposta geralmente tem esta estrutura:

```json
{
  "success": false,
  "error": "Mensagem de erro descritiva"
}
```

## Páginas Disponíveis

<CardGroup cols={2}>
  <Card
    title="Códigos de Erro"
    icon="triangle-exclamation"
    href="/tratamento-erros/codigos-erro"
  >
    Detalhes sobre cada código de erro
  </Card>
  <Card
    title="Exemplos"
    icon="code"
    href="/tratamento-erros/exemplos"
  >
    Exemplos práticos de tratamento de erros
  </Card>
</CardGroup>

## Exemplo Básico

```javascript
try {
  const response = await fetch(url, options);
  const data = await response.json();
  
  if (!response.ok) {
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
