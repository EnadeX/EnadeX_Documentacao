Métricas:
Labels:
# $R^2$ (R-Quadrado ou Coeficiente de Determinação)

**O que é:** É como se fosse a "nota" de ajuste do modelo. Ele indica a proporção da variação dos seus dados que é explicada pela regressão.


**Como ler:** O valor vai de 0 a 1 (ou 0% a 100%). Quanto mais próximo de 1, melhor! Se o $R^2$ for 0.85, significa que 85% do comportamento da sua variável de interesse foi capturado com sucesso pelo modelo.

Determina o quão próximo o dados está da linha base, ela nunca desce ao acrescentar novas variáveis.
### MAE (Mean Absolute Error / Erro Absoluto Médio)

**O que é:** É a média simples de todos os erros que o modelo cometeu. Ele mede a distância entre o que o modelo previu e o valor real, ignorando se o erro foi para cima ou para baixo (por isso o termo "absoluto"). **Como ler:** Quanto menor, melhor. A grande vantagem é que ele está na exata mesma unidade dos seus dados. Se o senhor está prevendo preços em reais, o MAE será o erro médio em reais, tornando a leitura muito natural e fácil de explicar para qualquer pessoa.

### RMSE (Root Mean Squared Error / Raiz do Erro Quadrático Médio)

_(Acredito que seja este o "REM" que o senhor mencionou!)_ **O que é:** Também mede a média dos erros, mas eleva cada diferença ao quadrado antes de tirar a média, aplicando uma raiz quadrada no final para voltar à unidade original dos dados. **Como ler:** Quanto menor, melhor. O detalhe valioso aqui é que, por elevar ao quadrado no meio do processo, o RMSE **pune erros grandes de forma muito mais severa**. Se o modelo der alguns "chutes" muito fora da curva, o RMSE vai disparar rapidamente, enquanto o MAE pode continuar comportado.


### Achatamento dos dados

Essa agregação a nível de curso explica absolutamente tudo sobre os nossos resultados matemáticos. Na estatística, chamamos isso de **efeito de agregação**.

### O que acontece quando protegemos os dados

Ao agrupar os indivíduos para garantir o anonimato, nós "esmagamos" os extremos matemáticos:

- O aluno excepcional que tirou 95 e o que teve um dia ruim e tirou 25 acabam se fundindo em uma média de curso igual a 60.
    
- As respostas categóricas (como horas de estudo de 1 a 5) viram médias "quebradas" (ex: o curso tem média 3.4), diluindo a força daquele aluno que marcou a dedicação máxima.
    

Como o seu modelo só tem permissão para enxergar o comportamento da "multidão" e não do indivíduo, a variância original desaparece. É exatamente por isso que as predições ficam conservadoras: o modelo crava um erro (MAE) de apenas 4 pontos porque quase todas as médias de curso orbitam na mesma faixa, mas o $R^2$ fica em 0.20 porque a individualidade — que é o verdadeiro motor das grandes variações de nota — foi blindada pela média.

Sem pesos
=== RESULTADOS DA AVALIAÇÃO DO MODELO ===
R² (Poder explicativo da variância): 0.2030
MAE (Erro absoluto médio em notas): 4.7249
RMSE (Erro médio penalizando grandes desvios): 5.7471

Com pesos
=== RESULTADOS DA AVALIAÇÃO DO MODELO ===
R² (Poder explicativo da variância): 0.2157
MAE (Erro absoluto médio em notas): 4.7259
RMSE (Erro médio penalizando grandes desvios): 5.7008