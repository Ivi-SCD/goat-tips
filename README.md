
<div align="center">

# Goat Tips

Plataforma de análise preditiva para futebol com dados ao vivo, modelo estatístico, agentes de IA e interface web.

[![Status](https://img.shields.io/badge/status-MVP%20Desenvolvido-0a7ea4?style=for-the-badge)](https://github.com/Ivi-SCD/goat-tips)
[![Stars](https://img.shields.io/github/stars/Ivi-SCD/goat-tips?style=for-the-badge)](https://github.com/Ivi-SCD/goat-tips/stargazers)
[![License](https://img.shields.io/badge/license-MIT-111111?style=for-the-badge)](./LICENSE)
[![Backend](https://img.shields.io/badge/backend-online-198754?style=for-the-badge)](https://goat-tips-backend-api.27s4ihbbhmjf.us-east.codeengine.appdomain.cloud)
[![Frontend](https://img.shields.io/badge/frontend-online-198754?style=for-the-badge)](https://goat-tips.vercel.app/)

</div>

## Visao geral

O `Goat Tips` é ima plataforma focada em analise inteligente de partidas da Premier League. O projeto combina ingestao de dados esportivos em tempo real, base historica consolidada, modelo Poisson para probabilidades de placar e agentes com LLM para gerar respostas narrativas em portugues.

Hoje, o repositorio centraliza quatro frentes principais:

- `backend`: API principal com FastAPI, analytics historico, previsoes e integracao com Telegram.
- `frontend`: aplicacao Next.js para consumo da API e experiencia do usuario.
- `daily fetch`: modulo de sincronizacao automatica de partidas encerradas para a base de dados.
- `retrain model`: modulo de retreinamento do modelo estatistico e publicacao do artefato atualizado.

## Objetivo do projeto

O objetivo do `Goat Tips` e transformar dados brutos de futebol em leitura acionavel. Em vez de mostrar apenas placares, odds e estatisticas isoladas, a plataforma busca responder perguntas como:

- Qual e a probabilidade mais realista para um jogo antes e durante a partida?
- Como a forma recente, o contexto historico e o momento do jogo influenciam a previsao?
- Como entregar isso em uma API e em canais conversacionais, como web e Telegram?

## Informacoes centrais

| Item | Valor |
|---|---|
| Status do repositorio | Em desenvolvimento |
| Licenca | MIT |
| Backend em producao | `https://goat-tips-backend-api.27s4ihbbhmjf.us-east.codeengine.appdomain.cloud` |
| Documentacao interativa da API | `https://goat-tips-backend-api.27s4ihbbhmjf.us-east.codeengine.appdomain.cloud/docs` |
| Frontend | Sera adicionado posteriormente |
| Stack principal | FastAPI, Next.js, Supabase, BetsAPI, Groq, IBM Cloud |
| Dominio de negocio | Analise preditiva da Premier League |

## Arquitetura resumida

```text
Frontend (Next.js)
    |
    v
Backend API (FastAPI)
    |-- /matches       -> dados ao vivo da BetsAPI
    |-- /predictions   -> modelo Poisson + agentes LangGraph + LLM
    |-- /analytics     -> historico de partidas e metricas
    |-- /telegram      -> interface conversacional
    |
    +-- Supabase       -> persistencia historica e sessoes
    +-- IBM Cloud      -> deploy da API e artefatos do modelo
    +-- Jobs auxiliares
        |-- daily fetch   -> sincronizacao diaria
        +-- retrain model -> retreinamento periodico
```

## Modulos do repositorio

| Modulo | Papel no ecossistema | Stack / execucao | Link |
|---|---|---|---|
| `goat-tips-backend` | Nucleo do produto. Expoe endpoints de partidas, previsoes, analytics, perguntas livres e Telegram. | FastAPI, LangGraph, Groq, Supabase, IBM Code Engine | [README](./goat-tips-backend/README.md) |
| `goat-tips-frontend` | Interface web da plataforma. Atualmente e uma base Next.js pronta para evolucao do produto. | Next.js, React, TypeScript | [README](./goat-tips-frontend/README.md) |
| `goat-tips-app-daily-fetch` | Servico de ingestao diaria que busca partidas encerradas e faz upsert no banco. | Azure Functions, BetsAPI, psycopg2, Supabase | codigo no modulo |
| `goat-tips-app-retrain-model` | Job de retreinamento do modelo Poisson com enriquecimento de dados e publicacao do artefato. | Python, pandas, joblib, Supabase | codigo no modulo |

## Detalhamento dos submodulos

### 1. Backend principal

O modulo [`goat-tips-backend`](./goat-tips-backend/README.md) concentra a inteligencia principal da plataforma.

Principais responsabilidades:

- Expor a API publica consumida pelo frontend e por clientes externos.
- Integrar dados ao vivo da BetsAPI para partidas, odds, lineup e H2H.
- Calcular previsoes com modelo Poisson e variacoes in-play.
- Gerar narrativas explicativas em portugues com LLM.
- Disponibilizar analytics historico com base em milhares de jogos da Premier League.
- Manter historico de conversa por sessao e integracao com Telegram.

Capacidades destacadas:

- Previsao pre-jogo e ao vivo.
- Analise completa por agente LangGraph.
- Endpoint de perguntas livres sobre partidas e liga.
- Ajustes por arbitro, clima, xG e contexto de jogo.
- Backtesting e calibracao do modelo.

URL atual do backend:

- `https://goat-tips-backend-api.27s4ihbbhmjf.us-east.codeengine.appdomain.cloud`
- `https://goat-tips-backend-api.27s4ihbbhmjf.us-east.codeengine.appdomain.cloud/docs`

### 2. Frontend

O modulo [`goat-tips-frontend`](./goat-tips-frontend/README.md) e a camada de apresentacao do projeto. Pelo README atual, ele foi inicializado com Next.js e esta preparado para rodar localmente durante o desenvolvimento.

Papel esperado no produto:

- Consumir os endpoints do backend.
- Exibir jogos, previsoes, narrativas e indicadores da analise.
- Concentrar a experiencia web do `Goat Tips`.

Observacao:

- O link de producao do frontend ainda nao foi definido neste README raiz, conforme solicitado.

### 3. Daily Fetch

O modulo `goat-tips-app-daily-fetch` funciona como a camada de sincronizacao operacional do projeto.

Responsabilidades identificadas no codigo:

- Executar uma rotina diaria as `03:00 UTC`.
- Buscar partidas encerradas da Premier League na BetsAPI.
- Fazer upsert de times, eventos, estatisticas e odds no Supabase.
- Disponibilizar um gatilho HTTP para refresh manual e backfill.

Pontos tecnicos importantes:

- Usa Azure Functions como mecanismo de disparo.
- Se conecta ao banco via `SUPABASE_DB_URL`.
- Usa `BETSAPI_TOKEN` para consulta dos dados.
- E um modulo auxiliar critico para manter a base historica atualizada.

### 4. Retrain Model

O modulo `goat-tips-app-retrain-model` e responsavel por manter o modelo de previsao atualizado.

Responsabilidades identificadas no codigo:

- Ler partidas encerradas e xG a partir do Supabase.
- Retreinar o modelo Poisson com base historica consolidada.
- Enriquecer `team_strengths` com dados de jogadores provenientes de CSV no formato FBref/Kaggle.
- Serializar o artefato do modelo.
- Publicar o modelo em storage para consumo posterior pelo backend.

Pontos tecnicos importantes:

- Foi desenhado para execucao automatizada recorrente.
- Usa `pandas`, `joblib` e `psycopg2`.
- Trabalha com variaveis como `SUPABASE_DB_URL`, `IBM_COS_*` e `KAGGLE_PLAYERS_CSV`.

## Fluxo operacional do monorepo

```text
1. Daily fetch coleta partidas encerradas e atualiza o Supabase
2. Retrain model usa os dados consolidados para gerar um novo modelo
3. Backend carrega o artefato e expoe previsoes e narrativas
4. Frontend consome a API e entrega a experiencia final ao usuario
```

## Estrutura do repositorio

```text
goat-tips/
|-- README.md
|-- LICENSE
|-- goat-tips-backend/
|-- goat-tips-frontend/
|-- goat-tips-app-daily-fetch/
`-- goat-tips-app-retrain-model/
```

## Como comecar

### Backend

Consulte o guia do modulo:

- [goat-tips-backend/README.md](./goat-tips-backend/README.md)

### Frontend

Comandos basicos descritos no README atual:

```bash
cd goat-tips-frontend
npm run dev
```

### Observacao sobre configuracao

Cada modulo possui suas proprias variaveis de ambiente e estrategia de deploy. O backend e o modulo mais completo em termos de documentacao tecnica neste momento.

## Casos de uso cobertos

- Analise de partidas ao vivo da Premier League.
- Consulta de previsao de placar e probabilidades.
- Interpretacao narrativa de contexto esportivo em portugues.
- Consulta historica de forma, confronto direto e padroes de jogo.
- Automacao de ingestao e retreinamento do modelo.

## Referencias

- Repositorio principal: [Ivi-SCD/goat-tips](https://github.com/Ivi-SCD/goat-tips)
- Backend: [Ivi-SCD/goat-tips-backend](https://github.com/Ivi-SCD/goat-tips-backend)
- Frontend: [MarcosvBueno/goat-tips-frontend](https://github.com/MarcosvBueno/goat-tips-frontend)
- Daily fetch: [Ivi-SCD/goat-tips-app-daily-fetch](https://github.com/Ivi-SCD/goat-tips-app-daily-fetch)
- Retrain model: [Ivi-SCD/goat-tips-app-retrain-model](https://github.com/Ivi-SCD/goat-tips-app-retrain-model)
- Documentacao detalhada do backend: [README do backend](./goat-tips-backend/README.md)
- Documentacao inicial do frontend: [README do frontend](./goat-tips-frontend/README.md)

## Licenca

Este repositorio esta licenciado sob os termos da [MIT License](./LICENSE).
