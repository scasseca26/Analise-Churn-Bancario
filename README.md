# Analise-Churn-Bancario
Projeto em Python para análise de churn bancário, explorando padrões de comportamento e retenção de clientes.

![Python](https://img.shields.io/badge/Linguagem-Python-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Biblioteca-Pandas-150458?style=flat&logo=pandas&logoColor=white)
![Plotly](https://img.shields.io/badge/Visualiza%C3%A7%C3%A3o-Plotly-3F4F75?style=flat&logo=plotly&logoColor=white)
![Status](https://img.shields.io/badge/Status-Concluído-brightgreen)

---

## Resumo Executivo

Este projecto analisa uma base de dados de **10.000 clientes** de um grande banco europeu, com o objectivo de compreender os factores que influenciam o **churn** (cancelamento de contas) e identificar o perfil típico do cliente que está a deixar o banco. A taxa geral de churn é de **20,38%**, mas esse número esconde um padrão crítico: **99,51% dos clientes que reclamaram** também cancelaram a conta, tornando a reclamação o sinal de alerta mais forte disponível para a equipa de retenção. Identificou-se ainda que clientes entre **50-59 anos** têm a maior taxa de churn (**56,04%**) e que os portadores do cartão **Diamond**, tipicamente os clientes premium apresentam a maior propensão ao cancelamento (**21,78%**). A análise foi feita inteiramente em **Python**, utilizando **Pandas** para manipulação dos dados e **Plotly** para visualizações interactivas.

---

## Problema do Negócio

Um grande banco europeu está a enfrentar um aumento significativo na taxa de cancelamento de contas correntes por parte dos clientes. A diretoria quer entender melhor o perfil dos clientes que estão a deixar o banco, e como prever e agir preventivamente antes que um cliente cancele a conta.

O time de ciência de dados recebeu um conjunto de dados contendo informações demográficas, financeiras e comportamentais dos clientes, incluindo o campo `Exited`, que indica se o cliente encerrou ou não o relacionamento com o banco.

**Objectivos do projecto:**
- Entender os factores que influenciam o churn dos clientes
- Identificar o perfil típico do cliente que está a saír

As questões analisadas foram:

1. Qual é a taxa geral de churn dos clientes?
2. Clientes com reclamações (`Complain = 1`) têm maior propensão a churn?
3. Qual é o perfil médio dos clientes que saíram em termos de idade?
4. Clientes com o score de crédito mais baixo estão mais propensos a saír?
5. Qual tipo de cartão tem maior taxa de churn?

---

## Contexto

A base de dados (`Customer-Churn-Records.csv`) contém **10.000 registos** de clientes com **18 colunas**, cobrindo informações demográficas (idade, género, geografia), financeiras (score de crédito, saldo, salário estimado) e comportamentais (número de produtos, reclamações, satisfação, tipo de cartão). A coluna `Exited` é a variável-alvo da análise, indicando se o cliente cancelou (1) ou permaneceu (0) com o banco.

---

## Premissas da Análise

- A análise foi realizada em **Python**, utilizando as bibliotecas **Pandas** para manipulação de dados e **Plotly** (`plotly.express` e `plotly.graph_objects`) para visualizações interactivas.
- A coluna `Exited` é a variável-alvo: **1 = Cliente Saiu (Churn)**, **0 = Cliente Permaneceu**.
- Para a análise por faixa etária, a coluna `Age` foi segmentada em intervalos: `18-29`, `30-39`, `40-49`, `50-59` e `60+`.
- Para a análise por score de crédito, a coluna `CreditScore` foi segmentada em intervalos: `0-599`, `600-699`, `700-799` e `800+`.
- Todas as taxas de churn foram calculadas com `value_counts(normalize=True)`, obtendo a proporção percentual de clientes que saíram dentro de cada grupo analisado.

---

## Estratégia da Solução

### Passo 1 — Resumo do Contexto em Pergunta Aberta
> *Por que os clientes estão a cancelar as suas contas e qual é o perfil de quem mais sai?*

### Passo 2 — Transformação em Perguntas Fechadas
> - Qual é a proporção de clientes que cancelaram a conta?
> - A insatisfação expressa via reclamação está associada ao cancelamento?
> - Que faixa etária apresenta maior risco de churn?
> - O score de crédito do cliente influencia a decisão de saír?
> - Algum tipo de cartão está associado a uma maior taxa de cancelamento?

### Passo 3 — Definição da Coluna Facto
A coluna facto principal é **Exited**, pois é a variável binária que classifica cada cliente como churn ou não-churn, servindo de base para todas as taxas e comparações da análise.

### Passo 4 — Identificação das Dimensões

| Dimensão | Coluna |
|----------|--------|
| Comportamento de Reclamação | `Complain` |
| Perfil Demográfico | `Age`, `Gender`, `Geography` |
| Perfil Financeiro | `CreditScore`, `Balance`, `EstimatedSalary` |
| Relacionamento com o Banco | `Tenure`, `NumOfProducts`, `IsActiveMember` |
| Produto | `Card Type` |
| Satisfação | `Satisfaction Score` |

### Passo 5 — Hipóteses Analíticas

- H1: A taxa de churn representa uma parcela significativa da base de clientes.
- H2: Clientes que reclamaram têm uma taxa de churn muito superior à dos que não reclamaram.
- H3: Determinadas faixas etárias apresentam maior propensão ao cancelamento.
- H4: Clientes com score de crédito mais baixo têm maior taxa de churn.
- H5: O tipo de cartão está associado a diferentes níveis de risco de cancelamento.

### Passo 6 — Critérios de Priorização

As hipóteses foram priorizadas com base em dois critérios:

- **Força do Sinal Preditivo** — quanto a variável discrimina claramente entre clientes que saem e que permanecem.
- **Capacidade de Acção** — quanto a equipa de retenção pode agir preventivamente com base no insight.

### Passo 7 — Priorização das Hipóteses Analíticas

| Prioridade | Hipótese | Justificativa |
|------------|----------|---------------|
| Alta | H2 — Reclamação vs. Churn | Sinal preditivo quase determinístico e accionável em tempo real |
| Alta | H3 — Faixa etária vs. Churn | Permite segmentar campanhas de retenção por idade |
| Média | H5 — Tipo de cartão vs. Churn | Orienta estratégias de fidelização por produto |
| Baixa | H4 — Score de crédito vs. Churn | Variável financeira com menor poder discriminativo neste dataset |
| Baixa | H1 — Taxa geral de churn | Métrica de base para contextualizar as demais análises |

---

## Insights da Análise

### 1 — Taxa Geral de Churn
A taxa geral de churn da base de clientes é de **20,38%** ou seja, aproximadamente **1 em cada 5 clientes** cancelou a conta corrente, enquanto **79,62%** permaneceram com o banco.
<img width="1338" height="686" alt="Taxa Clientes" src="https://github.com/user-attachments/assets/5b6ec578-cec3-4f6c-99c1-dc66e6c9a689" />


### 2 — Reclamações vs. Propensão ao Churn
A reclamação é o indicador mais forte de todo o dataset: **99,51% dos clientes que reclamaram (`Complain = 1`) cancelaram a conta**, contra apenas **0,05% de churn entre os que não reclamaram**. Isto indica que a reclamação não é apenas um sintoma de insatisfação, é praticamente um **prenúncio directo do cancelamento**, representando a oportunidade de intervenção mais clara para a equipa de retenção.
<img width="1338" height="689" alt="Taxa Reclamação" src="https://github.com/user-attachments/assets/33881df7-70d0-4e1d-a8f6-57e8da3a929a" />


### 3 — Perfil de Idade dos Clientes que Saem
A taxa de churn varia fortemente por faixa etária:

| Faixa Etária | Taxa de Churn |
|--------------|---------------|
| 18-29 | 7,56% |
| 30-39 | 10,88% |
| 40-49 | 30,83% |
| **50-59** | **56,04%** |
| 60+ | 27,95% |

A faixa etária de **50-59 anos** apresenta a maior taxa de churn, com mais da metade destes clientes a cancelarem a conta — um padrão muito mais acentuado do que nas faixas mais jovens.

<img width="1348" height="678" alt="Distibuicao idade cliente" src="https://github.com/user-attachments/assets/dfa5455f-5244-446e-85e7-5815924ef605" />
<img width="1320" height="666" alt="Taxa faixa etaria" src="https://github.com/user-attachments/assets/0664f662-0334-4471-b595-7543af0d64f0" />

### 4 — Score de Crédito vs. Propensão ao Churn
A relação entre score de crédito e churn é bastante estável e não revela um padrão forte:

| Faixa de Score | Taxa de Churn |
|----------------|---------------|
| 0-599 | 21,75% |
| 600-699 | 19,72% |
| 700-799 | 19,94% |
| 800+ | 20,14% |

Diferente do esperado inicialmente, **o score de crédito não é um factor discriminativo relevante** para prever churn neste dataset, as taxas mantêm-se relativamente constantes entre as faixas.

<img width="1384" height="661" alt="Taxa Score credito" src="https://github.com/user-attachments/assets/4949c492-7b2c-4a2a-b4bf-fe911d4e8d2e" />


### 5 — Tipo de Cartão com Maior Taxa de Churn
| Tipo de Cartão | Taxa de Churn |
|---------------|---------------|
| **DIAMOND** | **21,78%** |
| GOLD | 19,26% |
| PLATINUM | 20,36% |
| SILVER | 20,11% |

O cartão **Diamond** — geralmente associado aos clientes de maior valor para o banco, apresenta a maior taxa de churn entre todos os tipos, o que representa um risco financeiro desproporcional, já que estes são tipicamente os clientes mais rentáveis.

---

## Resultados

Com base na análise, é possível concluir que:

- A **reclamação do cliente deve accionar um alerta imediato** na equipa de retenção, dado que está associada a uma taxa de churn de praticamente 100%.
- A faixa etária de **50-59 anos** deve ser o foco de campanhas de retenção direccionadas, por apresentar o maior risco de cancelamento.
- O **score de crédito não deve ser usado isoladamente** como critério de previsão de churn, dado o seu baixo poder discriminativo.
- Os clientes com cartão **Diamond** merecem atenção prioritária da gestão de relacionamento, pois além de serem o segmento premium, apresentam a maior propensão ao cancelamento, representando o maior risco financeiro para o banco.
- O banco deve desenvolver um **modelo preditivo de churn** combinando reclamação, idade e tipo de cartão como variáveis centrais, permitindo intervenções preventivas antes do cancelamento efectivo.

---

## Estrutura do Projecto

```
Analise-churn-bancario
│
├── dados
│   └── Customer-Churn-Records-.csv      # Base de dados original
├── analises
│   └── Churn_Bancário.ipynb            # Notebook com a análise completa
└── 📄 README.md                        # Documentação do projecto
```

---

## Ferramentas Utilizadas

- **Python** — Linguagem de programação
- **Pandas** — Manipulação e análise de dados
- **Plotly** (Express e Graph Objects) — Visualizações interactivas
- **Jupyter Notebook** — Ambiente de desenvolvimento da análise

---

## 👤 Autor

**Santiago Casseca**
[LinkedIn](www.linkedin.com/in/santiago-casseca)
