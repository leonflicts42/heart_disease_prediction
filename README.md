# 🫀 Heart Disease Prediction — MVP de Classificação

> Projeto de experimentação e construção de um modelo baseline (MVP) para predição de doenças cardíacas, utilizando o dataset UCI Heart Disease.

---

## 📋 Descrição

Este projeto implementa um pipeline completo de ciência de dados — da análise exploratória até o treinamento e persistência de um modelo de machine learning — com foco em boas práticas de preparação de dados e rastreamento de experimentos com **MLFlow**.

O objetivo é prever a **presença ou ausência de doença cardíaca** em pacientes a partir de variáveis clínicas e laboratoriais.

---

## 🗂️ Estrutura do Projeto

```
projeto/
├── notebooks/
│   └── exercicio_experimentacao_mvp.ipynb   # Notebook principal
├── data/
│   └── heart_disease_uci_preprocessed.csv   # Dataset pré-processado (gerado pelo notebook)
├── models/
│   └── baseline_model.joblib                # Modelo treinado serializado
└── README.md
```

---

## 📊 Sobre o Dataset

| Atributo        | Valor |
|----------------|-------|
| **Fonte**       | [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/45/heart+disease) via [Kaggle](https://www.kaggle.com/datasets/redwankarimsony/heart-disease-data/data) |
| **Subconjuntos**| Cleveland, Hungarian, Switzerland, VA Long Beach |
| **Observações** | 920 pacientes |
| **Features**    | 16 colunas (inclui metadados) |
| **Target**      | `num` → binarizado em `target` (0 = sem doença, 1 = com doença) |

### Variáveis do Dataset

| Coluna      | Descrição |
|-------------|-----------|
| `age`       | Idade do paciente (anos) |
| `sex`       | Sexo (Male / Female) |
| `cp`        | Tipo de dor no peito (typical angina, atypical angina, non-anginal, asymptomatic) |
| `trestbps`  | Pressão arterial em repouso (mmHg) |
| `chol`      | Colesterol sérico (mg/dl) |
| `fbs`       | Glicemia em jejum > 120 mg/dl (True/False) |
| `restecg`   | Resultado do ECG em repouso |
| `thalch`    | Frequência cardíaca máxima alcançada |
| `exang`     | Angina induzida por exercício (True/False) |
| `oldpeak`   | Depressão do segmento ST induzida por exercício |
| `slope`     | Inclinação do segmento ST de pico |
| `ca`        | Número de vasos principais coloridos por fluoroscopia (0–3) |
| `thal`      | Estado talassêmico (normal, fixed defect, reversable defect) |
| `num`       | Diagnóstico original (0–4); binarizado como `target` |

---

## 🔬 Etapas do Pipeline

### 1. Configuração do Ambiente
Importação das bibliotecas essenciais: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`.

### 2. Carregamento e Inspeção dos Dados
- Leitura do dataset (920 × 16)
- Verificação de tipos, shape e primeiras linhas

### 3. Análise Exploratória de Dados (EDA)
- Distribuição da variável alvo
- Análise de **missing values** com visualização em gráfico de barras
- Identificação de **outliers** com IQR e boxplots
- Análise estatística descritiva das variáveis numéricas

### 4. Preparação dos Dados
- **Tratamento de valores ausentes**: imputação por mediana (numéricas) e moda (categóricas)
- **Tratamento de outliers**: remoção com base no método IQR
- **Binarização do target**: `num > 0` → `target = 1`
- **Remoção de colunas**: coluna `dataset` descartada
- **One-hot encoding** das variáveis categóricas: `sex`, `cp`, `restecg`, `slope`, `thal`

### 5. Modelagem e Experimentação (MVP)
- Divisão treino/teste: 80% / 20% (`random_state=42`)
- Modelo baseline: **Regressão Logística**
- Rastreamento de experimentos com **MLFlow**

### 6. Persistência
- Dataset pré-processado salvo em `data/heart_disease_uci_preprocessed.csv`
- Modelo serializado via **joblib** em `models/baseline_model.joblib`
- Modelo também registrado no MLFlow

---

## 📈 Resultados do Modelo Baseline

| Métrica            | Valor  |
|--------------------|--------|
| **Train Accuracy** | 82,47% |
| **Test Accuracy**  | 80,43% |
| **F1 Score**       | 83,02% |
| **Precision**      | 85,44% |
| **Recall**         | 80,73% |
| **Overfitting**    | 0,0204 |

> O modelo apresenta boa generalização, com overfitting mínimo (diferença de ~2% entre treino e teste).

---

## 🛠️ Tecnologias e Bibliotecas

| Biblioteca     | Uso |
|---------------|-----|
| `pandas`       | Manipulação de dados |
| `numpy`        | Operações numéricas |
| `matplotlib`   | Visualizações |
| `seaborn`      | Visualizações estatísticas |
| `scipy`        | Análise estatística |
| `scikit-learn` | Modelagem, métricas e pré-processamento |
| `mlflow`       | Rastreamento de experimentos |
| `joblib`       | Serialização do modelo |

---

## ⚙️ Como Executar

### Pré-requisitos

- Python 3.12
- Ambiente virtual configurado (recomendado)

### Criar um ambiente virtual compatível com as bibliotecas 
```py -3.12 -m venv venv
source venv/bin/activate  # No Windows: venv\Scripts\activate
```

### Instalação das dependências
```pip install -r requirements.txt
```
### Ou então:

### Instalação das dependências
```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn mlflow joblib
```

### Executar o notebook

```bash
jupyter notebook notebooks/exercicio_experimentacao_mvp.ipynb
```

### Visualizar experimentos no MLFlow

```bash
mlflow ui
```
Acesse em: [http://localhost:5000](http://localhost:5000)

---

## 📌 Observações

- O experimento MLFlow é registrado sob o nome `heart_disease_classification`, com a run `logistic_regression_baseline`.
- O modelo baseline serve como ponto de partida; próximos experimentos podem explorar algoritmos como Random Forest, Gradient Boosting e SVM.
- O dataset consolidado da UCI contém dados de quatro centros médicos, o que aumenta a diversidade das amostras mas também a quantidade de valores ausentes.

---

## 📚 Referências

- [UCI Heart Disease Dataset](https://archive.ics.uci.edu/dataset/45/heart+disease)
- [Kaggle — Heart Disease Data](https://www.kaggle.com/datasets/redwankarimsony/heart-disease-data/data)
- [MLFlow Documentation](https://mlflow.org/docs/latest/index.html)
- [scikit-learn Documentation](https://scikit-learn.org/stable/)

