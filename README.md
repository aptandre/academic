# Metadados

* Título: LLM Evaluators Recognize and Favor Their Own Generations
* Autores: Arjun Panickssery, Samuel R. Bowman, Shi Feng
* Instituição dos autores: NYU (New York University)
* Onde foi publicado: Arxiv
* Ano de publicação: 2024
* Link: https://arxiv.org/pdf/2404.13076

# Qual o problema?

Uma das formas de se avaliar o resultado gerado por um Large Language Model (LLM) é utilizando outro LLM para esta tarefa. Contudo, ao fazer isso, é possível observar que os modelos tendem a preferir os resultados gerados por eles mesmos em detrimento dos textos gerados por seres humanos ou modelos distintos do modelo avaliador, e isso levanta questionamentos acerca da neutralidade do modelo durante a avaliação, fazendo-se necessária uma investigação acerca da correlação entre o auto-reconhecimento e a auto-preferência desses LLMs.

# Qual a solução?

A priori, a preocupação principal do artigo era apresentar dados que sustentam a ideia de que o auto-viés dos LLMs decorria do fato de que esses modelos eram capazes de reconhecer os seus próprios trabalhos. Assim sendo, fora proposta uma metodologia para comprovar a teoria de que o auto-viés estava diretamente relacionado com o que foi descrito como "auto-reconhecimento" de textos.

## Como os autores tentam resolver o problema? Criando um algoritmo novo? uma metodologia? uma ferramenta? 

Inicialmente, os autores utilizaram uma metodologia separada em dois experimentos principais: a avaliação em pares e a avaliação individual, e estabeleceu métricas para ambos os experimentos, o autorreconhecimento e autopreferência, avaliando os resultados a partir do que o modelo produzia.

## Quais são os detalhes técnicos dessa solução? O que chama mais atenção? Qual a ideia geral e o que deve ser discutido em mais detalhes?

A priori, ao definir uma atividade-alvo a ser avaliada, as tarefas de sumarização de texto foram as escolhidas, tanto por serem uma tarefa comum para LLMs quanto por elas ocasionarem em um resultado fácil de avaliar e ser comparado por um humano. Então, o experimento foi conduzido conforme os seguintes passos:

Passo 1: Seleção de modelos, nesse caso, foram escolhidos três modelos: o Llama2, GPT-3.5 e GPT-4, cada modelo fora utilizado tanto como um gerador quanto como um avaliador.

Passo 2: Seleção da base de dados: foram separados dois datasets referentes à sumarização de textos (XSUM e CNN/Dailymail). 

Dessa forma, os autores conduziram dois tipos de experimentos: os experimentos em pares e os experimentos individuais.
No passo 3 foi realizado um experimento em pares, no qual o modelo avaliador era apresentado a dois textos, um deles tinha sido gerado pelo próprio avaliador e o segundo surgiu de uma fonte diferente (podendo essa fonte ser um modelo diferente ou um ser humano), então, o modelo era perguntado qual dos textos ele prefere (teste de auto-viés) e qual deles ele acha que foi gerado por ele mesmo (teste de auto-reconhecimento).

Adiante, no passo 4, foram realizados os experimentos individuais, o modelo receberia no prompt apenas um texto e teria de avaliar numa escala Likert de 1 a 5 a qualidade do texto (texto de auto-viés) e era questionado acerca de se ele reconhecia aquele determinado texto como sido generado por ele mesmo (teste de auto-reconhecimento).

Então, os pesquisadores realizaram os mesmos passos anteiores porém em versões ajustadas (através de fine-tuning) dos modelos supracitados.
Além disso, é perceptível que apesar dos autores levantaram uma hipótese causal, eles estão cientes que, até a data de escrita do artigo (Abril de 2024), não havia mecanismos existentes para validar a sua hipótese, além de explicitar as ameaças à validade e estarem cientes de que possivelmente o experimento deles apenas provê evidências em torno da hipótese inicial, e que para validá-la por completo seria necessário reproduzir o experimento diversas vezes, com dados e tarefas distintos a fim de fortalecer o argumento. Não obstante, as evidências provenientes dos experimentos apontam para uma correlação promissora entre o auto-reconhecimento e o auto-viés, conforme fora descrito na seção 3, que diz respeito ao experimento sendo realizado após um fine-tuning em cada modelo, sendo descoberta, assim, uma forte correlação linear entre as duas métricas.

