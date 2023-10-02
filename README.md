# desafio-ROX
Engenharia de Dados

O desafio ROX consiste em fazer a relação de 6 tabelas e extrair informações relavantes sobre produtos e vendas. 

Ingestão de dados: 
A cloud Escolhida foi a Google Cloud Plataform - GCP, onde foi feita a ingestão do dado, ou seja, o carregamento das tabelas que até então estavam local para a nuvem.
Utilizando o BigQuery, foi possível analisar tabela por tabela, para gerar os resultados esperados. 

Etapa 1: Visto que os dados estavam armazenados localmente, precisou-se então fazer o carregamento deles. Como mostra a figura a seguir:
![image](https://github.com/eldertaveira/desafio-ROX/assets/142034363/4c4b7bb1-4aab-4eb8-9f40-943ed4c226f5)

Etapa 2: A figura abaixo mostra que a segunda etapa já recebe o primeiro tratamento de dado, que é mudar o nome das tabelas para facilitar a análise. Pois o GCP não aceita "." em nome de tabela:

![image](https://github.com/eldertaveira/desafio-ROX/assets/142034363/8acd1144-6641-4c4d-a6f5-7a7d1401c104)

Etapa 3: Após detalhar que o esquema seja detectado automaticamente, e que os dados estão personalizados e separados por ";". O arquivo desafio_ROX foi salvo publicamente para o acesso do script SQL que faz a análise das tabelas. Mostrado na figura a seguir:

![image](https://github.com/eldertaveira/desafio-ROX/assets/142034363/c6f656bd-288a-4f4c-88d1-8b9426a360b8)


Link do scrip do desafio disponível em:
https://console.cloud.google.com/bigquery?sq=750614208518:2bd505e445904be1a2ea99b9b3e421da
