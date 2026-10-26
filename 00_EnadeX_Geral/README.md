# EnadeX — visão geral do ecossistema

> Texto resumido de `projeto/Fases do projeto ENADEX.pdf`. Para o conteúdo completo, consulte o PDF.

## Problema

O ENADE é uma das principais fontes públicas para avaliação do ensino superior no Brasil. Ele reúne microdados de desempenho acadêmico, informações institucionais e variáveis socioeconômicas do Questionário do Estudante. Segundo a proposta, esses dados ainda são pouco aproveitados, por três lacunas:

1. **Fragmentação analítica:** a maioria dos estudos cruza variáveis isoladas (renda vs nota, tipo de IES vs nota) e não captura interações complexas nem padrões multidimensionais.
2. **Baixa reprodutibilidade:** as análises costumam usar scripts ad hoc, sem arquitetura padronizada, controle automatizado de variáveis e relatórios replicáveis.
3. **Ausência de ambientes interativos e interpretáveis:** gestores e pesquisadores não têm dashboards que permitam explorar os dados, descobrir perfis latentes e ler tendências temporais com explicabilidade.

## Proposta

O EnadeX desenvolve e avalia artefatos computacionais orientados a evidência para explorar os microdados do ENADE de forma integrada e explicável, seguindo a metodologia **Design Science Research (DSR)**.

O projeto é um ecossistema de TCCs interligados. Cada TCC desenvolve um artefato próprio, e todos compartilham:

- Base conceitual (ENADE + análise multivariada + interpretabilidade)
- Metodologia DSR
- Infraestrutura de ingestão e pré-processamento (ETL)
- Harmonização e padronização de variáveis
- Pipeline de avaliação comparativa (tradicional vs artefato)
- Conjunto de métricas compartilhadas (AUC/R², Silhouette, Lift, SHAP, ICs etc.)

## Subprojetos

| Projeto | Responsável | Foco |
|---|---|---|
| 1. [MultiENADE](../01_MultiENADE/README.md) | Danilo Fernandes Araujo Gama | Artefato exploratório integrado de descoberta de padrões: análise descritiva multivariada, regressão (Random Forest/Boosting), clusterização, regras associativas (Apriori), SHAP e relatórios de insights |
| 2. [E-XplainENADE](../02_E-XplainENADE/README.md) | João Pedro Ribeiro Batista Araujo | Arquitetura para modelagem estatística explicável e reproduzível: pipeline modular, engine de regressão configurável, seleção de variáveis, VIF, SHAP, relatório automático e interface de hipóteses |
| 3. [EduCluster](../03_EduCluster/README.md) | Paulo Gosik Mascarenhas Moita | Perfis acadêmicos latentes com Visual Analytics: K-Means/Hierárquica, Elbow/Silhouette, PCA/UMAP, caracterização automática e dashboard com filtros |
| 4. [ENADE-Time](../04_ENADE-Time/README.md) | Lucas Eduardo Tavares Costa | Análise temporal longitudinal: consolidação multianual, harmonização entre ciclos, ICs por ano, tendência (regressão temporal/LOESS) e séries temporais com filtros |

## Como os projetos se conectam

**Pipeline compartilhado (ETL/harmonização)**
- ENADE-Time consolida e harmoniza vários anos e pode alimentar todos os projetos.
- MultiENADE e EduCluster dependem de uma padronização robusta para manter as análises consistentes.
- E-XplainENADE padroniza o "modo de testar hipóteses" com variáveis controladas.

**Métricas compartilhadas**
- Supervisionados: R², AUC, RMSE
- Clusterização: Silhouette, estabilidade, variância explicada
- Regras associativas: lift, suporte, confiança
- Explicabilidade: SHAP e análise interpretativa
- Temporal: tendência, ICs, significância

**Retroalimentação**
- MultiENADE identifica as variáveis mais relevantes → orienta hipóteses testáveis no E-XplainENADE.
- E-XplainENADE fornece resultados reprodutíveis e explicáveis → valida relações sugeridas no MultiENADE.
- EduCluster produz perfis latentes → podem ser comparados longitudinalmente no ENADE-Time.
- ENADE-Time detecta mudanças temporais → sugere recortes e segmentações para MultiENADE e EduCluster.

## Infraestrutura compartilhada

A proposta define uma camada comum com sete blocos (detalhados no PDF, seção *Arquitetura de Infraestrutura Compartilhada do Ecossistema EnadeX*):

1. Ingestão e versionamento
2. ETL padronizado
3. Harmonização longitudinal
4. Dataset curado consolidado
5. Feature Store / views analíticas
6. Orquestração e reprodutibilidade (configuração `.json`, seeds, log de execução Exec-ID)
7. Camada de serviços analíticos (API)

## Conteúdo desta pasta

| Pasta | O que tem | Origem |
|---|---|---|
| `projeto/` | `Fases do projeto ENADEX.pdf` (proposta, fases DSR de cada subprojeto, arquitetura) e `EnadeX_Entregaveis_Semestre.pdf` (entregáveis mínimos e critérios A, B e C por projeto) | nuvem `Documentos/` |
| `disciplina/` | `Apresentação da materia.pptx` (regras e prazos do Projeto Tecnológico I e II), `Modelo de TCC.docx/.pdf` e `2026__ ARTIGO FINAL - MODELO.docx` | nuvem `Documentos/` e `EndadeX_doc/` |
| `integracao/` | Matriz de conhecimentos por subprojeto (exploração de dados, regressão, clusterização, SHAP, ETL, dashboard etc.), imagem de 11/03/2026 | nuvem (raiz) |
| `interface/` | Prompt e especificação da interface web (`1 Prompt Ajustado para.md`), protótipos `home.jpg` e `subprojeto.jpg`, `PLANO-REFATORACAO.md` da identidade visual e `telas/` com as capturas da home do site | `EndadeX_doc/` e `EnadeX_Frontend/` |
| `marca/` | `enadex-marca/` (kit da marca: logos SVG/PNG, manual HTML/PDF, CSS/fontes/favicons), `logo_enadex/`, `outros/` (versões anteriores do logo) e `terceiros/supabase/` | `TCC apresentação/enadex-marca/`, `EndadeX_doc/`, nuvem `Logo/` |
| `instituicao/` | Logos da ULBRA Palmas e dos cursos de Ciência da Computação e Engenharia de Software | `logos/` |
| `referencias/` | Trabalhos de referência e artigos de contexto (PDF) | nuvem `Trabalhos de referencia/` e `Documentos/Artigos de contexto/` |
| `dados_enade/` | Microdados do ENADE (INEP) de 2005, 2008, 2011, 2014, 2017 e 2021: `1.LEIA-ME/` (manual, dicionário, questionários) e `2.DADOS/` (fora do git) | nuvem `Biblioteca de dados Enade/` |

### Arquivos idênticos que não foram duplicados

- `Fases do projeto ENADEX.pdf` e `Apresentação da materia.pptx` existiam em `EndadeX_doc/` e na nuvem com o mesmo conteúdo; foi mantida uma cópia.
- `IMG-20260311-WA0007.jpg` e `WhatsApp Image 2026-03-11 at 18.48.37.jpeg` são o mesmo arquivo; foi mantido o segundo.
- O kit `enadex-marca/` é igual em `TCC apresentação/` e em `EnadeX_Frontend/Identidade visual/`, assim como o `manual-enadex.pdf`.
