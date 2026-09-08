# Integracao de APIs e LLMs

Repositorio de estudo do modulo de integracao de APIs e modelos de linguagem.

Neste momento, o projeto contem uma API HTTP inicial em TypeScript usando Fastify. A rota `/chat` recebe uma pergunta e retorna uma resposta fixa. A integracao com um LLM ainda sera implementada.

## Tecnologias

- Node.js
- TypeScript
- Fastify
- npm

## Requisitos

- Node.js instalado
- npm instalado

## Instalacao

Clone o repositorio, entre na pasta do projeto e instale as dependencias:

```bash
npm install
```

## Executando o projeto

O comando configurado para iniciar o servidor em modo de desenvolvimento e:

```bash
npm run dev
```

O objetivo e disponibilizar o servidor em:

```text
http://localhost:3000
```

O modo de desenvolvimento tambem habilita o inspector do Node.js. No ambiente atual, com Node.js `v20.10.0`, esse comando ainda falha porque o Node.js nao executa arquivos `.ts` diretamente. E necessario configurar um runner TypeScript, como `tsx`, antes de executar a aplicacao.

## Endpoint disponivel

### `POST /chat`

Recebe uma pergunta no corpo da requisicao.

Exemplo usando `curl`:

```bash
curl -X POST http://localhost:3000/chat \
  -H "Content-Type: application/json" \
  -d '{"question":"O que e inteligencia artificial?"}'
```

Resposta atual:

```text
hello world
```

A propriedade `question` e obrigatoria e deve ser uma string com pelo menos um caractere. Requisicoes que nao atendem a esse formato sao rejeitadas pela validacao de schema do Fastify.

## Estrutura do projeto

```text
.
├── src/
│   ├── index.ts   # Inicializa o servidor e executa uma chamada interna de exemplo
│   └── server.ts  # Configura o Fastify e define a rota POST /chat
├── package.json   # Dependencias e scripts do projeto
├── tsconfig.json  # Configuracao do TypeScript
└── .gitignore     # Arquivos ignorados pelo Git
```

## Como a aplicacao deve funcionar

1. `src/index.ts` cria a aplicacao com `createServer()`.
2. O servidor escuta na porta `3000`, aceitando conexoes em `0.0.0.0`.
3. A rota `POST /chat` valida o corpo da requisicao.
4. A rota retorna a resposta fixa `hello world`.
5. Ao iniciar, o arquivo `index.ts` tambem faz uma requisicao interna de exemplo usando `app.inject()`.

## Proximos passos

- Integrar um provedor de LLM na rota `/chat`.
- Usar o valor de `question` para gerar a resposta.
- Configurar variaveis de ambiente para credenciais de API.
- Adicionar testes automatizados.
- Melhorar o tratamento e o formato das respostas de erro.

## Observacao

O script `npm test` ainda e apenas um placeholder e retorna erro ate que os testes sejam implementados.
