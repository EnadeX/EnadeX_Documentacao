# EnadeX — Documentação

**ENADE eXplainability Framework.** Arquitetura integrada de mineração, explicabilidade e Visual Analytics para exploração de microdados do ENADE.

Este repositório reúne a documentação do ecossistema EnadeX: o material comum a todos e uma pasta para cada um dos quatro subprojetos (TCCs). O código fica nos repositórios `EnadeX_Backend` e `EnadeX_Frontend`; a apresentação do TCC do MultiENADE fica em `TCC apresentação/`.

## Estrutura

```
EnadeX_Documentacao/
├── README.md               este arquivo
├── 00_EnadeX_Geral/        o ecossistema como um todo: proposta, entregáveis, marca, referências, microdados do INEP
├── 01_MultiENADE/          Projeto 1, Danilo Fernandes Araujo Gama
├── 02_E-XplainENADE/       Projeto 2, João Pedro Ribeiro Batista Araujo
├── 03_EduCluster/          Projeto 3, Paulo Gosik Mascarenhas Moita
└── 04_ENADE-Time/          Projeto 4, Lucas Eduardo Tavares Costa
```

A numeração e os responsáveis seguem `00_EnadeX_Geral/projeto/Fases do projeto ENADEX.pdf` e `00_EnadeX_Geral/projeto/EnadeX_Entregaveis_Semestre.pdf`.

## Os quatro subprojetos

| # | Subprojeto | Responsável | Título (conforme *Fases do projeto ENADEX*) |
|---|---|---|---|
| 1 | [MultiENADE](01_MultiENADE/README.md) | Danilo Fernandes Araujo Gama | MultiENADE: Mapeamento Multidimensional de Fatores Associados ao Desempenho no ENADE |
| 2 | [E-XplainENADE](02_E-XplainENADE/README.md) | João Pedro Ribeiro Batista Araujo | E-XplainENADE — Uma Arquitetura Computacional para Modelagem Explicável de Fatores Associados ao Desempenho Acadêmico |
| 3 | [EduCluster](03_EduCluster/README.md) | Paulo Gosik Mascarenhas Moita | EduCluster — Painel Interativo para Descoberta de Perfis Acadêmicos Latentes no ENADE |
| 4 | [ENADE-Time](04_ENADE-Time/README.md) | Lucas Eduardo Tavares Costa | ENADE-Time — Análise Temporal Interativa de Indicadores Acadêmicos com Base em Microdados do ENADE |

## Repositórios relacionados

Ficam ao lado desta pasta, em `EnadeX/`:

| Pasta | Conteúdo |
|---|---|
| `EnadeX_Backend/` | API em Python (`api_main.py`) e um módulo por subprojeto: `multi_enade/`, `e_xplainenade/`, `educluster/`, `enade_time/`, mais `util/` |
| `EnadeX_Frontend/` | Site em Next.js com uma rota por subprojeto: `app/multienade`, `app/e-xplainenade`, `app/educluster`, `app/enade-time` |
| `TCC apresentação/` | Apresentação (site de slides) do TCC do MultiENADE |

## Arquivos grandes fora do git

Estão nesta pasta, mas o `.gitignore` os deixa de fora do versionamento:

- `00_EnadeX_Geral/dados_enade/*/2.DADOS/`: microdados brutos do INEP, 2005 a 2021 (~3,3 GB)
- `01_MultiENADE/dados/Modelo_teste/`: base de teste com microdados de 2005 (~207 MB)
- `*.mp4`: vídeos (~633 MB)

## Como abrir

Abra esta pasta como cofre no Obsidian, ou leia os `README.md` direto no GitHub.
