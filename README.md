# P07 - Udacity - A/B testing 

# Experiment Design
## Metric Choice
Invariant metrics – Metricas invariantes

As métricas invariantes não sofrem alteração no grupo de controle ou de experimento, com base nesta orientação, foram escolhidas como métrica:

- Number of Cookies;
- Number of Clicks;
- Click-through Probability.

Evaluation metrics – Metricas de avaliação
1. Gross conversion: Para o propósito do teste a taxa de conversão bruta é interessante porque computa o total de usuários (user-ids) que se matricularam durante o período de experiência dividido pelo total de cookies que clicaram no botão "Start free trial"; Se a hipótese for verdadeira, espera-se uma diminuição no valor desta métrica porque algumas pessoas podem optar por não se inscreverem após a mensagem informando o tempo de comprometimento superior a 5 horas esperado do aluno. 
2. Net conversion: Esta métrica mede o total de alunos que fizeram pelo menos um pagamento em relação ao total de cookies que cliclaram no botão "Start free trial". Esta métrica complementa a de conversão bruta porque ela capta informações do segundo momento do teste, quando estamos mais interessados no total de alunos que permanceram matriculados e efetuaram o pagamento após os 14 dias de teste do curso. Não espero uma grande alteração neste valor em relação ao grupo de controle, porque neste período de teste estou supondo que após os 14 dias, mesmo no grupo de controle, ficam sempre os alunos com mais tempo de dedicação disponível.

Metricas Descartadas
1. Number of user-ids: O número de identificadores dos usuários não foi uma boa métrica invariante porque só é possível capturar este valor após o clique no botão de matrícula.
2. Retention: Poderia ser uma boa métrica, mas como pode ser confirmado nos cálculos realizados neste experimento, para obter o valor desta métrica foram estimados 119 dias e mais de 4 milhões de visualizações da página.

## Measuring Standard Deviation

Probability of enrolling, given click:	0.20625
Click-through-probability: 0.08
Sample:	5.000

Metric evaluation |	value   |	n	 |SD (RAIZ(p*(1-p)/n))	| m
------------------|--------:|----:|----------------------:|---------:
Gross conversion	| 0.20625 |  400|	           0.020230604|	0.039652
Net Conversion	  |0.109313 |  400|	           0.015601545|	0.030579

Para o teste estou assumindo que a distribuição dos dados é binomial e se aproxima de uma distribuição normal a medida que o tamanho da amostra aumenta. Por esta razão foi utilizada a formula (RAIZ(p*(1-p)/n)) para obtenção do desvio padrão.

O total de cookies por dia foi utilizado no cálculo do desvio e da análise, desta forma podemos assumir que a estimativa analítica é bem próxima da medida da variabilidade empírica e será considerada para análise do teste.

## Sizing
### Number of Samples vs. Power

O teste ou correção Bonferroni não foi aplicado porque as duas métricas escolhidas estão correlacionadas e a margem de erro é menor do que a diferença observada, então para estes casos a mudança não é significativa.

Metric avaliation |	value |	p |	d_min|	Sample size|	#Pageviews
------------------|----------:|-----:|--------:|--------:|------------------:
Gross conversion|	0.20625|	0.08|	0.01|	25835|	645875
Retention|	0.53	|0.0165	|0.01	|39115	|4.741.212.121
Net Conversion	|0.10931|	0.08|	0.0075|27413|	685325

Devido ao elevado volume de pageviews necessários para o teste, a métrica de avaliação Retention foi descartado como métrica.

Os dados foram calculados no site: http://www.evanmiller.org/ab-testing/sample-size.html
### Duration vs. Exposure

O experimento consiste na apresentação de uma tela para o usuário com um questionamento que não envolve questões éticas. É considerado de baixo risco para o aluno e para a Udacity e não envolve a manipulação de dados sensíveis ou que possam comprometer o aluno. Com um tráfégo de 100%, estão previstos 18 dias para a execução, o que pode ser um tempo muito longo para este teste. 
Considerando o baixo risco oferecido e o tempo para execução, recomento que 100% do trafego seja utilizado nos testes.

Dias (Pageviews ÷ tráfego ÷ cookies)
Gross conversion:   645.875 ÷ 1.0 ÷ 40.000 = 16 dias
Net Conversion: 685.325 ÷ 1.0 ÷ 40.000 = 18 dias

## Experiment Analysis
### Sanity Checks

Metrica         |Controle	|Experimento
------------|-----------|------------
Pageviews   |	345543	|344660
Clicks	    |    28378	|28325


 Metrica     |Probabilidade|SE	       |m       |CI_Lower    |CI_upper	 |Valor observado |Status
------------|-------------|------------|--------|------------|-----------|----------------|--------
Pageviews   |0.5	  |0.0006018   |0.00118 |0.49882     |	0.50117961|	0.500639667|	Passou
Clicks	    |0.5	  |0.0020997   |0.00412	|0.49588     |	0.5041155|	0.500467347|	Passou

As duas invariantes Pageviews e Clicks passaram na avaliação de sanidade.
Pageviews: CI (0.49882, 0.50117961) , valor observado = 0.500639667
Clicks: CI (0.49588, 0.5041155) , valor observado = 0.500467347

Como Click-through-probability é baseada nas invariantes Pageviews e clicks e as duas passaram na avaliação de sanidade, estou considerando que Click-through-probability também atende aos valores do intervalo de confiança (CI).


## Result Analysis
### Effect Size Tests

 Metrica    | Controle      |Experimento
  ----------|-------------|------------
