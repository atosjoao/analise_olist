
# 1. Base de Dados

A Olist disponibilizou uma bases de dados referente a dados de venda, clientes, vendedores e pedidos, retirados de setembro de 2016 a outubro de 2018. Esse período foi dividido em 8 tabelas, cada uma com 4 ou mais colunas. Essas colunas agregam informações pertinentes sobre os mais variados aspectos das suas operações. O período de analise vai de, setembro de 2016 a outubro de 2018, com os dados geográficos englobando todo o território brasileiro. As tabelas e suas respectivas colunas são:

# 1.1 Tabelas e Colunas:

-  **Tabela Customer - Cliente** 
    1. Customer ID - Código gerado a cada transação
    2. Customer Unique ID - Código gerado para cada cliente de maneira única
    3. Customer ZipCode prefix - CEP do Cliente
    4. Customer City - Cidade do Cliente
    5. Customer State - Estado do Cliente
-  **Tabela Geolocation - Geolocalização**
    1. Geolocation ZipCode prefix - CEP da Entrega
    2. Geolocation Lat - Latitude da Entrega
    3. Geolocation Lon - Longitude da Entrega
    4. Geolocation City - Cidade da Entrega
    5. Geolocation State - Estado da Entrega
-  **Tabela Order Items - Itens dos pedidos** 
    1. Order ID - Identificação do Pedido
    2. Order Item ID - Identificação do Item do Pedido
    3. Product ID - Identificação do Produto
    4. Seller ID - Identificação do Vendedor
    5. Shipping Limit Date - Data Limite de Envio
    6. Price - Preço do Produto
    7. Freight Value - Preço de Frete
-  **Tabela Order Payments - Pagamentos dos Pedidos** 
    1. Order ID - Identificação do Pedido
    2. Payment Sequential - Sequência do pagamento
    3. Payment Type - Tipo de pagamento
    4. Payment Installments - Parcelas do Pagamento
    5. Payment Value - Valor do Pagamento
-  **Tabela Order Reviews - Avaliações dos Pedidos** 
    1. Review ID - Identificação da Avaliação
    2. Order ID - Identificação do Pedido
    3. Review Comment Title - Título do comentário da avaliação
    4. Review Comment message - Mensagem do comentário da avaliação
    5. Review Creation date - Data de criação da avaliação
    6. Review Answer timestamp - Horário da resposta da avaliação
-  **Tabela Orders - Pedidos** 
    1. Order ID - Identificação do Pedido
    2. Customer ID - Identificação do Cliente
    3. Order Status - Status do Pedido
    4. Order Purchase Timestamp - Data da compra
    5. Order Approved At - Data da Aprovação da Compra
    6. Order Delivered Carrier Date - Data do Recebimento do pedido pela transportadora 
    7. Order Delivered Customer Date - Data do Recebimento do Pedido
    8. Order Estimated Delivery Date - Data estimada de entrega do pedido ao cliente
-  **Tabela Products - Produtos** 
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
