# Processo de Modelagem Dimensional (Star Schema) - Power BI & SQL

## Sobre o Projeto
Este projeto teve como objetivo transformar uma base de dados relacional e bruta em um **modelo dimensional em estrela (Star Schema)** no Power BI. A ideia principal foi otimizar a performance das consultas, organizar as tabelas de forma lógica e facilitar a criação de relatórios e análises de negócios.

O trabalho foi dividido em duas frentes:
1. **Modelagem no Power BI (Base Financials):** Reestruturação do modelo de vendas no Power Query, eliminação de dados duplicados, criação de chaves primárias/estrangeiras (SKs) e ligação das tabelas fato e dimensão.
2. **Modelagem Acadêmica (Foco em Professores):** Criação da estrutura de banco de dados e diagrama dimensional para análise de carga horária e disciplinas ministradas por docentes.

---

## Como o Diagrama Foi Construído

O processo de montagem e tratamento das tabelas seguiu estes passos:

### 1. Limpeza e Tratamento no Power Query
* **Ajuste na Tabela de Calendário (`D_Calendario`):** Corrigi o tipo de dado da coluna de data de `Data/Hora` para apenas `Data`, removendo os horários zerados (`00:00:00`) que estavam impedindo o relacionamento correto com as vendas.
* **Criação da Chave Única (`SK_ID`):** Na tabela `D_Detalhes`, criei uma coluna de Índice sequencial para servir como chave substituta (Surrogate Key).
* **Mesclagem com a Tabela Fato (`F_Vendas`):** Combinei as consultas do Power Query usando os campos de contexto (`Product`, `Country`, `Segment`) para trazer o código `SK_ID` para a tabela `F_Vendas`.

### 2. Relacionamentos no Power BI
* **Relacionamento de Data (`1:N`):** Conectei a `D_Calendario[Date]` com a `F_Vendas[Date]`, deixando a direção do filtro de um único sentido (da dimensão filtrando a fato).
* **Relacionamento de Detalhes (`1:N`):** Conectei a `D_Detalhes[SK_ID]` com a `F_Vendas[SK_ID]`.

---

## Recursos e Fórmulas DAX Utilizadas


### 1. Tabela Calendário
Gerada via DAX cobrindo o período de análises do projeto:

```dax
D_Calendario = 
ADDCOLUMNS (
    CALENDAR(DATE(2013, 01, 01), DATE(2014, 12, 31)),
    "Ano", YEAR([Date]),
    "Mês", FORMAT([Date], "mmmm"),
    "Número do Mês", MONTH([Date]),
    "Trimestre", "T" & FORMAT([Date], "Q"),
    "Semestre", IF(MONTH([Date]) <= 6, "1º Semestre", "2º Semestre"),
    "Dia", FORMAT([Date], "dddd")
)mestre", "2º Semestre"),
    "Dia", FORMAT([Date], "dddd")
)
