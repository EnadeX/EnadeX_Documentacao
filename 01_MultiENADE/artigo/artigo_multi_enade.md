# MultiENADE: uma abordagem multidimensional para análise exploratória de microdados do ENADE

## RESUMO

A exploração de microdados do Exame Nacional de Desempenho dos Estudantes (ENADE) apresenta desafios relacionados à fragmentação das análises, à dificuldade de reprodução dos procedimentos e à baixa integração entre diferentes técnicas de mineração de dados. Este trabalho apresenta o MultiENADE, componente do ecossistema EnadeX, desenvolvido como artefato computacional para análise multidimensional de fatores associados ao desempenho de cursos de Ciência da Computação e Sistemas de Informação no ENADE 2021. O estudo adotou abordagem quantitativa e exploratória, estruturada segundo princípios de Design Science Research, e integrou técnicas de regressão, agrupamento e mineração de regras de associação. O recorte considerou cursos das regiões Norte e Nordeste, resultando em 190 cursos de origem e 161 cursos com pelo menos cinco respondentes utilizados nos modelos de regressão e agrupamento. O modelo de regressão Random Forest apresentou \(R^2=0,498\), MAE=3,03 e RMSE=3,82 no conjunto de teste. O agrupamento identificou três perfis institucionais, com coeficiente de silhouette de 0,211. Na análise de associações, foram identificadas relações condicionais entre percepções acadêmicas e a faixa de desempenho dos cursos. Os resultados indicam que a integração de diferentes técnicas produz perspectivas complementares sobre os microdados, embora permaneçam limitações relacionadas ao recorte temporal, à ausência de avaliação por especialistas e às características estruturais dos dados do ENADE.

**Palavras-chave:** ENADE; Mineração de Dados; Mineração de Dados Educacionais; Análise Multidimensional; Exploração de Dados.

---

# 1 INTRODUÇÃO

O Exame Nacional de Desempenho dos Estudantes (ENADE) constitui uma importante fonte de dados para investigação das condições acadêmicas, socioeconômicas e institucionais relacionadas ao ensino superior brasileiro. Os microdados disponibilizados pelo Instituto Nacional de Estudos e Pesquisas Educacionais Anísio Teixeira (INEP) apresentam grande quantidade de informações sobre estudantes, cursos, instituições, condições de ingresso, aspectos socioeconômicos e percepções relacionadas à formação acadêmica. Apesar dessa riqueza, a exploração desses dados ainda pode ocorrer de forma fragmentada, com análises concentradas em relações isoladas entre poucas variáveis.

Essa característica é observada em estudos que utilizam dados educacionais por meio de estatísticas descritivas, regressões ou técnicas específicas de mineração aplicadas individualmente a determinados problemas. A revisão apresentada por Lima et al. (2019), por exemplo, evidencia a recorrência de abordagens descritivas e de análises direcionadas a relações específicas entre variáveis do ENADE e de outros exames educacionais. No campo da Mineração de Dados Educacionais, diferentes técnicas já foram aplicadas aos dados do ENADE, incluindo agrupamento, classificação, árvores de decisão, regressão e abordagens baseadas em KDD (ARAÚJO, 2020; CRISPIM NETO, 2020; GUILHEN, 2024). Entretanto, a utilização integrada dessas perspectivas permanece menos explorada.

Nesse contexto, surge o problema de pesquisa deste trabalho: **como analisar de forma integrada e multidimensional os fatores socioeconômicos, institucionais e acadêmicos associados ao desempenho no ENADE, superando abordagens isoladas e fragmentadas?**

O MultiENADE foi desenvolvido como resposta a esse problema e integra diferentes técnicas de exploração de dados em um mesmo artefato analítico. O objetivo geral consiste em desenvolver e avaliar um processo computacional para análise multidimensional de microdados do ENADE, capaz de combinar diferentes perspectivas analíticas e produzir padrões interpretáveis sobre o desempenho dos cursos.

Como objetivos específicos, buscou-se: desenvolver um processo de preparação e integração dos dados; investigar fatores associados ao desempenho médio dos cursos por meio de modelos supervisionados; identificar perfis de cursos a partir de técnicas de agrupamento; explorar relações de coocorrência por meio de regras de associação; incorporar mecanismos de interpretação dos resultados; e avaliar empiricamente a consistência das diferentes abordagens.

O trabalho integra o ecossistema EnadeX, uma proposta de arquitetura de mineração, explicabilidade e Visual Analytics aplicada aos microdados do ENADE. Nesse ecossistema, o MultiENADE assume o papel de motor de descoberta multidimensional, enquanto outros projetos concentram-se em aspectos como explicabilidade estatística, segmentação visual e análise temporal.

