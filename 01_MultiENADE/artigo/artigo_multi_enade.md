# MultiENADE: análise multidimensional do desempenho de cursos de Computação no ENADE 2021, além da divisão entre público e privado

## RESUMO

O desempenho de cursos no Exame Nacional de Desempenho dos Estudantes (ENADE) costuma ser comparado pela divisão entre instituições públicas e privadas. Este trabalho apresenta o MultiENADE, componente do ecossistema EnadeX, e o utiliza para verificar, com três técnicas sobre a mesma base, se essa divisão se reproduz e o que existe além dela. Foram analisados 161 cursos de Ciência da Computação e Sistemas de Informação das regiões Norte e Nordeste no ENADE 2021, com a nota centrada pela média nacional de cada área. O agrupamento K-Means formou três perfis que reproduzem a divisão entre públicas e privadas (índice de Rand ajustado de 0,855), mas apenas pela pergunta sobre financiamento das mensalidades: sem ela, a divisão desaparece. Na regressão Random Forest, avaliada por validação cruzada repetida, a categoria administrativa sozinha explicou 6% da variação da nota, enquanto o modelo com 14 variáveis explicou 49% (± 14%), incluindo 59% da variação entre os cursos públicos e 26% entre os privados. As variáveis de maior peso foram a bolsa de iniciação científica e um índice de trabalho e renda dos estudantes. As regras de associação mostraram vínculo fraco entre a percepção dos estudantes e o desempenho, e o único sinal encontrado também aponta para a iniciação científica. Conclui-se que a divisão entre públicas e privadas é real, mas estreita, e que a análise multidimensional revela diferenças dentro de cada setor que uma comparação isolada não mostra.

**Palavras-chave:** ENADE; Mineração de Dados Educacionais; Análise Multidimensional; Instituições Públicas e Privadas; Random Forest.

---

# 1 INTRODUÇÃO

O Exame Nacional de Desempenho dos Estudantes (ENADE) constitui uma importante fonte de dados para investigação das condições acadêmicas, socioeconômicas e institucionais relacionadas ao ensino superior brasileiro. Os microdados disponibilizados pelo Instituto Nacional de Estudos e Pesquisas Educacionais Anísio Teixeira (INEP) reúnem informações sobre estudantes, cursos, instituições, condições de ingresso, aspectos socioeconômicos e percepções relacionadas à formação acadêmica. Apesar dessa riqueza, a exploração desses dados ainda ocorre com frequência de forma fragmentada, com análises concentradas em relações isoladas entre poucas variáveis.

A revisão apresentada por Lima et al. (2019) evidencia a recorrência de abordagens descritivas e de análises direcionadas a relações específicas entre variáveis do ENADE e de outros exames educacionais. No campo da Mineração de Dados Educacionais, diferentes técnicas já foram aplicadas aos dados do ENADE, incluindo agrupamento, classificação, árvores de decisão, regressão e abordagens baseadas em KDD (ARAÚJO, 2020; CRISPIM NETO, 2020; GUILHEN, 2024). Entretanto, o uso de várias técnicas sobre a mesma base, de modo que uma possa verificar o que a outra encontrou, permanece menos explorado.

Um dos padrões mais conhecidos nos resultados do ENADE é a diferença de desempenho entre cursos de instituições públicas e privadas. Essa diferença, porém, se sobrepõe a outras: os dois setores diferem também no perfil socioeconômico dos estudantes, no turno dos cursos, na oferta de bolsas acadêmicas e nas formas de financiamento das mensalidades. Uma comparação direta entre públicas e privadas não distingue o que é efeito da categoria do que é efeito desses outros fatores, nem mostra o que diferencia os cursos dentro de cada setor.

Nesse contexto, surge o problema de pesquisa deste trabalho: **os padrões encontrados por agrupamento, regressão e regras de associação nos microdados do ENADE convergem para a divisão entre instituições públicas e privadas? E o que eles mostram além dela?**

O MultiENADE foi desenvolvido como artefato para responder a esse problema e integra as três técnicas em um mesmo processo de preparação, modelagem e interpretação dos dados. O objetivo geral consiste em desenvolver um processo computacional de análise multidimensional de microdados do ENADE e utilizá-lo para verificar e qualificar os fatores associados ao desempenho dos cursos.

Como objetivos específicos, buscou-se: desenvolver um processo de preparação e integração dos dados; identificar perfis de cursos por meio de agrupamento; investigar, por meio de modelos supervisionados, os fatores associados ao desempenho médio dos cursos; explorar relações de coocorrência entre percepções dos estudantes e desempenho por meio de regras de associação; e avaliar empiricamente a consistência entre as três abordagens, comparando-as com a análise tradicional por tipo de instituição.

O trabalho integra o ecossistema EnadeX, uma proposta de arquitetura de mineração, explicabilidade e Visual Analytics aplicada aos microdados do ENADE. Nesse ecossistema, o MultiENADE é o componente responsável pela análise multidimensional, enquanto outros projetos concentram-se em aspectos como explicabilidade estatística, segmentação visual e análise temporal.

A pesquisa caracteriza-se como aplicada e quantitativa, de natureza exploratória, conduzida segundo princípios de Design Science Research. O estudo foi delimitado ao ENADE 2021 e aos grupos de cursos 4004 (Ciência da Computação, bacharelado) e 4006 (Sistemas de Informação) das regiões Norte e Nordeste.

O artigo está organizado da seguinte forma. A seção 2 apresenta o referencial teórico. A seção 3 descreve os dados, a construção dos três modelos e o procedimento de verificação entre eles. A seção 4 apresenta os resultados na ordem do argumento: a diferença entre os setores na amostra, o agrupamento, a regressão e a associação, seguidos de uma síntese. A seção 5 traz as considerações finais.

---

# 2 REFERENCIAL TEÓRICO

## 2.1 Microdados educacionais e o ENADE

Os microdados educacionais constituem uma fonte relevante para estudos quantitativos sobre fatores relacionados ao ensino e ao desempenho acadêmico. No caso do ENADE, esses dados permitem investigar não apenas resultados de desempenho, mas também características socioeconômicas dos estudantes, condições institucionais, infraestrutura, formação acadêmica e percepções relacionadas à experiência universitária.

A disponibilidade dessas variáveis possibilita abordagens que ultrapassam a análise isolada da nota. Entretanto, a diversidade das informações também exige procedimentos adequados de preparação, harmonização e seleção das variáveis. A literatura brasileira apresenta aplicações variadas de técnicas de mineração sobre o ENADE, mostrando o potencial dos microdados para apoiar processos de descoberta de conhecimento e tomada de decisão (ARAÚJO, 2020; CRISPIM NETO, 2020).

Lima et al. (2019), ao realizarem uma revisão sistemática de estudos envolvendo ENADE e ENEM, apontam a predominância de análises descritivas e abordagens orientadas a relações específicas. Essa característica reforça a oportunidade de explorar métodos que integrem diferentes dimensões dos dados.

## 2.2 Mineração de Dados Educacionais

A Mineração de Dados Educacionais busca aplicar métodos computacionais para descobrir padrões, relações e informações úteis em grandes volumes de dados educacionais. Baker, Isotani e Carvalho (2011) destacam diferentes categorias de problemas nesse campo, incluindo predição, agrupamento, mineração de relações e descoberta de modelos.

Entre as possibilidades destacadas pelos autores encontram-se a classificação e regressão para predição, técnicas de clustering para identificação de grupos, mineração de relações para descoberta de associações e técnicas de descoberta de modelos para produção de conhecimento interpretável.

O MultiENADE incorpora essas diferentes perspectivas em uma arquitetura integrada, utilizando modelos supervisionados, agrupamento e regras de associação sobre a mesma base. Aplicar mais de uma técnica ao mesmo problema também permite verificar um achado: quando métodos com pressupostos diferentes apontam para o mesmo resultado, a confiança nele aumenta; quando divergem, a divergência indica onde o resultado precisa ser qualificado. Essa ideia, conhecida como triangulação (JICK, 1979), orienta o uso das três técnicas neste trabalho.

## 2.3 Modelos supervisionados e Random Forest

Modelos supervisionados são utilizados quando existe uma variável de interesse que deve ser estimada a partir de outras características observadas. No contexto deste trabalho, a variável-alvo é a nota geral média do curso, ajustada pela média nacional da sua área (seção 3.3).

O método Random Forest combina múltiplas árvores de decisão construídas a partir de amostras e subconjuntos de variáveis, permitindo representar relações não lineares e interações entre características (BREIMAN, 2001). Essa propriedade é particularmente relevante em dados educacionais, nos quais relações entre condições socioeconômicas, características institucionais e desempenho podem não apresentar comportamento linear.

Além da predição, a interpretação das contribuições das variáveis é importante em contextos de análise exploratória. Por esse motivo, o MultiENADE incorporou valores de importância derivados do modelo para apoiar a interpretação dos resultados.

