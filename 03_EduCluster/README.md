# EduCluster

**EduCluster — Painel Interativo para Descoberta de Perfis Acadêmicos Latentes no ENADE**
Projeto 3 do ecossistema [EnadeX](../00_EnadeX_Geral/README.md), de Paulo Gosik Mascarenhas Moita.

## Foco

> Conforme `00_EnadeX_Geral/projeto/Fases do projeto ENADEX.pdf`.

Construção de um artefato interativo para descobrir e interpretar perfis acadêmicos latentes. O EduCluster é responsável por:

- Seleção e padronização de variáveis
- Clusterização (K-Means / Hierárquica)
- Definição automática do número de clusters (Elbow/Silhouette)
- Redução dimensional (PCA/UMAP) para visualização
- Caracterização automática de clusters (variáveis dominantes)
- Dashboard interativo com filtros por curso, UF, tipo de IES, turno etc.
- Comparação de desempenho médio por perfil

Ele resolve o problema prático da exploração interativa e interpretável de perfis estruturais no ENADE.

As fases DSR deste projeto estão em `Fases do projeto ENADEX.pdf` (a partir da p. 31), e os entregáveis do semestre em `EnadeX_Entregaveis_Semestre.pdf` (seção *Projeto 3*).

## Conteúdo desta pasta

| Item | Descrição | Origem |
|---|---|---|
| `README_backend.md` | README do módulo: estrutura, análises implementadas (A1, A3, A4, A6, A7, A8, Fase 6), decisões de tratamento e como rodar | `EnadeX_Backend/educluster/README.md` |
| `resultados/` | CSV gerados por cada modelo (`mdl_curso_percepcao`, `mdl_perfil_desempenho`, `mdl_calibracao_prova`, `mdl_situacao_discursiva`, `mdl_dimensao_desempenho`, `mdl_comparacao_tradicional`) | `EnadeX_Backend/educluster/modelos/` |
| `interface/telas/` | Capturas das páginas do EduCluster no site (desktop claro/escuro e celular) | `EnadeX_Frontend/tests/e2e/educluster.spec.ts-snapshots/` |

## Código

- Backend: `EnadeX_Backend/educluster/`
- Frontend: `EnadeX_Frontend/app/educluster/` e `EnadeX_Frontend/lib/educluster/`