A pesquisa caracteriza-se como aplicada e quantitativa, de natureza predominantemente exploratória, sendo conduzida segundo princípios de Design Science Research. O processo envolveu revisão da lógica de negócio, preparação dos dados, exploração de diferentes variáveis e algoritmos, avaliação dos resultados, refinamento dos modelos e publicação dos resultados em estruturas persistentes. O estudo foi delimitado ao ENADE 2021, considerando cursos dos grupos definidos para Ciência da Computação e Sistemas de Informação localizados nas regiões Norte e Nordeste.

---

# 2 REFERENCIAL TEÓRICO

## 2.1 Microdados educacionais e o ENADE

Os microdados educacionais constituem uma fonte relevante para estudos quantitativos sobre fatores relacionados ao ensino e ao desempenho acadêmico. No caso do ENADE, esses dados permitem investigar não apenas resultados de desempenho, mas também características socioeconômicas dos estudantes, condições institucionais, infraestrutura, formação acadêmica e percepções relacionadas à experiência universitária.

A disponibilidade dessas variáveis possibilita abordagens que ultrapassam a análise isolada da nota. Entretanto, a diversidade das informações também exige procedimentos adequados de preparação, harmonização e seleção das variáveis. A literatura brasileira apresenta aplicações variadas de técnicas de mineração sobre o ENADE, mostrando o potencial dos microdados para apoiar processos de descoberta de conhecimento e tomada de decisão (ARAÚJO, 2020; CRISPIM NETO, 2020).

Lima et al. (2019), ao realizarem uma revisão sistemática de estudos envolvendo ENADE e ENEM, apontam a predominância de análises descritivas e abordagens orientadas a relações específicas. Essa característica reforça a oportunidade de explorar métodos que integrem diferentes dimensões dos dados.

## 2.2 Mineração de Dados Educacionais

A Mineração de Dados Educacionais busca aplicar métodos computacionais para descobrir padrões, relações e informações úteis em grandes volumes de dados educacionais. Baker, Isotani e Carvalho (2011) destacam diferentes categorias de problemas nesse campo, incluindo predição, agrupamento, mineração de relações e descoberta de modelos.

Entre as possibilidades destacadas pelos autores encontram-se a classificação e regressão para predição, técnicas de clustering para identificação de grupos, mineração de relações para descoberta de associações e técnicas de descoberta de modelos para produção de conhecimento interpretável.

O MultiENADE incorpora essas diferentes perspectivas em uma arquitetura integrada, utilizando modelos supervisionados, agrupamento e regras de associação como instrumentos complementares de exploração.

## 2.3 Modelos supervisionados e Random Forest

Modelos supervisionados são utilizados quando existe uma variável de interesse que deve ser estimada a partir de outras características observadas. No contexto deste trabalho, a variável-alvo é a média da nota geral (`NT_GER`) por curso.

Entre os algoritmos avaliados estiveram regressão linear, Ridge, Lasso, Elastic Net, árvore de decisão, Random Forest, Gradient Boosting, KNN e SVR. O Random Forest apresentou o melhor desempenho no processo de comparação realizado.

O método Random Forest combina múltiplas árvores de decisão construídas a partir de amostras e subconjuntos de variáveis, permitindo representar relações não lineares e interações entre características (BREIMAN, 2001). Essa propriedade é particularmente relevante em dados educacionais, nos quais relações entre condições socioeconômicas, características institucionais e desempenho podem não apresentar comportamento linear.

Além da predição, a interpretação das contribuições das variáveis é importante em contextos de análise exploratória. Por esse motivo, o MultiENADE incorporou valores de importância derivados do modelo para apoiar a interpretação dos resultados.

## 2.4 Agrupamento e redução de dimensionalidade

A análise de agrupamentos busca identificar conjuntos de observações com características semelhantes sem utilizar uma variável-alvo previamente definida. O K-Means é um dos métodos mais utilizados nessa finalidade e procura particionar as observações em \(k\) grupos minimizando a distância entre os elementos e o centro de seus respectivos grupos.

No MultiENADE, os agrupamentos foram formados a partir de proporções de respostas relacionadas à infraestrutura, cotas e financiamento ou bolsas. A nota geral não foi utilizada como variável de entrada, evitando circularidade entre a formação dos grupos e sua posterior interpretação pelo desempenho.

Para visualização espacial dos resultados, utilizou-se Análise de Componentes Principais (PCA). A PCA permite representar uma parte relevante da variabilidade dos dados por meio de um conjunto reduzido de componentes, facilitando a visualização de estruturas multidimensionais.

No modelo de regressão, a PCA também foi utilizada de forma específica para condensar as variáveis econômicas ordinais. O primeiro componente representou um eixo coerente de condição socioeconômica, permitindo reduzir a dimensionalidade sem eliminar o sinal preditivo encontrado nas variáveis originais.

