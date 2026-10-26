# E-XplainENADE

**E-XplainENADE — Uma Arquitetura Computacional para Modelagem Explicável de Fatores Associados ao Desempenho Acadêmico**
Projeto 2 do ecossistema [EnadeX](../00_EnadeX_Geral/README.md), de João Pedro Ribeiro Batista Araujo.

## Foco

> Conforme `00_EnadeX_Geral/projeto/Fases do projeto ENADEX.pdf`.

Construção da arquitetura computacional para modelagem estatística explicável e reproduzível. O E-XplainENADE investiga e implementa:

- Pipeline modular de dados (padronização e reprodutibilidade)
- Engine de regressão configurável
- Seleção automática de variáveis e testes de interação
- Detecção de multicolinearidade (ex.: VIF)
- Explicabilidade via SHAP
- Geração automática de relatório técnico e gráficos
- Interface para definição de hipóteses

Ele resolve o problema metodológico da padronização e auditabilidade científica das análises do ENADE.

As fases DSR deste projeto estão em `Fases do projeto ENADEX.pdf` (a partir da p. 23), e os entregáveis do semestre em `EnadeX_Entregaveis_Semestre.pdf` (seção *Projeto 2*).

## Código

- Backend: `EnadeX_Backend/e_xplainenade/` (aplicação Streamlit `app.py`, `modules/`, `config/`, `banco/`, `tests/`)
- Frontend: `EnadeX_Frontend/app/e-xplainenade/` e `EnadeX_Frontend/lib/e-xplainenade/`

## Documentação própria

Não havia documentação em Markdown/PDF do E-XplainENADE no material reunido. Esta pasta está pronta para receber os documentos do responsável.
