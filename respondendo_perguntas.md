# 7. Respondendo as Perguntas

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