## 2.5 Regras de associação

As regras de associação permitem identificar padrões de coocorrência entre características de um conjunto de observações. O algoritmo Apriori é tradicionalmente empregado para descobrir conjuntos frequentes de itens e construir regras a partir deles (AGRAWAL; SRIKANT, 1994).

Entre as principais medidas utilizadas estão suporte, confiança e lift. O suporte representa a frequência com que uma combinação aparece na base; a confiança mede a frequência com que o consequente ocorre quando o antecedente está presente; e o lift compara essa ocorrência com aquilo que seria esperado sob independência.

No contexto do MultiENADE, essas medidas foram utilizadas para investigar relações entre percepções sobre formação e experiência acadêmica, incorporando também a faixa de desempenho do curso como característica contextual.

## 2.6 Explicabilidade e interpretação de modelos

A capacidade de produzir previsões não é suficiente para uma análise exploratória orientada à descoberta de conhecimento. É necessário também compreender quais características possuem maior participação nos resultados.

Nesse contexto, técnicas de explicabilidade permitem examinar a contribuição das variáveis para as previsões produzidas por modelos complexos. O MultiENADE incorporou informações de importância das características e análise baseada em SHAP como mecanismo complementar de interpretação.

Essa preocupação está alinhada à proposta geral do EnadeX de combinar mineração de dados e explicabilidade, reduzindo a distância entre desempenho computacional e interpretação dos padrões encontrados.

## 2.7 Design Science Research

O desenvolvimento do MultiENADE foi orientado por princípios de Design Science Research (DSR), abordagem voltada à construção e avaliação de artefatos para problemas relevantes. Hevner et al. (2004) destacam a importância de construir artefatos e submetê-los a processos sistemáticos de avaliação, enquanto Peffers et al. (2007) apresentam um processo estruturado para investigação orientada à criação de soluções.

Nesse trabalho, o artefato corresponde ao processo computacional do MultiENADE, incluindo ingestão e preparação dos dados, construção de variáveis analíticas, aplicação de diferentes modelos e publicação dos resultados.

---

# 3 MÉTODOS E PROCEDIMENTOS

## 3.1 Caracterização da pesquisa

A pesquisa é de natureza aplicada, com abordagem quantitativa e caráter exploratório. O desenvolvimento do artefato foi conduzido segundo princípios de Design Science Research, tendo como foco a construção e avaliação empírica de um processo de análise multidimensional dos microdados do ENADE.

A unidade de análise variou conforme a técnica empregada. Nos modelos de regressão e agrupamento, os dados foram consolidados no nível do curso. Na análise de associação, foram utilizadas respostas individuais dos estudantes provenientes do arquivo que contém os itens Likert, sendo posteriormente incorporada a faixa de desempenho do respectivo curso.

## 3.2 Recorte dos dados

Foram utilizados microdados do ENADE 2021. O recorte foi definido pelos grupos de cursos 4004 e 4006 e pelas regiões Norte e Nordeste.

A aplicação do filtro regional resultou inicialmente em 190 cursos. Para os modelos de regressão e agrupamento, foi estabelecido um número mínimo de cinco respondentes por curso, resultando em 161 cursos efetivamente utilizados nessas análises. Esses cursos totalizam 4.467 alunos com valores válidos de `NT_GER`.

Os dados foram obtidos a partir de diferentes arquivos dos microdados do ENADE e armazenados em tabelas estruturadas para processamento. A regressão utilizou informações provenientes de 13 tabelas de origem; o clustering utilizou dados relacionados a desempenho, infraestrutura, cotas e financiamento; e a análise de associação utilizou os dados do arquivo de questões Likert e a informação de desempenho médio do curso.

Uma característica importante dos microdados utilizados é que os arquivos do ENADE são disponibilizados de forma independente, não permitindo estabelecer uma chave comum entre estudantes de diferentes arquivos. Dessa forma, `CO_CURSO` constitui a chave compartilhada entre as fontes, o que inviabiliza uma vinculação aluno-a-aluno entre os diferentes arquivos. Essa característica determinou o desenho metodológico adotado: regressão e clustering foram estruturados no nível do curso, enquanto a associação utilizou somente variáveis compatíveis dentro do mesmo arquivo, complementadas pela característica de desempenho do curso.

## 3.3 Preparação e tratamento dos dados

A preparação dos dados incluiu filtragem regional, seleção de grupos de cursos, transformação de tipos, tratamento de valores ausentes, agregação por curso, criação de proporções e codificação das variáveis categóricas.

