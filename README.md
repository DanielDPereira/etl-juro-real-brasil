# 📊 Monitor de Juro Real Brasileiro (ETL Pipeline)

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Pandas](https://img.shields.io/badge/Pandas-Data_Analysis-150458)
![Status](https://img.shields.io/badge/Status-Completed-success)
![License](https://img.shields.io/badge/License-MIT-green)

## 🎯 Sobre o Projeto

Este projeto consiste em um pipeline de **ETL (Extract, Transform, Load)** desenvolvido em Python para analisar a rentabilidade real da renda fixa no Brasil.

O script consome dados oficiais do governo, aplica correções financeiras utilizando a **Equação de Fisher** e gera visualizações analíticas. O objetivo principal é responder à pergunta: *"O investimento na taxa Selic superou a inflação nos últimos 12 meses?"*, permitindo monitorar o ganho real e a proteção do poder de compra.

## ⚙️ Arquitetura do Pipeline

O projeto segue o fluxo clássico de Engenharia de Dados:

### 1. Extract (Extração)
* Coleta automatizada de séries temporais diretamente da **API de Dados Abertos do Banco Central do Brasil (SGS)** via biblioteca `requests`.
* **Séries utilizadas:**
    * `4390`: Taxa Selic acumulada mensal (Juro Nominal).
    * `433`: IPCA (Índice de Preços ao Consumidor Amplo - Inflação Oficial).

### 2. Transform (Transformação)
* Limpeza e conversão de tipos de dados com **Pandas**.
* Unificação das bases (`Merge`) por data.
* **Cálculo Financeiro:** Aplicação da metodologia correta para juros reais (não apenas subtração).
    * *Fórmula de Fisher:* $$(1 + Selic) \div (1 + IPCA) - 1$$
* **Janelas Móveis (Rolling Windows):** Cálculo de acumulado de 12 meses para eliminar a volatilidade mensal e visualizar a tendência anualizada.

### 3. Load (Carga & Visualização)
* **Analytics:** Geração de gráfico destacando períodos de **Ganho Real** (Verde) vs **Perda de Poder de Compra** (Vermelho).
* **Exportação:** Persistência dos dados tratados em arquivo `.csv` (`relatorio_juro_real_brasil.csv`), formatado para ingestão em ferramentas de BI (Power BI, Tableau).

## 🛠️ Tecnologias Utilizadas

* **Linguagem:** Python 3
* **Manipulação de Dados:** Pandas
* **Integração API:** Requests
* **Visualização:** Matplotlib & Seaborn
* **Ambiente:** Jupyter Notebook

## 🚀 Como Executar

### Pré-requisitos
Certifique-se de ter o Python instalado. Instale as bibliotecas necessárias:

```bash
pip install pandas requests matplotlib seaborn

```

### Execução

1. Clone este repositório:

```bash
git clone https://github.com/DanielDPereira/etl-juro-real-brasil.git

```

2. Navegue até a pasta e execute o Notebook:

```bash
jupyter notebook ETL_Monitor_de_Juro_Real_Brasileiro.ipynb

```

3. O script irá gerar automaticamente o arquivo `relatorio_juro_real_brasil.csv` na raiz do projeto e exibir o gráfico de análise.

## 💻 Exemplo de Código

Trecho da lógica aplicada para o cálculo do Juro Real (Equação de Fisher):

```python
# Cálculo do Juro Real Mensal
df_final['juro_real_mensal'] = ((1 + df_final['selic_dec']) / (1 + df_final['ipca_dec'])) - 1

# Aplicação de Janela Móvel de 12 meses (Juros Compostos)
def acumular_juros(x):
    return (x + 1).prod() - 1

df_final['juro_real_12m'] = df_final['juro_real_mensal'].rolling(12).apply(acumular_juros)

```

## 📊 Resultado Visual

<img src="https://github.com/DanielDPereira/etl-juro-real-brasil/blob/main/Gr%C3%A1fico%20Selic%20x%20IPCA.png?raw=true"/>

## 👤 Autor

**Daniel Dias Pereira**

* [LinkedIn](https://www.linkedin.com/in/daniel-dias-pereira-40219425b/)
* [GitHub](https://github.com/DanielDPereira)

---
