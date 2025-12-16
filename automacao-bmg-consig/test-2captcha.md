---
title: "Testar 2Captcha"
api: "GET /api/v1/bmg-consig/test-2captcha"
description: "Testa se a API key do 2Captcha está configurada corretamente"
---

## Endpoint

<Endpoint method="get" url="/api/v1/bmg-consig/test-2captcha" />

## Descrição

Verifica se a API key do 2Captcha está configurada e acessível. Útil para validar a configuração antes de usar o login automatizado.

## Response

<ResponseField name="success" type="boolean">
  Indica se a API key está configurada
</ResponseField>

<ResponseField name="message" type="string">
  Mensagem de status
</ResponseField>

<ResponseField name="apiKeyPreview" type="string">
  Prévia da API key (primeiros e últimos caracteres)
</ResponseField>

## Exemplo de Requisição

<CodeGroup>

```bash cURL
curl -X GET http://localhost:5000/api/v1/bmg-consig/test-2captcha
```


```javascript JavaScript
const response = await fetch('http://localhost:5000/api/v1/bmg-consig/test-2captcha', {
  method: 'GET'
});

const data = await response.json();
console.log(data);
```

</CodeGroup>

## Exemplo de Resposta

```json
{
  "success": true,
  "message": "API key do 2Captcha está configurada",
  "apiKeyPreview": "b09b1e93...8b4b"
}
```

<Info>
  Este endpoint não faz requisições reais ao 2Captcha, apenas verifica se a chave está configurada.
</Info>