Durante a revisão do backend, foi identificado um erro crítico no tratamento das variáveis `QE_I63` e `QE_I57`, nas quais a categoria 6 correspondia à resposta positiva "Concordo totalmente", enquanto os códigos 7 e 8 representavam as respostas que deveriam ser excluídas. O tratamento originalmente aplicado removia também a categoria 6, provocando perda de aproximadamente 49% das respostas válidas. Após a correção, a média de `QE_I63` por curso passou de 4,02 para 5,00, eliminando o viés introduzido pelo filtro incorreto. fileciteturn7file0L13-L33

Também foram corrigidos problemas relacionados à persistência de valores ausentes, tipos de dados, atualização das fontes e encadeamento dos filtros regionais. Essas correções foram realizadas antes da consolidação dos resultados finais. fileciteturn7file0L105-L159

## 3.4 Construção e avaliação do modelo de regressão

Inicialmente, foram investigadas quatro variáveis originais. A avaliação com validação cruzada de cinco partes apresentou \(R^2=-0,139\), indicando desempenho inferior ao de uma previsão baseada simplesmente na média. Posteriormente, novas variáveis socioeconômicas, acadêmicas e institucionais foram incorporadas ao processo exploratório.

Foram comparados nove algoritmos: Linear Regression, Ridge, Lasso, Elastic Net, Decision Tree, Random Forest, Gradient Boosting, KNN e SVR. O Random Forest apresentou o melhor desempenho entre os modelos avaliados, tendo sido refinado por `GridSearchCV`.

A exploração também avaliou diferentes conjuntos de variáveis, demonstrando que a maior parcela do ganho de desempenho estava associada à escolha das variáveis, e não exclusivamente ao ajuste dos hiperparâmetros. O processo de seleção identificou 15 variáveis como configuração intermediária mais adequada e, posteriormente, o conjunto econômico formado por `QE_I08`, `QE_I09` e `QE_I10` foi condensado por PCA.

O primeiro componente do PCA econômico explicou aproximadamente 71% da variância dessas três variáveis, enquanto dois componentes explicaram 94,7%. Essa configuração apresentou \(R^2\) de validação cruzada entre 0,424 e 0,429 e \(R^2\) de teste entre 0,485 e 0,499.

O modelo final foi configurado como:

- Random Forest com 200 árvores;
- profundidade máxima igual a 5;
- `min_samples_leaf = 2`;
- ponderação por `QT_ALUNOS`;
- 14 variáveis de entrada;
- divisão treino/teste de 80/20.

Foram utilizados 128 cursos para treinamento e 33 para avaliação final. A avaliação foi realizada antes do reajuste do modelo com os 100% dos dados para publicação da versão final. O desempenho no conjunto de teste foi **\(R^2=0,498\), MAE=3,03 e RMSE=3,82**. fileciteturn7file0L453-L485

As 14 variáveis finais foram `PCA_ECON_1`, `PCA_ECON_2`, `QE_I13_A`, `QE_I13_B`, `QE_I13_D`, `QE_I24_A`, `QE_I04_media`, `QE_I15_C`, `QE_I15_E`, `CO_TURNO_GRADUACAO_4`, `QE_I57`, `QE_I19_B`, `QE_I11_A` e `QE_I12_A`.

## 3.5 Construção do modelo de clustering

O agrupamento foi realizado pelo algoritmo K-Means após padronização dos dados com `StandardScaler`. O número de clusters foi fixado previamente em três grupos. Não foi realizada, no desenvolvimento documentado, uma varredura sistemática de diferentes valores de \(k\); portanto, a escolha de três clusters não foi fundamentada na maximização empírica do silhouette.

Para evitar circularidade, `NT_GER` foi removida das variáveis utilizadas na formação dos grupos. A variável foi preservada apenas para caracterização posterior dos clusters e para definição dos rótulos de desempenho.

O conjunto final utilizou 23 variáveis de entrada, formadas pelas proporções das seis categorias de `QE_I63`, seis categorias de `QE_I15` e onze categorias de `QE_I11`.

A qualidade da partição foi avaliada pelo coeficiente de silhouette. O cenário final apresentou valor de **0,211**.

## 3.6 Construção das regras de associação

A análise de associação foi realizada no nível das respostas individuais dos estudantes, utilizando seis itens Likert do arquivo `arq4`: `QE_I57`, `QE_I61`, `QE_I34`, `QE_I44`, `QE_I30` e `QE_I56`.

A esses itens foi adicionada a faixa de desempenho do curso, calculada a partir da média de `NT_GER` e posteriormente dividida em tercis de desempenho baixo, médio e alto. Dessa forma, a informação de desempenho utilizada na associação representa uma característica do curso e não a nota individual do estudante.

Após experimentação de diferentes valores de suporte e confiança, foi adotada a configuração de suporte mínimo de 0,03 e confiança mínima de 0,50. Também foi estabelecido limite de três itens por regra e limite máximo de 50 regras publicadas.

