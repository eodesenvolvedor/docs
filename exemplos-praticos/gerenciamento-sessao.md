---
title: "Gerenciamento de Sessão"
description: "Classe helper para gerenciar sessões de forma fácil"
---

## Classe BMGAPIClient

Uma classe completa para gerenciar autenticação e requisições.

<CodeGroup>
```javascript JavaScript
class BMGAPIClient {
  constructor(baseURL = 'http://localhost:5000') {
    this.baseURL = baseURL;
    this.sessionId = null;
  }

  async login(username, password) {
    const response = await fetch(`${this.baseURL}/api/v1/autenticacao/login`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        username,
        password,
        reuseSession: true
      })
    });

    const data = await response.json();
    
    if (data.success) {
      this.sessionId = data.data.sessionId;
      return data;
    } else {
      throw new Error('Falha no login');
    }
  }

  async consultarCliente(cpf) {
    if (!this.sessionId) {
      throw new Error('Faça login primeiro');
    }

    const response = await fetch(`${this.baseURL}/api/v1/clientes/${cpf}`, {
      method: 'GET',
      headers: {
        'X-Session-Id': this.sessionId
      }
    });

    return await response.json();
  }

  async consultarOfertasPorEntidade(entidade, cpf, matricula, orgao = null) {
    if (!this.sessionId) {
      throw new Error('Faça login primeiro');
    }

    let url = `${this.baseURL}/api/v1/bmg/${entidade}/ofertas/cpf/${cpf}/matricula/${matricula}`;
    if (orgao) {
      url += `?orgao=${orgao}`;
    }

    const response = await fetch(url, {
      method: 'GET',
      headers: {
        'X-Session-Id': this.sessionId
      }
    });

    return await response.json();
  }

  async logout() {
    if (!this.sessionId) {
      return;
    }

    await fetch(`${this.baseURL}/api/v1/autenticacao/logout`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'X-Session-Id': this.sessionId
      },
      body: JSON.stringify({
        sessionId: this.sessionId
      })
    });

    this.sessionId = null;
  }
}

// Uso:
const client = new BMGAPIClient();

try {
  // Login
  await client.login('seu_usuario', 'sua_senha');
  console.log('✅ Login realizado!');

  // Consultar cliente
  const cliente = await client.consultarCliente('12345678901');
  console.log('✅ Cliente:', cliente);

  // Consultar ofertas
  const ofertas = await client.consultarOfertasPorEntidade('1581', '92899617753', '1877250560');
  console.log('✅ Ofertas:', ofertas);

  // Logout
  await client.logout();
  console.log('✅ Logout realizado!');
} catch (error) {
  console.error('❌ Erro:', error);
}
```
</CodeGroup>

<Info>
  Esta classe gerencia automaticamente o `sessionId`, facilitando o uso da API.
</Info>