# Como foi avaliado?

Inicialmente, os autores tinham a hipótese de que, ao utilizar fine-tuning para tarefas de sumarização nos modelos, o auto-viés iria aumentar, assim sendo, eles tinham o plano inicial de fazer esse ajuste, contudo, havia o risco de que isso gerasse confusão nos resultados produzidos. Para controlar fatores de confusão, os modelos também foram ajustados em tarefas não relacionadas, como:
comparação de comprimento (na qual o modelo deve identificar o texto maior), contagem de vogais, pontuação de legibilidade (identificar qual resumo era mais fácil de ler usando a pontuação de legibilidade Flesch-Kincaid). Isso ajudou a garantir que as mudanças na autopreferência não fossem devido a fatores não relacionados ao autorreconhecimento.

Métricas utilizadas:
Pontuação de autoreconhecimento:
A proporção de vezes que o LLM identifica corretamente se um resumo foi gerado por ele mesmo.
Pontuação de autopreferência:
A proporção de vezes que o LLM prefere seu próprio resumo em vez de resumos gerados por outros (humanos ou outros LLMs).
Classificações em escala Likert:
Para resumos individuais, os modelos classificaram a qualidade dos resumos em uma escala de 1 a 5, e essas pontuações foram usadas para calcular o viés de autopreferência.
Pontuações de confiança:
Probabilidades normalizadas geradas pelos LLMs ao escolher entre duas opções (autorreconhecimento ou autopreferência), usadas para avaliar a certeza das decisões dos modelos.Métodos estatísticos

- Métodos estatísticos:
Análise de correlação: A correlação linear entre as pontuações de autorreconhecimento e as pontuações de autopreferência foi medida. Este foi o método primário usado para testar a hipótese de que melhorar o autorreconhecimento por meio do ajuste fino aumenta a autopreferência. O coeficiente de correlação τ de Kendall foi usado para medir a correlação de classificação entre a confiança dos modelos no autorreconhecimento e na autopreferência em exemplos individuais (Tabela 1 no artigo). Uma correlação positiva dá suporte à hipótese de que essas duas propriedades estão vinculadas.

- Comparações individuais e em pares:
Para evitar o viés de ordenação, os pesquisadores solicitaram os modelos duas vezes, com a ordem dos resumos invertida. Isso ajudou a controlar qualquer viés causado pela tendência do modelo de preferir a primeira ou segunda opção apresentada em avaliações em pares.

- Análise de variância:
As diferenças entre os modelos (por exemplo, GPT-4, GPT-3.5 e Llama 2) foram testadas em conjuntos de dados e configurações de fine-tuning para garantir diferenças estatisticamente significativas no desempenho.

- Análise de intervalo de confiança:
Os intervalos de confiança foram computados para resultados de fine-tuning, particularmente nos experimentos que envolveram pequenos conjuntos de dados (por exemplo, 500 exemplos). Isso ajudou a garantir que os efeitos observados fossem robustos e não devidos à variação aleatória.

- Generalização fora do domínio:
Após o fine-tuning em um conjunto de dados, os modelos foram avaliados em um conjunto de dados diferente para testar a generalização das melhorias de autorreconhecimento e autopreferência. Esta etapa garantiu que as descobertas fossem aplicadas além do conjunto de dados usado para treinamento.

- Conjuntos de dados:
Os modelos foram avaliados em 1.000 resumos amostrados de cada um dos conjuntos de dados CNN/DailyMail (resumos de várias frases escritos por humanos de artigos de notícias) e XSUM (resumos de uma única frase escritos por humanos, oferecendo dados mais concisos).

- Dados gerados:
Os LLMs geraram resumos para todos os artigos em ambos os conjuntos de dados. Essas saídas foram usadas para tarefas de avaliação em pares e individuais.

- Dados do fine-tuning:
500 exemplos em pares de cada conjunto de dados para cada modelo. Tanto a generalização dentro do domínio (mesmo conjunto de dados) quanto a generalização fora do domínio (conjunto de dados diferente) foram testadas.

# Quais são os resultados?