O processo final resultou em 2.417 regras, reduzidas para 377 regras após o filtro de simplicidade e, finalmente, 50 regras publicadas.

A escolha dos parâmetros foi apoiada por uma validação em duas amostras independentes. Na configuração exploratória considerada mais promissora, seis das regras encontradas foram replicadas nas duas metades da base, apresentando lifts entre 1,23 e 1,49. Cinco dessas seis regras envolviam oportunidades de iniciação científica no nível 6, indicando um padrão recorrente, embora de magnitude moderada.

## 3.7 Avaliação do artefato

A avaliação realizada foi de natureza computacional e empírica. Na regressão, foram utilizados validação cruzada, comparação de nove algoritmos, seleção de variáveis sem vazamento para o conjunto de teste e métricas de desempenho. No clustering, foram comparadas diferentes composições de variáveis e utilizado o coeficiente de silhouette. Na associação, foram realizados testes com diferentes limiares de suporte e confiança e validação em amostras independentes.

Não foi realizada avaliação com especialistas, testes formais de usabilidade ou questionários de utilidade percebida. Portanto, a avaliação do artefato neste trabalho é restrita ao comportamento computacional, consistência dos resultados e capacidade de produzir padrões interpretáveis.

---

# 4 RESULTADOS E DISCUSSÕES

## 4.1 Qualidade dos dados e refinamento do artefato

A revisão completa do backend teve impacto direto nos resultados analíticos. O caso mais significativo ocorreu no tratamento das questões de escala Likert, em que a exclusão indevida da categoria 6 eliminava aproximadamente metade das respostas válidas. A correção do filtro alterou substancialmente as estatísticas de infraestrutura, reduzindo o risco de conclusões baseadas em dados incorretamente filtrados. fileciteturn7file0L13-L33

Outro ponto importante foi a separação entre avaliação e publicação do modelo de regressão. Na implementação final, o desempenho passou a ser mensurado em um conjunto de teste não utilizado no treinamento, antes do reajuste do modelo com todos os dados para sua versão final. Essa estratégia evita apresentar previsões de treinamento como se fossem medidas de generalização. fileciteturn7file0L35-L43

As revisões também evidenciaram que alterações aparentemente secundárias na implementação poderiam modificar o comportamento dos modelos. No caso das associações, por exemplo, a codificação incorreta das variáveis categóricas produziu inicialmente valores artificialmente próximos de 1,0 para confiança e lift. A correção da transformação one-hot foi necessária antes da obtenção dos resultados finais.

Esses achados reforçam que a etapa de preparação e validação do pipeline não constitui apenas uma atividade de implementação, mas parte essencial da validade analítica do artefato.

## 4.2 Resultados da regressão

O processo de exploração mostrou que as quatro variáveis originalmente utilizadas eram insuficientes para explicar a variação da nota dos cursos. O \(R^2\) de -0,139 obtido em validação cruzada indicou que o modelo apresentava desempenho inferior a uma previsão baseada na média.

A inclusão de variáveis socioeconômicas e institucionais produziu um ganho substancial. Na decomposição realizada durante a exploração, a mudança do hiperparâmetro teve efeito reduzido, enquanto a ampliação e seleção das variáveis respondeu pela maior parcela do ganho de desempenho. O cenário com variáveis combinadas apresentou \(R^2\) de validação cruzada superior aos cenários compostos apenas pelas variáveis originais. fileciteturn7file0L189-L221

A comparação entre treinamento regional e nacional também apresentou um resultado relevante. O treinamento restrito a Norte e Nordeste obteve \(R^2=0,482\) no teste regional, enquanto o treinamento com cursos de todo o país obteve \(R^2=0,462\) no mesmo conjunto de teste. Esse resultado sugere que a utilização de uma base maior não necessariamente produz maior capacidade de generalização quando as relações entre características e desempenho diferem entre regiões.

A utilização da PCA sobre as três variáveis econômicas apresentou o melhor resultado entre os cenários investigados. Com dois componentes, o modelo alcançou \(R^2=0,424\) em validação cruzada e \(R^2=0,499\) no teste, utilizando uma variável a menos que a configuração de produção anterior. fileciteturn7file0L361-L407

O modelo final apresentou \(R^2=0,498\), indicando que aproximadamente 49,8% da variabilidade observada na nota média dos cursos do conjunto de teste foi explicada pelo modelo. O MAE de 3,03 indica erro absoluto médio de aproximadamente três pontos na escala da nota, enquanto o RMSE de 3,82 penaliza mais fortemente erros maiores.

A análise das importâncias das variáveis mostrou predominância do componente socioeconômico `PCA_ECON_1`, com importância média de 1,735. Em seguida apareceram `QE_I13_B` (1,514), `QE_I13_A` (1,072), `QE_I24_A` (0,503) e `QE_I04_media` (0,449). Também apresentaram participação relevante variáveis relacionadas a cotas, turno, percepção sobre domínio dos professores, incentivo acadêmico e financiamento.

