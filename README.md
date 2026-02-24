# 🚕 Previsão de Cancelamento de Corridas (Machine Learning)

## 📌 Visão Geral do Projeto

Cancelamentos de corridas representam um problema relevante para plataformas de transporte, impactando a experiência do usuário, a eficiência operacional e a receita.

Este projeto tem como objetivo prever a probabilidade de cancelamento de uma corrida **antes que ela aconteça**, utilizando técnicas de **Análise Exploratória de Dados (EDA)** e **Machine Learning**.

Mais do que treinar um modelo com boa acurácia, o foco foi **entender o comportamento dos dados**, **tomar decisões conscientes ao longo do processo** e **avaliar trade-offs entre métricas**, especialmente *recall* e *falsos positivos*, sob uma ótica de negócio.

---

## 🧠 Motivação e Pergunta Central

A pergunta que guiou o projeto foi:

> **“É possível identificar, com antecedência razoável, corridas com alto risco de cancelamento sem gerar um volume excessivo de alarmes falsos?”**

Na prática, prever cancelamentos pode permitir ações como:

* incentivos ao motorista
* ajustes de preço
* benefícios ao passageiro

Porém, **falsos positivos têm custo**. Oferecer descontos ou benefícios para corridas que não seriam canceladas pode gerar prejuízo — e esse dilema orientou várias decisões do projeto.

---

## 📂 Estrutura dos Dados

Durante o desenvolvimento, foram utilizados **dois datasets distintos**, cada um com um propósito claro:

### 🔹 `base` — Dataset Original

* Dados crus, sem imputações
* Mantém distribuições reais, outliers e valores ausentes
* Utilizado exclusivamente para **Análise Exploratória (EDA)**

> 📌 Essa decisão garante interpretações honestas e evita distorções visuais ou estatísticas.

### 🔹 `df_model` — Dataset para Modelagem

Criado a partir de uma cópia da base original, contendo:

* imputação de valores ausentes (mediana)
* seleção de features
* engenharia de variáveis
* utilizado apenas para **treinamento e avaliação do modelo**

Essa separação evita misturar **análise de negócio** com dados artificialmente tratados para Machine Learning.

---

## 📊 Análise Exploratória de Dados (EDA)

A EDA teve como foco:

* diferenças entre corridas canceladas e não canceladas
* análise de distribuições (boxplots, histogramas)
* investigação de correlações
* padrões temporais (hora do dia, dia da semana)

📌 **Decisão importante:**
Todos os gráficos e conclusões da EDA foram feitos usando a **base original**, sem imputações, garantindo que os padrões observados refletissem o comportamento real dos dados.

---

## 🤖 Modelagem

O modelo escolhido foi o **XGBoost (XGBClassifier)**, devido à sua:

* robustez com dados tabulares
* capacidade de lidar com relações não lineares
* bom desempenho em problemas de classificação desbalanceada

### Features utilizadas:

* Distância da corrida
* Tempo médio até o embarque (VTAT)
* Tempo médio da corrida (CTAT)
* Valor da corrida
* Variáveis temporais (hora, dia da semana, horário de pico, fim de semana)
* Método de pagamento (dummies)

---

## 📈 Avaliação e Métricas

A avaliação inicial utilizou o **threshold padrão (0.5)**, apresentando:

* alta acurácia (~94%)
* bom equilíbrio geral
* recall razoável para cancelamentos

### 🎯 Foco no Recall

Como o objetivo principal é identificar cancelamentos, o *recall* da classe positiva recebeu atenção especial.

Foram testados:

* diferentes thresholds (0.25, 0.30, 0.35, 0.40)
* ajustes no `scale_pos_weight`

📌 **Insight importante:**
Reduzir o threshold aumenta o recall, mas **explode o número de falsos positivos**, o que não é aceitável do ponto de vista de negócio.

---

## ✅ Decisão Final

O **threshold 0.35** foi considerado o melhor compromisso entre:

* aumento moderado de recall
* controle de falsos positivos
* manutenção da acurácia e estabilidade do modelo

---

## ⚖️ Trade-offs de Negócio

Este projeto reforçou um ponto essencial:

* Melhor métrica ≠ melhor decisão
* Recall muito alto pode gerar custos excessivos
* Acurácia isolada é enganosa
* Modelos precisam ser avaliados no **contexto operacional real**

Todas as decisões foram tomadas de forma consciente e documentadas ao longo do projeto.

---

## 🛠️ Tecnologias Utilizadas

* Python
* Pandas, NumPy
* Matplotlib, Seaborn
* Scikit-learn
* XGBoost

---

## 📌 Possíveis Próximos Passos

* Análise de custo entre falso positivo vs falso negativo
* Curva Precision-Recall com foco em decisão operacional
* Testes com outros modelos (LightGBM, CatBoost)
* Simulação de impacto financeiro

---

## 📬 Contato

Se quiser conversar sobre o projeto, ideias ou melhorias, fique à vontade para entrar em contato.

---

# 🚕 Ride Cancellation Prediction (Machine Learning)

## 📌 Project Overview

Ride cancellations are a significant challenge for mobility platforms, affecting user experience, operational efficiency, and revenue.

This project aims to predict the likelihood of a ride being canceled **before it happens**, using **Exploratory Data Analysis (EDA)** and **Machine Learning** techniques.

Beyond achieving high accuracy, the main focus was on **understanding the data**, **making conscious modeling decisions**, and **evaluating trade-offs between recall and false positives from a business perspective**.

---

## 🧠 Motivation and Core Question

The guiding question of this project was:

> **“Can we identify rides with a high risk of cancellation early, without generating an excessive number of false alarms?”**

In practice, predicting cancellations may enable:

* driver incentives
* pricing adjustments
* passenger benefits

However, false positives have real costs, which strongly influenced the modeling decisions.

---

## 📂 Data Structure

Two datasets were used, each with a clear role:

### 🔹 `base` — Original Dataset

* Raw data without imputations
* Preserves real distributions, outliers, and missing values
* Used only for Exploratory Data Analysis

This ensures honest interpretation of patterns and visualizations.

### 🔹 `df_model` — Modeling Dataset

Derived from the original dataset, including:

* median imputation
* selected features
* feature engineering
* used exclusively for model training and evaluation

---

## 📊 Exploratory Data Analysis (EDA)

EDA focused on:

* differences between canceled and non-canceled rides
* distribution analysis
* correlation exploration
* temporal patterns (hour, weekday, peak times)

All EDA was performed using the original dataset, without imputation, to avoid distorted insights.

---

## 🤖 Modeling

The chosen model was **XGBoost (XGBClassifier)** due to its:

* strong performance on tabular data
* ability to model non-linear relationships
* robustness with imbalanced classes

---

## 📈 Evaluation and Metrics

* Initial evaluation used the default threshold (0.5)
* Multiple thresholds were tested to improve recall
* `scale_pos_weight` adjustments were explored

---

## ✅ Final Decision

A **0.35 threshold** was selected as the best balance between:

* recall improvement
* false positive control
* overall model stability

---

## ⚖️ Business Trade-offs

This project highlights an important lesson:

* The best metric is not always the best decision
* All modeling choices were made considering real operational impact, not just numerical optimization

---

## 🛠️ Tools and Libraries

* Python
* Pandas, NumPy
* Matplotlib, Seaborn
* Scikit-learn
* XGBoost

---

## 📌 Possible Next Steps

* Cost-sensitive evaluation
* Precision-Recall optimization
* Alternative model testing
* Financial impact simulation
