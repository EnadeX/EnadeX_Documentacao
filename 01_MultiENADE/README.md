# MultiENADE

**Mapeamento Multidimensional de Fatores Associados ao Desempenho no ENADE**
Projeto 1 do ecossistema [EnadeX](../00_EnadeX_Geral/README.md), de Danilo Fernandes Araujo Gama.

## Resumo

> Resumo de `artigo/artigo_multi_enade.md`.

O MultiENADE é o componente do EnadeX que funciona como **motor de descoberta multidimensional**. Ele integra regressão, agrupamento e regras de associação para analisar fatores associados ao desempenho de cursos de Ciência da Computação e Sistemas de Informação no ENADE 2021.

**Pergunta de pesquisa:** como analisar de forma integrada e multidimensional os fatores socioeconômicos, institucionais e acadêmicos associados ao desempenho no ENADE, superando abordagens isoladas e fragmentadas?

| Item | Valor |
|---|---|
| Metodologia | Quantitativa e exploratória, segundo princípios de Design Science Research |
| Dados | Microdados do ENADE 2021, grupos de curso 4004 e 4006, regiões Norte e Nordeste |
| Recorte | 190 cursos de origem; 161 cursos com pelo menos cinco respondentes (4.467 alunos com `NT_GER` válido) |
| Regressão | Random Forest: R² = 0,498, MAE = 3,03, RMSE = 3,82 no conjunto de teste (128 cursos de treino, 33 de teste) |
| Agrupamento | K-Means com três perfis institucionais, silhouette = 0,211 |
| Associação | Apriori: relações condicionais entre percepções acadêmicas e a faixa de desempenho dos cursos |
| Explicabilidade | Importância das variáveis e SHAP |

## Conteúdo desta pasta

```
01_MultiENADE/
├── artigo/                    textos do artigo do TCC
├── documento_apresentacao/    documento de apresentação do subprojeto
├── desenvolvimento/           diário de decisões, explorações dos modelos, lacunas
├── interface/                 especificação da tela e capturas do site
├── diagramas/                 fluxos, camadas de dados e mapeamento de arquivos
├── resultados/                CSV atuais dos três modelos e figuras
├── dados/                     bases refinadas usadas pelos modelos
├── historico/                 versões anteriores (proposta inicial e resultados antigos)
└── videos/                    gravações (fora do git)
```

### `artigo/`
| Arquivo | Descrição |
|---|---|
| `artigo_multi_enade.md` | Versão mais recente do artigo, de 25/10/2026 |
| `Artigo v1.0.md` | Versão do cofre Obsidian, de 12/10/2026. O texto é o mesmo do arquivo acima; só mudam as marcas de citação ao fim de alguns parágrafos e a formatação da tabela de clusters |

### `documento_apresentacao/`
| Arquivo | Descrição |
|---|---|
| `EnadeX - Danilo Fernandes.docx` | "Doc_apresentação": problema, objetivos, os quatro subprojetos, ferramentas, arquitetura, integração, telas e trechos de código |
| `Bem-vindo.md` | Mesmo conteúdo em Markdown (cofre Obsidian), com as imagens embutidas |

### `desenvolvimento/`
| Arquivo | Descrição |
|---|---|
| `DIARIO.md` | Diário de decisões (versão atual, do backend) |
| `EXPLORACAO_MODELO_REGRESSAO.md` | Exploração de variáveis e modelos de regressão |
| `EXPLORACAO_MODELO_CLUSTERS.md` | Exploração do modelo de clusters |
| `EXPLORACAO_MODELO_ASSOCIACAO.md` | Exploração do modelo de associação |
| `lacunas.txt` / `RESPOSTAS_LACUNAS.md` | Lacunas levantadas e respostas |
| `Anotações.md` | Notas sobre as métricas R², MAE e RMSE e o efeito de agregação por curso |

### `interface/`
- `A página do MultiENADE.txt`: especificação da tela do MultiENADE (layout, filtros e abas)
- `telas/`: capturas da página do MultiENADE no site (abas modelagem, agrupamento e regras; desktop claro/escuro e celular), tiradas dos testes e2e do `EnadeX_Frontend`

### `diagramas/`
Arquivos `.drawio` com as exportações `.png`: `camadas_processamento_dados` (bronze/prata/ouro), `fluxo_do_projeto`, `fluxo_seq_ambiente_usuario`, `mapeamento_de_arquivos` e `Dgr_enadex`. Em `excalidraw/` estão os desenhos do cofre Obsidian (estrutura de pastas e diagrama do projeto).

### `resultados/`
CSV copiados de `EnadeX_Backend/multi_enade/modelos/` (versão atual):

| Pasta | Arquivos |
|---|---|
| `mdl_regressao/` | `dados_regressao_refinados.csv`, `dados_regressao_pos_treino.csv`, `dados_shap_importancia.csv`, `metricas_regressao.csv` |
| `mdl_clusters/` | `dados_clusters.csv`, `dados_clusters_treinado.csv` |
| `mdl_associacao/` | `dados_associacao_binario.csv`, `dados_associacao_resultado.csv` |

`figuras/` tem os gráficos de dispersão da regressão (`Figure_1.png`, `Regressão_QE_I15.png`, `Regressão_QE_I63.png`), o SHAP (`Figure_2.png`, `SHARP.png`) e o mapa de clusters (`Clusters.png`).

### `dados/`
- `refinamento_dados/`: CSV refinados por arquivo do ENADE 2021 (`arq2` a `arq30`), copiados do backend
- `Modelo_teste/`: base de teste com microdados de 2005 (fora do git)

### `historico/`
- `proposta_inicial_bolsas/`: primeira proposta do subprojeto (eficácia de bolsas de assistência e auxílio): o `README.md` antigo do módulo, o `tab_seminario_01.pdf` e o `microdados2021_arq29.csv` analisado no início
- `DIARIO_versao_nuvem.md`: versão do diário que estava na nuvem, só com as entradas de 13/09
- `resultados_nuvem/`: CSV, figuras e `Plotagem_BI.pbix` (Power BI) da pasta `Resultados/` da nuvem. Os CSV são **diferentes** dos atuais em `resultados/`

### `videos/` (fora do git)
- `Banco de dados.mp4` (≈2 min)
- `Primeiros resultados.mp4` (≈5 min)

## Código e apresentação

- Backend: `EnadeX_Backend/multi_enade/` (`multi_enade_endpoints.py`, `modelos/`, `refinamento_dados/`)
- Frontend: `EnadeX_Frontend/app/multienade/` e `EnadeX_Frontend/lib/multienade/`
- Apresentação do TCC: `TCC apresentação/Apresentacao_multienade/` (ver o `README.md` daquela pasta)
