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

# Visualização dos dados
Para a visualização dos dados, optei em criar uma entidade com os dados, ou seja, relacionei através de joins as tabelas do desafio e selecionei as colunas mais relevantes para extrair um arquivo e conectar com o Power BI. Essa mesma tarefa poderia ser feita automazida, fazendo o carregamento direto do arquivo na cloud com o PBI.

O código implementado para criar a entidade está no BigQuery, ao final dos desafios: https://console.cloud.google.com/bigquery?sq=750614208518:2bd505e445904be1a2ea99b9b3e421da


# Atividade extra - PySpark
Sabendo que que o Apache Spark é um poderoso framework de processamento de dados e, como atuo diariamente com PySpark, descidi gerar os mesmos scripts gerados no BigQuery na linguagem PySpark. Essa linguagem oferece eficiência de escrita de ETL de forma simples, como mostrado a seguir:

Desafio 1: 
```
from pyspark.sql import SparkSession

# Criando uma secao SparkSession
spark = SparkSession.builder.appName("DesafioROXSpark").getOrCreate()

# Caminho para o arquivo CSV ou diretório que contém os dados 
caminho_do_arquivo = "/caminho/do/arquivo/sales_order_detail.csv"
resultado = (
    df.groupBy("SalesOrderID")
    .agg(count("*").alias("NumeroDeLinhas"))
    .filter("NumeroDeLinhas >= 3")
)
resultado.show()
```
Desafio 2: 
```
# Primeira etapa, juntar os dfs
joined_df = (
    sales_order_detail
    .join(special_offer_product, sales_order_detail["SpecialOfferID"] == special_offer_product["SpecialOfferID"])
    .join(product, special_offer_product["ProductID"] == product["ProductID"])
)

resultado = (
    joined_df
    .groupBy("DaysToManufacture", "Name")
    .agg(sum("OrderQty").alias("TotalVendido"))
    .orderBy(desc("TotalVendido"))
    .limit(3)
)
resultado.show()
```
Desafio 3: 
```
joined_df = (
    sales_order_detail
    .join(special_offer_product, sales_order_detail["SpecialOfferID"] == special_offer_product["SpecialOfferID"])
    .join(product, special_offer_product["ProductID"] == product["ProductID"])
)

resultado = (
    joined_df
    .groupBy("DaysToManufacture", "Name")
    .agg(sum("OrderQty").alias("TotalVendido"))
    .orderBy(desc("TotalVendido"))
    .limit(3)
)

resultado.show()
```
Desafio 4:
```
joined_df = (
    sales_order_detail
    .join(sales_order_header, sales_order_detail["SalesOrderID"] == sales_order_header["SalesOrderID"])
    .join(product, sales_order_detail["ProductID"] == product["ProductID"])
)
resultado = (
    joined_df
    .groupBy("ProductID", "OrderDate")
    .agg(sum("OrderQty").alias("SomaTotalProdutos"))
)

resultado.show()
```
Desafio 5: 
```
resultado = (
    sales_order_header
    .filter((year("OrderDate") == 2011) & (month("OrderDate") == 9) & (col("TotalDue") > 1000))
    .select("SalesOrderID", "OrderDate", "TotalDue")
    .orderBy("TotalDue", ascending=False)
)
resultado.show()
```
