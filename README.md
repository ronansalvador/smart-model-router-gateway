# Integracao de APIs e LLMs

Projeto de estudo do modulo de integracao de APIs e modelos de linguagem.

O projeto disponibiliza uma API HTTP em TypeScript usando Fastify. A rota `POST /chat` recebe uma pergunta e usa o OpenRouter para gerar uma resposta com um modelo de linguagem.

## Tecnologias

- Node.js
- TypeScript
- Fastify
- OpenRouter SDK (`@openrouter/sdk`)
- npm

## Requisitos

- Node.js instalado
- npm instalado
- Uma chave de API do OpenRouter

## Instalacao

Clone o repositorio, entre na pasta do projeto e instale as dependencias:

```bash
npm install
```

Crie um arquivo `.env` na raiz do projeto com a chave da API:

```env
OPENROUTER_API_KEY=sua-chave-do-openrouter
```

O arquivo `.env` e ignorado pelo Git e nao deve ser versionado.

## Executando o projeto

Para iniciar o servidor em modo de desenvolvimento:

```bash
npm run dev
```

O servidor fica disponivel em `http://localhost:3000`.

O script usa o suporte nativo do Node.js para executar arquivos TypeScript, habilita o modo `watch` e inicia o inspector na porta `9229`. Se a porta `3000` ou `9229` ja estiver em uso, encerre a outra instancia antes de executar o comando novamente.

## Testes

Os testes usam `app.inject()` do Fastify e fazem chamadas reais ao OpenRouter. Por isso, a variavel `OPENROUTER_API_KEY` deve estar configurada antes de executar:

```bash
npm test
```

Para executar os testes em modo de observacao:

```bash
npm run test:dev
```

## Endpoint disponivel

### `POST /chat`

Recebe uma pergunta no corpo da requisicao.

Exemplo usando `curl`:

```bash
curl -X POST http://localhost:3000/chat \
  -H "Content-Type: application/json" \
  -d '{"question":"O que e inteligencia artificial?"}'
```

Resposta:

```json
{
  "model": "nvidia/nemotron-3.5-lightning:free",
  "content": "Uma resposta gerada pelo modelo de linguagem."
}
```

A propriedade `question` e obrigatoria e deve ser uma string com pelo menos 5 caracteres. Requisicoes que nao atendem a esse formato sao rejeitadas pela validacao de schema do Fastify. Falhas na comunicacao com o OpenRouter resultam em status HTTP `500`.

## Configuracao atual

O servico envia as requisicoes para o modelo `nvidia/nemotron-3.5-lightning:free` e configura o provider do OpenRouter para priorizar throughput. A temperatura e `0.2`, o limite de resposta e `100` tokens e o prompt do sistema e `You are a helpful assistant.`. Esses valores podem ser alterados em `src/config.ts`.

## Estrutura do projeto

```text
.
├── src/
│   ├── config.ts            # Configuracoes do servidor e dos modelos
│   ├── index.ts              # Inicializa o servidor e executa uma chamada interna de exemplo
│   ├── openRouterService.ts  # Encapsula a comunicacao com o OpenRouter
│   └── server.ts             # Configura o Fastify e define a rota POST /chat
├── tests/
│   └── router.e2e.test.ts    # Testes de integracao da rota e do roteamento
├── package.json              # Dependencias e scripts do projeto
├── tsconfig.json             # Configuracao do TypeScript
└── .gitignore                # Arquivos ignorados pelo Git
```

## Como a aplicacao deve funcionar

1. `src/index.ts` cria o servico do OpenRouter e a aplicacao Fastify.
2. O servidor escuta na porta `3000`, aceitando conexoes em `0.0.0.0`.
3. A rota `POST /chat` valida o corpo da requisicao.
4. `OpenRouterService` envia a pergunta e o prompt do sistema para os modelos configurados.
5. O OpenRouter seleciona os modelos conforme a estrategia de provider configurada, priorizando throughput.
6. Ao iniciar, `index.ts` tambem faz uma requisicao interna de exemplo usando `app.inject()` e imprime o status e o corpo da resposta no console.

## Proximos passos

- Melhorar o tratamento e o formato das respostas de erro.
- Permitir configurar modelos e parametros por variaveis de ambiente.