Esses resultados sugerem que o desempenho dos cursos não está associado a uma única dimensão. A presença simultânea de variáveis socioeconômicas, acadêmicas e institucionais reforça a hipótese de que abordagens multidimensionais são mais adequadas para explorar esse tipo de fenômeno.

Entretanto, a importância preditiva de uma variável não deve ser interpretada como evidência de causalidade. Esse cuidado é especialmente relevante para `QE_I13_B`, relacionada à bolsa de iniciação científica. O próprio processo exploratório identificou que estudantes com bom desempenho podem ter maior probabilidade de receber esse tipo de bolsa, fazendo com que a variável possa funcionar como marcador de desempenho prévio, e não necessariamente como causa direta da nota. fileciteturn7file0L255-L289

## 4.3 Resultados do clustering

A revisão do modelo de agrupamento revelou inicialmente um problema de circularidade. Na configuração anterior, `NT_GER` participava da formação dos clusters e depois era usada para nomeá-los, produzindo uma separação de desempenho parcialmente induzida pela própria variável.

A retirada de `NT_GER` solucionou o problema e ainda produziu ligeiro aumento do silhouette, de 0,133 para 0,142 no cenário comparativo. A análise posterior mostrou que os grupos formados sem utilizar diretamente a nota apresentavam perfis diferenciados em relação a infraestrutura, cotas e financiamento.

A incorporação de `QE_I11`, relacionada a bolsas ou financiamento do curso, produziu novo ganho de qualidade, elevando o silhouette para **0,211**. A renda, isoladamente, não melhorou a estrutura dos grupos.

O modelo final produziu três perfis:

| Cluster | Cursos | Alunos | NT_GER médio |
|---|---:|---:|---:|
| Alto | 73 | 2.461 | 39,80 |
| Médio | 16 | 248 | 37,57 |
| Baixo | 72 | 1.758 | 34,75 |

Os clusters apresentaram forte diferenciação na variável `QE_I11_A`. No grupo de maior desempenho, aproximadamente 97,1% das respostas estavam associadas à categoria de ausência de bolsa em curso gratuito. No grupo de menor desempenho, essa proporção foi de apenas 0,5%, enquanto a categoria `QE_I11_B`, relacionada a curso pago sem bolsa, alcançou aproximadamente 31,3%.

Outro resultado relevante é que a melhor avaliação de infraestrutura não correspondeu ao maior desempenho. A proporção de `QE_I63_6`, correspondente a "Concordo totalmente", foi de 38,9% no grupo de maior desempenho, 61,8% no grupo médio e 58,9% no grupo de menor desempenho. Portanto, a relação entre percepção de infraestrutura e desempenho não apresentou comportamento monotônico.

Esse resultado constitui um exemplo importante do valor da análise multidimensional. Uma análise isolada de infraestrutura poderia sugerir uma relação simples entre melhor percepção e melhor desempenho. O agrupamento mostrou, entretanto, que os perfis encontrados combinam dimensões institucionais e acadêmicas de maneira mais complexa.

Ao mesmo tempo, o silhouette de 0,211 indica que a estrutura de agrupamento é limitada. Portanto, os três grupos devem ser interpretados como perfis exploratórios e não como categorias naturais ou rigidamente separadas da população.

Além disso, o número de clusters foi definido a priori e não foi submetido a uma busca sistemática de diferentes valores de \(k\). Essa limitação deve ser considerada ao interpretar os resultados.

## 4.4 Resultados da análise de associação

A configuração original da análise de associação utilizava somente `QE_I30`, `QE_I56` e `QE_I57`, todos relacionados a percepções sobre professores e metodologia. O resultado produzia poucas regras, mas predominantemente confirmava um efeito-halo: estudantes que avaliavam positivamente um aspecto tendiam a avaliar positivamente os demais.

A tentativa de ampliar os itens para diferentes temas aumentou consideravelmente o número de regras, mas não eliminou o caráter essencialmente correlacional das relações observadas. Quando a faixa de desempenho foi incorporada, nenhuma regra relacionada ao desempenho sobreviveu aos limiares originais de confiança 0,7.

A exploração sistemática de diferentes valores mostrou que confiança de pelo menos 0,6 era excessivamente restritiva para a descoberta de relações envolvendo desempenho. O melhor equilíbrio encontrado foi suporte 0,03 e confiança 0,5.

A validação em duas amostras independentes mostrou que parte das regras encontradas em uma única amostra não se repetia na segunda. Das 13 regras identificadas na primeira metade e 9 na segunda, apenas seis foram replicadas. Essa diferença demonstra que o espaço de regras de associação é sensível à amostra e exige cautela na interpretação.

