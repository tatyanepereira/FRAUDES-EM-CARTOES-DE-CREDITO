# 💳 Detecção de Fraudes em Cartões de Crédito com Machine Learning

> **Projeto Desenvolvido no Bootcamp DIO & Afya**  
> *Foco: Análise Avançada de Dados, Tratamento de Desbalanceamento de Classes e Detecção de Anomalias.*

---

## 📌 1. Introdução & Contextualização
O objetivo deste projeto é desenvolver um modelo preditivo capaz de identificar transações fraudulentas em cartões de crédito, minimizando o prejuízo financeiro de instituições e protegendo o consumidor final. 

A detecção de fraudes é um dos pilares mais críticos em **FinTechs** e engenharia de segurança, onde o principal desafio reside na identificação de comportamentos atípicos imersos em bilhões de transações legítimas diárias.

---

## 📊 2. O Conjunto de Dados
O estudo utiliza dados reais de transações realizadas por titulares de cartões europeus em setembro de 2013.

* **Total de Transações:** 284.807
* **Transações Fraudulentas:** 492
* **Proporção de Fraude:** `0,172%` (Classe Altamente Desbalanceada)
* **Características (Features):** Devido a questões de privacidade, os dados passaram por uma transformação PCA (Componentes Principais), resultando nas variáveis numéricas de `V1` a `V28`. As únicas variáveis não transformadas são `Time` (Tempo) e `Amount` (Valor da transação).

---

## 🛠️ 3. Análise Exploratória & Pré-processamento

### 📦 Importação das Dependências
O ambiente foi configurado com as principais bibliotecas do ecossistema Python para Ciência de Dados e Machine Learning Imbalanceado:

<img width="639" alt="Importação de Bibliotecas" src="https://github.com/user-attachments/assets/3f0e005b-8939-47f4-9643-f74ee88b9cd1" />

### 🔍 Carga e Inspeção dos Dados
O pipeline inicial realiza a leitura do dataset estruturado e valida as dimensões e integridade dos tipos de dados:

<img width="600" alt="Leitura dos Dados" src="https://github.com/user-attachments/assets/8c8b620d-0a77-43e6-b713-3facab84ff7c" />
<img width="867" alt="Estrutura do DataFrame" src="https://github.com/user-attachments/assets/64c92166-79f6-45a8-a99c-216bc7c374f6" />
<img width="163" alt="Verificação de Nulos" src="https://github.com/user-attachments/assets/a1a95b52-5fc2-4cba-b21e-9478d6c134f8" />

### 📈 Análise de Distribuição e Engenharia de Recursos
A análise estatística inicial confirma a severa assimetria entre as classes. Além disso, mapeou-se o comportamento do vetor `Amount`, identificando picos específicos de valor associados a condutas fraudulentas.

<img width="295" alt="Distribuição de Classes" src="https://github.com/user-attachments/assets/5bc12a0d-b6ff-462c-aa72-b8e47bf188ae" />
<img width="345" alt="Análise de Feature Amount" src="https://github.com/user-attachments/assets/e9a46c6c-f412-4d25-a957-dec1c844493d" />

Para mitigar a disparidade de escala da variável `Amount` frente às componentes PCA, aplicou-se a **Normalização dos Dados**, garantindo estabilidade numérica aos algoritmos:

<img width="515" alt="Normalização" src="https://github.com/user-attachments/assets/47e6f3ba-def2-408d-b01b-20f1873cdf50" />

---

## 🤖 4. Modelagem Preditiva & Abordagens

### 🚀 Abordagem 1: Detecção de Anomalias com Isolation Forest
Como estratégia inicial (*baseline* não supervisionada), utilizou-se o **Isolation Forest**, algoritmo ideal para isolar anomalias em conjuntos de dados volumosos sem a necessidade inicial de balanceamento artificial.

Abaixo, a configuração do modelo e a geração do vetor de predição `IF_Pred`:

<img width="828" alt="Treinamento Isolation Forest" src="https://github.com/user-attachments/assets/0d628128-370a-48df-96d5-5992ada54fdc" />
<img width="401" alt="Valores Preditos" src="https://github.com/user-attachments/assets/d5908bf2-7ac3-4ec0-92b6-0629b49726d8" />

#### 📊 Avaliação do Isolation Forest
A matriz de confusão evidencia a capacidade do modelo em separar as classes, embora apresente espaço para otimização de falsos alarmes:

<img width="403" alt="Matriz de Confusão IF" src="https://github.com/user-attachments/assets/b3d2e022-8c45-4204-8178-fd49e573f9b2" />

| Métrica | Desempenho |
| :--- | :--- |
| **Acurácia** | 1.00 |
| **Precisão** | 0.29 |
| **Recall (Sensibilidade)** | 0.28 |

---

### 🚀 Abordagem 2: Aprendizado Supervisionado com SMOTE + Random Forest
Para elevar o poder de generalização do sistema, estruturou-se um pipeline robusto utilizando técnica de **Oversampling (SMOTE)** para criar dados sintéticos da classe minoritária, combinado com o poder de conjunto do **Random Forest Classifier**:

<img width="582" alt="Pipeline SMOTE + Random Forest" src="https://github.com/user-attachments/assets/3f2422bc-c05d-4741-a70d-c3d62fff03bf" />

#### 🔄 Validação Cruzada (Cross-Validation)
Para garantir a robustez contra o *overfitting*, o pipeline foi submetido a uma validação cruzada rigorosa. Os resultados médios obtidos foram:

* **Acurácia Média:** `1.00`
* **Precisão Média:** `0.49`
* **Recall Médio:** `0.84` 🔥 *(Aumento significativo na capacidade de capturar fraudes reais)*

---

## 🏁 5. Conclusões & Próximos Passos

* **Desafio do Desbalanceamento:** Métricas tradicionais como a Acurácia ocultam falhas em dados desbalanceados. O foco deve ser sempre a relação **Precisão x Recall**.
* **SMOTE como Diferencial:** A introdução do SMOTE no pipeline da Random Forest reduziu drasticamente os Falsos Negativos, elevando o **Recall para 84%**, fator crítico para o negócio (deixar uma fraude passar custa mais caro do que uma checagem manual extra).
* **Isolation Forest:** Excelente para um deploy rápido e análise exploratória inicial de anomalias frias.

---

## 🧠 Competências Adquiridas
* Manipulação de Dados Desbalanceados (`imbalanced-learn`)
* Engenharia de Atributos e Normalização de Escala
* Algoritmos Supervisionados e Não-Supervisionados
* Validação Cruzada Avançada para Prevenção de Data Leakage
