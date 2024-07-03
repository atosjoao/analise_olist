# Análise Descritiva SQL - Olist

Arquivos e mídia: dataset-cover.png
Tags: Análise Descritiva, E-Commerce, Métricas de Pirata
Tecnologias: SQL
Natureza: Projeto Pessoal

# Case: Avaliação de Performance dos Três Pilares de Negócios da Empresa

---

<aside>
💡 **Contexto**
A empresa de e-commerce Olist, está buscando melhorar sua performance e entender melhor seus processos de negócios. Para isso, foi solicitada uma análise descritiva sobre o banco de dados disponível, com foco nos três pilares da empresa: Vendas, Produto e Comprador. Esta análise será crucial para identificar pontos fortes e áreas de melhoria, possibilitando a tomada de decisões mais informadas.

</aside>

> **Sobre a Organização:**
> 
> 
> A Olist, a maior loja de departamentos do mercado brasileiro. Ela conecta pequenas empresas de todo o Brasil a canais sem complicações e com um único contrato. Esses comerciantes podem vender seus produtos através da Loja Olist e enviá-los diretamente aos clientes usando parceiros de logística da empresa. 
> 
> Depois que um cliente compra o produto na Olist Store, um vendedor é notificado para atender a esse pedido. Uma vez que o cliente recebe o produto, ou a data de entrega estimada é devida, o cliente recebe uma pesquisa de satisfação por e-mail, onde ele pode dar uma nota para a experiência de compra e escrever alguns comentários.
> 
> Veja mais no site: [www.olist.com](http://www.olist.com/)
> 

# 1. Objetivo

---

Avaliar a performance dos pilares de Vendas, Produto e Comprador, através de uma análise descritiva dos dados disponíveis, utilizando a estatística descritiva e conhecimento de negócios para fornecer insights acionáveis para aprimoramento das estratégias da empresa.

# 2. Metodologia

---

A metodologia adotada será a utilização de técnicas da estatística descritiva para sumarização dos dados, a partir das métricas de Pirata (AARRR). A Métrica de Pirata é um modelo, bastante conhecido, que busca avaliar o desempenho do modelo de negócio a partir da análise do funil de clientes. Essa abordagem foca em 5 categorias:

1. Aquisição: Atrair Clientes
2. Ativação: Engajá-los
3. Retenção: Torná-los usuários mais ativos
4. Receita: Gerar Receita por essas atividades
5. Recomendação: Torná-los ferramentas na aquisição de novos clientes

Essa abordagem é muito importante, principalmente para um contexto de e-commerce, pois não torna o cliente um sujeito unidimensional, mas sim, trás aspectos relevantes sobre eles, não apenas sobre o quanto de receita eles geram, mas também como estão dispostos a usar o produto ou serviço ou se já estão prontos para o primeiro contato, etc. 

Para o cenário dessa análise, serão aplicáveis as métricas de Retenção, Receita e Recomendação.

# 3. Entregáveis

---

Relatório detalhado com a análise descritiva dos três pilares.

# 4. Ferramentas

---

1. SQL para extração e manipulação dos dados
2. Métricas de Pirata (AARRR) para metodologia de análise
3. Microsoft Excel para geração de gráficos

# 5. Base de Dados

---

A Olist disponibilizou uma bases de dados referente a dados de venda, clientes, vendedores e pedidos, retirados de setembro de 2016 a outubro de 2018. Esse período foi dividido em 8 tabelas, cada uma com 4 ou mais colunas. Essas colunas agregam informações pertinentes sobre os mais variados aspectos das suas operações. O período de analise vai de, setembro de 2016 a outubro de 2018, com os dados geográficos englobando todo o território brasileiro. As tabelas e suas respectivas colunas são:

## **5.1 Tabelas e Colunas:**

- **Tabela Customer - Cliente**
    1. Customer ID - Código gerado a cada transação
    2. Customer Unique ID - Código gerado para cada cliente de maneira única
    3. Customer ZipCode prefix - CEP do Cliente
    4. Customer City - Cidade do Cliente
    5. Customer State - Estado do Cliente
- **Tabela Geolocation - Geolocalização**
    1. Geolocation ZipCode prefix - CEP da Entrega
    2. Geolocation Lat - Latitude da Entrega
    3. Geolocation Lon - Longitude da Entrega
    4. Geolocation City - Cidade da Entrega
    5. Geolocation State - Estado da Entrega
- **Tabela Order Items - Itens dos pedidos**
    1. Order ID - Identificação do Pedido
    2. Order Item ID - Identificação do Item do Pedido
    3. Product ID - Identificação do Produto
    4. Seller ID - Identificação do Vendedor
    5. Shipping Limit Date - Data Limite de Envio
    6. Price - Preço do Produto
    7. Freight Value - Preço de Frete
- **Tabela Order Payments - Pagamentos dos Pedidos**
    1. Order ID - Identificação do Pedido
    2. Payment Sequential - Sequência do pagamento
    3. Payment Type - Tipo de pagamento
    4. Payment Installments - Parcelas do Pagamento
    5. Payment Value - Valor do Pagamento
- **Tabela Order Reviews - Avaliações dos Pedidos**
    1. Review ID - Identificação da Avaliação
    2. Order ID - Identificação do Pedido
    3. Review Comment Title - Título do comentário da avaliação
    4. Review Comment message - Mensagem do comentário da avaliação
    5. Review Creation date - Data de criação da avaliação
    6. Review Answer timestamp - Horário da resposta da avaliação
- **Tabela Orders - Pedidos**
    1. Order ID - Identificação do Pedido
    2. Customer ID - Identificação do Cliente
    3. Order Status - Status do Pedido
    4. Order Purchase Timestamp - Data da compra
    5. Order Approved At - Data da Aprovação da Compra
    6. Order Delivered Carrier Date - Data do Recebimento do pedido pela transportadora 
    7. Order Delivered Customer Date - Data do Recebimento do Pedido
    8. Order Estimated Delivery Date - Data estimada de entrega do pedido ao cliente
- **Tabela Products - Produtos**
    1. Product ID - Identificação do Produto
    2. Product Category Name - Categoria do Produto
    3. Product Name Lenght - Tamanho do nome do Produto
    4. Product Description Lenght - Tamanho da descrição do produto
    5. Product Photos Qty - Quantidade de Fotos do Produto
    6. Product Lenght (cm) - Comprimento do Produto
    7. Product Height (cm) - Altura do Produto
    8. Product Width (cm) - Largura do Produto
- **Tabela Sellers - Vendedores**
    1. Seller ID - Identificação do Vendedor
    2. Seller ZipCode - CEP do Vendedor
    3. Seller City - Cidade do Vendedor
    4. Seller State - Estado do Vendedor

## Data Schema

![HRhd2Y0.png](HRhd2Y0.png)

# 6. Perguntas de Negócios

---

## **6.1 Análise de Vendas:**

- Qual é o volume de vendas ao longo do tempo?
- Quais são as sazonalidades e tendências nas vendas?
- Qual é o desempenho das diferentes categorias de produtos?
- Qual é o ticket médio das vendas?
- Quais as 10 cidades/estados representam as maiores contribuições nas vendas?
- Quais as 10 cidades/estados  possuem as maiores médias de compras mensais?
- Qual o desempenho dos vendedores?
- Qual a taxa de contribuição cada meio de pagamento?

## **6.2 Análise de Produto e Categoria:**

- Quais os preços que mais venderam?
- Qual é a variedade dos produtos oferecidos?
- Qual categoria com a maior média de ítens por pedidos?
- Quais são as 10 categorias com a maior e menor média de preço?
- Quais são os produtos com maior e menor volume de vendas?
- Qual a distribuição geográfica das categorias?
- Qual o valor médio do frete de cada categoria?
- Qual a categoria com a maior taxa de parcelamentos?
- Qual a performance de avaliação das categorias?
- Quais categorias possuem o maior Shipping Time?

## **6.3 Análise do Cliente:**

- Qual é distribuição geográfica dos clientes?
- Qual o ranking dos 10 clientes mais compraram? (em valor e quantidade)
- Qual meio de pagamento é o preferido?
- Qual é o valor médio gasto por comprador?
- Quais são os segmentos de clientes mais lucrativos?

# 7. Respondendo as Perguntas

---

## **7.1 Análise de Vendas:**

- Qual é o volume de vendas ao longo do tempo?

```sql
-- TOTAL DE VENDAS POR ANO
SELECT
	strftime('%Y', o.order_purchase_timestamp) AS 'PERÍODO',
	SUM(op.payment_value) 
FROM order_payments op 
	INNER JOIN orders o ON op.order_id = o.order_id
GROUP BY strftime('%Y', o.order_purchase_timestamp)

*--* Output
*--* 2016	| R$ 59.362,34
*--* 2017	| R$ 7.249.746,72
*--* 2018	| R$ 8.699.763,04
-- TOTAL | R$ 16.008.872,11

*--* TOTAL DE VENDAS POR MÊS
**SELECT
	strftime('%Y-%m', o.order_purchase_timestamp) AS 'PERÍODO',
	SUM(oi.price) AS 'TOTAL DE VENDAS'
FROM orders o INNER JOIN order_items oi ON o.order_id = oi.order_id 
GROUP BY strftime('%Y-%m', o.order_purchase_timestamp)

*-- Output*
-- 2016-09: R$ 267,36
-- 2016-10: R$ 49.507,66
-- 2016-12: R$ 10,9
-- 2017-01: R$ 120.312,86
-- 2017-02: R$ 247.303,01
-- 2017-03: R$ 374.344,30
-- 2017-04: R$ 359.927,23
-- 2017-05: R$ 506.071,14
-- 2017-06: R$ 433.038,60
-- 2017-07: R$ 498.031,48
-- 2017-08: R$ 573.971,68
-- 2017-09: R$ 624.401,69
-- 2017-10: R$ 664.219,43
-- 2017-11: R$ 1.010.271,37
-- 2017-12: R$ 743.914,17
-- 2018-01: R$ 950.030,36
-- 2018-02: R$ 844.178,71
-- 2018-03: R$ 983.213,44
-- 2018-04: R$ 996.647,75
-- 2018-05: R$ 996.517,68
-- 2018-06: R$ 865.124,31
-- 2018-07: R$ 895.507,22
-- 2018-08: R$ 854.686,33
-- 2018-09: R$ 145,0
-- 2018-10: 0
```

- Quais são as sazonalidades e tendências nas vendas?

```sql
-- Qual o mês com o maior valor em vendas de 2016 a 2018?
SELECT
	strftime('%m', o.order_purchase_timestamp) AS 'MÊS',
	SUM(op.payment_value) AS 'TOTAL DE VENDAS'
FROM order_payments op INNER JOIN orders o ON op.order_id = o.order_id
GROUP BY strftime('%m', o.order_purchase_timestamp)
ORDER BY SUM(op.payment_value) DESC;

-- *Output*
--Maio:	1.746.900,96
--Agosto:	R$ 1.696.821,64
--Julho: R$ 1.658.923,67
--Março: R$ 1.609.515,71
--Abril: R$ 1.578.573,50
--Junho: R$ 1.535.156,88
--Fevereiro: R$ 1.284.371,35
--Janeiro: R$ 1.253.492,22
--Novembro: R$ 1.194.882,80
--Dezembro: R$ 878.421,10
--Outubro: R$ 839.358,02
--Setembro: R$ 732.454,23

**-- Qual o mês com o maior quantidade de vendas de 2016 a 2018?
SELECT
	strftime('%m', o.order_purchase_timestamp) AS 'MÊS',
	COUNT(o.order_id) AS 'QTD. VENDAS'
FROM orders o LEFT JOIN order_items oi ON o.order_id = oi.order_id 
GROUP BY strftime('%m', o.order_purchase_timestamp)
ORDER BY COUNT(o.order_id) DESC;

-- Output
-- Agosto: 10.843
-- Maio: 10.573
-- Julho: 10.318
-- Março: 9.893
-- Junho: 9.412
-- Abril: 9.343
-- Fevereiro: 8.508
-- Janeiro: 8.069
-- Novembro: 7.544
-- Dezembro: 5.674
-- Outubro: 4.959
-- Setembro: 4.305
```

- Qual é o desempenho das diferentes categorias de produtos?

```sql
-- Medindo a quantidade de vendas
SELECT 
	p.product_category_name AS 'NOME DA CATEGORIA', 
	COUNT(*) AS 'QTD. VENDAS'
FROM order_items oi 
	INNER JOIN products p ON oi.product_id = p.product_id
GROUP BY p.product_category_name
ORDER BY COUNT(*) DESC
LIMIT 10;

-- *Output*

-- cama_mesa_banho: 11.115
-- beleza_saude: 9.670
-- esporte_lazer: 8.641
-- moveis_decoracao: 8.334
-- informatica_acessorios: 7.827
-- utilidades_domesticas: 6.964
-- relogios_presentes: 5.991
-- telefonia: 4.545
-- ferramentas_jardim: 4.347
-- automotivo: 4.235
```

- Qual é o ticket médio das vendas?

```sql
-- O ticket médio é obtido através da soma do total vendido, 
-- divido pelo total de clientes
SELECT
	SUM(op.payment_value) / COUNT(DISTINCT c.customer_unique_id) AS 'TICKET MÉDIO'
FROM customer c INNER JOIN orders o ON (c.customer_id = o.customer_id )
INNER JOIN order_payments op ON o.order_id = op.order_id

-- *Output
-- R$ 166,59*
```

- Quais as 10 cidades/estados representam as maiores contribuições nas vendas?

```sql
-- Cidades
SELECT
	c.customer_city AS 'CIDADE',
	CAST(100. * COUNT(c.customer_city) / SUM(COUNT(op.payment_value)) OVER () AS DECIMAL(10,2)) AS 'PERCENTUAL'
FROM customer c INNER JOIN orders o ON (c.customer_id = o.customer_id )
INNER JOIN order_payments op ON o.order_id = op.order_id 
GROUP BY c.customer_city
ORDER BY CAST(100. * COUNT(c.customer_city) / SUM(COUNT(op.payment_value)) OVER () AS DECIMAL(10,2)) DESC
LIMIT 10;

-- *Output*
-- sao paulo: 15.61% 
-- rio de janeiro: 6.93%
-- belo horizonte	2.76%
-- brasilia	2.11%
-- curitiba	1.51%
-- campinas	1.45%
-- porto alegre	1.36%
-- salvador	1.29%
-- guarulhos	1.20%
-- sao bernardo do campo 0.94%

*-- Estados*
SELECT
	c.customer_state AS 'ESTADO',
	CAST(100. * COUNT(c.customer_state) / SUM(COUNT(op.payment_value)) OVER () AS DECIMAL(10,2)) AS 'PERCENTUAL'
FROM customer c INNER JOIN orders o ON (c.customer_id = o.customer_id )
INNER JOIN order_payments op ON o.order_id = op.order_id 
GROUP BY c.customer_state 
ORDER BY CAST(100. * COUNT(c.customer_state) / SUM(COUNT(op.payment_value)) OVER () AS DECIMAL(10,2)) DESC
LIMIT 10;

-- *Output*
-- SP	41.99%
-- RJ	13.02%
-- MG	11.64%
-- RS	5.45%
-- PR	5.06%
-- SC	3.61%
-- BA	3.47%
-- DF	2.12%
-- GO	2.03%
-- ES	2.02%
```

- Quais as 10 cidades/estados  possuem as maiores médias de compras mensais?

```sql
*--* ESTADOS COM A MAIOR MÉDIA DE COMPRAS MENSAIS
**SELECT
	c.customer_state AS 'ESTADO',
	COUNT(o.order_id) / COUNT(DISTINCT strftime('%Y-%m', o.order_purchase_timestamp)) AS 'MÉDIA VENDAS', 
	CAST(100. * COUNT(*) / SUM(COUNT(*)) OVER () AS DECIMAL(10,2)) AS 'PERCENTUAL'
FROM customer c 
	INNER JOIN orders o ON c.customer_id = o.customer_id 
GROUP BY c.customer_state
ORDER BY COUNT(o.order_id) / COUNT(DISTINCT strftime('%Y-%m', o.order_purchase_timestamp)) DESC
LIMIT 10;

-- Output

-- ESTADO | MÉDIA MENSAL | PERCENTUAL
-- SP | 1.739 | 41.98%
-- RJ | 558 | 12.92%
-- MG | 528 | 11.70%
-- RS | 248 | 5.49%
-- PR | 229 | 5.07%
-- SC | 165 | 3.65%
-- BA | 160 | 3.39%
-- DF | 101 | 2.15%
-- ES | 96 | 2.04%
-- GO | 96 | 2.03%

-- CIDADE COM A MAIOR MÉDIA DE COMPRAS MENSAIS
SELECT
	c.customer_city AS 'CIDADE',
	COUNT(o.order_id) / COUNT(DISTINCT strftime('%Y-%m', o.order_purchase_timestamp)) AS 'MÉDIA VENDAS', 
	CAST(100. * COUNT(*) / SUM(COUNT(*)) OVER () AS DECIMAL(10,2)) AS 'PERCENTUAL'
FROM customer c 
	INNER JOIN orders o ON c.customer_id = o.customer_id 
GROUP BY c.customer_city
ORDER BY COUNT(o.order_id) / COUNT(DISTINCT strftime('%Y-%m', o.order_purchase_timestamp)) DESC
LIMIT 10;

-- Output

-- CIDADE | MÉDIA MENSAL | PERCENTUAL
-- sao paulo | 706 | 15.62%
-- rio de janeiro | 312 | 6.92%
-- belo horizonte | 126 | 2.78%
-- brasilia | 101 | 2.14%
-- curitiba | 69 | 1.52%
-- campinas | 68 | 1.45%
-- porto alegre | 65 | 1.38%
-- salvador | 62 | 1.25%
-- guarulhos | 54 | 1.19%
-- sao bernardo do campo | 44 | 0.94%
```

- Qual o desempenho dos vendedores?

```sql
-- Ranking dos 10 vendedores que tiveram o maior valor acumulado de vendas
SELECT 
	oi.seller_id,
	SUM(op.payment_value) AS 'TOTAL VENDAS'
FROM order_items oi INNER JOIN order_payments op ON oi.order_id = op.order_id 
GROUP BY oi.seller_id
ORDER BY SUM(op.payment_value) DESC
LIMIT 10;

-- *Output (por conta da LGPD, o nome dos vendedores foram codificados)*
-- NOME DO VENDEDEOR | VALOR TOTAL | MÉDIA POR VENDA
**-- 7c67e1448b00f6e969d365cea6b010ab: R$ 507.166,91 | R$ 349.28
-- 1025f0e2d44d7041d6cf58b6550e0bfa: R$ 308.222,04 | R$ 210.82
-- 4a3ca9315b744ce9f8e9374361493884: R$ 301.245,27 | R$ 141.23
-- 1f50f920176fa81dab994f9023523100: R$ 290.253,42 | R$ 144.54
-- 53243585a1d6dc2643021fd1853d8905: R$ 284.903,08 | R$ 651.95
-- da8622b14eb17ae2831f4ac5b9dab84a: R$ 272.219,31 | R$ 166.08
-- 4869f7a5dfa277a7dca6462dcf3b52b2: R$ 264.166,12 | R$ 222.73
-- 955fee9216a65b617aa5c0531780ce60: R$ 236.322,29 | R$ 154.66
-- fa1c13f2614d7b5c4749cbc52fecda94: R$ 206.513,22 | R$ 339.10
-- 7e93a43ef30c4f03f38b393420bc753a: R$ 185.134,21 | R$ 525.94

-- Ranking dos 10 vendedores que acumularam a maior quantidade de vendas
SELECT 
	oi.seller_id,
	COUNT(op.payment_value) AS 'QTD. VENDAS'
FROM order_items oi INNER JOIN order_payments op ON oi.order_id = op.order_id 
GROUP BY oi.seller_id
ORDER BY COUNT(op.payment_value) DESC
LIMIT 10;

-- *Output (por conta da LGPD, o nome dos vendedores foram codificados)*
-- *4a3ca9315b744ce9f8e9374361493884: 2.133*
-- *6560211a19b47992c3666cc44a7e94c0: 2.122*
-- *1f50f920176fa81dab994f9023523100: 2.008*
-- *cc419e0650a3c5ba77189a1882b7556a: 1.847*
-- *da8622b14eb17ae2831f4ac5b9dab84a: 1.639*
-- *955fee9216a65b617aa5c0531780ce60: 1.528*
-- *1025f0e2d44d7041d6cf58b6550e0bfa: 1.462*
-- *7c67e1448b00f6e969d365cea6b010ab: 1.452*
-- *7a67c85e85bb2ce8582c35f2203ad736: 1.240*
-- *ea8482cd71df3c1969d7b9473ff13abc: 1.239*

-- Ranking dos vendedores por estados maior valor acumulado de vendas)
SELECT
	s.seller_state,
	SUM(op.payment_value) AS 'TOTAL DE VENDAS'
FROM order_items oi INNER JOIN order_payments op ON oi.order_id = op.order_id INNER JOIN sellers s ON oi.seller_id = s.seller_id 
GROUP BY s.seller_state 
ORDER BY SUM(op.payment_value) DESC
LIMIT 10;

-- *Output

-- SP: R$ 13.369.880,60
-- PR: R$ 1.846.047,65
-- MG: R$ 1.564.757,79
-- RJ: R$ 1.098.242,22
-- SC: R$ 886.745,46
-- RS: R$ 560.236,38
-- BA: R$ 367.899,46
-- DF: R$ 137.784,97
-- PE: R$ 124.894,83
-- GO: R$ 112.183,09*

-- Ranking dos vendedores por estados (maior quantidade vendas)

**SELECT
	s.seller_state,
	COUNT(op.payment_value) AS 'QTD. DE VENDAS'
FROM order_items oi INNER JOIN order_payments op ON oi.order_id = op.order_id INNER JOIN sellers s ON oi.seller_id = s.seller_id 
GROUP BY s.seller_state 
ORDER BY COUNT(op.payment_value) DESC
LIMIT 10;

-- *Output*

-- SP: 83.854
-- MG: 9.260
-- PR: 9.014
-- RJ: 5.017
-- SC: 4.257
-- RS: 2.283
-- DF: 947
-- BA: 698
-- GO: 549
-- PE: 465
```

- Qual a taxa de contribuição cada meio de pagamento?

```sql
SELECT 
	op.payment_type,
	SUM(op.payment_value) AS 'VALOR TOTAL',
	AVG(op.payment_value) AS 'MÉDIA POR TIPO DE PAGAMENTO',
	CAST(100. * COUNT(op.payment_type) / SUM(COUNT(op.payment_value)) OVER () AS DECIMAL(10,2)) AS 'PERCENTUAL'
FROM order_payments op 
GROUP BY op.payment_type
ORDER BY SUM(op.payment_value) DESC 

-- *Output: 
-- TOTAL POR TIPO DE PAGAMENTO | MÉDIA POR TIPO DE PAGAMENTO | PERC.*

-- credit_card: R$ 12.542.084,18 | R$ 163.31 |73.92 %
-- boleto: R$ 2.869.361,26 | R$ 145.03 |19.04 %
-- voucher: R$ 379.436.87 | R$ 65.70 |5.55 %
-- debit_card: R$ 217.989,79 | R$ 142.57 |1.47 %

SELECT 
	op.payment_type,
	COUNT(op.payment_value) AS 'VALOR TOTAL',
	CAST(100. * COUNT(op.payment_type) / SUM(COUNT(op.payment_value)) OVER () AS DECIMAL(10,2)) AS 'PERCENTUAL'
FROM order_payments op 
GROUP BY op.payment_type
ORDER BY COUNT(op.payment_value) DESC 

-- *Output*
-- credit_card: 76.795 | 73.92 %
-- boleto: 19.784	| 19.04 %
-- voucher: 5.775	| 5.55 %
-- debit_card: 1.529	| 1.47 %
```

## **7.2 Análise de Produto e Categoria:**

- Quais os preços que mais venderam?

```sql
-- 10 PREÇOS QUE MAIS VENDEM
SELECT
	oi.price,
    COUNT(oi.order_id) AS 'QTD. DE VENDAS', 
	SUM(op.payment_value) AS 'VALOR TOTAL DE VENDAS'
FROM order_items oi INNER JOIN order_payments op ON oi.order_id = op.order_id 
GROUP BY oi.price 
ORDER BY COUNT(oi.order_id) DESC
LIMIT 10;

-- Output
-- PREÇO | QTD. VENDAS | VALOR TOTAL DE VENDAS
-- R$ 59.9 | 2.606 | R$ 289.998,92
-- R$ 69.9 | 2.092 | R$ 226.492,55
-- R$ 49.9 | 2.045 | R$ 195.248,68
-- R$ 89.9 | 1.623 | R$ 212.805,51
-- R$ 99.9 | 1.510 | R$ 216.420,86
-- R$ 39.9 | 1.395 | R$ 111.928,52
-- R$ 29.9 | 1.378 | R$ 87.673,24
-- R$ 19.9 | 1.279 | R$ 61.915,47
-- R$ 79.9 | 1.276 | R$ 163.626,55
-- R$ 29.99 | 1.221 | R$ 90.184,88
```

- Qual é a variedade dos produtos oferecidos?

```sql
-- Quantidade de categorias disponíveis
SELECT 
	COUNT(DISTINCT p.product_category_name) AS 'QTD. DA CATEGORIA'
FROM products p

-- *Output*
-- 73 Categorias
```

- Qual categoria com a maior média de ítens por pedidos?

```sql
SELECT
	p.product_category_name,
	AVG(oi.order_item_id) AS 'MÉDIA DE ÍTENS'
FROM order_items oi
	INNER JOIN products p ON oi.product_id = p.product_id 
GROUP BY oi.order_id
ORDER BY AVG(oi.order_item_id) DESC
LIMIT 10;

-- Output

-- CATEGORIA | MÉDIA DE ÍTENS POR PEDIDO
-- beleza_saude | 11.0
-- automotivo | 10.5
-- informatica_acessorios | 10.5
-- ferramentas_jardim | 8.0
-- moveis_decoracao | 8.0
-- telefonia | 7.5
-- ferramentas_jardim | 7.5
-- telefonia | 7.0
-- utilidades_domesticas | 6.5
-- bebes | 6.5
```

- Quais são as 10 categorias com a maior e menor média de preço?

```sql
-- AS 10 COM MAIOR MÉDIA DE PREÇO
SELECT
	  p.product_category_name,
    AVG(oi.price) AS 'MÉDIA DE PREÇO',
    COUNT(oi.order_id) AS 'QTD. DE VENDAS' 
FROM order_items oi INNER JOIN products p ON oi.product_id = p.product_id 
GROUP BY p.product_category_name
ORDER BY AVG(oi.price) DESC
LIMIT 10;

-- Output
-- CATEGORIA | MÉDIA DE PREÇO | QTD. DE VENDAS
-- pcs | R$ 1.098,34 | 203
-- portateis_casa_forno_e_cafe | R$ 624,28 | 76
-- eletrodomesticos_2 | R$ 476,12 | 238
-- agro_industria_e_comercio | R$ 342,12 | 212
-- instrumentos_musicais | R$ 281,61 | 680
-- eletroportateis | R$ 280,77 | 679
-- portateis_cozinha_e_preparadores_de_alimentos | R$ 264,56 | 15
-- telefonia_fixa | R$ 225,69 | 264
-- construcao_ferramentas_seguranca | R$ 208,99 | 194
-- relogios_presentes | R$201,13 | 5.991

-- AS 10 COM MENOR MÉDIA DE PREÇO
SELECT
	  p.product_category_name,
    AVG(oi.price) AS 'MÉDIA DE PREÇO',
    COUNT(oi.order_id) AS 'QTD. DE VENDAS' 
FROM order_items oi INNER JOIN products p ON oi.product_id = p.product_id 
GROUP BY p.product_category_name
ORDER BY AVG(oi.price) ASC
LIMIT 10;

-- *Output*

-- CATEGORIA | MÉDIA DE PREÇO | QTD. DE VENDAS
-- casa_conforto_2 | R$ 25,34 | 30
-- flores | R$ 33,63 | 33
-- fraldas_higiene | R$40,19 | 39
-- cds_dvds_musicais | R$ 52,14 | 14
-- alimentos_bebidas | R$ 54,60 | 278
-- artigos_de_natal | R$ 57,52 | 153
-- alimentos | R$ 57,63 | 510
-- eletronicos | R$ 57,91 | 2.767
-- fashion_roupa_feminina | R$ 58,40 | 48
-- bebidas | R$ 59,17 | 379

```

- Quais são os produtos com maior e menor volume de vendas?

```sql
-- OS 10 PRODUTOS MAIS VENDIDOS E SUAS CATEGORIAS
**SELECT 
	p.product_id, 
	p.product_category_name, 
	COUNT(oi.order_id) AS 'QTD. VENDAS',
	SUM(op.payment_value) AS 'VALOR TOTAL DE VENDAS'
FROM products p 
	INNER JOIN order_items oi ON p.product_id = oi.product_id 
	INNER JOIN order_payments op ON oi.order_id = op.order_id 
GROUP BY p.product_id 
ORDER BY COUNT(oi.order_id) DESC
LIMIT 10;

-- *Output (Por conta da sensibilidade dos dados, o nome do produto foi codificado)*

-- PRODUTO | CATEGORIA | QTD. VENDAS | VALOR ACUMULADO
*-- aca2eb7d00ea1a7b8ebd4e68314663af* | *moveis_decoracao* | *536* | R$ *63.788,12
-- 99a4788cb24856965c36a24e339b6058* | *cama_mesa_banho* | *525* | R$ *63.161.39
-- 422879e10f46682990de24d770e7f83d* | *ferramentas_jardim* | *505* | R$ *79.512,22
-- 389d119b48cf3043d311335e499d9c6b* | *ferramentas_jardim* | *406* | R$ *48.616,43
-- 368c6c730842d78016ad823897a372db* | *ferramentas_jardim* | *395* | R$ *52.508,62
-- 53759a2ecddad2bb87a079a1f1519f73* | *ferramentas_jardim* | *389* | R$ *53.445,34
-- d1c427060a0f73f6b889a5c7c61f2ac4* | *informatica_acessorios* | *357* | R$ *70.557,90
-- 53b36df67ebb7c41585e8d54d6772e08* | *relogios_presentes* | *327* | R$ *48.994,30
-- 154e7e31ebfa092203795c972e5804a6* | *beleza_saude* | *283* | R$ *11.826,06
-- 3dd2a17168ec895c781a9191c1e95ad7* | *informatica_acessorios* | *278* | R$ *58.962,13*

-- 10 CATEGORIAS COM O MAIOR VOLUME DE VENDAS MENSAIS
SELECT
	p.product_category_name,
	COUNT(o.order_id) / COUNT(DISTINCT strftime('%Y-%m', o.order_purchase_timestamp)) AS 'MÉDIA VENDAS', 
	CAST(100. * COUNT(*) / SUM(COUNT(*)) OVER () AS DECIMAL(10,2)) AS 'PERCENTUAL'
FROM products p 
	INNER JOIN order_items oi ON p.product_id = oi.product_id 
	INNER JOIN order_payments op ON oi.order_id = op.order_id
	INNER JOIN orders o ON oi.order_id = o.order_id 
GROUP BY p.product_category_name
ORDER BY COUNT(o.order_id) / COUNT(DISTINCT strftime('%Y-%m', o.order_purchase_timestamp)) DESC
LIMIT 10;

-- *Output 

--* CATEGORIA | MÉDIA DE VENDAS MENSAIS | PERCENTUAL DAS VENDAS MENSAIS
*--* cama_mesa_banho | 563 | 10.05%
*--* beleza_saude | 474 | 8.47%
*--* esporte_lazer | 425 | 7.60%
*--* moveis_decoracao | 397 | 7.43%
*--* informatica_acessorios | 384 | 6.87%
*--* utilidades_domesticas | 350 | 6.25%
*--* relogios_presentes | 295 | 5.27%
*--* ferramentas_jardim | 217 | 3.88%
*--* telefonia | 214 | 4.01%
*--* automotivo | 208 | 3.72%
```

- Qual a distribuição geográfica das categorias?

```sql
-- Categoria mais vendida pelas 10 principais cidades
SELECT
	c.customer_city AS 'CIDADE',
	p.product_category_name 'CATEGORIA',
	COUNT(oi.order_id) AS 'QTD. VENDAS',
	SUM(op.payment_value) AS 'VALOR TOTAL DE VENDAS',
	ROW_NUMBER() OVER(PARTITION BY c.customer_city ORDER BY COUNT(oi.order_id) DESC) AS RANKING
FROM products p 
	INNER JOIN order_items oi ON p.product_id = oi.product_id 
	INNER JOIN order_payments op ON oi.order_id = op.order_id
	INNER JOIN orders o ON oi.order_id = o.order_id
	INNER JOIN customer c ON o.customer_id = c.customer_id 
GROUP BY c.customer_city, p.product_category_name
ORDER BY COUNT(oi.order_id) DESC

-- *Output*

-- CIDADE | CATEGORIA | QTD. TOTAL DE VENDAS | VALOR TOTAL DE VENDAS
-- sao paulo | cama_mesa_banho| 2.146 | R$ 284.303,94
-- rio de janeiro | cama_mesa_banho | 910 | R$ 127.201,50
-- belo horizonte | cama_mesa_banho | 368 | R$ 51.212,78
-- brasilia | beleza_saude | 252 | R$ 38.685,01
-- campinas	cama_mesa_banho	| 193	| R$ 26.592,30
-- porto alegre	cama_mesa_banho	| 183	| R$ 26.136,38
-- curitiba	| moveis_decoracao	| 152	| R$ 23.031,00
-- salvador	| beleza_saude	| 151	| R$ 27.237,93
-- guarulhos	| cama_mesa_banho	| 141	| R$ 16.929,26
-- niteroi	| cama_mesa_banho	| 120	| R$ 12.277,83
```

- Qual o valor médio do frete de cada categoria?

```sql
-- 10 CATEGORIAS COM O MAIOR CUSTO MÉDIO DE FRETE
SELECT
	p.product_category_name AS 'CATEGORIA',
	AVG(oi.freight_value) AS 'MÉDIA DO VALOR DE FRETE',
	MAX(oi.freight_value) AS 'MÁXIMO VALOR DE FRETE'
FROM products p 
	INNER JOIN order_items oi ON p.product_id = oi.product_id 
GROUP BY p.product_category_name
ORDER BY AVG(oi.freight_value) DESC
LIMIT 10;

-- Output

-- CATEGORIA | CUSTO MÉDIO DE FRETE | VALOR MÁXIMO DE FRETE
-- pcs | R$ 48.45 | 193.21
-- eletrodomesticos_2 | R$ 44.53 | R$ 170.8
-- moveis_colchao_e_estofado | R$ 42.90 | R$ 82.7
-- moveis_cozinha_jantar_e_jardim | R$ 42.70 | R$ 251.39
-- moveis_quarto | R$ 42.49 | R$ 147.65
-- moveis_escritorio | R$ 40.55 | R$ 256.13
-- portateis_casa_forno_e_cafe | R$ 36.15 | R$ 115.63
-- moveis_sala | R$ 35.72 | R$ 237.11
-- sinalizacao_e_seguranca | R$ 32.70 | R$ 299.16
-- industria_comercio_e_negocios | R$ 29.42 | R$ 317.47

```

- Qual a categoria com a maior taxa de parcelamentos?

```sql
-- PERCENTUAL DE COMPRAS PARCELADAS POR CATEGORIA
SELECT
	p.product_category_name 'CATEGORIA',
	CAST(100. * COUNT(*) / SUM(COUNT(*)) OVER () AS DECIMAL(10,2)) AS 'PERCENTUAL',
	COUNT(oi.order_id) AS 'QTD. VENDAS'
FROM products p 
	INNER JOIN order_items oi ON p.product_id = oi.product_id 
	INNER JOIN order_payments op ON oi.order_id = op.order_id
WHERE op.payment_installments != 1
GROUP BY p.product_category_name
ORDER BY COUNT(op.payment_sequential) DESC
LIMIT 10;

-- Output

-- CATEGORIA | QTD DE VENDAS PARCELADAS
-- cama_mesa_banho | 7.133
-- beleza_saude | 5.539
-- moveis_decoracao | 4.503
-- relogios_presentes | 4.012
-- esporte_lazer | 3.961
-- utilidades_domesticas | 3.779
-- informatica_acessorios | 3.055
-- cool_stuff | 2.309
-- ferramentas_jardim | 2.218
-- brinquedos | 2.153
```

- Qual a performance de avaliação das categorias?

```sql
-- AS 10 CATEGORIAS COM A MAIORES PONTUAÇÕES MÉDIAS NAS AVALIAÇÕES
SELECT
	p.product_category_name 'CATEGORIA',
	AVG(or2.review_score) AS 'NOTA MÉDIA DE AVALIAÇÃO',
	COUNT(or2.review_score) AS 'CONTAGEM'
FROM products p 
	INNER JOIN order_items oi ON p.product_id = oi.product_id 
	INNER JOIN order_reviews or2 ON oi.order_id = or2.order_id
GROUP BY p.product_category_name
ORDER BY AVG(or2.review_score) DESC
LIMIT 10;

-- *Output

--* CATEGORIA | NOTA MÉDIA | QTD. AVALIAÇÕES
**-- cds_dvds_musicais | 4.64 | 14
-- fashion_roupa_infanto_juvenil | 4.5 | 8
-- livros_interesse_geral | 4.43 | 553
-- livros_importados | 4.4 | 60
-- construcao_ferramentas_ferramentas | 4.35 | 103
-- livros_tecnicos | 4.33 | 269
-- malas_acessorios | 4.30 | 1092
-- alimentos_bebidas | 4.30 | 280
-- portateis_casa_forno_e_cafe | 4.30 | 76
-- fashion_esporte | 4.258064516129032 | 31

-- AS 10 CATEGORIAS COM AS PIORES AVALIAÇÕES
SELECT
	p.product_category_name 'CATEGORIA',
	AVG(or2.review_score) AS 'NOTA MÉDIA DE AVALIAÇÃO'
FROM products p 
	INNER JOIN order_items oi ON p.product_id = oi.product_id 
	INNER JOIN order_reviews or2 ON oi.order_id = or2.order_id
GROUP BY p.product_category_name
ORDER BY AVG(or2.review_score) DESC
LIMIT 10;

-- Output

-- CATEGORIA | NOTA MÉDIA | QTD. AVALIAÇÕES
-- seguros_e_servicos | 2.5 | 2
-- fraldas_higiene | 3.25 | 39
-- portateis_cozinha | 3.26 | 15
-- pc_gamer | 3.33 | 9
-- casa_conforto_2 | 3.36 | 30
-- moveis_escritorio | 3.48 | 1701
-- fashion_roupa_masculina | 3.62 | 132
-- telefonia_fixa | 3.67 | 265
-- artigos_de_festas | 3.76 | 43
-- fashion_roupa_feminina | 3.78 | 50
```

- Quais categorias possuem o maior Shipping Time?

```sql
-- SHIPPING TIME SE REFERE AO TEMPO MÉDIO (EM DIAS) DA ENTREGA DOS PRODUTOS
SELECT
	p.product_category_name,
  AVG(CAST(julianday(o.order_estimated_delivery_date) - julianday(o.order_purchase_timestamp) AS INTEGER)) AS shipping_time_days
FROM orders o
	INNER JOIN order_items oi ON o.order_id = oi.order_id 
	INNER JOIN products p ON oi.product_id = p.product_id 
WHERE o.order_status = 'delivered'
GROUP BY p.product_category_name
ORDER BY shipping_time_days DESC
LIMIT 10;

-- Output

*--* CATEGORIA | SHIPPING TIME
-- moveis_escritorio | 31.6 DIAS
-- seguros_e_servicos | 31.0 DIAS
-- fashion_calcados | 29.2 DIAS
-- artigos_de_natal | 26.7 DIAS
-- cds_dvds_musicais | 26.4 DIAS
-- telefonia_fixa | 26.3 DIAS
-- market_place | 25.7 DIAS
-- moveis_quarto | 25.6 DIAS
-- climatizacao | 25.3 DIAS
-- eletrodomesticos_2 | 25.3 DIAS
```

## **7.3 Análise do Cliente:**

- Qual é distribuição geográfica dos clientes?

```sql
SELECT 
	c.customer_city AS 'CIDADE',
	COUNT(DISTINCT c.customer_unique_id) AS 'QTD. CLIENTES',
	CAST(100. * COUNT(*) / SUM(COUNT(*)) OVER () AS DECIMAL(10,2)) AS 'PERCENTUAL'
FROM customer c 
GROUP BY c.customer_city 
ORDER BY COUNT(DISTINCT c.customer_unique_id) DESC
LIMIT 10;

-- Output
-- CIDADE | QTD. CLIENTES | PERCENTUAL
-- sao paulo | 14.984 | 15.62%
-- rio de janeiro | 6.620 | 6.92%
-- belo horizonte | 2.672 | 2.78%
-- brasilia | 2.069 | 2.14%
-- curitiba | 1.465 | 1.52%
-- campinas | 1.398 | 1.45%
-- porto alegre | 1.326 | 1.38%
-- salvador | 1.209 | 1.25%
-- guarulhos | 1.153 | 1.19%
-- sao bernardo do campo | 908 | 0.94%

SELECT 
	c.customer_state,
	COUNT(DISTINCT c.customer_unique_id) AS 'QTD. CLIENTES',
	CAST(100. * COUNT(*) / SUM(COUNT(*)) OVER () AS DECIMAL(10,2)) AS 'PERCENTUAL'
FROM customer c 
GROUP BY c.customer_state 
ORDER BY COUNT(DISTINCT c.customer_unique_id) DESC
LIMIT 10;

-- Output
-- ESTADO | QTD. CLIENTES | PERCENTUAL
-- SP | 40.302 | 41.98%
-- RJ | 12.384 | 12.92%
-- MG | 11.259 | 11.70%
-- RS | 5.277 | 5.49%
-- PR | 4.882 | 5.07%
-- SC | 3.534 | 3.65%
-- BA | 3.277 | 3.39%
-- DF | 2.075 | 2.15%
-- ES | 1.964 | 2.04%
-- GO | 1.952 | 2.03%
```

- Qual o ranking dos 10 clientes mais compraram?

```sql
SELECT 
	c.customer_unique_id,
	c.customer_state AS 'ESTADO',
	c.customer_city AS 'CIDADE', 
	COUNT(oi.product_id) AS 'COUNT_PROD'
FROM customer c
	INNER JOIN orders o ON c.customer_id = o.customer_id
	INNER JOIN order_payments p ON o.order_id = p.order_id
	INNER JOIN order_items oi ON o.order_id = oi.order_id 
GROUP BY c.customer_unique_id 
ORDER BY COUNT(oi.product_id) DESC
LIMIT 10;

-- Output (por conta da LGPD o nome dos clientes foram codificados)

-- CLIENTE | ESTADO | CIDADE | QTD. PEDIDOS
-- 9a736b248f67d166d2fbb006bcb877c3 | SP | sao paulo | 75
-- 6fbc7cdadbb522125f4b27ae9dee4060 | RJ | rio de janeiro | 38
-- f9ae226291893fda10af7965268fb7f6 | SP | guarulhos | 35
-- 8af7ac63b2efbcbd88e5b11505e8098a | MT | cuiaba | 29
-- 569aa12b73b5f7edeaa6f2a01603e381 | SP | sao paulo | 26
-- db1af3fd6b23ac3873ef02619d548f9c | RS | itaqui | 24
-- c8460e4251689ba205045f3ea17884a1 | RS | porto alegre | 24
-- 85963fd37bfd387aa6d915d8a1065486 | SP | atibaia | 24
-- 5419a7c9b86a43d8140e2939cd2c2f7e | MT | sinop | 24
-- 2524dcec233c3766f2c2b22f69fd65f4 | SP | tupa | 22

```

- Qual meio de pagamento é o preferido?

```sql
SELECT 
	op.payment_type TIPO,
	COUNT (c.customer_id) CONTAGEM 
FROM customer c 
	INNER JOIN orders o ON c.customer_id = o.customer_id 
	INNER JOIN order_payments op ON o.order_id = op.order_id 
GROUP BY op.payment_type 
ORDER BY COUNT( c.customer_id) DESC

-- Output

-- TIPO DE PAGAMENTO | QTD. CLIENTES QUE USARAM
-- credit_card | 76.795
-- boleto | 19.784
-- voucher | 5.775
-- debit_card | 1.529
-- not_defined | 3
```

- Qual é o valor médio gasto por comprador?

```sql
-- OS 10 CLIENTES COM AS MAIORES MÉDIAS DE COMPRAS
SELECT 
	c.customer_unique_id,
	c.customer_state AS ESTADO, 
	COUNT(oi.product_id) AS count_compra,
	AVG(op.payment_value) AS avg_spent
FROM customer c
	INNER JOIN orders o ON c.customer_id = o.customer_id
	INNER JOIN order_payments op ON o.order_id = op.order_id
	INNER JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY c.customer_unique_id 
HAVING COUNT(oi.product_id) > 2
ORDER BY AVG(op.payment_value) DESC
LIMIT 10;

-- Output
-- CLIENTE | ESTADO | QUANTIDADE DE COMPRAS | VALOR MÉDIO DE COMPRA
-- 0a0a92112bd4c708ca5fde585afaa872 | RJ | 8 | R$ 13.664,08
-- 763c8b1c9c68a0229c42c9fc6f662b93 | ES | 4 | R$ 7.274,88
-- 4007669dec559734d6f53e029e360987 | MG | 6 | R$ 6.081,54
-- 931eabdf0636b8fd60369a8d759917d6 | SC | 3 | R$ 3.666,42
-- adfa1cab2b2c8706db21bb13c0a1beb1 | MT | 6 | R$ 3.242,84
-- 1b76903617af13189607a36b0469f6f3 | MA | 6 | R$ 3.195,73
-- be825ddd3b40db3f91bf05b4e9435d56 | BA | 4 | R$ 3.122,72
-- ef8d54b3797ea4db1d63f0ced6a906e9 | RJ | 10 | R$ 3.018,59
-- fff5eb4918b2bf4b2da476788d42051c | PB | 6 | R$ 2.844,95
-- 26b5839e505ee33e6a449f7c2c403f99 | SP | 3 | R$ 2.607,09
```

- Quais são os segmentos de clientes mais lucrativos?

```sql
-- ESTADOS COM MAIS VALOR ACUMULADO 
SELECT 
	c.customer_state, 
	SUM(p.payment_value) AS 'VALOR TOTAL'
FROM customer c 
	JOIN orders o ON c.customer_id = o.customer_id
	JOIN order_payments p ON o.order_id = p.order_id
GROUP BY c.customer_state
ORDER BY SUM(p.payment_value) DESC
LIMIT 10;

-- Output
-- ESTADO | VALOR TOTAL DE VENDAS
-- SP | 5.998.226,95
-- RJ | 2.144.379,68
-- MG | 1.872.257,26
-- RS | 890.898,53
-- PR | 811.156,37
-- SC | 623.086,42
-- BA | 616.645,82
-- DF | 355.141,79
-- GO | 350.092,31
-- ES | 325.967,54

-- RANKING DE QUANTIDADE MÉDIA DE COMPRAS E VALORES
SELECT
	c.customer_state,
	COUNT(oi.product_id) / COUNT(DISTINCT strftime('%Y-%m', o.order_purchase_timestamp)) AS AVG_COMPRA, 
	AVG(op.payment_value) AS avg_spent
FROM customer c
	INNER JOIN orders o ON c.customer_id = o.customer_id
	INNER JOIN order_payments op ON o.order_id = op.order_id
	INNER JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY c.customer_state
ORDER BY AVG_COMPRA DESC
LIMIT 10;
-- ESTADO | MÉDIA DE COMPRAS POR MÊS | VALOR MÉDIO DE COMPRAS POR MÊS 
-- SP | 2.253 | R$ 153,27
-- RJ | 729 | R$ 180,68
-- MG | 649 | R$ 170,56
-- RS | 294 | R$ 176,88
-- PR | 271 | R$ 178,56
-- SC | 204 | R$ 182,78
-- BA | 192 | R$ 196,98
-- DF | 117 | R$ 174,93
-- GO | 115 | R$ 211,47
-- ES | 111 | R$ 173,56
```

# 8. Relatório

---

## Contexto Histórico

> O período de 2016 a 2018 foi bastante importante para o e-commerce brasileiro. Desde 2014, há um cenário otimista sobre esse setor, iniciando 2016 com o número de e-consumidores ativos 22% maior na comparação com 2015, de 39,14 milhões para 47,93 milhões, segundo dados do G1 e esse número chegaria a incríveis 73 Milhões em 2018, o que traria uma conclusão de que ainda havia muito o que explorar nesse segmento.
> 

## 1. Vendas

Para a Olist, as vendas no período resultaram em um valor acumulado de R$ 16.008.872,11. Esse valor, foi distribuído em 73 categorias, com 3.095 vendedores e 96.096 clientes espalhados pelo território brasileiro. Na série histórica, a empresa vem em uma crescente, seja no número de vendas, quanto no valor acumulado destas. 

Apesar do último quadrimestre do ano de 2016 terminar com um total acumulado de R$ 59.362,34, com mais de 90% desse valor se concentrando no mês de outubro que, sozinho, foi responsável por R$ 49.507,66, o ano de 2017 tornou o cenário mais otimista. O ano fechou com R$ 7.249.746,72, uma considerável melhora em relação ao ano anterior, principalmente se compararmos com o último quadrimestre de 2016, totalizando R$ 3.042.806,66, uma valorização de mais de 5.000%.

### 2017

| Mês | Valor |
| --- | --- |
| Setembro | R$ 624.401,69 |
| Outubro | R$ 664.219,43 |
| Novembro | R$ 1.010.271,37 |
| Dezembro | R$ 743.914,17 |

### 2016

| Mês | Valor |
| --- | --- |
| Setembro | R$ 267,36 |
| Outubro | R$ 49.507,66 |
| Novembro | Sem Registro |
| Dezembro | R$ 10,90 |

Esses resultados estão longe de serem outliers, pois no ano de 2018 o resultado foi ainda melhor, atingindo a marca de R$ 8.699.763,04, sendo este o melhor resultado acumulado da série histórica. Vale a pena ressaltar, que na base de dados não estão presentes os resultados do último trimestre de 2018, o que pode ser um indício de resultados ainda mais expressivos.

Em uma perspectiva sazonal, as vendas costumam acumular-se no período entre o 2º e 3º trimestres, compreendendo de março a agosto o intervalo de maior atividade, sendo o mês de Maio como campeão em valor arrecadado, com R$ 1.746.900,96 ao longo dos anos de 2017 e 2018 e Agosto como o mês em que houve a maior quantidade de vendas, com 10.843 transações.

![Vendas acumuladas em cada mês do período. Fonte: Própria](chart_(1).png)

Vendas acumuladas em cada mês do período. Fonte: Própria

Essas vendas, estão distribuídas em 73 categorias de produtos, sendo: Cama Mesa Banho, Beleza e Saúde, Esporte e Lazer, Móveis e Decoração e Acessórios de Informática as 5 categorias que mais venderam no período, com um ticket médio, ou seja, o valor médio que cada cliente transaciona é de aproximadamente R$ 166,59 e definitivamente, o cartão de crédito é o meio de pagamento mais utilizado.

| Categoria | Quantidade de Vendas | Valor Total |
| --- | --- | --- |
| Cama Mesa Banho | 11.115 | R$ 1.712.553,67 |
| Beleza e Saúde | 9.670 | R$ 1.657.373,12 |
| Esporte e Lazer | 8.641 | R$ 1.392.127,56 |
| Móveis e Decoração | 8.334 | R$ 1.430.176,39 |
| Informática e Acessórios  | 7.827 | R$ 1.585.330,44 |
| Utilidades Domésticas  | 6.964 | R$ 1.094.758,12 |
| Relógios | 5.991 | R$ 1.429.216,68 |
| Telefonia | 4.545 | R$ 486.882,05 |
| Ferramentas de Jardim | 4.347 | R$ 838.280,75 |
| Automotivo | 4.235 | R$ 852.294,32 |

Apenas por esse meio de pagamento, foram realizadas 76.795 transações, o que corresponde a 73,92%, totalizando um valor acumulado de R$ 12.542.084,18, gerando em torno de R$ 163,31 em média por mês. Se compararmos com o cartão de débito, que é uma opção nativa da maioria dos cartões, percebe-se um verdadeiro abismo, com o cartão de débito somando apenas 1.529 operações, acumulando apenas R$ 217.989,79, representando 1,47%, ficando atrás até do voucher. 

![Vendas efetuadas por cada meio de pagamento. Fonte: Própria](chart_(2).png)

Vendas efetuadas por cada meio de pagamento. Fonte: Própria

Geograficamente, a distribuição destes resultados é bastante desigual entre as regiões brasileiras. 68,67% das atividades estão concentradas na região sudeste, sendo São Paulo, Rio de Janeiro e Minas Gerais os 3 que mais acumularam vendas no período. Em segundo lugar está a região sul, apresentando 14,12% de contribuição, com seus 3 estados presentes entre os 10 que contribuíram percentualmente, com Rio Grande do Sul ocupando o 4º lugar neste ranking. Em Terceiro está a região Centro Oeste, com Brasília e Goiás representando 4,15% e ocupando a 8ª e 9ª posição no Ranking, respectivamente. Por fim, a Bahia é a única representante do nordeste, com 3.47% e em 7ª no ranking.

Entre as cidades, o cenário se regionaliza ainda mais para o estado de São Paulo, com 4 das 10 cidades que mais venderam, percentualmente.

| Cidade | Percentual de Vendas | Média de Vendas Mensais |
| --- | --- | --- |
|  São Paulo  | 15.61% | 706 |
|  Rio de Janeiro  | 6.93% | 312 |
|  Belo Horizonte  | 2.76% | 126 |
|  Brasília  | 2.11% | 101 |
|  Curitiba | 1.51% | 69 |
|  Campinas  | 1.45% | 68 |
|  Porto Alegre | 1.36% | 65 |
|  Salvador | 1.29% | 62 |
|  Guarulhos | 1.20% | 54 |
|  Sao Bernardo do Campo | 0.94% | 44 |

Por fim, temos o ranking de vendedores. Na base de dados, existem 3.095 vendedores, espalhados pelo território brasileiro. Os nomes dos vendedores são codificados por conta de LGPD, nesse caso, o vendedor sob código o vendedor que trouxe o maior valor acumulado em vendas, obteve um total de R$ 507.166,91, em média por venda, o primeiro do ranking possui uma média de R$ 525,94 e o que possui mais vendas, chegou a marca de 2.133 vendas no período.

![Quantidade de Vendas acumuladas pelos vendedores de cada estado. Fonte: Própria.](chart_(2)%201.png)

Quantidade de Vendas acumuladas pelos vendedores de cada estado. Fonte: Própria.

Geograficamente, os vendedores da cidade de São Paulo possuem, com uma margem astronômica, o posto de maior valor acumulado com R$ 13.369.880,60. A surpresa fica para o segundo lugar, que se antes, Rio de Janeiro possuía o lugar cativo, da perspectiva de vendedores, o estado que mais acumulou foi o Paraná, com R$ 1.846.047,65, seguido por Minas Gerais, com R$ 1.564.757,79 e em quarto lugar aparece o Rio de Janeiro R$ 1.098.242,22. Uma outra surpresa, é o estado de Pernambuco, que não aparecia no ranking de vendas, mas aparece aqui, em 9º Lugar com R$ 124.894,83.

## 2. Produto/Categoria

Para analisarmos a performance dessas categorias, é preciso entender que, na base de dados, o nome dos produtos estão codificados. Dessa forma, será pouco produtivo apenas observar códigos sem alguma referência visual para facilitar os insights, portanto, o foco será mais nas categorias do que nos produtos propriamente ditos.

A Olist possui 73 categorias de produtos. A primeira pergunta que pode vir a cabeça é: “Qual produto vendeu mais?” Analisando apenas a quantidade de vendas, a categoria de “móveis e decoração” possui o produto mais vendido e entre o produto que mais acumulou valor em vendas foi um da categoria de Telefonia Fixa. Entretanto, aprofundando a análise, encontramos um padrão nas duas óticas de performance. A categoria de Ferramentas de Jardim e Informática possuem mais de um produto em cada um dos rankings. 

| Produto | Categoria | Qtd. Vendas | Valor Acum. |
| --- | --- | --- | --- |
| Produto 1 - Móveis | Móveis Decoracao | 536 | R$  63.788,12 |
| Produto 1 - Cama, Mesa | Cama Mesa Banho | 525 | R$  63.161,39 |
| Produto 1 - Ferramentas de Jardim | Ferramentas Jardim | 505 | R$  79.512,22 |
| Produto 2 - Ferramentas de Jardim | Ferramentas Jardim | 406 | R$  48.616,43 |
| Produto 3 - Ferramentas de Jardim | Ferramentas Jardim | 395 | R$  52.508,62 |
| Produto 4 - Ferramentas de Jardim | Ferramentas Jardim | 389 | R$  53.445,34 |
| Produto 1 - Informática | Informática Acessórios | 357 | R$  70.557,90 |
| Produto 1 - Relógios | Relógios Presentes | 327 | R$  48.994,30 |
| Produto 1 - Beleza e Saúde | Beleza e Saúde | 283 | R$  11.826,06 |
| Produto 2 - Informática | Informática Acessórios | 278 | R$  58.962,13 |

No caso das Ferramentas de Jardim, 4 dos 10 produtos que mais venderam, pertencem a essa categoria, ocupando da 3ª a 6ª posição no ranking. Além disso, o produto mais vendido dessa categoria também é o 3º que mais acumulou valor em vendas, com 505 pedidos e um acumulado de R$ 79.512,22 no período de análise. Juntos, esses 4 ítens acumularam 1.695 pedidos e R$ 234.082,61 em arrecadação.

Quando olhamos apra a categoria de Informática, a perspectiva se torna ainda mais atrativa. No ranking dos 10 produtos que mais venderam, dois são deste segmento, mas o que chama a atenção, é que no ranking dos 10 produtos que mais acumularam valor, existem 3 produtos de informática. Sozinhos, eles representam mais de 700 pedidos e quase R$ 200.000,00 em valor acumulado.

| Produto | Categoria | Qtd. Vendas | Valor Acum. |
| --- | --- | --- | --- |
| Produto 1 - Telefonia Fixa | Telefonia Fixa | 8 | R$  109.312,64 |
| Produto 1 - Beleza e Saúde | Beleza e Saúde | 209 | R$  81.887,42 |
| Produto 1 - Ferramentas de Jardim | Ferramentas Jardim | 505 | R$  79.512,22 |
| Produto 1 - Informática | Informática Acessórios | 357 | R$  70.557,90 |
| Produto 2 - Beleza e Saúde | Beleza e Saúde | 159 | R$  64.825,67 |
| Produto 2 - Informática | Informática Acessórios | 106 | R$  64.143,25 |
| Produto 1 - Móveis | Móveis Decoracao | 536 | R$  63.788,12 |
| Produto 1 - Relógios | Relógios Presentes | 228 | R$  63.167,36 |
| Produto 1 - Cama, Mesa | Cama Mesa Banho | 525 | R$  63.161,39 |
| Produto 3 - Informática | Informática Acessórios | 278 | R$  58.962,14 |

Mas não podemos resumir as performances apenas pela quantidade de vendas brutas. Pelas métricas de pirata, um bom indicador de performance é o Basket Size Purchase, ou seja, o tamanho da cesta de compra. Em outras palavras, significa quantidade média de produtos que cada cliente está comprando em um único pedido. Nesse quesito, há uma disputa acirrada entre beleza e saúde, automotivo e informática, com uma média de 11 para o primeiro e 10,5 para os seguintes. Isso quer dizer que, para cada pedido dessas categorias, a quantidade média de itens comprados juntos em um mesmo pedido é de 11 e 10,5 respectivamente.  Esses resultados são importantes para gerar insights de recomendação para clientes.

| Categoria | Basket Size Purchase |
| --- | --- |
| Beleza Saude | 11,0 |
| Automotivo | 10,5 |
| Informatica Acessorios | 10,5 |
| Ferramentas Jardim | 8,0 |
| Moveis Decoracão | 8,0 |
| Telefonia | 7,5 |
| Ferramentas Jardim | 7,5 |
| telefonia | 7,0 |
| utilidades_domesticas | 6,5 |
| bebes | 6,5 |

Uma outra ótica de observação interessante, é em relação as categorias que alcançaram o maior valor médio por pedido. Nesse quesito, destacam-se as categorias de PCs e Relógios. Esse último, apesar de possuir a menor média de preços entre as 10 principais categorias, registrou um alto índice de compras, obtendo um bom saldo entre o preço médio e quantidade de vendas.

Apesar disso, é nas faixas mais baratas que há a maior concentração de vendas e valor acumulado. No período, o preço que mais houve transações foi R$ 59,90, com 2.606 vendas e R$ 289.998,92 em valor acumulado. Só entre as 10 precificações que mais transacionaram, esse número representa 15,87% das vendas e 17,51% do valor acumulado total. Se considerarmos uma faixa que vai de R$ 29,90 a R$ 69,90, obtemos aproximadamente 50% das vendas e do valor acumulado deste ranking. 

![chart (4).png](chart_(4).png)

Entre as cidades brasileiras, a preferência é clara a produtos de Cama, Mesa e Banho. Das 10 cidades com mais vendas, 7 aparecem como maiores compradoras de ítens desta categoria, isso inclui São Paulo, Rio de Janeiro, Belo Horizonte, Campinas, Porto Alegre, Guarulhos e Niterói. Com essa variedade de produtos, aliada a crescente demanda em outras regiões do país, é salutar observarmos outra métrica, que é o custo médio de frete, em especial o Shipping Time. 

| Categoria | Shipping Time |
| --- | --- |
| moveis escritorio | 31.6 DIAS |
| seguros e servicos | 31.0 DIAS |
| fashion calcados | 29.2 DIAS |
| artigos de natal | 26.7 DIAS |
| cds dvds musicais | 26.4 DIAS |
| telefonia fixa | 26.3 DIAS |
| market place | 25.7 DIAS |
| moveis quarto | 25.6 DIAS |
| climatizacao | 25.3 DIAS |
| eletrodomesticos 2 | 25.3 DIAS |

| Categoria | Frete Médio | Maior Frete |
| --- | --- | --- |
| PCs | R$  48,45 | R$  193,21 |
| Eletrodomesticos  | R$  44,53 | R$  170,80 |
| Móveis, Colchao e Estofado | R$  42,90 | R$  82,70 |
| Móveis, Cozinha, Jantar e Jardim | R$  42,70 | R$  251,39 |
| Móveis Quarto | R$  42,49 | R$  147,65 |
| Móveis Escritório | R$  40,55 | R$  256,13 |
| Portáteis, Casa, Forno e Café | R$  36,15 | R$  115,63 |
| Móveis Sala | R$  35,72 | R$  237,11 |
| Sinalização e Seguranca | R$  32,70 | R$  299,16 |
| Indústria, Comércio e negócios | R$  29,42 | R$  317,47 |

O shipping time é uma métrica de retenção muito importante para os e-commerces, pois é a medida do tempo médio, dado em dias, que um produto leva pra chegar ao cliente. Em outras palavras, quanto maior o shipping time, maior a probabilidade de um cliente fazer um churn - deixam de utilizar o produto ou serviço - assim, é imperativo que haja um controle no tempo gasto no translado do produto, visando cada vez mais a redução desse indicador. 

| Região | Ship. Time |
| --- | --- |
| Sudeste | 24,7 |
| Sul | 25,5 |
| Centro Oeste | 26,1 |
| Nordeste | 30,8 |
| Norte | 42,9 |

| Estado | Ship. Time |
| --- | --- |
| AP | 45,6 |
| RR | 45,5 |
| AM | 45,2 |
| AC | 40,6 |
| RO | 38,6 |

| Estado | Ship. Time |
| --- | --- |
| ES | 25,2 |
| PR | 24,3 |
| MG | 24,2 |
| DF | 24,0 |
| SP | 18,8 |

## 3. Cliente

Na base de dados, estão registrados 96.096 clientes em mais de 100.000 ordens, ou seja, a maioria dos consumidores realizaram apenas uma compra. Mas, entre os que fizeram mais de uma, qual é sua distribuição e aspectos a serem notados? Nessa seção, vale o mesmo aviso da anterior. Pela LGPD, os nomes dos clientes foram codificados, tornando inviável a sua identificação. 

A distribuição geográfica dos clientes, segue a tendência já comentada nas outras seções, com São Paulo, Rio de Janeiro e Minas nas 3 primeiras colocações, respectivamente, seja entre as cidades ou entre os estados. Ainda entre as 10 cidades que possuem a maior taxa de clientes cadastrados estão Brasília, Curitiba, Porto Alegre e Salvador, em 4º, 5º, 7º e 8º respectivamente. Entre os estados, a região sul ocupa, o 4º, 5º e 6º lugares, com RS, PR e SC, respectivamente. Bahia, Distrito Federal, Espírito Santo e Goiás fecham o ranking.

No ranking dos 10 clientes que mais compraram ao longo dos dois anos da base de dados, o que está na primeira colocação realizou a compra de 75 produtos, quase o dobro do segundo lugar com 38. Ambos, são das cidades de São Paulo e Rio de Janeiro, o que não é nenhuma novidade. Entretanto, em 4º e 6º estão clientes do Mato Grosso, localizados em Cuiabá e Sinop, com a compra de 29 e 24 produtos, respectivamente. Outra surpresa, são clientes de Porto Alegre e Itaqui no Rio Grande do Sul, com 24 produtos cada um. Em valor acumulado de cada cliente por estado, há uma mesma conclusão.

| Estado | Valor Acum. |
| --- | --- |
| SP | R$  5.998.226,95 |
| RJ | R$  2.144.379,68 |
| MG | R$  1.872.257,26 |
| RS | R$  890.898,53 |
| PR | R$  811.156,37 |
| SC | R$  623.086,42 |
| BA | R$  616.645,82 |
| DF | R$  355.141,79 |
| GO | R$  350.092,31 |
| ES | R$  325.967,54 |

| Estado | Qtd. Vendas/mês | Valor Médio |
| --- | --- | --- |
| SP | 2.253 | R$  153,27 |
| RJ | 729 | R$  180,68 |
| MG | 649 | R$  170,56 |
| RS | 294 | R$  176,88 |
| PR | 271 | R$  178,56 |
| SC | 204 | R$  182,78 |
| BA | 192 | R$  196,98 |
| DF | 117 | R$  174,93 |
| GO | 115 | R$  211,47 |
| ES | 111 | R$  173,56 |

Entre os que possuem as maiores médias de compras, temos o primeiro indicador em que São Paulo não lidera ou sequer está no top5. O Rio de Janeiro lidera o ranking, com um comprador que efetuou 8 transações com uma média de R$  13.664,08 por pedido, o estado carioca ainda possui mais um representante, na 8º colocação, com 10 compras e um valor médio de R$3.018,59. A surpresa está em um cliente representando o estado da Paraíba. Este, que não constuma estar no ranking dos 10 primeiros, possui um comprador, em 9º com 6 comrpas e um valor médio de R$  2.844,95.

| Cliente | Estado | Compras | Valor Médio |
| --- | --- | --- | --- |
| Cliente 1 | RJ | 8 | R$  13.664,08 |
| Cliente 2 | ES | 4 | R$  7.274,88 |
| Cliente 3 | MG | 6 | R$  6.081,54 |
| Cliente 4 | SC | 3 | R$  3.666,42 |
| Cliente 5 | MT | 6 | R$  3.242,84 |
| Cliente 6 | MA | 6 | R$  3.195,73 |
| Cliente 7 | BA | 4 | R$  3.122,72 |
| Cliente 8 | RJ | 10 | R$  3.018,59 |
| Cliente 9 | PB | 6 | R$  2.844,95 |
| Cliente 10 | SP | 3 | R$  2.607,09 |

## 4. Conclusão

A partir da análise descritiva dos três pilares e dos resultados das métricas de pirata, podemos concluir inicialmente que das 10 categorias que mais venderam, o segmento de Casa e Decoração, comportando Cama Mesa Banho, Móveis e Decoração e Utilidades Domésticas, se posiciona como a faixa com mais retorno dentre as outras 73 da base de dados, com Tecnologia e Eletrônicos figurando o segundo lugar por uma margem pequena. Além disso, todas essas vendas foram fortemente canalizadas para o meio de pagamento por cartão de crédito, com os vouchers sendo o ponto a ser observado como uma boa alternativa.

Uma parte representativas das vendas concentrada numa faixa que vai de R$ 29,90 a R$ 69,90, torna-se um bom horizonte de crescimento o incentivo a produtos nessa faixa, entretanto, não podemos abdicar das outras faixas de preço. Principalmente para as categorias de “Telefonia Fixa”, “Beleza e Saúde”, “Ferramentas de Jardim” e “Informática”, as quais possuem uma boa relação entre quantidade de vendas e valor acumulado, principalmente a primeira, que conseguiu acumular o maior valor, com apenas 8 vendas. Além disso, a categoria “Beleza e Saúde” possui a maior taxa de ítens comprados juntos, o que torna um atrativo para estratégias de retenção e receita, como promoções, combos e recomendações.

O segmento mais lucrativo é o de decoração do lar, com “Móveis”, “Cama, Mesa e Banho” e “Ferramentas de Jardim” presentes nos rankings de vendas e valor acumulado, mesmo com um valor médio de frete mais alto, mas com um baixo shipping time. O segmento que se mostra como emergente é o de eletrônicos, em especial o de informática. Esta, se mostra como uma categoria bastante consistente, seja em vendas, valor acumulado e na média de mais ítens em um mesmo pedido, empatado tecnicamente com “Beleza e Saúde”. Possui o maior valor médio de frete, bem como o de frete mais alto já registrado em um pedido, mas que também não apresenta um alto valor de shipping time.

Falando em shipping time, é imperativo que as regiões com menor tempo médio de entrega sejam a sudeste e sul, mas o que chama atenção negativamente, é o shipping time para a região norte, não por ser o que mais demora - o que é de se esperar - mas por apresentar um valor acima da média de diferença entre regiões. Este é um sinal de alerta e que necessita do desenvolvimento de iniciativas para o controle do tempo médio de entrega, com o objetivo de evitar o aumento de churn.

Geograficamente, não há surpresas que São Paulo lidera a maioria dos quesitos com uma margem quilométrica. Minas Gerais e Rio de Janeiro revezam o cargo de segundo lugar, com a capital carioca sendo mais compradora e a capital mineira mais vendedora. A região sul se posiciona como a principal força em vendas fora do sudeste, com Paraná e Rio Grande do Sul se estabelecendo como grandes centros de consumo, tanto em quantidades de vendas, como em valor agregado e desempenho dos vendedores. 

Por fim, apesar do Sudeste ainda possuir uma forte influência nos resultados, a região sul pode ser um ótimo ponto de investimento a longo prazo. Para além do eixo Sul-Sudeste, Brasília e Salvador são opções alternativas visando conquistar o centro e nordeste do país, com Goiás também possuindo bom desempenho entre vendas e vendedores. Por se tratarem de estados vizinhos, logisticamente pode ser vantajoso pensar nessa localidade como um ponto estratégico de desenvolvimento. 

# 9. Considerações Finais

---

Este relatório, é uma primeira versão do projeto final que seria entregue ao solicitante. Ele foi desenvolvido em 2 dias e meio, contabilizando o processo de organização, consultas e escrita. Ao entregar esta primeira versão, os responsáveis por tomar as decisões serão capazes de obter os primeiros insights sobre o desempenho dos três pilares e nortear as consultas e análises do segundo ciclo de entregas deste projeto.

Este segundo ciclo, caso houvesse tempo, seria focado inicialmente em criar novas perguntas a partir das respostas destas primeiras, como por exemplo “Quais foram categorias que mais venderam nos estados de Goiás, DF e Bahia?”, alinhando com conceitos da análise diagnóstica, com intuito de entender os fatores que contribuíram para estes resultados, inserindo uma segunda dimensão às conclusões acerca do fenômeno estudado.

O número de ciclos, bem como os processos de cada um, serão norteados pelo tempo disponível para a entrega do projeto final. Dessa forma, toda a cadeia de desenvolvimento dessas soluções torna-se mais dinâmica e personalizável, com as respostas sendo cada vez mais diretas naquilo que os tomadores de decisão necessitam, dando celeridade e eficiência no processo de análise de dados.