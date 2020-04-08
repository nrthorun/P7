
P7 - Testes A/B
===========================
### Por Nikolas Thorun

### Resumo

Este experimento baseia-se em uma modificação feita após o clique no botão _'start free trial'_ no site da Udacity. Em vez de levar o estudante direto à parte de matrícula, uma janela aparece com um campo para que este responda quantas horas por semana tem disponível para os estudos. Se responder que dispõe de mais de 5 horas por semana, o aluno segue para a área de matrícula como o usual, se não, ele recebe uma mensagem dizendo que os cursos da Udacity geralmente requerem maior tempo para serem completados e sugere que ele acesse os materiais gratuitos.
A hipótese é que as expectativas sejam alinhadas, de maneira a reduzir o número de estudantes frustrados por não disporem de tempo suficiente sem diminuir significativamente o número de alunos que se matriculam e que terminam o curso.

### Escolha das métricas

**Métricas Invariantes:** Número de cookies, número de cliques, probabilidade de clique  <br>
**Métricas de Avaliação:** Conversão, retenção e conversão líquida

* Número de cookies: Número único de cookies relacionados aos visitantes do site. Não deve variar entre os grupos controle e de experimento e, por isso, uma boa escolha para métrica invariante.

* Número de cliques: Número de cliques no botão _'start free trial'_. Também não deve variar entre os grupos, já que se encontra antes da parte do experimento. Métrica invariante.

* Probabilidade de clique: Relação entre as duas métricas anteriores. Como se baseia em métricas que não devem sofrer mudanças, também não deve mudar. Métrica invariante

* Conversão: Número único de usuários a matricularem no free trial dividido pelo número único de cookies que clicaram no botão _'start free trial'_. No grupo de experimento deve-se aparecer a janela com a pergunta a respeito das horas semanais disponíveis e no grupo controle, não. Por isso, é uma boa métrica de avaliação. O esperado neste experimento é que a conversão diminua, já que uma parte dos alunos será direcionada para a página de materiais gratuitos.


* Retenção: Número de usuários que permaneceram matriculados por mais de 14 dias (e, portanto, fazendo pelo menos 1 pagamento) sobre o número de usuários matriculados. Essas etapas acontecem após o experimento, os números nos grupos podem ser diferentes.  Poderia ser utilizada como métrica de avaliação, mas não será por motivos que serão abordados posteriormente.


* Conversão líquida: Número de usuários que permaneceram matriculados por mais de 14 dias sobre o número de cliques nobotão _'start free trial'_. Como o número de usuários matriculados pode ser diferente nos dois grupos, usaremos como métrica de avaliação. 


Métricas não utilizadas: Número de usuários únicos. Como depende do experimento na página de _'start free trial'_, não pode ser utilizado como métrica invariante. Poderia ser utilizada como métrica de avaliação, porém não é tão robusta por não ser normalizada e é redundante às outras métricas de avaliação.

O resultado esperado é que o número de alunos frustrados diminua, sem que isso acarrete em uma diminuição dos lucros da empresa. Dessa forma, esperamos que a conversão diminua e podemos até tolerar uma queda na conversão líquida, porém esta não pode ultrapassar o limite prático de significância.

### Medindo a variabilidade

Considerando uma distribuição binomial com população `N` e probabilidade `p`, temos que o erro padrão é `SE = sqrt((p*(1-p)/N)`.

* Conversão:
> `p = 0,20625` // probabilidade de matricular por clique <br>
> `N = 5000 * 0,08` // número de pessoas a clicar no botão <br>
> `SE = 0,0202`

A unidade de análise é uma pessoa que clica no botão e a unidade de desvio é um cookie. As unidades não são iguais, o que indica que as variabilidades empírica e analítica não coincidem. Porém, são próximas o bastante para sugerir estas não serão muito divergentes.