## 2.4 Agrupamento e redução de dimensionalidade

A análise de agrupamentos busca identificar conjuntos de observações com características semelhantes sem utilizar uma variável-alvo previamente definida. O K-Means é um dos métodos mais utilizados nessa finalidade e procura particionar as observações em \(k\) grupos minimizando a distância entre os elementos e o centro de seus respectivos grupos (MACQUEEN, 1967). Como o resultado depende dos centros iniciais, sorteados aleatoriamente, o algoritmo costuma ser executado várias vezes, mantendo-se a solução de menor distância total.

A qualidade de uma partição pode ser avaliada pelo coeficiente de silhouette, que compara a proximidade de cada observação ao próprio grupo com a proximidade ao grupo vizinho e varia de −1 a 1 (ROUSSEEUW, 1987). Para comparar duas partições, como a produzida pelo algoritmo e uma classificação conhecida, utiliza-se o índice de Rand ajustado (ARI), que vale 1 quando as partições coincidem e fica próximo de 0 quando a coincidência é a esperada ao acaso (HUBERT; ARABIE, 1985).

No MultiENADE, os agrupamentos foram formados a partir de proporções de respostas sobre a avaliação das aulas práticas, o ingresso por ação afirmativa e o financiamento das mensalidades. A nota geral não foi utilizada como variável de entrada, evitando circularidade entre a formação dos grupos e sua posterior interpretação pelo desempenho.

Para visualização espacial dos resultados, utilizou-se Análise de Componentes Principais (PCA). A PCA permite representar uma parte relevante da variabilidade dos dados por meio de um conjunto reduzido de componentes, facilitando a visualização de estruturas multidimensionais (JOLLIFFE; CADIMA, 2016).

No modelo de regressão, a PCA também foi utilizada de forma específica para condensar as variáveis econômicas ordinais. O primeiro componente resume a situação de trabalho e renda dos estudantes do curso (seção 3.5), permitindo reduzir a dimensionalidade sem eliminar o sinal preditivo encontrado nas variáveis originais.

## 2.5 Regras de associação

As regras de associação permitem identificar padrões de coocorrência entre características de um conjunto de observações. O algoritmo Apriori é tradicionalmente empregado para descobrir conjuntos frequentes de itens e construir regras a partir deles (AGRAWAL; SRIKANT, 1994).

Entre as principais medidas utilizadas estão suporte, confiança e lift. O suporte representa a frequência com que uma combinação aparece na base; a confiança mede a frequência com que o consequente ocorre quando o antecedente está presente; e o lift compara essa ocorrência com aquilo que seria esperado sob independência.

No contexto do MultiENADE, essas medidas foram utilizadas para investigar relações entre percepções sobre formação e experiência acadêmica, incorporando também a faixa de desempenho do curso como característica contextual.

## 2.6 Explicabilidade e interpretação de modelos

A capacidade de produzir previsões não é suficiente para uma análise exploratória orientada à descoberta de conhecimento. É necessário também compreender quais características possuem maior participação nos resultados.

Nesse contexto, técnicas de explicabilidade permitem examinar a contribuição das variáveis para as previsões produzidas por modelos complexos. O MultiENADE utiliza valores SHAP (LUNDBERG; LEE, 2017), que decompõem cada previsão em contribuições aditivas de cada variável, na mesma unidade da variável-alvo. Isso permite saber não só quais variáveis pesam mais, mas em que direção atuam e com que tamanho de efeito.

Essa preocupação está alinhada à proposta geral do EnadeX de combinar mineração de dados e explicabilidade, reduzindo a distância entre desempenho computacional e interpretação dos padrões encontrados.

## 2.7 Design Science Research

O desenvolvimento do MultiENADE foi orientado por princípios de Design Science Research (DSR), abordagem voltada à construção e avaliação de artefatos para problemas relevantes. Hevner et al. (2004) destacam a importância de construir artefatos e submetê-los a processos sistemáticos de avaliação, enquanto Peffers et al. (2007) apresentam um processo estruturado para investigação orientada à criação de soluções.

Nesse trabalho, o artefato corresponde ao processo computacional do MultiENADE, incluindo ingestão e preparação dos dados, construção de variáveis analíticas, aplicação de diferentes modelos e publicação dos resultados.

## 2.8 Diferenças entre instituições públicas e privadas

A diferença de desempenho entre cursos de instituições públicas e privadas é um dos padrões mais recorrentes nos resultados do ENADE. Bervian e Corrêa (2015), ao analisarem os conceitos do ENADE 2012 dos cursos de Administração de Santa Catarina, encontraram desempenho significativamente maior nas instituições públicas, enquanto a organização acadêmica (universidade, centro universitário ou faculdade) não apresentou diferença significativa. Os próprios dados do exame a confirmam para as áreas deste estudo (seção 4.1). Essa diferença, porém, não decorre apenas da categoria administrativa. Instituições públicas e privadas diferem também no perfil socioeconômico dos estudantes, na proporção de cursos noturnos, na oferta de bolsas de pesquisa e de monitoria e nas formas de financiamento das mensalidades, como FIES e ProUni.

