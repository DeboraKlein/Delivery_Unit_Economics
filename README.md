## Delivery Unit Economics Pipeline

![badge databricks](https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white)
![badge apachespark](https://img.shields.io/badge/Delta_Lake-0099E5?style=for-the-badge&logo=apachespark&logoColor=white)
![badge pbi](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![badge sqlpostgre](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=postgresql&logoColor=white)

![badge status](https://img.shields.io/badge/STATUS-PRODUCTION_READY-green?style=for-the-badge)
![badge versão](https://img.shields.io/badge/VERS%C3%83O-2.0-blue?style=for-the-badge)
![badge custo mensal](https://img.shields.io/badge/CUSTO_MENSAL-~$0.75-success?style=for-the-badge)

### Visão Geral do Projeto

Pipeline de dados construído em **Databricks/Delta Lake** para análise de **Unit Economics (UE)** em marketplace de delivery two-sided. O objetivo é transformar dados transacionais brutos em métricas estratégicas de **P&L unitário** para tomada de decisão empresarial, com foco em:
1.  **Resolver a inconsistência 1:1** (`Fan-out`) na união dos dados de Pedidos.
2.  Garantir a **Governança Financeira** sobre o cálculo da UE.
3.  Modelar dados otimizados (`ZORDER`) para consumo em **Power BI**.

---

### 1. Arquitetura e Fluxo (Modelo Medalhão Modularizado)

O pipeline foi modularizado em **5 notebooks** (Job) seguindo a arquitetura **Bronze $\rightarrow$ Silver $\rightarrow$ Gold**.

#### Fluxo de Execução do Job

| # | Notebook | Camada | Função Principal |
| :--- | :--- | :--- | :--- |
| **00** | `00_Setup_Logging.ipynb` | Log | Cria o esquema e as tabelas de auditoria (`log_processamento`, `metricas_qualidade`). |
| **01** | `01_bronze_ingestion.ipynb` | **Bronze** | Ingestão dos dados brutos (Full Load) e padronização inicial. |
| **02** | `02_silver_ue_calculation_dq.ipynb` | **Silver** | **Deduplicação (ROW\_NUMBER)** e **Cálculo da Unit Economics (UE)**. |
| **03** | `03_gold_dimensional_model.ipynb` | **Gold** | Criação do Star Schema, Modelagem Dimensional e Otimização com **ZORDER**. |
| **04** | `04_Final_Quality_Checks.ipynb` | **Data Testing** | Executa testes de **Integridade Referencial** e **Validação de Constantes**. |

---

### 2. Governança e Qualidade de Dados (DQ)

O projeto implementa um sistema de *Data Testing* de alta criticidade, garantindo a **confiabilidade** dos dados no Power BI:

| Teste Crítico | Resultado Atual | Confirmação |
| :--- | :--- | :--- |
| **Integridade Referencial** | **0.00 Violações** | Todo `store_id` na Fato Gold tem uma Dimensão Loja válida. |
| **Validação de Constantes** | **0.00 Violações** | As premissas de cálculo da UE (`0.18`, `0.70`, `0.02`) estão intactas. |
| **Log de Auditoria** | **Completo** | O status de cada etapa do Job é registrado na tabela `log_processamento`. |

---

### 3. Roadmap de Evolução e Otimização de Custo

#### CONCLUÍDO (Core Pipeline)
- Pipeline completo Full Refresh (`Bronze $\rightarrow$ Gold`).
- **Resolução da Inconsistência 1:1** (Deduplicação Silver).
- **Data Governance** completa via testes de Integridade e Constantes.
- Modelagem dimensional Gold (`ZORDER` aplicado).
- Cálculo de Unit Economics e Integração Power BI.

#### EM ANDAMENTO (Otimização e Sustentabilidade)
- **Otimização incremental (Meta: 94% de economia):** Desenvolvimento da lógica *Merge/Upsert* nas camadas Bronze e Silver.
- Monitoramento de custos em tempo real (DBU).
- Documentação completa (README e Glossário atualizados).

#### PRÓXIMOS PASSOS
- Agendamento automático noturno do Job final.
- Alertas de qualidade em tempo real (ex: PagerDuty) ao detectar violações.
- Expansão do modelo Gold para incluir métricas de **CAC/LTV**.
---

### 4. Dashboard Power BI - Visualizações

#### Dashboard P&L Unit Economics

Dashboard - Visão Tática

![Dashboard 1](https://github.com/DeboraKlein/Delivery_Unit_Economics/raw/main/docs/Assets/Power%2520BI%2520Desktop%252023_11_2025%252023_53_03.png)


Dashboard - Visão Analítica

![Dashboard 2](https://github.com/DeboraKlein/Delivery_Unit_Economics/raw/main/docs/Assets/Power%2520BI%2520Desktop%252023_11_2025%252023_53_18.png)


##### ⚠️ Alertas e Contextos Implementados

MARKUP OPERACIONAL: 2.06
ALERTA: Reflete apenas eficiência das transações
• Não inclui custos de estrutura corporativa
• Markup líquido projetado: ~0.82


Comissão Loja (Waterfall)

O valor negativo representa SAÍDA do GMV
• Correto para visualização em waterfall
• Equação financeira validada: GMV = Comissão + Receita + Custos


#### Acesso ao Relatório Power BI


 URL do Relatório Publicado: [https://app.powerbi.com/view?r=eyJrIjoiY2FmMjBlOTgtNGUyZi00MzgxLWJjN2MtZWJlYTFlY2JkYzUwIiwidCI6IjY1OWNlMmI4LTA3MTQtNDE5OC04YzM4LWRjOWI2MGFhYmI1NyJ9]


##### Estrutura Técnica do Relatório
Conexões de Dados:

 Fontes: 
  - Tabelas Gold do Databricks
  - Arquivo Delta Lake otimizado
  - Modelo estrela (fato + dimensões)

##### Medidas Principais Implementadas:

 Métricas validadas no relatório:
- Receita Bruta GMV
- Lucro Bruto Unitário  
- Receita Líquida Plataforma
- COGS Total
- Comissão Loja (waterfall)
- Markup Operacional
- Taxa de Prejuízo
- Ticket Médio

---

### 5. Estrutura do Repositório

````
.
├── Pipelines/
│   ├── 00_Setup_Logging.ipynb         #  Configuração inicial de Schema e Tabelas de Auditoria (log_processamento, metricas_qualidade)
│   ├── 01_bronze_ingestion.ipynb      #  **BRONZE:** Ingestão dos dados CSV brutos (Full Refresh)
│   ├── 02_silver_ue_calculation_dq.ipynb #  **SILVER:** Cálculo da Unit Economics (UE), Deduplicação (ROW_NUMBER) e DQ inicial
│   ├── 03_gold_dimensional_model.ipynb #  **GOLD:** Modelagem Dimensional (Star Schema) e Otimização ZORDER
│   └── 04_Final_Quality_Checks.ipynb  #  **DATA TESTING:** Testes de Integridade Referencial e Validação de Constantes
│
├── Documentacao/
│   ├── README.md                      #  Documentação Executiva (Visão geral, Status, Roadmap)
│   └── Glossario.ipynb                #  Definições de Termos de Negócio, Fórmulas e Arquitetura Delta Lake
│
├── Dashboards_PowerBI/
│   └── Unit_Economics_Overview.pbix   #  Dashboard de consumo final das métricas de Lucro Bruto Unitário
│
└── Relatorios/
    └── Plano_Migracao_Incremental.pdf #  Proposta e Detalhamento da Otimização de Custo (Redução de 94% DBU)
````

### Fonte dos Dados

#### Dataset Original

Fonte: Brazilian Delivery Center - Kaggle

Tipo: Dados públicos de marketplace de delivery

Período: Dados históricos de transações

Licença: CC0: Public Domain








