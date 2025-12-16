---
title: "Tratamento de Erros - Introdução"
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

## Próximos Passos

<CardGroup cols={2}>
  <Card
    title="Códigos de Erro"
    icon="triangle-exclamation"
    href="/tratamento-erros/codigos-erro"
  >
    Veja detalhes sobre cada código de erro
  </Card>
  <Card
    title="Exemplos"
    icon="code"
    href="/tratamento-erros/exemplos"
  >
    Exemplos práticos de tratamento de erros
  </Card>
</CardGroup>
