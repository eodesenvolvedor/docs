# Documentação Mintlify - API BMG Automação

## Estrutura Criada

Esta documentação está configurada para uso com [Mintlify](https://mintlify.com).

## Arquivos Principais

- `mint.json` - Configuração do Mintlify
- `introduction.md` - Página de introdução
- `quickstart.md` - Guia rápido de início

## Estrutura de Pastas

```
docs/
├── mint.json
├── introduction.md
├── quickstart.md
├── autenticacao/
│   ├── login.md
│   ├── refresh-session.md
│   ├── logout.md
│   └── sessoes-ativas.md
├── clientes/
│   ├── consultar-cliente.md
│   └── consultar-lote.md
├── ofertas/
│   ├── consultar-ofertas.md
│   ├── consultar-ofertas-auto.md
│   └── consultar-lote.md
├── ofertas-por-entidade/
│   ├── introducao.md
│   ├── entidade-1581.md
│   ├── entidade-4277.md
│   ├── entidade-3.md
│   └── entidade-164.md
├── automacao-bmg-consig/
│   ├── login-automatizado.md
│   └── test-2captcha.md
├── exemplos-praticos/
│   ├── fluxo-completo.md
│   ├── multiplas-entidades.md
│   ├── consultas-lote.md
│   └── gerenciamento-sessao.md
├── tratamento-erros/
│   ├── introducao.md
│   ├── codigos-erro.md
│   └── exemplos.md
└── rotas-legadas/
    └── introducao.md
```

## Como Usar

1. **Conectar ao Mintlify**:
   - Acesse [mintlify.com](https://mintlify.com)
   - Conecte seu repositório GitHub
   - Selecione a pasta `docs`

2. **Personalizar**:
   - Edite `mint.json` para ajustar cores, logo, etc.
   - Adicione logos em `/logo/` (opcional)
   - Adicione favicon em `/favicon.svg` (opcional)

3. **Deploy**:
   - O Mintlify faz deploy automático
   - Atualizações são feitas automaticamente ao fazer push

## Componentes Mintlify Usados

- `<Endpoint>` - Exibe endpoints da API
- `<ParamField>` - Documenta parâmetros
- `<ResponseField>` - Documenta respostas
- `<CodeGroup>` - Agrupa exemplos de código
- `<Card>` - Cards de navegação
- `<CardGroup>` - Grupos de cards
- `<Warning>` - Avisos importantes
- `<Info>` - Informações úteis

## Próximos Passos

1. Adicione logos personalizados
2. Configure domínio customizado (opcional)
3. Ajuste cores no `mint.json` conforme sua marca
4. Adicione mais exemplos conforme necessário
