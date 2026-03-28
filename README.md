# Workshop-Data-Mart-Tematico

#  Projeto de Análise de Dados – Lava Jato

##  Visão Geral

Este projeto tem como objetivo estruturar um ambiente analítico para um lava jato, permitindo a extração de insights relevantes sobre faturamento, volume de serviços e comportamento ao longo do tempo.

A solução utiliza modelagem dimensional com **schema Snowflake** e construção de **Data Marts**, possibilitando análises eficientes e escaláveis.

---

##  Objetivos do Projeto

* Calcular o **faturamento médio** do lava jato
* Identificar o **desvio padrão do faturamento**, destacando variações relevantes
* Detectar **meses com baixa atividade (meses parados)**
* Analisar o **volume médio diário de carros lavados por mês**
* Apoiar decisões estratégicas para aumentar receita e eficiência operacional

---

##  Modelagem de Dados

O projeto utiliza um modelo dimensional no formato **Snowflake Schema**, com separação clara entre fatos e dimensões.

###  Tabela Fato

**Fato_Lavagens**

* ID_Lavagem
* ID_Carro
* ID_Servico
* ID_Tempo
* Valor_Servico
* Quantidade (geralmente 1 por lavagem)

---

###  Dimensões

####  Dimensão Carro

* ID_Carro
* Modelo
* Marca
* Categoria (Pequeno, Médio, Grande)

####  Dimensão Serviço

* ID_Servico
* Tipo de Serviço (Simples, Completa, Polimento, etc.)
* Preço Base
* Categoria de Serviço

####  Dimensão Tempo

* ID_Tempo
* Data
* Dia
* Mês
* Ano
* Nome do Mês
* Dia da Semana
* Indicador de Fim de Semana

---

##  Estrutura Snowflake

As dimensões podem ser normalizadas, por exemplo:

* Dim_Carro → Dim_Marca
* Dim_Tempo → Dim_Mês → Dim_Ano
* Dim_Servico → Dim_Categoria_Servico

Essa abordagem reduz redundância e melhora a consistência dos dados.

---

##  Data Marts

Serão criados Data Marts específicos para facilitar análises:

###  Data Mart Financeiro

* Faturamento por mês
* Faturamento médio
* Desvio padrão de faturamento

###  Data Mart Operacional

* Quantidade de carros lavados por dia
* Média diária por mês
* Volume por tipo de serviço

###  Data Mart Temporal

* Análise de sazonalidade
* Identificação de meses mais fracos
* Comparação mês a mês

---

##  Métricas e Indicadores

###  Faturamento Médio

```sql
SELECT AVG(Valor_Servico) AS faturamento_medio
FROM Fato_Lavagens;
```

###  Desvio Padrão do Faturamento

```sql
SELECT STDDEV(Valor_Servico) AS desvio_padrao
FROM Fato_Lavagens;
```

###  Média de Carros Lavados por Dia (por mês)

```sql
SELECT 
    Mes,
    AVG(qtd_dia) AS media_diaria
FROM (
    SELECT 
        ID_Tempo,
        COUNT(*) AS qtd_dia
    FROM Fato_Lavagens
    GROUP BY ID_Tempo
) sub
GROUP BY Mes;
```

---

##  Análises Esperadas

Com esse modelo, será possível responder perguntas como:

* Qual é o faturamento médio mensal?
* Quais meses apresentam maior variabilidade (instabilidade)?
* Quais meses têm menor movimento (meses "parados")?
* Qual a média de carros lavados por dia em cada mês?
* Existe sazonalidade no negócio?

---

##  Insights de Negócio

A partir das análises, é possível:

* Identificar períodos de baixa demanda e criar promoções
* Ajustar equipe conforme o fluxo de clientes
* Criar campanhas específicas para meses fracos
* Otimizar o portfólio de serviços

---

##  Tecnologias Sugeridas

* Banco de Dados: PostgreSQL / MySQL / SQL Server
* ETL: Python (Pandas) ou ferramentas como Airflow
* Visualização: Power BI / Tableau
* Armazenamento: Data Warehouse relacional

---

##  Conclusão

Este projeto fornece uma base sólida para análise de dados em um lava jato, permitindo transformar dados operacionais em decisões estratégicas.

Com a estrutura proposta, o negócio ganha visibilidade sobre seu desempenho e capacidade de agir de forma proativa para melhorar resultados.

---
