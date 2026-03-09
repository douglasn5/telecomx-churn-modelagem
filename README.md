# 📡 TelecomX — Análise e Predição de Evasão de Clientes (Churn)

> Pipeline completo de Data Science: ETL → EDA → Machine Learning para identificar e prever a evasão de clientes da TelecomX.

---

## 📋 Sobre o Projeto

A **TelecomX** é uma empresa fictícia de telecomunicações que enfrenta um alto índice de cancelamento de clientes. Este projeto é dividido em **duas partes**:

- **Parte 1:** ETL + Análise Exploratória de Dados (EDA)
- **Parte 2:** Modelagem Preditiva com Machine Learning

---

## 🗂️ Estrutura do Projeto

```
TelecomX/
│
├── TelecomX_BR.ipynb                  # Parte 1 — ETL + EDA + Relatório
├── TelecomX_Parte2_Modelagem.ipynb    # Parte 2 — ML + Avaliação + Conclusão
├── TelecomX_Data.json                 # Dados brutos (obtidos via API)
├── TelecomX_dicionario.md             # Dicionário de dados
└── README.md                          # Este arquivo
```

---

## 🚀 Como Executar

### ✅ Google Colab (recomendado)

1. Acesse [colab.research.google.com](https://colab.research.google.com)
2. **Arquivo → Fazer upload de notebook**
3. Selecione o `.ipynb` desejado
4. **Runtime → Run all**

> Os dados são carregados automaticamente via API do GitHub — sem necessidade de upload de arquivos.

### ✅ Localmente

```bash
pip install pandas numpy matplotlib seaborn scikit-learn imbalanced-learn requests
jupyter notebook
```

---

## 📦 Dependências

| Biblioteca | Uso |
|---|---|
| `pandas` | Manipulação de dados |
| `numpy` | Operações numéricas |
| `matplotlib` + `seaborn` | Visualizações |
| `scikit-learn` | Modelos de ML e métricas |
| `imbalanced-learn` | SMOTE (balanceamento de classes) |
| `requests` | Chamada à API |

---

## 📊 Parte 1 — ETL + EDA (`TelecomX_BR.ipynb`)

### Extração
- Dados carregados via API (GitHub raw) em JSON aninhado
- Normalização para DataFrame tabular (7.267 registros → 21 colunas)

### Transformação
- Remoção de 224 registros sem rótulo de Churn
- Conversão de tipos e correção de 11 valores nulos
- Criação de variáveis auxiliares: `Contas_Diarias`, `Faixa_Mensal`, `Qtd_Servicos`
- Padronização Yes/No → 1/0, One-Hot Encoding

### Análise Exploratória (8+ gráficos)
- Distribuição geral do Churn
- Churn por perfil demográfico, contrato, pagamento, internet
- Distribuição de tenure (tempo de contrato)
- Heatmap de correlação entre variáveis
- Impacto dos serviços adicionais na evasão

### Principais Resultados da EDA

| Fator | Taxa de Churn |
|---|---|
| Taxa geral | **~26,5%** |
| Contrato mês a mês | ~42,7% |
| Pagamento cheque eletrônico | ~45,3% |
| Internet fibra óptica | ~41,9% |
| Sem segurança online | ~41,8% |
| **Contrato bianual** | ~2,8% ✅ |

---

## 🤖 Parte 2 — Modelagem Preditiva (`TelecomX_Parte2_Modelagem.ipynb`)

### Pré-Processamento
- Remoção de colunas irrelevantes (`customerID`)
- **One-Hot Encoding** para variáveis categóricas
- Verificação e correção do desbalanceamento de classes (73%/27%)
- **SMOTE** — geração de exemplos sintéticos da classe minoritária
- **StandardScaler** — normalização para modelos sensíveis à escala
- Divisão **70% treino / 30% teste** com estratificação

### Modelos Treinados

| Modelo | Normalização | Característica |
|---|---|---|
| Regressão Logística | ✅ Sim | Baseline linear interpretável |
| Árvore de Decisão | ❌ Não | Regras visuais, simples |
| Random Forest | ❌ Não | Ensemble robusto, resistente a overfitting |
| Gradient Boosting | ❌ Não | Alta performance, aprendizado sequencial |

### Avaliação dos Modelos
- Métricas: Acurácia, Precisão, Recall, F1-Score, ROC-AUC
- Matrizes de confusão para cada modelo
- Curvas ROC comparativas
- Análise de overfitting (F1 treino vs teste)

### Importância das Variáveis
- Coeficientes da Regressão Logística
- Feature importance do Random Forest (impureza de Gini)
- Feature importance do Gradient Boosting
- **Ranking de consenso** entre os 3 modelos

### 🔑 Top Fatores de Churn Identificados
1. **Tipo de contrato** — mensal vs longo prazo (maior fator individual)
2. **Fibra óptica** — alto custo percebido
3. **Método de pagamento** — cheque eletrônico indica baixo engajamento
4. **Cobrança mensal elevada** — custo como gatilho de cancelamento
5. **Tenure baixo** — primeiros 12 meses são o período mais crítico

---

## 💡 Recomendações Estratégicas

| Ação | Prioridade |
|---|---|
| Usar modelo preditivo para identificar clientes em risco mensalmente | 🔴 Alta |
| Incentivos para migração de contrato mensal → anual nos primeiros 6 meses | 🔴 Alta |
| Programa de onboarding proativo nos primeiros 12 meses | 🔴 Alta |
| Revisar custo-benefício e qualidade percebida da fibra óptica | 🟠 Média |
| Incluir segurança online desde o plano básico | 🟠 Média |
| Estimular débito automático como padrão de pagamento | 🟡 Baixa |

---

## 🗃️ Dicionário de Dados

| Coluna | Descrição |
|---|---|
| `customerID` | ID único do cliente |
| `Churn` | Evasão: Yes / No |
| `gender` | Gênero do cliente |
| `SeniorCitizen` | Idoso ≥65 anos: 0 / 1 |
| `Partner` | Possui parceiro(a): Yes / No |
| `Dependents` | Possui dependentes: Yes / No |
| `tenure` | Meses de contrato |
| `Contract` | Tipo de contrato (mensal / anual / bianual) |
| `PaymentMethod` | Forma de pagamento |
| `InternetService` | Tipo de serviço de internet |
| `OnlineSecurity` | Segurança online contratada |
| `TechSupport` | Suporte técnico contratado |
| `Charges_Monthly` | Cobrança mensal (R$) |
| `Charges_Total` | Cobrança total acumulada (R$) |

> Consulte `TelecomX_dicionario.md` para a descrição completa de todas as colunas.

---

## 👤 Autor

Projeto desenvolvido como parte do desafio **Churn de Clientes — TelecomX**  
Equipe de Data Science · TelecomX

---

*🐍 Python · Pandas · NumPy · Matplotlib · Seaborn · Scikit-learn · Imbalanced-learn*
