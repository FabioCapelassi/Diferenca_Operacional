# Análise dos Fatores Associados à Diferença Operacional em Plantas de Processamento

#### Aluno: Fábio Capelassi Gavazzi de Marco
#### Orientadora: Manoela Kohler

\---

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

\---

### Resumo

O código implementa um fluxo de processamento que utiliza métodos estatísticos e técnicas de aprendizado de máquina para identificar quais variáveis operacionais estão mais associadas à variável Diferença Operacional em plantas de processamento de gás natural.
A análise é realizada a partir de dados diários do balanço energético das plantas, incluindo os fluxos volumétricos de entrada de gás natural, as correntes de saída dos produtos (gás natural processado, GLP, LGN, C5+ e condensado), o consumo interno de gás combustível, a queima em flare e a composição do gás na entrada da unidade.
A Diferença Operacional é definida como o resultado do balanço energético calculado pela soma das correntes de entrada menos a soma das correntes de saída, considerando também o consumo de gás combustível e a queima. Em condições ideais, o resultado desse balanço deveria ser igual a zero. Entretanto, devido a fatores como incertezas e erros de medição, variações na qualidade dos dados e outras limitações operacionais, a Diferença Operacional apresenta oscilações ao longo do tempo.
O objetivo do código é identificar e priorizar as variáveis com maior potencial de explicar as variações observadas na Diferença Operacional em um determinado período. Para isso, são aplicados diferentes métodos analíticos capazes de medir a relevância de cada variável em relação ao indicador estudado. Os resultados gerados fornecem subsídios para direcionar as investigações da equipe de operação e manutenção, auxiliando na identificação das possíveis causas dos desvios observados e na definição de ações corretivas para melhoria do fechamento do balanço energético da planta.   

### Abstract

The code implements a processing workflow that uses statistical methods and machine learning techniques to identify which operational variables are most strongly associated with the Operational Difference variable in natural gas processing plants.
The analysis is based on daily plant energy balance data, including natural gas feed volumetric flow rates, product outlet streams (processed natural gas, LPG, NGL, C5+, and condensate), internal fuel gas consumption, flare gas combustion, and the composition of the gas entering the facility.
The Operational Difference is defined as the result of the energy balance calculated by subtracting the sum of all outlet streams from the sum of all inlet streams, while also accounting for fuel gas consumption and flaring. Under ideal conditions, the result of this balance should be equal to zero. However, due to factors such as measurement uncertainties and errors, variations in data quality, and other operational limitations, the Operational Difference exhibits fluctuations over time.
The objective of the code is to identify and rank the variables with the greatest potential to explain the variations observed in the Operational Difference over a given period. To achieve this, different analytical methods are applied to assess the relevance of each variable with respect to the target indicator. The resulting outputs provide valuable insights to guide operations and maintenance teams in their investigations, helping them identify potential causes of the observed deviations and define corrective actions to improve the plant's energy balance closure.

### 1\. Introdução

O fechamento adequado do balanço energético é um dos principais indicadores de desempenho operacional das Unidades de Tratamento de Gás Natural (UTGNs). Em condições ideais, a soma da energia associada às correntes de entrada deve ser equivalente à soma da energia das correntes de saída, acrescida dos consumos internos e perdas operacionais da unidade. Entretanto, devido às incertezas de medição, falhas instrumentais, variações operacionais e inconsistências nos dados, é comum observar diferenças entre os valores calculados de entrada e saída, fenômeno aqui denominado Diferença Operacional.
A identificação das variáveis responsáveis por essas diferenças representa um desafio relevante para as equipes de operação, manutenção, automação e medição. A análise convencional baseada apenas em avaliações visuais ou investigações pontuais muitas vezes não é suficiente para identificar os fatores que mais influenciam os desvios observados.
Com o objetivo de tornar esse processo mais sistemático e orientado por dados, foi desenvolvido um modelo analítico capaz de avaliar simultaneamente um grande conjunto de variáveis operacionais, incluindo vazões de entrada e saída, consumos internos, composição do gás natural e propriedades energéticas dos produtos. O modelo utiliza técnicas estatísticas e algoritmos de aprendizado de máquina para identificar, hierarquizar e monitorar os principais direcionadores da Diferença Operacional ao longo do tempo.
Os resultados obtidos permitem direcionar esforços investigativos para as variáveis com maior potencial de influência sobre o balanço energético da planta, contribuindo para a melhoria contínua da qualidade dos dados e da confiabilidade operacional.

