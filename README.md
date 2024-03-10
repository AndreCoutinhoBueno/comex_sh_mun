Neste projeto, através de regressão linear e da regressão por floresta randômica, utilizando uma ou múltiplas variáveis, se faz a previsão de importação anual por município.



A base principal de dados foi extraída do site [Comexstat](https://comexstat.mdic.gov.br/)

O objetivo é dar suporte ao planjamento financeiro, entre outros. 

Antes da previsão final, vários testes comparativos serão conduzidos, buscando maior acertividade da previsão:  
* Regressão Linear vs Regressão por Floresta Randômica  
* Regressão Simples vs Regressão com Multiplas Variáveis
* Combinação de uso entre as multiplas variáveis
* Mês de início do grupamento de 12 meses

Ao final, após identificada e adotada a melhor combinação de técnicas e de variáveis, a previsão será emitida.  

### Identifica os municípios foco

Os municípios da região e Pouso Alegre foram os escolhidos para foco deste o projeto. Eles foram todos identificaos através da planilha do [IBGE](https://www.ibge.gov.br/) sobre estruturação geográfica do Brasil.  

![image](https://github.com/AndreCoutinhoBueno/Importacao-por-SH-e-Municipio/assets/152298881/aa899a2d-002c-4aaa-bc92-0b7c45d121a5)

Após filtrar as importações deixando passar somente os municípios da região de Pouso Alegre, são identificados e descartados registros de importação fora da normalidade.  

![image](https://github.com/AndreCoutinhoBueno/Importacao-por-SH-e-Municipio/assets/152298881/03acb605-3c8c-4fb9-9941-5ab358c209d3)

### Visualiza o histórico de importações

Conclui-se que, ao final, as importações foram dominadas e exclusivamente performadas pelo município de Pouso Alegre.

![image](https://github.com/AndreCoutinhoBueno/Importacao-por-SH-e-Municipio/assets/152298881/320f32f2-ef3f-4fc3-8ec9-51355f13f1c8)

Desse ponto em diante, as análises e previsões serão feitas considerando a região agrupada, e não mais por município.  

### Testa e forma preliminar e comparativa os dois modelos de regressão, Linear e por Floresta Randômica.

![image](https://github.com/AndreCoutinhoBueno/Importacao-por-SH-e-Municipio/assets/152298881/2698a272-63a5-4146-b7be-358fe65178e9)

* Métricas dos testes preliminares
  * Regressão Linear*:
    * Média do Erro Percentual:9%
    * Desvio Padrão do Erro Percentual:30%  

  * Random Forest:
    * Média do Erro Percentual: 3%
    * Desvio Padrão do Erro Percentual: 33%  

* Conclusão do teste:
Nessa primeira tentativa de previsão, não parece ter havido  
diferença significativa entre os dois métodos de regressão testados.  

### Agrega Multiplas Variáveis  

As importações de fertilizantes são normalmente previstas em função do seu comportamento ao longo dos anos. Porém, existem fatores, ou variáveis, que afetam as importações diferencialmente ao longo dos anos. Esses fatores podem ser climáticos, financeiros outros.  

Nesse projeto, com objetivo de melhorar as métricas da previsão, agregaremos:

* Exportação de Café:
  * Mais uma vez os dados são extraídos da fonte principal, o site Comexstat.
  * Da mesma forma, as exportações são filtradas, deixando passar somente os registros relativos aos municípios da região de Pouso Alegre.
  * Em seguida, são descartadosos registros cmom valores fora da normalidade.
![image](https://github.com/AndreCoutinhoBueno/Importacao-por-SH-e-Municipio/assets/152298881/69a9c23f-8e43-4591-976a-ff0d1ffff874)

* Volumes de Chuva na Região:  
  * Extraídas do site do [INMET](https://portal.inmet.gov.br/)
  * Os municípios da região de Pouso Alegre que possuem estação meteorológica automática do INMET insataladas em seu território são:
    * CONCEICAO DAS ALAGOAS
    * PASSA QUATRO
    * CALDAS

> Uma séria de detalhes são necessários par se extrair e agrupar mensalmente os dados sobre chuvas, que podem ser conferidos neste link.

![image](https://github.com/AndreCoutinhoBueno/Importacao-por-SH-e-Municipio/assets/152298881/37cff1ff-ed9e-49f0-8c22-b50a3149ac9d)

Mesmo após medidas de limpeza de dados incosistentes, ao visualizar o histograma acima, se constata ainda alguma anormalidade no formato da curva. O autor acredita que muito se pode fazer utilizando os demais dados meteorológicos contidos na base de dados, de forma a aproximar mais da normalidade os dados obtidos.  

* Funde as tabelas de importações de fertilizantes, exportação de café e chuvas na região.
 * ![image](https://github.com/AndreCoutinhoBueno/Importacao-por-SH-e-Municipio/assets/152298881/4af91731-1b26-4c70-9e17-339ee31baef3)

### Formata teste para previsão de 12 meses  

* Insere novas colunas demarcando os 12 tipos de agrupamento de 12 meses, cada um iniciado em um dos meses do ano  
  * O objetivo é testar para saber qual mês é ideal para se fazer previsão de 12 meses.
  * Por ideal se entende, que produzam previsões mais acertivas.  
* A tabela ficou assim:  
  * ![image](https://github.com/AndreCoutinhoBueno/Importacao-por-SH-e-Municipio/assets/152298881/908158f3-e43e-48b4-a942-e547fa395d4a)
 
### Executa teste multiplos

* Grupamento de 12 meses
* Cominações de multiplas variáveis
* Diferentes durações do período de treino do modelo preditivo

Durante a execução dos testes é executada uma operação que denomino aqui de "Adiantamento de Inputs" que permite prever com inputs do período anterior ao previsto. É um ponto complexo do assunto, sobre o qual pretendo desenvolver uma explicação escrita mais efetiva, mas desde já, me ponho a disposição por telefone para esclarecimento adcional imediato.

Os resultados dos testes foram salvos e podem ser visualizados na tabela a seguir:  

![image](https://github.com/AndreCoutinhoBueno/Importacao-por-SH-e-Municipio/assets/152298881/11c908a1-72a9-440a-a992-4ad557ae35f9)  

Podemos ver que, para testar todas as combinações possíveis, foram executados mais de 6 mil testes.  

### Analisa Resultado dos Teste  

O teste multiplo demonstrou que, com multiplas variáveis, o desempenho dos dois modelos de regressão, o Linear e o por Floresta Randômica, parecem diferir intensamente, não do erro percentual médio, mas sim no desvio padrão percentual médio:

![image](https://github.com/AndreCoutinhoBueno/Importacao-por-SH-e-Municipio/assets/152298881/92013809-bc7d-41c1-9d3d-f83fd4da2386)  

![image](https://github.com/AndreCoutinhoBueno/Importacao-por-SH-e-Municipio/assets/152298881/c47593cf-95f1-4ba1-9429-0d24a738908c)  


Devido a maior desvio padrão produzido pelo modelo de Regressão Linear, seguiremos o estudo
somente processando a Regressão por Floresta Randômica.  

* Ranquea os melhores resultados
 * ![image](https://github.com/AndreCoutinhoBueno/Importacao-por-SH-e-Municipio/assets/152298881/c26425fb-16f0-44e4-80cc-503329cdead6)
 * Dos mais de 6 mil testes realizados, acima podemos ver os 15 de melhor resultado ou, de menor erro e desvio padrão sobre o percentual do peso da importação de fertilizantes.
 * Interessante notar que no ranque há a presença constante do agrupamento de 12 meses iniciado no mês 1, ou seja, o ano comum, iniciado em Janeiro. Sendo assim, podemos concluir que Janeiro é o mês do ano onde se conseue a melhor previsão para 12 meses a frente.
 * Considerando todos os valores de desvio_maximo_FR sem diferença significativa, faremos opção por a combinação de fatores da linha 4, que além do Ano1 , leva em conta também as chuvas e a relação de preço entre os fertilizantes e o café.

 * Duração do período de treino
  * ![image](https://github.com/AndreCoutinhoBueno/Importacao-por-SH-e-Municipio/assets/152298881/b4a17485-950a-4030-8332-0b0467e92c9e)
  * Podemos notar tendência de melhor resultado para testes com periodos de treino maiores a não ser para a maior de todas as durações.
    
  * O autor acredita que essa queda de acertividade para período de treino igual a 13 meses é em função deste período limitar os testes aos últimos anos da base de dados, onde o período da **Pandemia por Covid** afetou gravemente os valores do comércio exterior. POr tanto, para a previsão de 2024, o maior período de teste possível será o escolhido.

  * Como testemunha dos testes multiplos podemos considerar a previsão feita apenas considerando o ano comum:
    *  ![image](https://github.com/AndreCoutinhoBueno/Importacao-por-SH-e-Municipio/assets/152298881/e1a2485d-4ff5-4f7b-a1fe-7efc743615e2)
    *  Podemo então notar que as multiplas variáveis adcionadas reduziram o erro máximo esperado de 28% para 22%.  



### Prevê Importação do Ano de 2024  

![image](https://github.com/AndreCoutinhoBueno/Importacao-por-SH-e-Municipio/assets/152298881/a6dc32a2-32c8-47cf-ad6e-2ae906a01091)  
Previsão de Importação de Fertilizantesda na Região de Pouso Alegre
no ano de 2024 é de 115 mil toneladas.

Expectativa média de erro é de -6%   
  
Expectativa de desvio padrão no erro 21%   


### Alternativa Extra  

Os cultivos de forma geral tem diferentes necessidades de água conforme o mês ou da estação do ano. Anos com o mesmo volume de chuva mas com diferentes distribuição ao longo do ano tem diferente aproveitação pelo cultivo. Considerando isso, uma nova previsão foi feita, porém desta vez mantendo a informação de chuva por mês. Para isso foram criads novas columnas na tabela, com uma coluna para cada chuva-mês combinação. Assim, para cada linha de ano, o algorítmo de predição podera diferencia-los pela inicidência  a cada meses.

A aprência da tabela ficou assim:

![image](https://github.com/AndreCoutinhoBueno/Importacao-por-SH-e-Municipio/assets/152298881/f2aa9901-3701-4cd6-bbd0-6843e4f38f96)


As métricas foram melhores do que as anteriores, neste trabalho:

Média de Erro Percentual: 0,8%  
Desvio Padrâo do erro Percentual: 20%  
Erro máximo esperado: 21%  
 

A técnica, de manter as informações mensais em novas colunas de uma tabela anual, será utilizada pelo autor nos próximos projetos.
