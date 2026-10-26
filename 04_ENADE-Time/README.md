# ENADE-Time

**ENADE-Time — Análise Temporal Interativa de Indicadores Acadêmicos com Base em Microdados do ENADE**
Projeto 4 do ecossistema [EnadeX](../00_EnadeX_Geral/README.md), de Lucas Eduardo Tavares Costa.

## Foco

> Conforme `00_EnadeX_Geral/projeto/Fases do projeto ENADEX.pdf`.

Construção de um ambiente computacional para exploração temporal ano a ano. O ENADE-Time é responsável por:

- Consolidação multianual de microdados
- Harmonização de variáveis entre ciclos avaliativos
- Agregação dinâmica por grupos
- Cálculo de médias e intervalos de confiança por ano
- Análise de tendência (regressão temporal/LOESS)
- Visualização em séries temporais com filtros
- Comparação longitudinal entre grupos
- Geração automática de insights de tendência

Ele resolve o problema prático da identificação de tendências estruturais e mudanças ao longo do tempo. Pelos entregáveis do semestre, o ENADE-Time é o responsável principal pelo **dicionário unificado de variáveis** do ecossistema.

As fases DSR deste projeto estão em `Fases do projeto ENADEX.pdf` (a partir da p. 43), e os entregáveis do semestre em `EnadeX_Entregaveis_Semestre.pdf` (seção *Projeto 4*).

## Conteúdo desta pasta

| Item | Descrição | Origem |
|---|---|---|
| `README_backend.md` | README do módulo (ENADE-Time Distribuído): objetivo, recorte, como rodar | `EnadeX_Backend/enade_time/README.md` |
| `docs/` | Documentação do módulo: arquitetura, dicionário de variáveis, design log, integração com o ecossistema, guias de execução e apresentação, benchmarks, evidências de testes etc. Inclui `README_frontend_react.md` e `NOTAS_ORGANIZACAO.md` do frontend próprio do módulo | `EnadeX_Backend/enade_time/docs/` e `enade_time/frontend/` |
| `dados_processados/` | CSV filtrados por edição (2005 a 2021), consolidado e análises | `EnadeX_Backend/enade_time/dados_processados/` |
| `interface/telas/` | Capturas das páginas do ENADE-Time no site (desktop claro/escuro e celular) | `EnadeX_Frontend/tests/e2e/enade-time.spec.ts-snapshots/` |

## Código

- Backend: `EnadeX_Backend/enade_time/`
- Frontend: `EnadeX_Frontend/app/enade-time/` e `EnadeX_Frontend/lib/enade-time/`
- Repositório original citado no README: `lucasetculbra/enade-time-distribuido` (GitHub)