### 2\. Modelagem

A metodologia foi estruturada para identificar, de forma robusta, quais variáveis operacionais apresentam maior associação com a Diferença Operacional (Dif_Ope). Para aumentar a confiabilidade dos resultados, a análise combina métodos estatísticos clássicos, modelos de regressão regularizada e algoritmos de aprendizado de máquina, avaliando os dados sob diferentes perspectivas matemáticas.
A seguir são apresentadas a principais etapas do fluxo de análise: 

#### 2.1\. Preparação dos Dados

A base de dados é construída a partir das informações diárias do balanço energético da unidade, contemplando: Vazões energéticas de entrada de gás natural por ponto de recebimento; Correntes energéticas de saída dos produtos; Consumo de gás combustível; Queima em flare; Composição cromatográfica do gás de entrada; Poder calorífico superior (PCS) dos produtos.
Os dados são consolidados em uma única base diária, na qual cada linha representa um dia operacional e cada coluna representa uma variável potencialmente relacionada à Diferença Operacional.
A variável resposta é calculada conforme : Dif_Ope = Entradas − Saídas − Consumos − Queima.
Em condições ideais, a Diferença Operacional deveria permanecer próxima de zero. Desvios positivos ou negativos indicam possíveis inconsistências de medição, problemas operacionais ou limitações inerentes ao processo.

#### 2.2\. Avaliação Mensal

A primeira abordagem adotada consiste na realização de análises independentes para cada mês do período avaliado. Nessa estratégia, cada mês é tratado como uma amostra operacional independente, permitindo identificar quais variáveis apresentaram maior associação com a Diferença Operacional naquele intervalo específico.
A análise mensal tem como principal objetivo identificar alterações estruturais nos fatores que influenciam o fechamento energético ao longo do tempo.
Para cada mês são aplicados quatro métodos independentes (correlação, Regressão Ridge, Regressão Elastic Net e Randon Forest).

##### 2.2.1\. Correlação

A análise de correlação busca medir o grau de associação entre cada variável operacional e a Diferença Operacional. O modelo permite a utilização de diferentes coeficientes de correlação, de acordo com o objetivo da análise: Pearson (avalia relações lineares entre as variáveis, sendo mais indicado quando os dados apresentam comportamento aproximadamente linear e distribuição próxima da normalidade); Spearman (avalia relações monotônicas a partir do ordenamento dos dados, apresentando maior robustez frente a distribuições não normais, presença de outliers e relações não estritamente lineares) e Kendall (mede a concordância entre os rankings das observações, sendo particularmente útil em conjuntos de dados com amostras reduzidas ou elevada presença de empates).
Neste trabalho, foi adotado o coeficiente de Spearman, devido à sua maior robustez para dados operacionais, que frequentemente apresentam distribuições assimétricas, valores extremos e comportamentos não lineares.
Para cada mês: Calcula-se a correlação entre cada variável explicativa e a Diferença Operacional; As variáveis são classificadas pelo valor absoluto da correlação; São selecionadas as variáveis com maior magnitude de associação.
A interpretação dos resultados é direta, uma correlação positiva indica que o aumento da variável tende a aumentar a Diferença Operacional. Por outro lado, uma correlação negativa indica que o aumento da variável tende a reduzir a Diferença Operacional e a correlação próxima de zero indica baixa associação estatística.

##### 2.2.2\. Regressão Ridge

