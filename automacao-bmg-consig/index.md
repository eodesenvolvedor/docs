---
title: "Automação BMG Consig"
description: "Login automatizado no BMG Consig com Puppeteer e 2Captcha"
---

## Visão Geral

A API oferece automação completa para login no BMG Consig usando Puppeteer para controlar o navegador e 2Captcha para resolver captchas automaticamente.

## Endpoints Disponíveis

<CardGroup cols={2}>
  <Card
    title="Login Automatizado"
    icon="robot"
    href="/automacao-bmg-consig/login-automatizado"
  >
    Realiza login automatizado com Puppeteer + 2Captcha
  </Card>
  <Card
    title="Testar 2Captcha"
    icon="vial"
    href="/automacao-bmg-consig/test-2captcha"
  >
    Verifica se a API key do 2Captcha está configurada
  </Card>
</CardGroup>

## Como Funciona

1. **Puppeteer** abre um navegador Chrome/Chromium
2. Navega até a página de login do BMG Consig
3. Preenche usuário e senha
4. Detecta o captcha e envia para **2Captcha**
5. Aguarda a resolução do captcha
6. Completa o login e captura os cookies

## Exemplo Rápido

```javascript
const response = await fetch('http://localhost:5000/api/v1/bmg-consig/login-automation', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    username: 'paula.famdin',
    password: 'Hoje1711*',
    headless: false,
    keepBrowserOpen: true
  })
});

const data = await response.json();
console.log('Cookies capturados:', data.data.cookies);
```

<Warning>
  Certifique-se de que a API key do 2Captcha está configurada corretamente no ambiente antes de usar.
</Warning>

<Info>
  O modo `headless: false` permite ver o navegador durante o processo, útil para debug.
</Info>