* Retenção:
>`p = 0,53` // probabilidade de pagamento por matriculado <br>
>`N = 5000 * 0,08 * 0,20625` // probabilidade de matrícula <br>
>`SE = 0,0549`

A unidade de análise é uma pessoa que se matriculou e a unidade de desvio é um user-id. Como as unidades não são iguais, as variabilidades não coincidem. Mas como é bem provável que cada pessoa só se matricule uma vez, variabilidades devem ser aproximadas.

* Conversão líquida:
>`p = 0,1093125` // Probabilidade de pagamento por clique <br>
>`N = 5000 * 0,08` // Número de pessoas a clicar no botão <br>
>`SE = 0,0156`

Aqui as unidades de análise e de desvio são iguais e, portanto, as variabilidades coincidem. Sendo assim, devemos ter a variabilidade analítica mais precisa.


### Tamanho da amostra

Os cálculos a seguir foram feitos utilizando a [calculadora online](http://www.evanmiller.org/ab-testing/sample-size.html)
com `alfa` = 0,05 e 1 - `beta` = 0,2. 

* Conversão:
Com a probabilidade de conversão igual a 0,20625 e o efeito mínimo detectável (`d_min`) igual a 0,01 temos que o número de amostras dado pela calculadora é igual a 25835. Como temos dois grupos e a probabilidade de clique é 0,08, temos que o número de visualizações (`V`) necessárias é igual a:  <br>
>`V = 25835 * 2 / 0,08 = 645875`

* Retenção:
Com a probabilidade de retenção igual a 0,53 e `d_min` = 0,01, temos que o número de amostras dado pela calculadora é 39155.
Como queremos a probabilidade de matrícula para os dois grupos, temos:  <br>
>`V = 39155 * 2 / 0,08 / 0,20625 = 4741212`

* Conversão Líquida:
Com a probabilidade de conversão igual a 0,1093125 e `d_min` = 0,0075, temos que o número de amostras dado pela calculadora é 27413. Para dois grupos e com a probabilidade de clique igual a 0,08 temos:  <br>
>`V = 27413 * 2 / 0,08 = 685325`

Neste caso, vamos desclassificar a Retenção pois, para conseguirmos 4,7 milhões de visualizações de página, precisaríamos de 119 dias com 100% do tráfico direcionado para o experimento, o que não soa razoável.


### Duração x Exposição


Se considerarmos 100% do fluxo para o experimento, precisariamos de 18 dias para colhermos os resultados:  <br>
`685325 / 40000 = 17,13`

O risco é baixo já que não é esperada uma queda radical nas matrículas.
Porém, se redirecionarmos apenas 60% do trafico para o experimento, teremos:  <br>
`685325 / (40000 * 0,6) = 28,555`

Ou seja, 29 dias, que é um tempo razoável e com menor risco, já que não utiliza todo o tráfico.



### Verificações de Sanidade

* Número de cookies:

Controle: `345543`  <br>
Experimento: `344660`

Considerando a distribuição binomial com a probabilidade de 0,5, o erro padrão deve ser: `SE = sqrt(0.5 *0.5 / N1 + N2)`
Sendo que a margem de erro será `SE * 1,96`
O limite inferior será `0,5 - ME` e o limite superior será `0,5 + ME`
Observado é o numero do controle sobre a soma do controle e do experimento

>`SE = 0,0006018` <br>
>`ME = 0,0006018 * 1,96` = 0,001179 <br>
>`LS = 0,5 - 0,001179 = 0,4988` <br>
>`LI = 0,5 + 0,001179 = 0,5012` <br>
>`Observado = 345543 / (345543 + 344660) = 0,5006`

* Número de cliques:

Controle: `28378 ` <br>
Experimento: `28325`

>`SE = 0,002099` <br>
>`ME = 0,0006018 * 1,96 = 0,004116` <br>
>`LS = 0,5 - 0,004116 = 0,4959` <br>
>`LI = 0,5 + 0,004116 = 0,5041` <br>
>`Observado = 28378/ (28378 + 28325) = 0,5005`

* Probabilidade de clique:

Probabilidade de clique do grupo controle = `28378 / 345543 = 0,082126` <br>
Cookies do grupo de experimento = `344660`

>`SE = sqrt(0,082126 * (1-0.082126) / 344660) = 0,000468` <br>
>`ME = 1,96 * 0,000468 = 0,00092` <br>
>`LI = 0,082126 - 0,00092 = 0,0812` <br>
>`LS = 0,082126 + 0,00092 = 0,0830` <br>
>`Observado  = 28325 / 344660 = 0,082182`


### Testes efeito de tamanho

Para calcular uma probabilidade de clique, por exemplo, o numerador (`X`) é o número de cliques e o denominador (`N`) é a quantidade de cookies. No `P_pooled`, somam-se os numeradores dos grupos controle e de experimento (1 e 2, respectivamente).

 `P_pooled = (X1 + X2) / (N1 + N2)`  <br>
 `SE_pooled = sqrt(P_pooled * (1 - P_pooled) * (1 / N1 + 1 / N2))`


A diferença das probabilidades se dá por:  <br>
 `D = X2 / N2 - X1 / N1`


Assim, o intervalo de confiança se dá com a margem de erro ao meio e os limites inferior e superior como segue:  <br>
 `ME = SE * 1,96` (Intervalo de Confiança de 95%)  <br>
 `LI = D - ME`  <br>
 `LS = D + ME`


* Conversão:
Para calcular a probabilidade pooled da conversão, o numerador é a quantidade de matrículas e o denominador é a quantidade de cliques no botão _'start free trial'_.

>`P_pooled = (3725 + 3423) / (17293 + 17260) = 0,2086` <br>
>`SE_pooled = sqrt(0,2086 * (1-0,2086) * (1 / 17293 + 1 / 17260)) = 0,00437` <br>
>`ME = 0,00437 * 1,96 = 0,00857` <br>
>`D = 3423/ 17260 - 3785 / 17293 = -0,02055` <br>
>`LI = -0,0291` <br>
>`LS = -0,0120`

O intervalo não contem 0, então a métrica é estatisticamente relevante. O intervalo também não contém o 0,01 ou -0,01 (`d_min`), então, também é praticamente significante. 

* Conversão líquida:
Para calcular a probabilidade pooled da conversão líquida, o numerador é a quantidade de usuários que permaneceram matriculados após 14 dias e o denominador é a quantidade de cliques no botão _'start free trial'_.

>`P_pooled = (2033 + 1945) / (17293 + 17260) = 0,1151` <br>
>`SE_pooled = sqrt(0,1151 * (0,1151) * (1 / 17293 + 1 / 17260)) = 0,00343` <br>
>`ME = 0,00343 * 1,96 = 0,00672` <br>
>`D = 1945 / 17260 - 2033 / 17293 = -0,0048` <br>
>`LI = -0,0116` <br>
>`LS = 0,0019`

O intervalo contém 0, então não é estatisticamente significante e, portanto, não é praticamente significante.

### Testes de sinal

Usando a [calculadora online](http://www.evanmiller.org/ab-testing/sample-size.html) para fazer os testes de sinal, temos:
* Para conversão, no grupo de experimento temos 4 dias de sucesso no total de 23 dias. Para o teste de sinal de 0,5 a calculadora dá o `valor-p` de 0,0026. Como este é menor do que o `alfa` (0,05), a mudança é estatisticamente significativa.
* Para conversão líquida, no grupo de experimento temos 10 dias de sucesso no total de 23 dias. Para o teste de sinal de 0,5 a calculadora nos dá o `valor-p` de 0,6776. Como este é maior do que o `alfa`, a mudança não é estatisticamente significativa.

### Sumário de resultados

Os resultados dos testes de tamanho de efeito e de sinal são condizentes entre si. Como descrito anteriormente, era esperada uma queda na conversão do grupo de experimento, e isso de fato aconteceu. Observou-se também uma queda na conversão líquida, porém esta não é praticamente significante. A hipótese nula, neste caso, é não haver mudanças praticamente significativa nas duas métricas de avaliação e, portanto, não conseguiremos rejeitá-la.

A correção de Bonferroni não foi utilizada, já que esta é adequada para reduzir as chances de falsos-positivos (erros do tipo I) em experimentos nos quais qualquer métrica praticamente significante pode validar a hipótese. No caso do nosso experimento, como necessitamos dos resultados de todas as métricas, o risco de falsos negativos (erros do tipo II) aumenta conforme aumenta-se o número de métericas e, portanto, não se faz necessário o controle de falsos positivos. 


### Recomendação

Este experimento foi desenvolvido para filtrar os alunos mais engajados, a fim de melhorar a experiência destes e otimizar o tempo dos mentores sem diminuir significativamente a quantidade de alunos que permanecem matriculados. Após a análise dos resultados percebemos que há uma diminuição estatisticamente e praticamente significativa na conversão, o que é bom e era esperado. A conversão líquida não apresenta mudança estatisticamente significativa, porém seu intervalo de confiança inclui o valor negativo do limite de significância prática. Isso significa que não podemos ter certeza desse efeito no nível de confiança de 95%. Em outras palavras, é possível que o número de alunos que permanecem matriculados por mais de 14 dias tenha diminuido de maneira que influenciaria significativamente o lucro da empresa. Por este motivo, minha recomendação é não lançar essa experiência e partir para novos experimentos.

### Experimento subsequente

Para o experimento a ser feito em seguida, eu tentaria uma abordagem conhecida como _onboarding_. Este é o termo para descrever o primeiro contato com o cliente, no qual tentamos entregar valor inicial a ele e fazer seus olhos brilharem. _Onboarding_ é aproveitar a energia inicial de compra do cliente para fazê-lo se engajar. Isso pode ser feito de várias maneiras, mas o foco aqui é em entregar o first value, ou primeiro valor. Neste caso, seria bastante interessante a criação de um vídeo institucional explicando o que é a Udacity, mostrar seu posicionamento no mercado, trazer um leve portfólio de parceiros e destacar os seus pontos fortes em comparação aos concorrentes. Também é válido mostrar casos de sucesso, nos quais alunos certificados contam como seus aprendizados os ajudaram a superar barreiras, a conseguirem melhores empregos ou melhores salários. Estatísticas podem servir de apoio para os casos de suporte, mostrando a relação de alunos que completaram os estudos com a melhoria de vida profissional, índices de satisfação e etc. Enfim, a idéia é causar o brilho nos olhos do aluno recém matriculado, motivando-o a fazer parte do time de sucesso dos alunos certificados da Udacity. 
Ao adicionar este vídeo como primeiro vídeo do curso, não trazemos risco de diminuição de cliques ou de matrículas. O esperado é que o número de alunos matriculados por mais de 14 dias aumente significativamente.

* Inicialização: Após a matrícula, os alunos serão aleatoriamente divididos entre os grupos controle e de experimento. No grupo de experimento o vídeo institucional aparecerá como primeiro vídeo do curso, no grupo controle não.
* Hipótese nula: O número de alunos que permanecem matriculados após 14 dias não aumentará em uma quantidade significativa.
* Unidade de desvio: A unidade de desvio será o user-id, pois a mudança do experimento acontece após a matrícula.
* Métricas Invariantes: Cliques, cookies e user-ids podem ser usados como métricas invariantes, neste caso. 
* Métricas de Avaliação: A métrica de avaliação seria Conversão líquida. Um aumento estatisticamente e praticamente significante significaria sucesso do experimento.

Se observado aumento estatisticamente e praticamente significante da retenção, o experimento será lançado.