A Regressão Ridge é empregada para tratar a multicolinearidade, no problema em questão diversas variáveis do balanço energético possuem forte correlação entre si. Nessa situação, regressões convencionais podem gerar coeficientes instáveis e difíceis de interpretar.
A regressão Ridge adiciona um termo de penalização aos coeficientes do modelo, reduzindo a variância das estimativas e tornando os resultados mais robustos.
Para cada mês: As variáveis são normalizadas utilizando StandardScaler; O fator de regularização (alfa) é selecionado automaticamente por validação cruzada; O modelo é ajustado para explicar a Diferença Operacional; Os coeficientes obtidos são utilizados como medida de relevância.
O sinal do coeficiente indica a direção do efeito da variável sobre a Diferença Operacional de forma similar ao conceito empregado na análise por correlação. 
Adicionalmente são calculadas métricas de desempenho do modelo para avaliar o quanto as variáveis operacionais explicam a variabilidade observada no balanço energético.

##### 2.2.3\. Elastic Net

O Elastic Net combina as vantagens das regressões Ridge e Lasso. Além de reduzir os efeitos da multicolinearidade, o método realiza seleção automática de variáveis, eliminando fatores com baixa contribuição explicativa.
A modelagem ocorre em três etapas: Remoção de variáveis altamente redundantes; Normalização das variáveis através de RobustScaler; Ajuste do modelo Elastic Net utilizando validação cruzada.
Os parâmetros de regularização são obtidos automaticamente: Alfa (intensidade da penalização); L1 Ratio (equilíbrio entre Ridge e Lasso).
A principal vantagem dessa abordagem é produzir modelos mais compactos e interpretáveis, destacando apenas as variáveis com maior relevância estatística.
O desempenho é avaliado através do coeficiente de determinação obtido para cada mês.

##### 2.2.4\. Random Forest

O algoritmo Random Forest é utilizado para capturar relacionamentos não lineares que normalmente não são identificados pelos modelos de regressão tradicionais. O método é baseado na construção de múltiplas árvores de decisão e apresenta elevada capacidade de modelar interações complexas entre variáveis.
A modelagem ocorre em dois estágios: Etapa 1 – Seleção preliminar (Construção de uma floresta inicial e Seleção das variáveis mais relevantes) e Etapa 2 – Modelo final (Construção de uma nova floresta apenas com as variáveis selecionadas e importância das variáveis é calculada utilizando a técnica de Permutation Importance).
Na técnica de Permutation Importance, o modelo é treinado normalmente e uma variável é embaralhada aleatoriamente e o desempenho do modelo é avaliado, quanto maior a perda observada, maior a importância da variável. Como essa técnica não possui sinal, este é estimado a partir da correlação da variável com a Diferença Operacional.

##### 2.2.5\. Consenso entre Métodos

Cada método possui vantagens e limitações específicas e por essa razão, os resultados não são avaliados isoladamente.
Após a execução dos quatro modelos é calculado um índice de consenso que quantifica quantas vezes uma mesma variável aparece entre os principais direcionadores identificados pelos diferentes métodos.
Uma variável é considerada consensual quando é selecionada simultaneamente por pelo menos três dos quatro métodos utilizados.
Essa estratégia reduz o risco de falsos positivos e aumenta a confiabilidade das conclusões.
Além disso, é elaborado um ranking consolidado contendo: Frequência de ocorrência da variável; Sinal predominante; Score acumulado de consenso; Número de meses em que a variável foi identificada.

#### 2.3\. Avaliação por Janela Móvel

Enquanto a análise mensal busca identificar tendências estruturais, a análise por janela móvel tem como objetivo identificar eventos operacionais transitórios e mudanças dinâmicas de comportamento.Nessa abordagem, uma janela temporal deslizante é aplicada sobre a série histórica.
Para reduzir o ruído observado nos dados, foram utilizadas múltiplas janelas de observação, por exemplo, 15 dias e 120 dias. Cada dia da série passa a possuir uma análise própria, construída com base nos dados ao seu redor.
Para minimizar distorções próximas às extremidades da série, foi utilizada a técnica de espelhamento dos dados nas bordas.
Para cada janela são aplicados três métodos independentes (correlação, Regressão Ridge e Randon Forest).

##### 2.3.1\. Correlação em Janela Móvel

Para cada dia: Seleciona-se a janela de análise; Calcula-se a correlação entre todas as variáveis e a Diferença Operacional; São identificadas as variáveis com maior magnitude de correlação.
O procedimento é repetido para toda a série histórica e o resultado é um mapa temporal que evidencia quando determinadas variáveis passaram a apresentar maior associação com os desvios do balanço.

