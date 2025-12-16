---
title: "Login Automatizado"
api: "POST /api/v1/bmg-consig/login-automation"
description: "Realiza login automatizado no BMG Consig usando Puppeteer + 2Captcha"
---

## Endpoint

<Endpoint method="post" url="/api/v1/bmg-consig/login-automation" />

## Descrição

Realiza login automatizado no BMG Consig usando Puppeteer para controlar o navegador e 2Captcha para resolver o captcha automaticamente.

## Body Parameters

<ParamField body="username" type="string" required>
  Usuário do BMG Consig
</ParamField>

<ParamField body="password" type="string" required>
  Senha do BMG Consig
</ParamField>

<ParamField body="headless" default="false" type="boolean">
  Se o navegador deve rodar em modo headless (sem interface gráfica)
</ParamField>

<ParamField body="keepBrowserOpen" default="true" type="boolean">
  Se o navegador deve permanecer aberto após o login
</ParamField>

## Response

<ResponseField name="success" type="boolean">
  Indica se a operação foi bem-sucedida
</ResponseField>

<ResponseField name="message" type="string">
  Mensagem de sucesso
</ResponseField>

<ResponseField name="data.cookies" type="array">
  Array com todos os cookies da sessão
</ResponseField>

<ResponseField name="data.sessionInfo" type="object">
  Informações da sessão criada
</ResponseField>

<ResponseField name="data.url" type="string">
  URL final após o login
</ResponseField>

<ResponseField name="data.totalCookies" type="number">
  Total de cookies capturados
</ResponseField>

## Exemplo de Requisição

<CodeGroup>

```bash cURL
curl -X POST http://localhost:5000/api/v1/bmg-consig/login-automation \
  -H "Content-Type: application/json" \
  -d '{
    "username": "paula.famdin",
    "password": "Hoje1711*",
    "headless": false,
    "keepBrowserOpen": true
  }'
```


```javascript JavaScript
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

</CodeGroup>

## Exemplo de Resposta

```json
{
  "success": true,
  "message": "Login realizado com sucesso!",
  "data": {
    "cookies": [
      {
        "name": "sessionId",
        "value": "...",
        "domain": ".bmgconsig.com.br"
      }
    ],
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

<Info>
  O modo `headless: false` permite ver o navegador durante o processo, útil para debug.
</Info>

<Warning>
  Certifique-se de que a API key do 2Captcha está configurada corretamente no ambiente.
</Warning>