Clicks	    |17293        |	17260
Enrollments |	3785	  |    3423
Payments    |	2033	  |   1945


		 	 	 		 
Metrica    |p^	 |SE	|m	|d^	|CI Lower bound	|CI Upper bound	|d_min
----------------|-------------|------------|---------------|------------|-----------|----------|--------
Gross conversion|   0.20860707|	0.004371675|	0.008568484|	-0.02055|    -0.0291|	-0.0120|   0.01
Net conversion	|   0.11512749|	0.003434134|	0.006730902|	0.004874|    -0.0116|	 0.0019|  0.0075


Gross conversion: Com o valor observado de -0.0205, CI (-0.0291, -0.012), dmin=0.01, o resultado tem significância estatística e prática. 
Net conversion: Com o valor observado de 0.004874, CI (-0.0019, 0.0116), dmin=0.0075, o resultado não tem significância estatística, nem prática.

### Sign Tests

Probabilidade |	Experimentos |	Sucesso Gross	|Sucesso Net
--------------|--------------|------------------|------------
0.5           |23	     |4          	|10

Gross Conversion: Para o teste de duas caudas foi obtido um valor de p = 0.0026 < α=0.05, demonstrando uma significância estatística.
Net conversion: Para o teste de duas caudas foi obtido um valor de p = 0.6776 > α=0.05, com este valor maior que o α, não há significância estatística.

Os dados foram calculados no site: https://www.graphpad.com/quickcalcs/binomial2/

## Summary

A correção de Bonferroni não foi aplicada aos dados do experimento, porque utilizei variáveis que estavam correlacionadas e usar a correção de Bonferroni neste caso seria muito conservador.

Não foi identificada nenhuma discrepância entre os resultados obtidos nos testes de tamanho do efeito e de sinal. Ambos demonstraram que havia significância estatística nos testes realizados para a métrica de conversão bruta (Gross conversion) e que não havia a mesma significância para o conversão liquida (Net conversion).

Todos os cálculos estão registrados na planilha: https://github.com/liebycardoso/AB_testing/blob/master/Final_Project_Baseline_Values.xlsx

## Recommendation

As métricas escolhidas para o teste A/B foram a conversão bruta (Gross Conversion) e a conversão liquida (Net conversion), com base nos resultados encontrados não recomendo o lançamento desta mudança no site da Udacity.
Foi observado que no grupo experimental menos pessoas clicaram no botão de inscrição, sugerindo que menos alunos ficariam frustrados futuramente no curso por não terem tido tempo suficiente para se dedicar. Este resultado foi apoiado por um valor de p maior que o alfa de 0.05, demonstrando sua significância estatística.  Por outro lado, com um intervalo de confiança de 95% não obtivemos um nível de significância estatística e prática para a métrica de conversão líquida. Seria muito arriscado recomendar o prosseguimento desta mudança no site, uma vez que, não conseguimos demonstrar com um nível de certeza que haveria ganho para a Udacity em seu objetivo de reduzir o número de alunos frustrados que abandonam o período de teste gratuito, sem que houve perda significativa no número de alunos inscritos após este período. 

A ausência de ganhos financeiros que podem decorrer do lançamento desta mudança, conforme sugere a falta de significância estatística na conversão líquida deve ser avaliada pela equipe Udacity, caso a empresa decida seguir com os testes.

## Follow-Up Experiment

Vou usar minha própria experiência com o nanodegree da Udacity para sugerir um experimento futuro; acredito que exista um grupo de alunos como eu que possa ter experimentado uma frustação inicial, não por não ter o a disponibilidade necessária, mas por não ter a base de Python que era esperada. Para este grupo, atender os primeiros meses de curso são necessárias mais que 5 horas semanais e provavelmente existe um outro de alunos com mais experiência que poderá atender todos os requisitos do curso com menos tempo de dedicação.

Neste sentido a Udacity poderia testar um experimento em que:
1. Ao demonstrar o interesse no curso, o potencial aluno é direcionado para um teste de conhecimento;
2. Após o teste de conhecimento o aluno é alertado quanto ao nível de comprometimento necessário para sua atualização de conhecimento e para execução do nanodegree;
3. Para os alunos com baixa pontuação a Udacity pode sugerir cursos complementares;
4. Para o caso especifico do nanodegree de Análise de dados, o curso pode ser dividido em dois. E para os alunos com baixa pontuação fosse sugerido que completassem primeiro o nanodegree de Análise de dados nível 1, por exemplo.

A hipótese é que após o teste de conhecimento, o aluno que decidir se matricular no curso estará mais ciente do nível de exigência/dedicação e será mais provável que complete todo o curso, aumentando o percentual de retenção de alunos.

Métricas sugeridas para o experimento:
1. A unidade de desvio é o cookies.
2. Invariantes:  Número de visualização de páginas (pageviews), o número de clicks no teste de conhecimento e o número de clicks no botão de inicio do teste (Free trial) de 14 dias.
3. Avaliação: As mesmas métricas de conversão bruta e líquida usadas e descritas neste teste. Incluiria também o teste de retenção para verificar o total de alunos que permaneceram inscritos no curso após o teste inicial de 14 dias.

## Referências
- http://www.evanmiller.org/ab-testing/sample-size.html
- https://rpubs.com/superseer/ab_testing
- https://www.graphpad.com/quickcalcs/binomial2/
- http://soniavieira.blogspot.com.br/2016/11/teste-de-bonferoni.html
- http://adalee2future.github.io/udacity_data_analyst/AB_Test.pdf
- http://lilychang.net/AB_Testing/