No estado final, o pipeline gerou 2.417 regras, das quais 377 atendiam ao limite de simplicidade e 50 foram publicadas. Apenas quatro das regras publicadas envolviam a faixa de desempenho:

1. `desempenho_baixo` + `metodologia_inovadora_5` → `formacao_critica_5`, suporte de 3,95%, confiança de 54,44% e lift de 2,127;

2. `desempenho_baixo` + `oportunidades_ic_5` → `profs_disponibilidade_5`, suporte de 3,63%, confiança de 52,32% e lift de 1,997;

3. `desempenho_baixo` + `profs_disponibilidade_5` → `professores_dominio_5`, suporte de 4,44%, confiança de 57,98% e lift de 1,957;

4. `desempenho_baixo` + `professores_dominio_5` → `profs_disponibilidade_5`, suporte de 4,44%, confiança de 50,74% e lift de 1,937.

Essas regras precisam ser interpretadas cuidadosamente. O desempenho baixo aparece no **antecedente**, e não como consequente. Portanto, elas não indicam que determinada percepção produz baixo desempenho nem funcionam como modelos de previsão. Elas indicam que, **dentro do conjunto de cursos classificados como de baixo desempenho**, determinadas percepções aparecem conjuntamente com frequência superior à esperada.

Esse resultado é diferente das regras encontradas na exploração independente, nas quais combinações envolvendo oportunidades de iniciação científica e percepções sobre professores apresentaram lifts entre 1,23 e 1,49 em amostras independentes. Essas relações exploratórias foram úteis para orientar a investigação, mas não correspondem diretamente às quatro regras publicadas no modelo final.

Assim, a principal contribuição da associação não está em estabelecer uma relação causal ou em prever desempenho, mas em revelar padrões de coocorrência que podem orientar investigações posteriores.

## 4.5 Integração das três perspectivas analíticas

A análise conjunta demonstra que os três modelos produziram resultados complementares.

A regressão identificou variáveis associadas à variação da nota média dos cursos e apresentou capacidade preditiva moderada, com destaque para a dimensão socioeconômica sintetizada pelo `PCA_ECON_1`.

O clustering, por sua vez, mostrou que cursos podem ser organizados em perfis distintos principalmente em função de características institucionais relacionadas a financiamento, cotas e infraestrutura. O fato de o grupo de maior desempenho apresentar menor proporção de avaliações máximas de infraestrutura demonstra que relações observadas em uma única variável podem desaparecer ou assumir outra interpretação quando diferentes dimensões são analisadas simultaneamente.

A associação acrescentou uma terceira perspectiva ao revelar padrões de coocorrência entre percepções acadêmicas, especialmente no contexto de cursos de baixo desempenho. Mesmo sem estabelecer causalidade, essas relações podem servir como hipóteses para estudos posteriores.

Dessa forma, os resultados sustentam a premissa central do MultiENADE: diferentes técnicas não necessariamente encontram o mesmo padrão, mas podem produzir visões complementares de um mesmo fenômeno. A integração dessas perspectivas permite explorar simultaneamente capacidade preditiva, formação de perfis e relações de coocorrência.

---

# 5 CONSIDERAÇÕES FINAIS

Este trabalho apresentou o desenvolvimento e a avaliação do MultiENADE, um artefato computacional destinado à exploração multidimensional de microdados do ENADE. O estudo partiu do problema da fragmentação das análises e buscou integrar diferentes técnicas de mineração em um processo único de preparação, modelagem e interpretação dos dados.

A primeira contribuição do trabalho foi estrutural. A revisão completa do backend mostrou que a qualidade dos resultados depende diretamente da correção das etapas de preparação dos dados. A identificação e correção de filtros inadequados, problemas de codificação, falhas na persistência e diferenças entre as populações de treinamento e previsão foram necessárias para consolidar uma versão analiticamente confiável do artefato.

No modelo supervisionado, a comparação de diferentes algoritmos e conjuntos de variáveis demonstrou que o ganho de desempenho esteve principalmente associado à escolha das características utilizadas. O modelo Random Forest final, composto por 14 variáveis e incluindo dois componentes do PCA econômico, apresentou \(R^2=0,498\), MAE=3,03 e RMSE=3,82 no conjunto de teste. O resultado indica capacidade moderada de explicar a variação da nota média dos cursos, com predominância de variáveis relacionadas à dimensão socioeconômica.

No agrupamento, a retirada de `NT_GER` das variáveis de formação dos clusters eliminou a circularidade presente na versão inicial. A incorporação de variáveis relacionadas a bolsas ou financiamento produziu uma partição com silhouette de 0,211 e revelou diferenças predominantemente institucionais entre os grupos. Entretanto, como o número de clusters foi fixado previamente, os resultados devem ser interpretados como perfis exploratórios e não como uma estrutura definitiva da população.

