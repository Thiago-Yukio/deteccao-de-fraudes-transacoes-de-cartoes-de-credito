# Detecção de Anomalias em Transações de Cartão de Crédito

Este projeto tem como objetivo identificar possíveis transações fraudulentas em cartões de crédito utilizando técnicas de Machine Learning em Python. O maior desafio do projeto consiste em lidar com o desbalanceamento extremo dos dados, onde as fraudes representam apenas uma fração mínima do total de transações.

## Objetivo:

* Identificar com precisão transações fraudulentas (Classe 1), minimizando falsos positivos para evitar transtornos a clientes legítimos e garantindo uma alta taxa de captura de fraudes reais.

---

## O Dataset:

Os dados utilizados foram obtidos a partir do repositório público do TensorFlow. 
* **Total de registros:** 284.807 transações
* **Variáveis (31 colunas):** * `Time`: Tempo em segundos decorrido entre a transação atual e a primeira transação.
  * `V1, V2, ..., V28`: Componentes principais obtidos através de PCA (técnica para proteger a privacidade dos dados originais).
  * `Amount`: Valor da transação.
  * `Class`: Variável alvo (0 para transações normais, 1 para fraudes).

### Desbalanceamento dos Dados
* **Transações normais (Classe 0):** 99,83%
* **Transações fraudulentas (Classe 1):** 0,17%

---

## Tecnologias e Técnicas Utilizadas:

* **Linguagem:** Python
* **Manipulação e Análise de Dados:** `pandas`, `numpy`
* **Pré-processamento e Escalonamento:** `StandardScaler` (Scikit-Learn)
* **Tratamento de Desbalanceamento:** `SMOTE` (Imbalanced-Learn) e balanceamento nativo via algoritmos (`scale_pos_weight`, `class_weight`).
* **Modelos de Machine Learning Testados:**
  * Regressão Logística (`LogisticRegression`)
  * Random Forest (`RandomForestClassifier`)
  * XGBoost (`XGBClassifier`)
  * K-Nearest Neighbors (`KNeighborsClassifier`)

---

## Experimentos e Resultados:

Para avaliar o desempenho na detecção de fraudes, o foco principal foi a análise das métricas da **Classe 1 (Fraude)** no `classification_report`:

| Modelo | Cenário / Tratamento | Precision | Recall | F1-Score |
| :--- | :--- | :---: | :---: | :---: |
| Logistic Regression | Com SMOTE (Balanceado) | 0.12 | 0.92 | 0.22 |
| Logistic Regression | Dados Originais | 0.85 | 0.66 | 0.74 |
| Random Forest | Com SMOTE (Balanceado) | 0.61 | **0.96** | 0.75 |
| Random Forest | Dados Originais | 0.83 | 0.81 | 0.82 |
| **XGBClassifier** | **Sem SMOTE (Com `scale_pos_weight=10`)** | **0.90** | **0.82** | **0.86** |
| KNN | Com SMOTE (Balanceado) | 0.05 | 0.99 | 0.10 |
| KNN | Dados Originais | 0.92 | 0.07 | 0.14 |

---

## Conclusão:

O **XGBClassifier** foi consagrado como o **melhor modelo** para este cenário. 

Enquanto técnicas como o SMOTE inflaram o *Recall* (sensibilidade), elas geraram um volume inaceitável de alarmes falsos (baixa *Precisão*). O XGBoost, configurado com o parâmetro `scale_pos_weight=10`, conseguiu o melhor equilíbrio do projeto, entregando um **F1-Score de 0.86**, garantindo que 90% dos alertas gerados sejam fraudes reais (*Precision*) e capturando 82% do total de crimes cometidos no período de teste (*Recall*).

---

