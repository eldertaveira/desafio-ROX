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


Finalmente, será apresentado os códigos do desafio, em SQL, desenvolvidos no BigQuery. Como prévia, segue o script do primeiro desafio:
Para essa questão, foi selecionado a coluna SalesOrderDetailID, depois feito um count() através da tabela sales_order_detail onde seja mostrado IDs que tenham contagem de pelo menos 3 IDs.
```
/*Selecionando os SalesOrder que tenham 3 linhas de detalhes,
ou seja, apareça mais de uma vez.*/ 
SELECT DISTINCT 
SalesOrderDetailID
, COUNT(*)
FROM strange-terra-384711.desafio_ROX.sales_order_detail
GROUP BY 1
HAVING COUNT(*) >= 3;
```

Link do script do desafio disponível em:
https://console.cloud.google.com/bigquery?sq=750614208518:2bd505e445904be1a2ea99b9b3e421da

# Visualização dos dados
Para a visualização dos dados, optei em criar uma entidade com os dados, ou seja, relacionei através de joins as tabelas do desafio e selecionei as colunas mais relevantes para extrair um arquivo e conectar com o Power BI. Essa mesma tarefa poderia ser feita automazida, fazendo o carregamento direto do arquivo na cloud com o PBI.

O código implementado para criar a entidade está no BigQuery, é mostrado logo a seguir, e também consta no final do desafio: https://console.cloud.google.com/bigquery?sq=750614208518:2bd505e445904be1a2ea99b9b3e421da . 
```
SELECT 
sod.SalesOrderID
, sod.SalesOrderDetailID
, sod.OrderQty
, sod.ProductID
, sod.UnitPrice
, sod.ModifiedDate
, p.Name
, p.Color
, p.SafetyStockLevel
, p.ListPrice
, p.DaysToManufacture
, p.Class
, p.SellStartDate
, p.SellEndDate
, soh.OrderDate
, soh.CustomerID
, pp.PersonType
, pp.FirstName
, pp.LastName
, c.TerritoryID
FROM strange-terra-384711.desafio_ROX.sales_order_detail sod
LEFT JOIN strange-terra-384711.desafio_ROX.special_offer_product sop 
ON sod.SpecialOfferID = sop.SpecialOfferID
LEFT JOIN strange-terra-384711.desafio_ROX.product p 
ON sop.ProductID = p.ProductID
LEFT JOIN strange-terra-384711.desafio_ROX.sales_order_header soh 
ON sod.SalesOrderID = soh.SalesOrderID
LEFT JOIN strange-terra-384711.desafio_ROX.person_person pp 
ON soh.CustomerID = pp.BusinessEntityID
LEFT JOIN strange-terra-384711.desafio_ROX.sales_customer c 
ON pp.BusinessEntityID = c.CustomerID
WHERE sod.ModifiedDate >= TIMESTAMP("2011-01-01 00:00:00") AND sod.ModifiedDate < TIMESTAMP("2012-01-01 00:00:00");
```
1. Power Bi: Nessa ferramenta de visualização após a seleção das colunas pelo script acima, ficou fácil fazer o carregamento para a criação do dashboard. Portanto, a imagem a seguir mostra a criação básica de um dashboard com os dados selecionados. Recursos mais avançados não foram implementados, como ferramenta de dica e criação de ua lista de detalhamento, mas saliento que isso são técnicas as quais tenho conhecimento e que poderão ser implementadas em demais projetos. Outros recursos como o tratamento do dado no PowerQuery podem ser utilizados junto com o tratamend do dado em Python. O PowerBi é uma ferramenta poderosa.

![image](https://github.com/eldertaveira/desafio-ROX/assets/142034363/f95a710d-3347-460e-88ec-10f6e6cc8399)

Visualização final de um modelo de dashboard para o desafio.

![image](https://github.com/eldertaveira/desafio-ROX/assets/142034363/2040b841-d6ca-4568-8aa1-77b71d9237df)

   
2. Looker Studio: Para complemento extra do desafio, fiz a integração da entidade gerada para com o Looker Studio, a plataforma de visualização integrada no GCP. A Imagem a seguir mostra a criação de alguns gráficos *apenas para ilustração da integração com a entidade criada*, como uma possibilidade de ingestáo do dados desde do BigQuery até a visualização.

![image](https://github.com/eldertaveira/desafio-ROX/assets/142034363/b197d828-4afa-497c-8b02-5071177eead2)

Portanto, a o desafio final proposto foi desenvolvido usando GCP como ingestão dos dados, desde coleta, análise e visualização dos dados. Como mostra a figura a seguir:

![image](https://github.com/eldertaveira/desafio-ROX/assets/142034363/ea39a748-faf6-454a-9ea2-676729b56f96)

Estruturas mais complexas também podem ser implementadas, assim como ferramentas para o gerenciamento e a criação de pipelines. Por exemplo, Apache Airflow pode ser uma excelente ferramenta de pipelines para executar tarefas e rotinas em projetos. Destaco também a utilização de outras linguagem para leitura, tratamento e carregamento dos dados como PySpark. 

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