Na análise de associação, o refinamento dos limiares de suporte e confiança e a validação em amostras independentes demonstraram a sensibilidade desse tipo de mineração à composição da amostra. As regras finais não devem ser interpretadas como relações causais nem como modelos preditivos de desempenho. Seu principal papel é identificar padrões de coocorrência que possam orientar novas hipóteses de investigação.

Considerando a perspectiva de Design Science Research, o trabalho produziu e avaliou empiricamente um artefato capaz de combinar preparação de dados, regressão, agrupamento, análise de associação e mecanismos de interpretação. A avaliação realizada, entretanto, foi exclusivamente computacional e empírica. Não foram realizados testes com especialistas, estudos formais de usabilidade ou avaliações externas da utilidade dos relatórios produzidos.

Entre as principais limitações estão o recorte temporal restrito ao ENADE 2021, a concentração em dois grupos de cursos e duas regiões brasileiras, a pequena quantidade de cursos no conjunto de teste da regressão, a ausência de uma busca sistemática do número ótimo de clusters, a ausência de teste formal de significância das regras de associação e as próprias características de anonimização dos microdados do ENADE, que impedem o vínculo individual entre diferentes arquivos.

Como trabalhos futuros, recomenda-se ampliar o estudo para múltiplos anos do ENADE, incorporar procedimentos de harmonização longitudinal, avaliar sistematicamente diferentes valores de \(k\), investigar a estabilidade dos clusters, realizar avaliações com especialistas e explorar modelos temporais e técnicas capazes de investigar relações mais próximas de mecanismos causais. Também é pertinente integrar os resultados do MultiENADE aos demais componentes do EnadeX, especialmente às camadas de explicabilidade, Visual Analytics e análise histórica.

Em síntese, os resultados obtidos indicam que a combinação de técnicas supervisionadas, não supervisionadas e de mineração de relações oferece uma perspectiva mais abrangente para exploração dos microdados educacionais do que a aplicação isolada de uma única técnica. O MultiENADE, portanto, demonstra viabilidade como artefato de apoio à descoberta de padrões multidimensionais no contexto do ENADE, mantendo como princípio central a distinção entre associação, capacidade preditiva e causalidade.

---

# REFERÊNCIAS

AGRAWAL, Rakesh; SRIKANT, Ramakrishnan. Fast algorithms for mining association rules. In: **Proceedings of the 20th International Conference on Very Large Data Bases (VLDB)**. Santiago de Chile: Morgan Kaufmann, 1994.

ARAÚJO, Rodrigo Alexandrino. **Análise dos microdados do Enade: proposta de uma ferramenta de exploração utilizando mineração de dados**. 2020. 69 f. Dissertação (Mestrado em Ciência da Computação) — Universidade Federal de Goiás, Goiânia, 2020.

BAKER, Ryan Shaun Joazeiro de; ISOTANI, Seiji; CARVALHO, Adriana Maria Joazeiro Baker de. Mineração de Dados Educacionais: oportunidades para o Brasil. **Revista Brasileira de Informática na Educação**, v. 19, n. 02, p. 03-13, 2011.

BREIMAN, Leo. Random forests. **Machine Learning**, v. 45, p. 5-32, 2001.

CRISPIM NETO, Wilton Ribeiro. **O uso de mineração de dados educacionais sob o ENADE como apoio ao processo de tomada de decisão de gestores do ensino superior**. 2020. Trabalho de Conclusão de Curso — Universidade Federal do Ceará, Fortaleza, 2020.

GUILHEN, Bruno Anselmo. **Avaliação de algoritmos para a análise preditiva das notas de Engenharia no ENADE utilizando dados socioeconômicos**. 2024. Dissertação (Mestrado) — Universidade de São Paulo, São Paulo, 2024.

HEVNER, Alan R. et al. Design science in information systems research. **MIS Quarterly**, v. 28, n. 1, p. 75-105, 2004.

JOLLIFFE, Ian T.; CADIMA, Jorge. Principal component analysis: a review and recent developments. **Philosophical Transactions of the Royal Society A**, v. 374, 2016.

LIMA, Priscila da Silva Neves et al. Análise de dados do Enade e Enem: uma revisão sistemática da literatura. **Avaliação: Revista da Avaliação da Educação Superior**, v. 24, n. 1, p. 89-107, 2019.

LUNDBERG, Scott M.; LEE, Su-In. A unified approach to interpreting model predictions. In: **Advances in Neural Information Processing Systems**, 30., 2017.

PEFFERS, Ken et al. A design science research methodology for information systems research. **Journal of Management Information Systems**, v. 24, n. 3, p. 45-77, 2007.
