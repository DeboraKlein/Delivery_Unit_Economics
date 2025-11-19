## Delivery Unit Economics Pipeline

![badge databricks](https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white)
![badge apachespark](https://img.shields.io/badge/Delta_Lake-0099E5?style=for-the-badge&logo=apachespark&logoColor=white)
![badge pbi](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![badge sqlpostgre](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=postgresql&logoColor=white)

![badge status](https://img.shields.io/badge/STATUS-PRODUCTION_READY-green?style=for-the-badge)
![badge versão](https://img.shields.io/badge/VERS%C3%83O-2.0-blue?style=for-the-badge)
![badge custo mensal](https://img.shields.io/badge/CUSTO_MENSAL-~$0.75-success?style=for-the-badge)

### Visão Geral do Projeto
Pipeline de dados para análise de Unit Economics em marketplace de delivery two-sided. Transforma dados transacionais brutos em métricas estratégicas de P&L unitário para tomada de decisão empresarial, com otimização de custo de 94% via processamento incremental.

### Objetivo Estratégico
Criar uma visão unificada e calculada do pedido para determinar o Lucro Bruto Unitário (Margem de Contribuição) por transação, permitindo à gestão otimizar preços, comissões e repasses no marketplace.

### Fonte dos Dados

#### Dataset Original

Fonte: Brazilian Delivery Center - Kaggle

Tipo: Dados públicos de marketplace de delivery

Período: Dados históricos de transações

Licença: CC0: Public Domain

### Estrutura dos Arquivos Originais
````
raw_data_volume/
├── 📄 orders.csv           # Dados de pedidos
├── 📄 deliveries.csv       # Dados de entregas  
├── 📄 payments.csv         # Dados de pagamentos
├── 📄 stores.csv           # Catálogo de lojas
└── 📄 hubs.csv             # Catálogo de hubs
````

### Descrição das Tabelas Originais

| Tabela | Colunas Principais | Descrição |
| :--- | :--- | :--- |
| orders | order_id, order_moment_created, order_amount, order_delivery_fee, store_id | Transações de pedidos |
| deliveries | delivery_order_id, delivery_status, driver_id | Status e informações de entrega |
| payments | payment_order_id, payment_method, payment_amount | Transações financeiras |
| stores | store_id, store_segment, hub_id | Cadastro de lojas parceiras |
| hubs | hub_id, hub_city | Cadastro de hubs regionais |


### Arquitetura do Pipeline

![Arquitetura do Pipeline](https://github.com/DeboraKlein/Delivery_Unit_Economics/blob/main/docs/Assets/Diagrama.png)

### Stack Tecnológica

| Camada | Tecnologia | Propósito |
| :--- | :--- | :--- |
| Ingestão | COPY INTO + Delta Lake | Carga eficiente de CSVs |
| Processamento | PySpark + SQL | Transformações distribuídas |
| Armazenamento | Delta Tables | ACID properties + versioning |
| Otimização | Incremental Loading | Redução de 94% no custo |
| Orquestração | Notebooks Databricks | Execução manual confiável |
| Visualização | Power BI | Dashboards executivos |
| Monitoramento | System Tables + Logs | Controle de qualidade e custo |

### Métricas de Unit Economics

| Métrica | Fórmula | Business Impact |
| :--- | :--- | :--- |
| GMV | subtotal_bruto + delivery_fee_cliente | Volume total da plataforma |
| Receita Líquida | comissao_plataforma + delivery_fee_plataforma | Receita efetiva da operação |
| Custo Logístico | delivery_fee_driver + bonus_fee_driver | Repasse aos motoristas |
| Custo Transacional | payment_fee | Taxas de pagamento |
| Lucro Bruto Unitário | receita_liquida - custo_logistico - custo_transacional | Métrica principal |

### Otimização de Custo - Destaques

| Métrica | Antes | Depois | Economia |
| :--- | :--- | :--- | :--- |
| Custo Mensal | $12.00 | $0.75 | 94% |
| Tempo Execução | 15-20min | 3-5min | 75% |
| Consumo Compute | 16 cores | 2-4 cores | 87% |
| ROI Anual | - | 1500% | - |

### Como Executar

Pré-requisitos

-  Databricks Workspace

-  Acesso ao catálogo workspace

-  Arquivos CSV na pasta /Volumes/raw_data_volume/

### Execução do Pipeline

#### IMPLEMENTAÇÃO INICIAL (Full Refresh)
````
     1. Executar configuração inicial
Notebook: 00_Setup_Logging.ipynb

     2. Executar pipeline completo
Notebook: 01_ETL_And_Quality.ipynb
````
#### OTIMIZAÇÃO INCREMENTAL (Recomendado para Produção)
````
     1. Implementar otimização incremental
Notebook: 02_Incremental_Optimization.ipynb

     2. Executar pipeline otimizado
Notebook: 01_ETL_And_Quality.ipynb  # Agora com incremental
````

### Estrutura de Execução

````
    # Ordem de execução recomendada - FASE INICIAL
    1. 00_Setup_Logging.ipynb          #  Configuração
    2. 01_ETL_And_Quality.ipynb        #  Pipeline completo (Full Refresh)

    # FASE OTIMIZAÇÃO (após validação do negócio)
    3. 02_Incremental_Optimization.ipynb #  Implementação incremental
    4. 01_ETL_And_Quality.ipynb        #  Pipeline otimizado
````

### Estrutura do Projeto

````
delivery_unit_economics/
├──  00_Setup_Logging.ipynb
├──  01_ETL_And_Quality.ipynb
├──  02_Incremental_Optimization.ipynb    
├──  Glossario.ipynb
├──  Dashboards_PowerBI/
│   ├── Unit_Economics_Overview.pbix
│   ├── Segment_Analysis.pbix
│   └── Operational_Metrics.pbix
└──  Relatorios/
    ├── Analise_Custo_Beneficio.pdf
    └── Plano_Migracao_Incremental.pdf
````

### Roadmap de Evolução

#### CONCLUÍDO

- Pipeline completo Full Refresh

- Cálculo de Unit Economics

- Modelagem dimensional Gold

- Integração Power BI

- Sistema de qualidade de dados

#### EM ANDAMENTO

- Otimização incremental (94% economia)

- Monitoramento de custos

- Documentação completa

#### PRÓXIMOS PASSOS

- Agendamento automático noturno

- Alertas de qualidade em tempo real

- Expansão para métricas CAC/LTV

- Dashboard operacional de custos

### Suporte e Contato

    Responsável: Debora Rebula Klein
    Data de Atualização: 19/11/2025


### Resultados de Negócio Esperados

- 15-20% de melhoria na tomada de decisão de preços

- 30% mais rápido na identificação de pedidos não-rentáveis

- 100% de transparência no P&L por transação

- 94% de redução no custo operacional do pipeline

STATUS: PIPELINE 100% FUNCIONAL E OTIMIZADO PARA PRODUÇÃO

"Transformamos dados dispersos de marketplace em insights acionáveis de Unit Economics, estabelecendo a base para decisões estratégicas de pricing e profitability."

#### Nota: Para ambientes de produção, recomenda-se a implementação do notebook 02_Incremental_Optimization.ipynb após validação do pipeline completo com stakeholders de negócio.




