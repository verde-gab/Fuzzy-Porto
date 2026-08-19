# Fuzzy-Porto — Axiomatic Honesty in Time Series Forecasting

**A Escolha do Modelo como Decisão Axiomática: Misspecification e Fuzzy Time Series**

Pesquisa de Iniciação Científica (PIC) na [FATEC Baixada Santista "Rubens Lara"](https://fatecrl.edu.br/), Curso Superior de Tecnologia em Ciência de Dados.

**Autor:** Gabriel Gomes  
**Orientador:** Prof. Dr. Mauricio Conceição Mario

---

## Resumo

Esta pesquisa investiga a escolha de modelos de previsão para séries temporais como uma **decisão axiomática**, argumentando que todo modelo carrega premissas formais cujo respeito delimita o alcance legítimo de suas inferências. Propõe-se o conceito de **honestidade axiomática** — critério normativo segundo o qual um modelo é honesto quando nenhuma afirmação inferencial que dele se extrai excede o que suas premissas autorizam.

O caso motivador é o trabalho de Santos, Campos e Correa (2023), que aplica doze regressões lineares simples à série de movimentação de carga do Porto de Santos (2005–2022), sem auditar os resíduos. Uma bateria de testes de *misspecification* demonstra violação robusta de independência e homocedasticidade. Em contraste, aplica-se o modelo de Fuzzy Time Series de Chen (1996) e o PWFTS (Probabilistic Weighted FTS) aos mesmos dados.

### Resultados-Chave

| Modelo | RMSE (ton) | MAPE | Ljung-Box₁₂ (*p*) | Diagnóstico |
|---|---|---|---|---|
| OLS (12 modelos) | — | — | ≈ 0 | Viola Gauss-Markov |
| Sazonal Naïve | 1.863.004 | 10,34% | ≈ 0 | Enviesado |
| HW Aditivo | — | — | 0,342 | Passa |
| HW Multiplicativo | — | — | ≈ 0 | Falha |
| Chen FTS (Δy) | 1.835.156 | 9,05% | ≈ 0 | Falha (lags sazonais) |
| **PWFTS (Δ₁₂y, ord.3)** | **1.503.526** | **8,21%** | **0,280** | **Passa** |

---

## Estrutura do Repositório

```
Fuzzy-Porto/
├── notebooks/                ← Jupyter notebooks (pipeline completo)
│   ├── ETL.ipynb             ← Extração e transformação de dados
│   ├── EDA.ipynb             ← Análise exploratória (STL, ACF, ADF, KPSS)
│   ├── Forecasting.ipynb     ← Modelos FTS e clássicos
│   └── M-S_Testing.ipynb     ← Diagnóstico de resíduos e Gauss-Markov
├── data/
│   ├── processed/            ← Dados limpos/transformados
│   └── external/             ← Dados de referência (Santos et al., 2023)
└── requirements.txt          ← Dependências Python
```

## Tecnologias

- **Python 3** (pyFTS, statsmodels, pandas, matplotlib)
- **LaTeX** (abnTeX2, Beamer)
- **Docker** (jupyter/scipy-notebook)

## Referências Principais

- **Mayo, D. G.** (2018). *Statistical Inference as Severe Testing*. Cambridge University Press.
- **Spanos, A.** (2007). Curve Fitting, the Reliability of Inductive Inference. *Philosophy of Science*, 74(5).
- **Krause, D.** (2022). Models and modeling in science. *Principia*, 26(1).
- **Chen, S. M.** (1996). Forecasting enrollments based on fuzzy time series. *Fuzzy Sets and Systems*, 81(3).
- **Santos, D. C.; Campos, R. G.; Correa, J. S.** (2023). Porto de Santos: Análise da movimentação de carga. *Semana Acadêmica*, 11(237).

## Licença

Este repositório contém material de pesquisa acadêmica. Os notebooks e código são de uso livre para fins educacionais e de pesquisa.
