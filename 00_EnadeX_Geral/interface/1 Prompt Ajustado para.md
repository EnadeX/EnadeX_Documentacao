### 1\. Prompt Ajustado para Gerador de Interface WEB (AI UI Generator)

```
Crie uma interface web moderna, minimalista e científica para o ecossistema "EnadeX" — uma plataforma de análise multidimensional e explicável dos microdados do ENADE.

Diretrizes de Design e Estilo:
- Estilo: Design System minimalista, limpo e profissional (inspiração em dashboards acadêmicos e analíticos modernos como Vercel, Tremor UI e Tailwind Dashboard).
- Paleta de Cores: Fundo claro/neutro (#F8FAFC ou #FFFFFF), tons de cinza slate para tipografia (#0F172A, #475569), e cor de acento primária sóbria (#2563EB - Azul Índigo ou #0D9488 - Teal) para destaques e botões.
- Tipografia: Sans-serif limpa e legível (Inter ou Roboto).
- Layout: Responsivo, focado em alta densidade de informação sem poluição visual, com uso de cartões (cards), bordas sutis (1px solid #E2E8F0) e sombras leves.

A interface deve contemplar dois modelos de tela:
1. Tela Inicial (Home do Ecossistema EnadeX) com indicadores de KPI (Total de Estudantes, Cursos, Edições Harmonizadas e Link para o Repositório do Projeto) e Grid de Subprojetos.
2. Modelo Base para Páginas de Subprojeto (MultiENADE, E-XplainENADE, EduCluster, ENADE-Time).

```

---

### 2\. Descrição Atualizada da Tela Inicial (Home)

* **Cabeçalho (Header / Navbar):**
  * **Esquerda:** Logotipo minimalista `EnadeX` com badge discreto `v2021+`.
  * **Centro:** Links de navegação rápida (`Visão Geral`, `Subprojetos`, `Metodologia DSR`, `Documentação`).
  * **Direita:** Seleção de ciclo/ano do ENADE (ex: `2021`) e botão de acesso ao repositório.
* **Seção Hero (Apresentação Principal &amp; KPIs):**
  * **Título Principal:** "Mapeamento Multidimensional e Explicável dos Microdados do ENADE".
  * **Subtítulo:** "Plataforma integrada baseada em Design Science Research para descoberta de padrões socioeconômicos, institucionais e pedagógicos no ensino superior."
  * **Barra de Status do Sistema (4 KPI Cards em linha):**
    1. **Total de Estudantes:** Contagem global de registros processados na base.
    2. **Cursos Analisados:** Número de cursos de graduação mapeados.
    3. **Edições Harmonizadas:** Indicador do período longitudinal (2017–2021).
    4. **Repositório do Projeto:** Card/atalho interativo com o link direto para o repositório de código fonte do projeto no GitHub.
* **Grid de Subprojetos (4 Cards Principais):**
  * **MultiENADE:** "Descoberta de Padrões Multidimensionais" (Tag: *Machine Learning / SHAP*).
  * **E-XplainENADE:** "Modelagem Estatística e Explicabilidade" (Tag: *Regressão / VIF*).
  * **EduCluster:** "Agrupamento de Perfis Latentes" (Tag: *Clusterização K-Means*).
  * **ENADE-Time:** "Análise Temporal e Séries Históricas" (Tag: *Longitudinal*).
  * *Cada card possui descrição sucinta, ícone e botão "Acessar Módulo →".*
* **Rodapé (Footer):**
  * Créditos institucionais, licença de código aberto, conformidade com LGPD e referências ABNT/INEP.