<!-- PENDENTE (D14): conferir BERVIAN; CORRÊA (2015) no original (https://periodicos.ufpe.br/revistas/ADED/article/view/2475): páginas 6-27 deduzidas do PDF (22 páginas a partir da 6), sem DOI nem ISSN na página da revista. O estudo é regional (Administração, SC, 2012); se possível, acrescentar uma fonte nacional (por exemplo, um relatório do INEP). -->

Por isso, a categoria administrativa funciona ao mesmo tempo como um fator e como um resumo de vários outros. Separar essas duas leituras é um dos objetivos deste trabalho: verificar se as técnicas reproduzem a divisão entre os setores e identificar o que, entre as variáveis observadas, explica o desempenho além dela.

---

# 3 MÉTODOS E PROCEDIMENTOS

## 3.1 Caracterização da pesquisa

A pesquisa é de natureza aplicada, com abordagem quantitativa e caráter exploratório. O desenvolvimento do artefato foi conduzido segundo princípios de Design Science Research, tendo como foco a construção e avaliação empírica de um processo de análise multidimensional dos microdados do ENADE.

A unidade de análise variou conforme a técnica empregada. Nos modelos de regressão e agrupamento, os dados foram consolidados no nível do curso. Na análise de associação, foram utilizadas respostas individuais dos estudantes provenientes do arquivo que contém os itens Likert (escala de concordância de 1 a 6), sendo posteriormente incorporada a faixa de desempenho do respectivo curso.

O artefato foi implementado em Python, e os resultados são publicados por uma API consumida pelo painel do ecossistema EnadeX. O Quadro 1 resume as bibliotecas usadas e o papel de cada uma.

**Quadro 1 – Tecnologias usadas no MultiENADE**

| Etapa | Bibliotecas (versão) | Uso no artefato |
|---|---|---|
| Preparação dos dados | Python 3.14, pandas 3.0, NumPy 2.4 | Leitura dos microdados, recorte, agregação por curso |
| Modelos | scikit-learn 1.9 | Random Forest, K-Means, padronização, PCA e validação cruzada |
| Explicação | SHAP 0.54 | Contribuição de cada variável na previsão |
| Associação | mlxtend 0.25 | Algoritmo Apriori e regras de associação |
| Persistência e API | joblib 1.6, FastAPI 0.162, cliente Supabase 2.33 | Salvar os modelos, servir os resultados, banco PostgreSQL |
| Interface | Next.js 16, React 19, Recharts 3 | Painel do ecossistema EnadeX |

Fonte: elaborado pelo autor.

## 3.2 Recorte e amostra

Foram utilizados os microdados do ENADE 2021, restritos aos grupos 4004 (Ciência da Computação, bacharelado) e 4006 (Sistemas de Informação) e às regiões Norte e Nordeste. O recorte reúne 190 cursos e 6.303 inscritos. Destes, 169 cursos têm pelo menos um estudante presente com nota válida e 161 cursos têm pelo menos cinco estudantes com nota geral maior que zero. Os 161 cursos, com 4.464 estudantes, formam a base da regressão e do agrupamento. A análise de associação usa os 4.657 estudantes dos 169 cursos que responderam aos seis itens de percepção analisados.

O recorte é aplicado já na leitura dos dados, a partir do arquivo de cursos do INEP: ficam os cursos cuja área (`CO_GRUPO`) é 4004 ou 4006 e cuja região (`CO_REGIAO_CURSO`) é 1, Norte, ou 2, Nordeste. A função devolve o código de cada curso selecionado (`CO_CURSO`), que identifica o curso em todos os arquivos (Código 1).

**Código 1 – Seleção dos cursos do recorte**

```python
def obter_cursos_recorte(caminho_arq_1: str, grupos=("4004", "4006"), regioes=("1", "2")) -> pd.Series:
    dataf_arq1 = leitura_inicial_dados(caminho_arq_1, ["CO_CURSO", "CO_REGIAO_CURSO", "CO_GRUPO"])
    dataf_arq1 = dataf_arq1.dropna(how="all")
    dataf_arq1 = dataf_arq1[dataf_arq1["CO_REGIAO_CURSO"].isin(regioes)]
    dataf_arq1 = dataf_arq1[dataf_arq1["CO_GRUPO"].isin(grupos)]
    return dataf_arq1["CO_CURSO"]
```

Fonte: repositório EnadeX_Backend, arquivo multi_enade/refinamento_dados/util_etl.py.

A Tabela 1 mostra a composição dos 161 cursos. A amostra é quase equilibrada entre instituições públicas (74 cursos) e privadas (87 cursos), mas a proporção muda entre as áreas: em Ciência da Computação predominam os cursos públicos; em Sistemas de Informação, os privados.

**Tabela 1 – Cursos analisados por categoria administrativa e área**

| Categoria administrativa | Ciência da Computação | Sistemas de Informação | Total |
|---|---:|---:|---:|
| Pública federal | 28 | 27 | 55 |
| Pública estadual | 12 | 6 | 18 |
| Pública municipal | 1 | 0 | 1 |
| Privada com fins lucrativos | 24 | 47 | 71 |
| Privada sem fins lucrativos | 5 | 11 | 16 |
| **Total** | **70** | **91** | **161** |

Fonte: elaborado pelo autor a partir dos microdados do ENADE 2021 (INEP).

Os arquivos dos microdados do ENADE são disponibilizados de forma independente e não permitem vincular as respostas de um mesmo estudante entre arquivos diferentes. O código do curso (`CO_CURSO`) é a única chave comum. Por isso, a regressão e o agrupamento foram feitos no nível do curso, com médias e proporções calculadas a partir de cada arquivo, e a associação usou apenas itens de um mesmo arquivo, complementados por uma característica do curso (a faixa de desempenho).

A categoria administrativa (`CO_CATEGAD`) não entrou em nenhum dos modelos. Ela foi usada somente depois, para verificar se os resultados reproduzem a divisão entre instituições públicas e privadas (seção 3.7).

## 3.3 Preparação dos dados e variável-alvo

A preparação incluiu filtragem regional, seleção dos grupos de cursos, transformação de tipos, tratamento de valores ausentes, agregação por curso, cálculo de proporções e codificação das variáveis categóricas. Nos itens de percepção em escala de 1 ("discordo totalmente") a 6 ("concordo totalmente"), os códigos 7 ("não sei responder") e 8 ("não se aplica") foram tratados como ausentes. Uma versão anterior do processo removia também a resposta 6, o que descartava cerca de 49% das respostas válidas; a correção elevou a média do item de aulas práticas por curso de 4,02 para 5,00.

O Código 2 mostra o filtro corrigido, usado nos itens de percepção.

**Código 2 – Filtro dos itens de percepção em escala de 1 a 6**

```python
def _filtrar_likert(df: DataFrame, coluna: str) -> pd.Series:
    """Escala de concordância 1-6 (6=Concordo totalmente, resposta legítima);
    só 7 (Não sei responder) e 8 (Não se aplica) quebram a ordinalidade."""
    s = pd.to_numeric(df[coluna], errors='coerce')
    return s.where(~s.isin([7, 8]))
```

Fonte: repositório EnadeX_Backend, arquivo multi_enade/modelos/mdl_associacao/modelo_associacao.py.

A nota geral do ENADE (`NT_GER`) combina a formação geral (25%) e o componente específico da área (75%). Como as provas de Ciência da Computação e de Sistemas de Informação são diferentes, a nota bruta mistura duas escalas: a média nacional dos cursos é 35,61 pontos em Ciência da Computação (304 cursos) e 37,91 em Sistemas de Informação (364 cursos). Para que os modelos não aprendessem apenas a diferença entre as provas, a variável-alvo principal foi a **nota centrada por área** (`NT_GER_c`): a média de `NT_GER` dos estudantes do curso menos a média nacional dos cursos da mesma área, calculada com todos os cursos do país com pelo menos cinco estudantes. Um valor de +3 indica um curso três pontos acima da média nacional da sua área.

No sistema, as duas médias nacionais ficam registradas no código e o alvo é calculado antes de cada treino (Código 3).

**Código 3 – Cálculo da nota centrada por área**

```python
MEDIA_NACIONAL_AREA = {'4004': 35.6128543859, '4006': 37.9142157510}

def _entradas_e_alvo(df_consolidado: DataFrame):
    x = df_consolidado[COLUNAS_DIRETAS + COLUNAS_ECONOMICAS]
    media_area = media_nacional_por_curso(df_consolidado['CO_CURSO'])
    y_centrado = df_consolidado['NT_GER'] - media_area   # NT_GER_c
    return x, y_centrado, media_area
```

Fonte: repositório EnadeX_Backend, arquivo multi_enade/modelos/mdl_regressao/modelo_regressao.py.

Para verificar se as conclusões dependem dessa escolha, todos os testes foram repetidos com três alvos alternativos: a nota padronizada por área (z-score nacional), a combinação das notas de formação geral e de componente específico padronizadas separadamente e a nota bruta.

## 3.4 Construção do modelo de agrupamento

O agrupamento foi feito com o algoritmo K-Means sobre 23 variáveis por curso, padronizadas com `StandardScaler`. As variáveis são as proporções de estudantes do curso em cada resposta de três perguntas do questionário do estudante:

- `QE_I63`, "os ambientes e equipamentos destinados às aulas práticas foram adequados ao curso" (seis níveis de concordância);
- `QE_I15`, forma de ingresso por ação afirmativa (seis categorias, como cota por renda, por critério étnico-racial ou por ter estudado em escola pública);
- `QE_I11`, bolsa ou financiamento das mensalidades (onze categorias, como "nenhum, curso gratuito", "nenhum, curso não gratuito", FIES, ProUni integral e ProUni parcial).

A nota (`NT_GER`) não entrou na formação dos grupos, para evitar circularidade, e foi usada apenas para caracterizá-los depois.

O número de grupos foi escolhido por varredura de \(k\) entre 2 e 8. Para cada \(k\), o K-Means foi executado com 20 sementes aleatórias diferentes. Foram medidas a **estabilidade**, como a concordância média entre as partições obtidas com sementes diferentes, e a **qualidade da separação**, pelo coeficiente de silhouette. A concordância entre duas partições foi medida pelo índice de Rand ajustado (ARI), que vale 1 quando as partições são idênticas e fica perto de 0 quando a coincidência é a esperada ao acaso. O silhouette compara, para cada curso, a distância média aos cursos do próprio grupo com a distância média ao grupo vizinho mais próximo; varia de −1 a 1, e valores próximos de 0 indicam grupos pouco separados.

A regra, fixada antes da execução, foi considerar elegíveis os valores de \(k\) com estabilidade de pelo menos 0,80 e escolher, entre eles, o de maior silhouette, mantendo \(k=3\) se a diferença para o melhor fosse menor que 0,02. O modelo final usa 50 inicializações do K-Means (`n_init = 50`) e semente fixa (42). Com menos inicializações, o algoritmo pode parar em soluções diferentes conforme a semente; com 50, a mesma solução, a de menor soma das distâncias aos centros, foi obtida de forma consistente.

O Código 4 mostra a padronização das 23 variáveis e o K-Means com as 50 inicializações.

**Código 4 – Padronização e K-Means (trechos)**

```python
N_INICIALIZACOES = 50

df_matematica = df_dados.drop(columns=['CO_CURSO', 'QT_ALUNOS', 'NT_GER'])
scaler = StandardScaler()
dados_padronizados = scaler.fit_transform(df_matematica)
kmeans = KMeans(n_clusters=num_clusters, random_state=42, n_init=N_INICIALIZACOES)
kmeans.fit(dados_padronizados)
```

Fonte: repositório EnadeX_Backend, arquivo multi_enade/modelos/mdl_clusters/modelo_clusters.py.

## 3.5 Construção do modelo de regressão

O modelo de regressão estima a nota centrada do curso a partir de 14 variáveis, descritas no Quadro 2. Elas resultaram de um processo exploratório que comparou nove algoritmos (regressão linear, Ridge, Lasso, Elastic Net, árvore de decisão, Random Forest, Gradient Boosting, KNN e SVR) e diferentes conjuntos de variáveis. Com as quatro variáveis originais, o \(R^2\) em validação cruzada foi de −0,139, pior do que prever a média. A maior parte do ganho posterior veio da escolha das variáveis, e não do ajuste dos hiperparâmetros.

As três variáveis econômicas ordinais (`QE_I08`, renda familiar; `QE_I09`, situação financeira; `QE_I10`, situação de trabalho) foram condensadas por Análise de Componentes Principais (PCA). O primeiro componente (`PCA_ECON_1`) explica 71,0% da variância das três e tem cargas positivas em todas, maiores em trabalho e situação financeira. Valores altos indicam cursos em que os estudantes trabalham mais horas, contribuem mais para o sustento da família e têm maior renda familiar. Por isso, ele é chamado aqui de **índice de trabalho e renda**. O segundo componente (`PCA_ECON_2`, 23,7%) separa a renda familiar das outras duas.

**Quadro 2 – Variáveis do modelo de regressão (médias ou proporções por curso)**

| Variável | Descrição | Bloco |
|---|---|---|
| `QE_I11_A` | Proporção sem bolsa ou financiamento em curso gratuito | Substitutas do tipo de instituição |
| `QE_I13_A` | Proporção sem nenhuma bolsa acadêmica | Substitutas do tipo de instituição |
| `QE_I13_B` | Proporção com bolsa de iniciação científica | Substitutas do tipo de instituição |
| `QE_I13_D` | Proporção com bolsa de monitoria ou tutoria | Substitutas do tipo de instituição |
| `QE_I12_A` | Proporção sem auxílio de permanência | Substitutas do tipo de instituição |
| `CO_TURNO_GRADUACAO_4` | Proporção de estudantes no turno noturno | Substitutas do tipo de instituição |
| `PCA_ECON_1` | Índice de trabalho e renda (1º componente de `QE_I08`, `QE_I09` e `QE_I10`) | Índice econômico |
| `PCA_ECON_2` | 2º componente econômico (renda familiar em contraste com trabalho) | Índice econômico |
| `QE_I04_media` | Escolaridade média do pai (0 = nenhuma; 5 = pós-graduação) | Demais |
| `QE_I19_B` | Proporção cujo maior incentivo para cursar veio dos pais | Demais |
| `QE_I24_A` | Proporção que estudou idioma estrangeiro na própria instituição | Demais |
| `QE_I15_C` | Proporção que ingressou por cota de renda | Demais |
| `QE_I15_E` | Proporção que ingressou por cota com dois ou mais critérios | Demais |
| `QE_I57` | Concordância média com "os professores demonstraram domínio dos conteúdos" (1 a 6) | Demais |

Fonte: elaborado pelo autor a partir do dicionário de variáveis do ENADE 2021 (INEP).

O bloco de **substitutas do tipo de instituição** reúne as seis variáveis com correlação de pelo menos 0,59 (em valor absoluto) com o indicador de instituição pública. A mais forte é `QE_I11_A`, com correlação de 0,986: em cursos públicos quase todos os estudantes respondem "curso gratuito". Assim, embora a categoria administrativa não esteja entre as variáveis, o modelo recebe uma informação quase equivalente a ela.

O modelo final é um Random Forest com 200 árvores, profundidade máxima 5, `min_samples_split = 10`, `min_samples_leaf = 2` e treino ponderado pelo número de estudantes de cada curso (`QT_ALUNOS`). O Random Forest combina muitas árvores de decisão treinadas em amostras diferentes dos dados, o que permite representar relações não lineares e interações entre variáveis (BREIMAN, 2001).

O PCA econômico e o Random Forest formam um único modelo (Pipeline). Assim, em cada avaliação, o PCA é ajustado apenas com os cursos de treino, e o treino é ponderado pelo número de estudantes (Código 5).

**Código 5 – Modelo de regressão: PCA econômico e Random Forest (trechos)**

```python
def _criar_pipeline() -> Pipeline:
    bloco_economico = Pipeline([('padroniza', StandardScaler()),
                                ('pca', PCA(n_components=2, random_state=42))])
    pre = ColumnTransformer([('direto', 'passthrough', COLUNAS_DIRETAS),
                             ('econ', bloco_economico, COLUNAS_ECONOMICAS)])
    return Pipeline([('pre', pre), ('rf', _criar_random_forest())])

modelo = _criar_pipeline()
modelo.fit(x.iloc[idx_tr], y.iloc[idx_tr], rf__sample_weight=pesos.iloc[idx_tr].to_numpy())
```

Fonte: repositório EnadeX_Backend, arquivo multi_enade/modelos/mdl_regressao/modelo_regressao.py.

O desempenho foi medido por **validação cruzada repetida**: os 161 cursos foram divididos em cinco partes, o modelo foi treinado em quatro e avaliado na quinta, e esse ciclo foi repetido com 10 embaralhamentos diferentes, totalizando 50 avaliações. Em cada uma, a PCA econômica foi ajustada apenas com os cursos de treino, para que nenhuma informação dos cursos de avaliação entrasse no ajuste. As métricas são o \(R^2\), que é a parte da variação da nota explicada pelo modelo (0 equivale a prever a média; valores negativos são piores do que isso), e o erro absoluto médio (MAE), em pontos da nota. Uma parte da variação das médias dos cursos é apenas ruído de amostragem, porque muitos cursos têm poucos estudantes; estimou-se que o \(R^2\) máximo alcançável com esse alvo é cerca de 0,82.

A versão anterior do MultiENADE media o desempenho em uma única divisão de 128 cursos para treino e 33 para teste, com \(R^2=0,498\) na nota bruta. Esse número é mantido como referência na seção 4.3, mas não é usado como medida principal, porque depende de qual divisão foi sorteada.

## 3.6 Construção das regras de associação

A análise de associação foi realizada no nível do estudante, com seis itens de percepção do mesmo arquivo de respostas, todos na escala de 1 a 6:

- `QE_I57`, os professores demonstraram domínio dos conteúdos (nas regras, `professores_dominio`);
- `QE_I61`, a infraestrutura das salas de aula foi adequada (`infra_salas`);
- `QE_I34`, o curso desenvolveu o pensamento crítico (`formacao_critica`);
- `QE_I44`, houve oportunidades de iniciação científica (`oportunidades_ic`);
- `QE_I30`, houve experiências de aprendizagem inovadoras (`metodologia_inovadora`);
- `QE_I56`, os professores estavam disponíveis fora do horário das aulas (`profs_disponibilidade`).

A cada estudante foi acrescentada a faixa de desempenho do seu curso (baixo, médio ou alto), obtida dividindo os 169 cursos em tercis da nota média. A faixa é, portanto, uma característica do curso, e não a nota do estudante. Nas regras, cada item aparece com o nome entre parênteses acima seguido do nível de concordância (por exemplo, `formacao_critica_5` indica a resposta 5 ao item `QE_I34`), e a faixa aparece como `desempenho_baixo`, `desempenho_medio` ou `desempenho_alto`.

As regras foram geradas pelo algoritmo Apriori (AGRAWAL; SRIKANT, 1994), com suporte mínimo de 0,03, confiança mínima de 0,50 e no máximo três itens por regra. São publicadas as 50 regras de maior lift. O processo gera 2.417 regras, das quais 377 respeitam o limite de três itens. A escolha dos limiares foi apoiada por uma validação em duas metades independentes da base: na configuração adotada, seis regras se repetiram nas duas metades, com lift entre 1,23 e 1,49, e cinco delas envolviam a resposta máxima sobre oportunidades de iniciação científica.

O Código 6 mostra a divisão dos cursos em faixas de desempenho e a geração das regras.

**Código 6 – Faixas de desempenho e regras de associação (trechos)**

```python
media_por_curso = df.groupby('CO_CURSO')['NT_GER'].mean()
return pd.qcut(media_por_curso, q=3, labels=['desempenho_baixo', 'desempenho_medio', 'desempenho_alto'])

df_binario = pd.get_dummies(base, columns=nomes_itens + ['desempenho'], dtype=bool).drop(columns=['CO_CURSO'])
itemsets_frequentes = apriori(df_binario, min_support=suporte_minimo, use_colnames=True)
regras = association_rules(itemsets_frequentes, metric="confidence", min_threshold=confianca_minima)
```

Fonte: repositório EnadeX_Backend, arquivo multi_enade/modelos/mdl_associacao/modelo_associacao.py.

Para verificar se o uso da nota bruta nos tercis afetava o resultado, as faixas foram recalculadas dentro de cada área com a nota centrada. Mudaram de faixa 30 dos 169 cursos, e as regras publicadas que envolvem desempenho permaneceram as mesmas. A versão de produção, com a nota bruta, foi mantida.

## 3.7 Verificação entre as técnicas

A verificação procurou responder a duas perguntas: os resultados das três técnicas reproduzem a divisão entre instituições públicas e privadas? E o que eles mostram além dela? Os critérios de cada teste foram definidos antes da execução das análises e não foram alterados depois de conhecidos os resultados.

**Agrupamento.** A partição foi comparada com a categoria administrativa pelo ARI. Para saber de quais perguntas vem a separação, o agrupamento foi refeito sem `QE_I11` e com cada pergunta isoladamente. Foi definido que a separação "continua" sem `QE_I11` se o ARI com a categoria ficasse acima de 0,50 e "some" se ficasse abaixo de 0,20.

**Regressão.** O modelo foi comparado com uma escada de modelos mais simples, todos avaliados nas mesmas 50 divisões da validação cruzada:

- **0:** a média da nota (nenhuma variável);
- **1:** só o indicador de instituição pública, em regressão linear, que corresponde à análise tradicional público × privado;
- **2:** o indicador de instituição pública e os dois componentes econômicos, em regressão linear;
- **3:** o modelo final (Random Forest com as 14 variáveis);
- **3L:** as mesmas 14 variáveis em regressão linear, para medir o efeito do algoritmo;
- **4:** o modelo final acrescido da categoria administrativa, para saber se ela traz informação que as 14 variáveis não têm;
- **5:** o modelo final sem as seis substitutas do tipo de instituição.

Considerou-se que um modelo supera outro quando o ganho médio de \(R^2\) é de pelo menos 0,05 e o ganho é positivo em pelo menos 40 das 50 divisões. Diferenças que não atendem às duas condições foram tratadas como empate.

Além do \(R^2\) geral, foi calculado o \(R^2\) **dentro de cada setor**: a parte da variação da nota entre os cursos públicos, e entre os privados, explicada pelas previsões feitas fora do treino. Um modelo que só soubesse se o curso é público ou privado teria \(R^2\) zero dentro de cada setor; o que passa disso é explicação além da categoria. A direção e o tamanho do efeito de cada variável foram obtidos com valores SHAP (LUNDBERG; LEE, 2017), que decompõem cada previsão em contribuições das variáveis, em pontos da nota.

No sistema, os valores SHAP são calculados sobre o Random Forest do modelo final, e a importância de cada variável é a média do valor absoluto entre os cursos (Código 7).

**Código 7 – Cálculo dos valores SHAP (trecho)**

```python
explainer = shap.TreeExplainer(modelo_treinado)
shap_values = explainer.shap_values(X)
importancia_media = np.abs(shap_values).mean(axis=0)
```

Fonte: repositório EnadeX_Backend, arquivo multi_enade/modelos/mdl_regressao/modelo_regressao.py.

**Associação.** Verificou-se se os itens de percepção variam entre as faixas de desempenho, se as regras que envolvem desempenho dependem da faixa e se a percepção, agregada por curso, se correlaciona com a nota centrada (correlação de Spearman).

## 3.8 Avaliação do artefato

A avaliação realizada foi de natureza computacional e empírica: validação cruzada repetida, comparação com modelos mais simples, varredura do número de grupos com teste de estabilidade, testes de robustez com alvos alternativos e sem a variável de financiamento, e validação das regras em metades independentes da base. As análises de verificação foram implementadas em scripts versionados que apenas leem os dados e conferem, antes de qualquer medida, se reproduzem os resultados publicados pelo sistema. Os scripts da fase exploratória inicial não foram preservados; os números dessa fase citados na seção 3.5 vêm da documentação do processo.

Não foi realizada avaliação com especialistas, testes formais de usabilidade ou questionários de utilidade percebida. A avaliação do artefato é, portanto, restrita ao comportamento computacional, à consistência dos resultados e à capacidade de produzir padrões interpretáveis.

---

# 4 RESULTADOS E DISCUSSÕES

## 4.1 A diferença entre públicas e privadas na amostra

Antes dos modelos, a Tabela 2 mostra o tamanho da diferença entre cursos públicos e privados, medida diretamente nas médias dos cursos. Os cursos públicos têm notas mais altas nas duas áreas, mas a diferença é bem maior em Ciência da Computação (7,8 pontos) do que em Sistemas de Informação (2,8 pontos). Os valores do recorte Norte e Nordeste são muito próximos dos calculados para o país inteiro, o que indica que a diferença não é uma particularidade da amostra.

**Tabela 2 – Nota geral média dos cursos por área e tipo de instituição**

| Área | Recorte: públicas | Recorte: privadas | Recorte: diferença | Brasil: diferença |
|---|---:|---:|---:|---:|
| Ciência da Computação | 39,32 | 31,56 | 7,76 | 8,71 |
| Sistemas de Informação | 40,10 | 37,27 | 2,83 | 2,85 |

Fonte: elaborado pelo autor a partir dos microdados do ENADE 2021 (INEP). Média simples das médias dos cursos com pelo menos cinco estudantes; Brasil = 668 cursos das duas áreas.

Na nota centrada, os cursos públicos ficam em média 3,0 pontos acima da média nacional da área, e os privados, 1,8 ponto abaixo. A diferença é real, mas a variação dentro de cada setor é maior do que ela: o desvio-padrão é de 5,9 pontos entre os cursos públicos e de 5,3 entre os privados. Por isso, saber apenas se o curso é público ou privado prevê mal a sua nota (seção 4.3).

## 4.2 O agrupamento reproduz a divisão entre públicas e privadas

A varredura do número de grupos indicou \(k=3\) (Figuras 1 e 2). Apenas \(k=2\) e \(k=3\) foram estáveis (concordância média entre sementes de 0,98 e 0,97); a partir de \(k=4\), a partição muda conforme a semente (estabilidade de 0,75 ou menos). Entre os dois estáveis, \(k=3\) teve o maior silhouette (0,212, contra 0,191).

**Figura 1 – Estabilidade do agrupamento para k de 2 a 8**

![Figura 1 – Estabilidade do agrupamento para k de 2 a 8](../../../MultiENADE_Analises/figuras/figura_1_estabilidade_k.png)

Fonte: elaborado pelo autor a partir dos microdados do ENADE 2021 (INEP).

**Figura 2 – Silhouette do agrupamento para k de 2 a 8**

![Figura 2 – Silhouette do agrupamento para k de 2 a 8](../../../MultiENADE_Analises/figuras/figura_2_silhouette_k.png)

Fonte: elaborado pelo autor a partir dos microdados do ENADE 2021 (INEP).

A Tabela 3 cruza os três grupos com a categoria administrativa, que não participou da sua formação. A coincidência é quase completa: um grupo reúne 73 dos 74 cursos públicos e os outros dois são formados por cursos privados, com uma exceção em cada lado. O ARI entre a partição e a divisão pública × privada é de 0,855.

**Tabela 3 – Grupos formados pelo K-Means por categoria administrativa**

| Categoria administrativa | Públicas gratuitas | Privadas pagas ou com FIES | Privadas com ProUni parcial | Total |
|---|---:|---:|---:|---:|
| Pública federal | 55 | 0 | 0 | 55 |
| Pública estadual | 18 | 0 | 0 | 18 |
| Pública municipal | 0 | 1 | 0 | 1 |
| Privada com fins lucrativos | 1 | 65 | 5 | 71 |
| Privada sem fins lucrativos | 0 | 13 | 3 | 16 |
| **Cursos** | **74** | **79** | **8** | **161** |
| Estudantes | 2.481 | 1.863 | 120 | 4.464 |
| Nota geral média | 39,71 | 35,27 | 35,91 | – |
| Nota centrada média | +3,04 | −1,77 | −2,01 | – |

Fonte: elaborado pelo autor a partir dos microdados do ENADE 2021 (INEP).

Os nomes dos grupos descrevem o perfil de financiamento, e não o desempenho. No primeiro grupo, 96% dos estudantes, em média, declaram estudar em curso gratuito sem financiamento. No segundo, predominam estudantes que pagam o curso sem bolsa (30%) ou com FIES (19%). O terceiro reúne oito cursos privados de Sistemas de Informação do Nordeste em que 41% dos estudantes têm ProUni parcial e 29% combinam ProUni parcial e FIES. As duas exceções da Tabela 3 confirmam que o critério é o financiamento. O curso privado do primeiro grupo tem 73% dos estudantes com ProUni integral, ou seja, sem pagar mensalidade. O curso municipal aparece com os privados porque é pago: 51% dos estudantes declaram curso não gratuito sem bolsa.


O teste sem a pergunta de financiamento mostra de onde vem essa separação (Tabela 4 e Figura 3). Sem `QE_I11`, o ARI com a divisão pública × privada cai para 0,142, abaixo do limite de 0,20 fixado antes da análise: a separação some. As perguntas de aulas práticas e de ação afirmativa, sozinhas, produzem grupos estáveis, mas sem relação com o tipo de instituição. A pergunta de financiamento, sozinha, reproduz quase a mesma partição do modelo completo (ARI de 0,963 com ela).

**Tabela 4 – Concordância com a divisão pública × privada conforme as perguntas usadas**

| Perguntas usadas no agrupamento | ARI com pública × privada | Silhouette |
|---|---:|---:|
| Todas (aulas práticas, ação afirmativa e financiamento) | 0,855 | 0,212 |
| Sem financiamento (aulas práticas e ação afirmativa) | 0,142 | 0,142 |
| Só aulas práticas (`QE_I63`) | 0,059 | 0,300 |
| Só ação afirmativa (`QE_I15`) | 0,110 | 0,265 |
| Só financiamento (`QE_I11`) | 0,868 | 0,470 |

Fonte: elaborado pelo autor a partir dos microdados do ENADE 2021 (INEP). Em todos os casos, \(k=3\), 50 inicializações e semente 42.

**Figura 3 – Concordância dos grupos com a divisão pública × privada conforme as perguntas usadas**

![Figura 3 – Concordância dos grupos com a divisão pública × privada conforme as perguntas usadas](../../../MultiENADE_Analises/figuras/figura_3_ari_perguntas.png)

Fonte: elaborado pelo autor a partir dos microdados do ENADE 2021 (INEP).

O agrupamento, portanto, reproduz a divisão entre públicas e privadas, mas por um eixo estreito: a forma de pagamento do curso. Esse resultado também muda a leitura de um achado da versão anterior do trabalho. A proporção de estudantes que concordam totalmente que os ambientes de aulas práticas são adequados é menor no grupo das públicas (38,7%) do que nos privados (59,4% e 63,9%). Isso não indica que melhor infraestrutura se associe a menor nota. Indica que estudantes de cursos privados avaliam melhor essa infraestrutura, e esses cursos têm, em média, notas menores. A relação passa pela divisão entre os setores.

O silhouette de 0,212 indica grupos pouco separados, e quatro cursos ficam mais próximos de outro grupo do que do seu. Os grupos devem ser lidos como perfis de financiamento, e não como categorias naturais dos cursos.

## 4.3 A regressão confirma e qualifica a divisão

A Tabela 5 e a Figura 4 mostram a escada de modelos. Saber apenas se o curso é público ou privado (modelo 1) explica em média 6% da variação da nota, com grande oscilação entre as divisões (desvio de 0,21). O ganho sobre a média é positivo em 37 das 50 divisões, abaixo do critério de 40, e conta como empate. A categoria descreve uma diferença real de médias (seção 4.1), mas, sozinha, prevê de forma fraca e instável.

**Tabela 5 – Escada de modelos na validação cruzada repetida (alvo: nota centrada por área)**

| Modelo | Variáveis | \(R^2\) médio | Desvio | MAE (pontos) |
|---|---|---:|---:|---:|
| 0 | Nenhuma (média) | −0,08 | 0,12 | 4,92 |
| 1 | Pública (linear) | 0,06 | 0,21 | 4,53 |
| 2 | Pública + componentes econômicos (linear) | 0,31 | 0,15 | 3,86 |
| **3** | **14 variáveis (Random Forest, modelo final)** | **0,49** | **0,14** | **3,38** |
| 3L | 14 variáveis (linear) | 0,36 | 0,26 | 3,52 |
| 4 | 14 variáveis + categoria administrativa (Random Forest) | 0,49 | 0,14 | 3,38 |
| 5 | 14 variáveis sem as substitutas (Random Forest) | 0,34 | 0,15 | 3,80 |

Fonte: elaborado pelo autor a partir dos microdados do ENADE 2021 (INEP). Validação cruzada de cinco partes repetida 10 vezes (50 avaliações).

**Figura 4 – R² dos modelos da escada nas 50 avaliações da validação cruzada**

![Figura 4 – R² dos modelos da escada nas 50 avaliações da validação cruzada](../../../MultiENADE_Analises/figuras/figura_4_escada_modelos.png)

Fonte: elaborado pelo autor a partir dos microdados do ENADE 2021 (INEP).

Acrescentar o índice de trabalho e renda (modelo 2) eleva o \(R^2\) para 0,31, e o modelo final chega a **0,49 ± 0,14**, com erro médio de 3,4 pontos. Os dois ganhos atendem ao critério (46 de 50 divisões cada um). Três comparações qualificam esse resultado:

- **A categoria administrativa não acrescenta nada ao modelo final** (modelo 4: diferença média de −0,001). As substitutas do tipo de instituição já carregam toda a informação da categoria.
- **O modelo depende das substitutas.** Sem elas (modelo 5), o \(R^2\) cai 0,14 em média, com perda em 45 das 50 divisões.
- **O algoritmo importa menos do que as variáveis.** O modelo linear com as mesmas 14 variáveis (3L) não fica claramente atrás pelo critério adotado: perde em 36 das 50 divisões, abaixo do limite de 40. Mas fica 0,13 abaixo em média e é bem menos estável (desvio de 0,26, contra 0,14). Com a nota bruta como alvo, a desvantagem do modelo linear atende ao critério.

Com exceção dessa última comparação, todos os vereditos foram iguais nos quatro alvos testados. O resultado não depende, portanto, de como a nota foi ajustada entre as áreas.

O \(R^2\) de 0,498 publicado na versão anterior foi obtido em uma única divisão de 33 cursos para teste, com a nota bruta. Repetindo o mesmo procedimento em 100 divisões diferentes (Figura 5), o \(R^2\) médio foi de 0,44, com 90% dos valores entre 0,25 e 0,56. O 0,498 ficou acima de 69% delas: uma divisão favorável, mas não fora do comum. Ajustar a PCA econômica somente com os cursos de treino não alterou o resultado (diferença média de 0,0005).

**Figura 5 – R² do modelo final em 100 divisões treino/teste diferentes**

![Figura 5 – R² do modelo final em 100 divisões treino/teste diferentes](../../../MultiENADE_Analises/figuras/figura_5_r2_100_divisoes.png)

Fonte: elaborado pelo autor a partir dos microdados do ENADE 2021 (INEP).

**Direção dos efeitos.** A Figura 6 resume os valores SHAP calculados nas previsões fora do treino. As contribuições foram agrupadas nos três blocos do Quadro 2. As substitutas do tipo de instituição respondem por 59% da contribuição total, o índice econômico por 23% e as demais variáveis por 18%. A variável de maior peso é a proporção de estudantes com bolsa de iniciação científica (`QE_I13_B`): entre os cursos do quartil mais alto e os do quartil mais baixo dessa proporção, a contribuição para a nota prevista difere em 4,1 pontos. A proporção sem nenhuma bolsa acadêmica (`QE_I13_A`) atua no sentido oposto (−2,7 pontos). O índice de trabalho e renda vem em seguida, com efeito positivo (+2,7 pontos): cursos cujos estudantes trabalham mais e têm maior renda familiar tendem a ter notas mais altas.

**Figura 6 – Contribuição das variáveis para a nota prevista (SHAP), por bloco e com direção**

![Figura 6 – Contribuição das variáveis para a nota prevista (SHAP), por bloco e com direção](../../../MultiENADE_Analises/figuras/figura_6_shap.png)

Fonte: elaborado pelo autor a partir dos microdados do ENADE 2021 (INEP).

A importância preditiva não indica causalidade. Esse cuidado é especialmente necessário para a bolsa de iniciação científica, que costuma ser concedida a estudantes de bom desempenho e pode funcionar como marcador do desempenho, e não como sua causa.

**Dentro de cada setor.** Os dois blocos principais atuam em níveis diferentes. Na média de cada grupo do agrupamento, a contribuição das substitutas é de +2,2 pontos no grupo das públicas e de cerca de −3,0 nos dois grupos privados, enquanto a do índice econômico é próxima de zero nos três. As substitutas, portanto, explicam a diferença **entre** os setores, e o índice de trabalho e renda explica diferenças **dentro** deles. A Tabela 6 e a Figura 7 confirmam isso. O modelo final explica 59% da variação da nota entre os cursos públicos e 26% entre os privados. Um modelo que só soubesse o tipo de instituição explicaria zero nos dois casos.

**Tabela 6 – Variação da nota explicada dentro de cada setor (previsões fora do treino)**

| Modelo | Públicas (74 cursos) | Privadas (87 cursos) |
|---|---:|---:|
| 1: só pública (linear) | 0 | 0 |
| 2: pública + componentes econômicos (linear) | 0,36 | 0,09 |
| 3: modelo final (Random Forest) | 0,59 | 0,26 |

Fonte: elaborado pelo autor a partir dos microdados do ENADE 2021 (INEP). Média de 10 repetições da validação cruzada; desvio de no máximo 0,04 em todos os casos.

**Figura 7 – Variação da nota explicada dentro de cada setor**

![Figura 7 – Variação da nota explicada dentro de cada setor](../../../MultiENADE_Analises/figuras/figura_7_r2_dentro_setor.png)

Fonte: elaborado pelo autor a partir dos microdados do ENADE 2021 (INEP).

Retirar apenas `QE_I11_A`, a variável quase idêntica à categoria, não muda o desempenho (\(R^2\) de 0,490, contra 0,488). As outras substitutas compensam a sua ausência. Diferentemente do agrupamento, a regressão não depende de um único item.

## 4.4 A associação como contraponto

A análise de associação quase não encontra ligação entre a percepção dos estudantes e a faixa de desempenho do curso. Para cada item e nível de resposta, a proporção dentro de cada faixa ficou entre 0,84 e 1,11 vezes a proporção geral (1 indica ausência de associação). O maior desvio é o da resposta máxima sobre oportunidades de iniciação científica, menos frequente na faixa baixa (0,84) e mais frequente na alta (1,11).

As 50 regras publicadas são dominadas por um efeito halo: estudantes que avaliam bem um aspecto do curso tendem a avaliar bem os demais. Nenhuma delas tem a faixa de desempenho na conclusão. Isso, porém, é efeito do filtro de publicação, e não ausência de regras. Entre as 377 regras com até três itens, dez têm "desempenho alto" na conclusão, com confiança entre 0,50 e 0,58 e lift entre 1,20 e 1,41. Todas ficam fora das 50 publicadas porque o menor lift publicado é 1,90. Seis dessas dez envolvem a resposta máxima sobre oportunidades de iniciação científica, o mesmo padrão das regras que se repetiram nas duas metades da base (seção 3.6).

Quatro regras publicadas têm "desempenho baixo" na condição, como `desempenho_baixo` + `metodologia_inovadora_5` → `formacao_critica_5` (confiança de 0,54 e lift de 2,13). A Tabela 7 mostra que essas regras dizem respeito à relação entre os itens, e não ao desempenho. Sem a faixa, as mesmas relações têm lift de cerca de 1,8 e confiança entre 0,47 e 0,54. Nas três faixas, a confiança é parecida; a faixa baixa apenas a eleva um pouco acima do corte de 0,50, e por isso a regra aparece com ela.

**Tabela 7 – Confiança das regras publicadas com desempenho na condição, por faixa**

| Regra (sem a faixa) | Lift sem a faixa | Confiança: baixo | Confiança: médio | Confiança: alto |
|---|---:|---:|---:|---:|
| metodologia inovadora 5 → formação crítica 5 | 1,84 | 0,54 | 0,45 | 0,43 |
| oportunidades de IC 5 → disponibilidade dos professores 5 | 1,80 | 0,52 | 0,46 | 0,44 |
| disponibilidade dos professores 5 → domínio dos professores 5 | 1,82 | 0,58 | 0,55 | 0,50 |
| domínio dos professores 5 → disponibilidade dos professores 5 | 1,82 | 0,51 | 0,47 | 0,46 |

Fonte: elaborado pelo autor a partir dos microdados do ENADE 2021 (INEP). IC = iniciação científica.

No nível do curso, o resultado é o mesmo. A correlação de Spearman entre a proporção de estudantes que concordam com cada item (respostas 5 ou 6) e a nota centrada fica entre 0,005 e 0,117 para cinco dos seis itens. A exceção é o item de oportunidades de iniciação científica (0,315).

Um fator que enfraquece essa análise é a repetição de respostas: 24,8% dos estudantes deram a mesma resposta aos seis itens, e em 82,3% desses casos foi a resposta máxima. Esse padrão é menos frequente na faixa alta (19,7%) do que nas outras duas (cerca de 28%), o que sugere que parte das percepções positivas reflete uma avaliação geral do curso, e não de cada aspecto.

A associação funciona, assim, como contraponto: a percepção do curso pelos estudantes quase não se liga ao desempenho. O único sinal consistente, embora fraco, é o da iniciação científica, que converge com a variável de maior peso na regressão (`QE_I13_B`).

## 4.5 O que convergiu e o que não convergiu

O Quadro 3 resume o que cada técnica mostrou e a sua relação com a divisão entre instituições públicas e privadas.

**Quadro 3 – Síntese da verificação entre as técnicas**

| Técnica | O que recebeu | O que mostrou | Relação com público × privado |
|---|---|---|---|
| Agrupamento | Proporções de financiamento, ação afirmativa e avaliação das aulas práticas, por curso | Três perfis de financiamento | Reproduz a divisão (ARI 0,855), mas só pela pergunta de financiamento; sem ela, a divisão some (0,142) |
| Regressão | 14 variáveis socioeconômicas, acadêmicas e institucionais, por curso | \(R^2\) de 0,49 ± 0,14; maiores pesos: bolsa de iniciação científica e índice de trabalho e renda | Confirma, porque as substitutas da categoria têm o maior peso e a categoria não acrescenta nada a elas. Qualifica, porque a categoria sozinha prevê mal (0,06) e o modelo explica 59% da variação dentro das públicas e 26% dentro das privadas |
| Associação | Seis itens de percepção, por estudante, com a faixa de desempenho do curso | Efeito halo entre percepções; vínculo fraco com o desempenho | Não se relaciona diretamente; o único sinal (iniciação científica) converge com a regressão |

Fonte: elaborado pelo autor.

A integração das técnicas teve um papel que nenhuma delas cumpriria sozinha. Isolado, o agrupamento levaria a uma conclusão quase óbvia: os cursos se dividem em públicos e privados. A regressão mostrou que essa divisão, embora real, explica pouco da nota por si só. O que explica a nota é uma combinação de acesso à pesquisa e às bolsas acadêmicas e da situação de trabalho e renda dos estudantes, e essa combinação também atua dentro de cada setor, onde o agrupamento não enxerga diferenças. A associação mostrou que a percepção do curso pelos estudantes é uma dimensão quase independente do desempenho, mas apontou para o mesmo fator, a iniciação científica, que a regressão destacou. Por fim, o teste sem a pergunta de financiamento só foi possível porque havia mais de uma técnica para comparar, e ele revelou que a separação dos grupos depende de um único item do questionário.

---

# 5 CONSIDERAÇÕES FINAIS

Este trabalho apresentou o MultiENADE, um artefato computacional para análise multidimensional de microdados do ENADE, e o utilizou para responder a uma pergunta: os padrões encontrados por agrupamento, regressão e regras de associação convergem para a divisão entre instituições públicas e privadas, e o que eles mostram além dela?

A resposta tem três partes. O agrupamento reproduz a divisão: os três perfis formados sem a nota e sem a categoria administrativa coincidem quase totalmente com ela (ARI de 0,855). Mas a separação vem de uma única pergunta, a de financiamento das mensalidades, e desaparece sem ela. A regressão confirma e qualifica a divisão. As variáveis mais ligadas ao tipo de instituição têm o maior peso, e a categoria administrativa não acrescenta nada a elas. Porém, saber apenas se o curso é público ou privado explica 6% da variação da nota, enquanto o modelo com 14 variáveis explica 49% (± 14%). Esse modelo explica 59% da variação entre os cursos públicos e 26% entre os privados. Os fatores de maior peso são a proporção de estudantes com bolsa de iniciação científica e o índice de trabalho e renda. A associação funciona como contraponto: a percepção dos estudantes sobre o curso quase não se liga ao desempenho, e o único sinal encontrado, também fraco, aponta para a iniciação científica.

A principal contribuição do trabalho é esse resultado de verificação. A divisão entre públicas e privadas é real, mas estreita. Ela resume diferenças de acesso à pesquisa e às bolsas acadêmicas e de situação de trabalho e renda dos estudantes, e essas mesmas diferenças continuam a distinguir os cursos dentro de cada setor. Nenhuma das técnicas, isoladamente, mostraria isso.

A segunda contribuição é o próprio artefato. O MultiENADE reúne preparação de dados, três técnicas de modelagem e mecanismos de interpretação em um processo reproduzível, com resultados publicados no ecossistema EnadeX. As análises de verificação foram implementadas em scripts que conferem se reproduzem os números do sistema antes de qualquer medida, e os critérios de cada teste foram definidos antes dos resultados.

A terceira contribuição é a documentação do processo exploratório. Ela mostrou que a escolha das variáveis pesou mais do que a do algoritmo ou dos hiperparâmetros, que a retirada da nota das variáveis de agrupamento eliminou uma circularidade, e que a forma de medir o desempenho do modelo altera o número apresentado: o \(R^2\) de 0,498 obtido em uma única divisão de teste ficou acima de 69% das divisões possíveis.

Considerando a perspectiva de Design Science Research, a avaliação realizada foi exclusivamente computacional e empírica. Não foram realizados testes com especialistas, estudos formais de usabilidade ou avaliações externas da utilidade dos relatórios produzidos.

Entre as limitações, destacam-se:

- **a variável de financiamento** (`QE_I11_A`) é quase idêntica ao indicador de instituição pública (correlação de 0,986), e a separação dos grupos depende dela;
- **o alvo reúne duas provas diferentes**, de Ciência da Computação e de Sistemas de Informação; o ajuste pela média nacional da área reduz, mas não elimina, essa diferença, e os resultados foram confirmados com três alvos alternativos;
- **as conclusões valem no nível do curso**, e não do estudante, porque os arquivos dos microdados não permitem vincular as respostas de um mesmo estudante;
- **a amostra tem 161 cursos**, o que se reflete na oscilação do \(R^2\) entre as divisões da validação (desvio de 0,14);
- **as relações observadas não são causais**, em especial a da bolsa de iniciação científica, que pode ser consequência do bom desempenho;
- **um quarto dos estudantes deu a mesma resposta aos seis itens de percepção**, o que enfraquece a análise de associação;
- **a edição de 2021 foi aplicada durante a pandemia**, e o recorte se limita a duas áreas e duas regiões; a ausência de teste formal de significância das regras de associação também deve ser considerada.

Como trabalhos futuros, recomenda-se:

- ampliar o estudo para outras edições do ENADE, com harmonização das variáveis entre os anos;
- refazer o agrupamento dentro das instituições públicas e dentro das privadas, para identificar perfis que a divisão entre os setores encobre;
- repetir a análise de associação no nível do curso, com o desempenho na conclusão das regras;
- avaliar o artefato com especialistas e integrá-lo às camadas de explicabilidade, Visual Analytics e análise histórica do EnadeX.

Em síntese, a integração de técnicas mostrou que a divisão entre instituições públicas e privadas, visível no agrupamento, encobre diferenças dentro de cada setor que só a regressão e a associação revelam. O MultiENADE demonstra, assim, que a análise multidimensional serve não apenas para produzir visões diferentes dos dados, mas para verificar e qualificar um achado, mantendo a distinção entre associação, capacidade preditiva e causalidade.

---

# REFERÊNCIAS

AGRAWAL, Rakesh; SRIKANT, Ramakrishnan. Fast algorithms for mining association rules. In: **Proceedings of the 20th International Conference on Very Large Data Bases (VLDB)**. Santiago de Chile: Morgan Kaufmann, 1994.

ARAÚJO, Rodrigo Alexandrino. **Análise dos microdados do Enade: proposta de uma ferramenta de exploração utilizando mineração de dados**. 2020. 69 f. Dissertação (Mestrado em Ciência da Computação) — Universidade Federal de Goiás, Goiânia, 2020.

BAKER, Ryan Shaun Joazeiro de; ISOTANI, Seiji; CARVALHO, Adriana Maria Joazeiro Baker de. Mineração de Dados Educacionais: oportunidades para o Brasil. **Revista Brasileira de Informática na Educação**, v. 19, n. 02, p. 03-13, 2011.

BERVIAN, Luciana Merlin; CORRÊA, Marianna. ENADE: impactos da categoria administrativa, organização acadêmica e número de participantes no desempenho dos estudantes. **Revista de Administração Educacional**, Recife, v. 1, n. 2, p. 6-27, jul./dez. 2015.

BREIMAN, Leo. Random forests. **Machine Learning**, v. 45, p. 5-32, 2001.

CRISPIM NETO, Wilton Ribeiro. **O uso de mineração de dados educacionais sob o ENADE como apoio ao processo de tomada de decisão de gestores do ensino superior**. 2020. Trabalho de Conclusão de Curso — Universidade Federal do Ceará, Fortaleza, 2020.

GUILHEN, Bruno Anselmo. **Avaliação de algoritmos para a análise preditiva das notas de Engenharia no ENADE utilizando dados socioeconômicos**. 2024. Dissertação (Mestrado) — Universidade de São Paulo, São Paulo, 2024.

HEVNER, Alan R. et al. Design science in information systems research. **MIS Quarterly**, v. 28, n. 1, p. 75-105, 2004.

HUBERT, Lawrence; ARABIE, Phipps. Comparing partitions. **Journal of Classification**, v. 2, n. 1, p. 193-218, 1985.

JICK, Todd D. Mixing qualitative and quantitative methods: triangulation in action. **Administrative Science Quarterly**, v. 24, n. 4, p. 602-611, 1979.

JOLLIFFE, Ian T.; CADIMA, Jorge. Principal component analysis: a review and recent developments. **Philosophical Transactions of the Royal Society A**, v. 374, 2016.

LIMA, Priscila da Silva Neves et al. Análise de dados do Enade e Enem: uma revisão sistemática da literatura. **Avaliação: Revista da Avaliação da Educação Superior**, v. 24, n. 1, p. 89-107, 2019.

LUNDBERG, Scott M.; LEE, Su-In. A unified approach to interpreting model predictions. In: **Advances in Neural Information Processing Systems**, 30., 2017.

MACQUEEN, James. Some methods for classification and analysis of multivariate observations. In: **Proceedings of the Fifth Berkeley Symposium on Mathematical Statistics and Probability**. Berkeley: University of California Press, 1967. v. 1, p. 281-297.

PEFFERS, Ken et al. A design science research methodology for information systems research. **Journal of Management Information Systems**, v. 24, n. 3, p. 45-77, 2007.


ROUSSEEUW, Peter J. Silhouettes: a graphical aid to the interpretation and validation of cluster analysis. **Journal of Computational and Applied Mathematics**, v. 20, p. 53-65, 1987.

<!-- PENDENTE: conferir os dados de HUBERT; ARABIE (1985), JICK (1979), MACQUEEN (1967) e ROUSSEEUW (1987) com as fontes originais. -->

---

# APÊNDICE – PROMPTS DE USO DE IA

**Ferramenta utilizada:** Gemini (Google), usado pelo navegador; Claude Code (Anthropic), com os modelos Claude Opus 5.5, Claude Sonnet e Claude Fable.

**Finalidade de utilização:** as ferramentas foram organizadas em uma arquitetura de agentes, usada apenas no subprojeto MultiENADE, com um papel para cada modelo:

- Gemini: agente de pesquisa, usado para levantamento de bibliografia e leitura de artigos e documentos. Ele não faz parte da arquitetura de software do Claude Code: as consultas foram feitas pelo navegador, e o autor trazia os resultados para o projeto. Foi escolhido para essa função por estar integrado ao mecanismo de busca do Google, o que torna a localização e a consulta de fontes mais eficientes.
- Claude Opus 5.5: gestor do projeto. Interagia com o autor, planejava as etapas, os testes e os critérios de análise, organizava os diretórios e criava subagentes para executar as tarefas. A divisão entre gestor e executores foi adotada para economizar tokens: o modelo de maior custo planeja e coordena, e a execução, que consome a maior parte dos tokens, fica com modelos menores.
- Claude Sonnet: subagentes executores, criados pelo gestor para aplicar as alterações de código e de texto. Por serem modelos menores e de menor custo, assumiam o trabalho de execução.
- Claude Fable: subagente revisor, acionado periodicamente pelo gestor para revisar por completo o código, o artigo e a documentação do MultiENADE. Por ser o modelo de maior custo e maior capacidade entre os usados, atuava de forma isolada e pontual: não corrigia erros nem escrevia código, apenas avaliava o conjunto e verificava se as partes do projeto estavam coerentes entre si.

**Prompts ou instruções fornecidas:** O autor dava as instruções ao agente gestor, que as transformava em tarefas para os subagentes. Exemplos de instruções do autor, transcritas como foram escritas:

- "Antes de decidir, me dá um resumo do caminho que tomamos, estamos chegando em algum lugar?"
- "Me explica melhor isso do 1-0"
- "Pode seguir, se for fazer coisas que consomem muita ram, confere se tem disponibilidade"
- "Confere também mais uma coisa, um outro critério era que todas as variáveis do Enade precisavam ser explicadas na primeira vez que ela apareceu no projeto."
- "Tá, então como já está tudo planejado, atualiza logo o back-end, já pode fazer o push, primeiro confere se tem algum pull, alguma alteração externa pendente"

As instruções aos subagentes executores e revisores eram escritas pelo próprio agente gestor, a partir do plano aprovado pelo autor.

**Revisão e edição:** todo o conteúdo produzido pelas ferramentas foi revisado pelo autor. As decisões de método e de argumento, como a tese do artigo, os critérios de análise, a escolha do R² principal e os nomes dos grupos, foram tomadas pelo autor a partir de propostas do agente gestor, e os critérios de análise foram fixados antes de os resultados serem conhecidos. Os números do artigo vêm de scripts versionados, que conferem se reproduzem os resultados do sistema antes de qualquer medida. Alterações com efeito em produção, como a regravação do banco de dados e a publicação do site, só foram feitas com autorização do autor.

**Limitações e considerações éticas:** modelos de linguagem podem gerar informações incorretas, inclusive números e referências. Durante o trabalho, as conferências encontraram erros desse tipo em rascunhos, que foram corrigidos antes de entrar no texto. Por isso, nenhum número foi aceito sem o script que o produz, e as referências sugeridas foram conferidas nas fontes originais. Os dados analisados são os microdados públicos e anonimizados do ENADE 2021. A responsabilidade pelo conteúdo, pelas análises e pelas conclusões é do autor.