##### 2.3.2\. Regressão Ridge em Janela Móvel

O mesmo conceito é aplicado à regressão Ridge, para cada dia: É criada uma janela temporal centrada na data de interesse; As variáveis são normalizadas; O modelo Ridge é ajustado; São armazenados os coeficientes obtidos. A evolução temporal dos coeficientes permite identificar mudanças graduais no comportamento da planta.

##### 2.3.3\. Random Forest em Janela Móvel

A mesma metodologia é utilizada com o algoritmo Random Forest, para cada janela: É realizado um processo de seleção de variáveis; O modelo Random Forest é ajustado; É calculada a importância por permutação; É atribuído um sinal à importância obtida.
Os resultados permitem identificar eventos que possuam comportamento não linear e que poderiam não ser detectados pelos métodos estatísticos tradicionais.

##### 2.3.4\. Consenso Diário e Consolidação Mensal

Após a execução dos três métodos em todas as janelas temporais, é realizado um processo de consenso em duas etapas: Etapa 1 – Consenso entre janelas (Uma variável recebe maior relevância quando aparece simultaneamente em diferentes tamanhos de janela e mantendo o mesmo sinal) e Etapa 2 – Consenso entre métodos (São comparados os resultados da: Correlação, Regressão Ridge e Random Forest)
As variáveis identificadas simultaneamente pelos métodos recebem maior pontuação e são classificadas como os principais direcionadores do dia.
Posteriormente, os resultados diários são consolidados mensalmente, permitindo identificar quais variáveis aparecem de forma recorrente durante cada período operacional.

#### 2.4\. Clusterização dos Resultados

Como etapa complementar, os resultados gerados pelos diferentes métodos são utilizados como entrada para algoritmos de clusterização. O objetivo é agrupar períodos que apresentem padrões semelhantes de direcionadores da Diferença Operacional. Antes da clusterização, é aplicada Análise de Componentes Principais (PCA), responsável por reduzir a dimensionalidade do problema preservando aproximadamente 90% da variabilidade dos dados.
O modelo permite a utilização de diferentes algoritmos de aprendizado não supervisionado, possibilitando a comparação de abordagens distintas para identificação dos agrupamentos: K-Means (particiona os dados em grupos definidos previamente, buscando maximizar a similaridade interna dos elementos de cada cluster); Agglomerative Clustering (método hierárquico que constrói os agrupamentos de forma progressiva a partir da proximidade entre as observações); Gaussian Mixture Model (abordagem probabilística que estima a probabilidade de cada observação pertencer a cada grupo identificado); Spectral Clustering (técnica baseada em grafos capaz de identificar agrupamentos com fronteiras complexas e não lineares); HDBSCAN (método baseado em densidade que identifica agrupamentos de tamanhos variados e permite classificar observações atípicas como ruído); DBSCAN (algoritmo de agrupamento por densidade adequado para identificação de padrões não lineares e detecção de anomalias).
Após a formação dos clusters, são calculadas as variáveis que mais diferenciam cada grupo em relação aos demais períodos analisados. Essa avaliação permite identificar quais direcionadores foram mais relevantes para caracterizar cada agrupamento.
Os agrupamentos são então gerados utilizando algoritmos de aprendizado não supervisionado, permitindo identificar: Períodos com comportamento operacional semelhante; Mudanças de regime operacional; Eventos recorrentes associados aos mesmos direcionadores; Padrões históricos de ocorrência da Diferença Operacional.
Dessa forma, a clusterização fornece uma visão complementar às análises individuais de correlação, regressão e aprendizado de máquina, permitindo compreender não apenas quais variáveis estão associadas à Diferença Operacional, mas também em quais contextos operacionais essas associações tendem a ocorrer e se repetem ao longo da série histórica.

### 3\. Resultados

