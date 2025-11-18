## Delivery Unit Economics Pipeline

https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white
https://img.shields.io/badge/Delta_Lake-0099E5?style=for-the-badge&logo=apachespark&logoColor=white
https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black
https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white
https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=postgresql&logoColor=white

### Visão Geral do Projeto
Pipeline de dados para análise de Unit Economics em marketplace de delivery two-sided. Transforma dados transacionais brutos em métricas estratégicas de P&L unitário para tomada de decisão empresarial.

https://img.shields.io/badge/STATUS-PRODUCTION_READY-green?style=for-the-badge
https://img.shields.io/badge/VERS%C3%83O-1.0-blue?style=for-the-badge
https://img.shields.io/badge/LICEN%C3%87A-MIT-blue?style=for-the-badge

### Objetivo Estratégico
Criar uma visão unificada e calculada do pedido para determinar o Lucro Bruto Unitário (Margem de Contribuição) por transação, permitindo à gestão otimizar preços, comissões e repasses no marketplace.

### Arquitetura do Pipeline
Medallion Architecture Implementada
````
graph TB
    A[📥 Fontes CSV] --> B[🥉 Camada Bronze]
    B --> C[🛠️ Data Quality Checks]
    C --> D[🥈 Camada Silver]
    D --> E[📈 Cálculo Unit Economics]
    E --> F[🥇 Camada Gold]
    F --> G[📊 Power BI]
    F --> H[🔍 Quality Dashboard]
    
    style A fill:#e1f5fe
    style B fill:#fff3e0
    style D fill:#e8f5e8
    style F fill:#f3e5f5
    style G fill:#e0f2f1
````

### Stack Tecnológica

| Camada | Tecnologia | Propósito |
| :--- | :--- | :--- |
| Ingestão | COPY INTO + Delta Lake | Carga eficiente de CSVs |
| Processamento | PySpark + SQL | Transformações distribuídas |
| Armazenamento | Delta Tables | ACID properties + versioning |
| Orquestração | Notebooks Databricks | Execução manual confiável |
| Visualização | Power BI | Dashboards executivos |

### Métricas de Unit Economics

| Métrica | Fórmula | Business Impact |
| :--- | :--- | :--- |
| GMV | subtotal_bruto + delivery_fee_cliente | Volume total da plataforma |
| Receita Líquida | comissao_plataforma + delivery_fee_plataforma | Receita efetiva da operação |
| Custo Logístico | delivery_fee_driver + bonus_fee_driver | Repasse aos motoristas |
| Custo Transacional | payment_fee | Taxas de pagamento |
| Lucro Bruto Unitário | receita_liquida - custo_logistico - custo_transacional | Métrica principal |

### Como Executar

### Pré-requisitos

 Databricks Workspace

 Acesso ao catálogo workspace

 Arquivos CSV na pasta /Volumes/raw_data_volume/

### Execução do Pipeline

````
# 1. Executar configuração inicial
Notebook: 00_Setup_Logging.ipynb

# 2. Executar pipeline completo
Notebook: 01_ETL_And_Quality.ipynb
````

### Estrutura de Execução

````
# Ordem de execução recomendada
1. 00_Setup_Logging.ipynb      # ✅ Configuração
2. 01_ETL_And_Quality.ipynb    # ✅ Pipeline completo
````

### Estrutura do Projeto

````
delivery_unit_economics/
├── 📓 00_Setup_Logging.ipynb
├── 📓 01_ETL_And_Quality.ipynb
├── 📊 Glossario_Projeto.ipynb
└── 📈 Dashboards_PowerBI/
    ├── Unit_Economics_Overview.pbix
    ├── Segment_Analysis.pbix
    └── Operational_Metrics.pbix
````