O artigo provê evidências, ainda que iniciais, de que os LLMs tendem a criar um auto-viés por reconhecerem os textos que eles mesmos criaram, mas não o suficiente para validar completamente a sua hipótese. Ademais, os resultados obtidos também apresentam uma forte correlação linear entre o auto-reconhecimento e a auto-preferência ao ajustar os modelos com fine-tuning especificamente para a tarefa de sumarização, além de estabelecer o auto-reconhecimento como fator crucial em auto-avaliação não-enviesada, bem como fator de alta importância para a segurança da avaliação.

# Resenha crítica

Durante a leitura do artigo, é perceptível que a questão central advém da seguinte reflexão: dado um determinado modelo de linguagem ele tende a preferir os resultados gerados por si mesmo por ele gerar os melhores resultados ou isso tem a ver com o modelo conseguir identificar que foi ele que gerou aquilo e que, portanto, a distribuição de probabilidade daqueles tokens é a mais similar ao que o modelo geraria? Afinal, se o modelo tende apenas a priorizar os resultados os quais ele mesmo gera, e, mais do que isso, superestimar a qualidade desses resultados, temos, portanto, uma ameaça gravíssima à validade da avaliação, a qual, por sua vez, deveria ser imparcial e, mais precisamente, sem nenhum viés.

Assim sendo, o artigo investiga justamente a capacidade de autorreconhecimento dos modelos, a sua capacidade de reconhecer que eles geraram um determinado texto, e tenta relacioná-la ao resultado da auto-preferência. Contudo, não seria a avaliação acerca da autopreferência trivial? Afinal, os resultados produzidos por um determinado modelo seguem uma distribuição probabilística suficientemente similar para que o modelo a priorize em detrimento dos resultados produzidos pelos outros.

Atrelado a isso, há o fato de que ainda que um LLM tente ser imparcial, ele estará sempre limitado à sua própria arquitetura, a qual, por sua vez, possui suas regras e demais critérios de qualidade, ocasionando, assim, uma espécie de "retroalimentação", na qual o modelo sempre estará dando mais valor ao que ele produziu, já que isso vai de conforme com o que ele aprendeu. 

Sob outro ângulo, o teste da hipótese de que o fine-tuning iria aumentar a auto-preferência foi bastante relevante na busca de evidências acerca da correlação entre o auto-viés e o auto-reconhecimento, afinal, os resultados do experimento avaliaram que há uma forte correlação linear entre essas duas variáveis, e, muito embora ainda não hajam mecanismos o suficiente para corroborar com a hipótese original, os resultados apresentados parecem promissores quanto à validação desta.

Por fim, os autores do artigo também estão cientes de que o seu experimento não era o suficiente para comprovar a hipótese inicial por completo, indicando um possível caminho de como seguir com a pesquisa, explicitando que é necessário fazer um teste com outros modelos, para datasets mais variados e utilizando tarefas diversificadas além das utilizadas no experimento.


# Discussão

Deixar aqui os comentários e insights importantes que surgiram durante a discussão do paper com o grupo. 

# O que eu tenho a ver com isso?

Bom, inicialmente o que é algo que chama bastante atenção no artigo é esse fator de auto-viés por parte dos LLMs, então, inicialmente, entender que para a tarefa de avaliação de LLMs tem-se esse desafio inicial a ser considerado ao realizar experimentos de avaliação. No mais, a fim de comprovar a tese é possível estender o experimento e realizá-lo com outras bases de dados, possivelmente sem utilizar o Llama2 (pois a versão do modelo utilizada no experimento foi uma implementação dos autores), com outras bases de dados e possivelmente direcionado a outras tarefas. No mais, a metodologia adotada nos experimentos do artigo é satisfatória e não apresenta ameaças ulteriores à validade, podendo, pois, ser utilizada em experimentos futuros.

# Replicação

* Onde estão os dados? https://github.com/ArjunPanickssery/self_recognition/tree/master/articles | Os prompts utilizados para consulta/fine-tuning estão presentes no artigo
* Onde está o código? https://github.com/ArjunPanickssery/self_recognition
* Conseguiu entender/rodar? Não consegui rodar pois para rodar o código são necessárias as chaves de API da OpenAI as quais eu não possuo.
* Dá para replicar o experimento/estudo? Dado que tenhamos as keys necessárias (OPENAI_API_KEY, ANTHROPIC_API_KEY, HF_TOKEN, *mas nem todas são necessárias a depender do experimento*), sim.