A metodologia produz diferentes camadas de resultados destinadas a apoiar a investigação operacional.
Inicialmente são gerados mapas de calor contendo a evolução temporal das correlações e coeficientes obtidos pelos diferentes modelos. Esses gráficos permitem visualizar alterações no comportamento das variáveis ao longo dos meses e identificar períodos de maior estabilidade ou mudanças significativas no processo.
Para cada mês analisado são produzidos rankings contendo as variáveis com maior influência sobre a Diferença Operacional, separados por método analítico. Esses rankings indicam não apenas a relevância da variável, mas também o sentido de sua influência, evidenciando se o aumento da variável tende a aumentar ou reduzir o desvio observado na Diferença Operacional.
A aplicação simultânea dos métodos de Correlação, Ridge, Elastic Net e Random Forest permite comparar os resultados obtidos por diferentes abordagens matemáticas, aumentando a robustez da análise.
Como etapa complementar, é calculado um Índice de Consenso entre os métodos. Variáveis identificadas simultaneamente por múltiplos algoritmos recebem maior pontuação, resultando em uma priorização consolidada dos possíveis direcionadores da Diferença Operacional.
Na análise diária, realizada através de janelas móveis, são identificados os principais direcionadores associados a períodos específicos de desvio. Essa abordagem possibilita investigar eventos operacionais transitórios que poderiam não ser perceptíveis na análise agregada mensal.
Adicionalmente, técnicas de agrupamento (clustering) são utilizadas para classificar meses e períodos com características semelhantes, permitindo identificar padrões recorrentes de comportamento da planta e grupos de eventos associados aos mesmos direcionadores operacionais.

### 4\. Conclusões

A metodologia desenvolvida demonstrou ser capaz de transformar grandes volumes de dados operacionais em informações úteis para a investigação das causas associadas às diferenças observadas no balanço energético das unidades de processamento de gás natural.
A utilização conjunta de métodos estatísticos e algoritmos de aprendizado de máquina reduz a dependência de análises exclusivamente subjetivas, proporcionando uma abordagem mais objetiva e sistemática para identificação dos principais fatores que influenciam a Diferença Operacional.
A estratégia de consenso entre múltiplos modelos aumenta a confiabilidade dos resultados ao destacar variáveis que apresentam relevância consistente sob diferentes perspectivas analíticas, reduzindo o risco de interpretações decorrentes das limitações de um único método.
A combinação entre análises mensais, análises em janelas móveis e técnicas de clusterização permite capturar tanto tendências estruturais quanto eventos transitórios do processo, ampliando a capacidade de diagnóstico da ferramenta.
O modelo desenvolvido foi avaliado por meio da comparação de seus resultados com os registros de ocorrências e intervenções reportados pelas equipes de manutenção e operação. De maneira geral, observou-se elevada aderência entre os principais direcionadores da Diferença Operacional identificados pelo modelo e os eventos efetivamente registrados em campo, indicando coerência entre os resultados analíticos e a realidade operacional da planta.
Em diversos casos, as variáveis apontadas pelo modelo como potenciais causas dos desvios apresentaram correspondência direta com falhas, anomalias ou intervenções documentadas pelas equipes responsáveis. Adicionalmente, foram identificadas situações em que determinadas variáveis relevantes foram destacadas pelo modelo sem que houvesse registro associado nas bases de manutenção ou operação. Essas ocorrências podem indicar tanto a capacidade do modelo de detectar comportamentos anormais que não foram percebidos ou formalmente registrados pelas equipes de campo, quanto possíveis limitações, inconsistências ou fontes de ruído presentes nos dados analisados.
Os resultados obtidos demonstram que a abordagem proposta possui potencial para atuar como uma ferramenta complementar de diagnóstico operacional, ampliando a capacidade de identificação de desvios e contribuindo para uma investigação mais direcionada das causas associadas ao fechamento do balanço energético.
Dessa forma, o modelo constitui um importante instrumento de apoio à tomada de decisão, permitindo direcionar investigações operacionais, priorizar ações de manutenção, instrumentação e metrologia, aprimorar a qualidade dos dados utilizados no balanço energético e reduzir as incertezas associadas à contabilização das correntes energéticas das plantas de processamento de gás natural.

\---

Matrícula: 241.100.135

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*

