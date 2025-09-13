<!-- Powered by BMAD™ Core -->

# econometric-modeling

**Elicit:** true
**Interactive:** true

## MODELAGEM ECONOMÉTRICA PARA MERCADO BRASILEIRO E DETECÇÃO DE CARTEIS

Este processo guiado conduz modelagem econométrica avançada com foco em análise de mercado brasileiro e detecção de cartéis.

### PASSO 1: TIPO DE MODELO ECONOMÉTRICO
Selecione o tipo de modelo econométrico a ser desenvolvido:

**Categoria Principal:**
- [ ] **Modelos de Séries Temporais:** Análise de séries temporais econômicas
- [ ] **Modelos de Painel:** Análise de dados em painel para empresas/setores
- [ ] **Modelos de Inferência Causal:** Identificação de relações causais
- [ ] **Modelos de Cartel:** Detecção e análise de cartéis
- [ ] **Modelos de Fusão:** Simulação de efeitos competitivos
- [ ] **Modelos de Preços:** Análise de paralelismo de preços
- [ ] **Modelos de Licitações:** Análise de padrões de licitação

**Subcategoria Específica:**
1. **Séries Temporais:**
   - ARIMA/SARIMA para previsão
   - VAR/VECM para relações multivariadas
   - GARCH para volatilidade
   - Modelos de estado-espaço

2. **Inferência Causal:**
   - Variáveis instrumentais (2SLS, GMM)
   - Diferenças-em-Diferenças
   - Regressão Descontínua
   - Matching e Controle Sintético

3. **Detecção de Cartel:**
   - Testes de desvio de preços
   - Análise de variância de preços
   - Modelos de leilão
   - Screening de licitações

### PASSO 2: DADOS E VARIÁVEIS
Configure os dados e variáveis para modelagem:

**Fonte de Dados:**
- **IBGE:** Dados macroeconômicos e setoriais
- **BACEN:** Séries financeiras e monetárias
- **CADE:** Dados de investigações e casos
- **Portal da Transparência:** Dados de licitações
- **Fontes Setoriais:** Dados específicos do setor
- **Dados Coletados:** Dados primários da pesquisa

**Variável Dependente (Y):**
- **Definição:** O que você está tentando explicar ou prever?
- **Tipo:** Contínua, discreta, binária, contagem
- **Unidade:** Unidade de medida e escala
- **Frequência:** Diária, semanal, mensal, trimestral, anual

**Variáveis Independentes (X):**
- **Variáveis Chave:** Fatores principais que influenciam Y
- **Variáveis de Controle:** Fatores a serem controlados
- **Variáveis Instrumentais:** Para resolver endogeneidade
- **Dummies:** Efeitos fixos, sazonalidade, eventos

**Características dos Dados:**
- **Estrutura:** Cross-section, painel, séries temporais
- **Amostra:** Tamanho e período de análise
- **Qualidade:** Dados faltantes, outliers, viés
- **Disponibilidade:** Acesso e restrições de uso

### PASSO 3: METODOLOGIA ECONOMÉTRICA
Defina a metodologia econométrica detalhada:

**Linguagem Preferida:**
- [ ] **R (Recomendado)** - Melhor para econometria clássica e séries temporais
- [ ] **Python** - Para ML e integrações complexas
- [ ] **Ambos** - R para econometria, Python para ML quando necessário

**Abordagem de Séries Temporais (se aplicável):**
```r
# R implementation for time series analysis
library(forecast)
library(vars)
library(urca)
library(tseries)
library(ARDL)
library(nlts)

# Step 1: Stationarity testing - Multiple test approach
adf_test <- adf.test(data$y)
kpss_test <- kpss.test(data$y)
pp_test <- pp.test(data$y)
za_test <- ur.za(data$y, model = "both", lag = 2)

# Step 2: Start with VAR for understanding relationships
# Determine optimal lag length
var_lag_select <- VARselect(data, type = "const")
optimal_lag <- var_lag_select$selection["SC(n)"]

# Estimate VAR model
var_model <- VAR(data, p = optimal_lag, type = "const")

# Step 3: If VAR results are not satisfactory, use ARDL
# ARDL bounds testing approach
ardl_model <- ardl(y ~ x1 + x2 + x3, data = data, order = c(2,1,1,1))

# Step 4: Non-linear analysis if needed
# Test for non-linearity using multiple approaches
terasvirta_test <- terasvirta.test(data$y ~ data$x1)
white_test <- white.test(data$y ~ data$x1)
keenan_test <- keenan.test(data$y ~ data$x1)

# Step 5: Advanced causality testing
# Transfer Entropy for non-linear causality
library(RTransferEntropy)
te_result <- transfer_entropy(data$x1, data$y, nboot = 100)

# Convergence Cross Mapping for non-linear causal inference
library(rEDM)
ccm_result <- ccm(data, E = 3, lib_column = "x1", target_column = "y")
```

**Abordagem de Inferência Causal (se aplicável):**
```r
# Causal inference methods
library(AER)
library(ivreg)
library(did)
library(MatchIt)
library(synth)

# Instrumental Variables approach
iv_model <- ivreg(y ~ x1 + x2 + x3 | z1 + z2 + x3, data = data)
summary(iv_model, diagnostics = TRUE)

# Difference-in-Differences
did_model <- att_gt(
  yname = "y",
  gname = "group",
  tname = "period",
  data = data
)

# Regression Discontinuity
# Implement RD design with bandwidth selection
library(rdrobust)
rd_results <- rdrobust(y, x, c = 0, p = 1)

# Matching methods
match_obj <- matchit(treat ~ x1 + x2 + x3, data = data, method = "nearest")
match_data <- match.data(match_obj)
```

**Abordagem de Detecção de Cartel (se aplicável):**
```r
# Cartel detection methods
library(cartel)
library(auction)
library(microbenchmark)

# Price deviation tests for collusion screening
price_deviation_test <- function(prices, periods) {
  # Implementation of price deviation screening
  # Based on Harrington (2008) methodology
  mean_price <- mean(prices)
  std_price <- sd(prices)

  deviations <- abs(prices - mean_price) / std_price
  threshold <- qnorm(0.975)  # 95% confidence level

  return(list(
    suspicious_periods = which(deviations > threshold),
    deviation_scores = deviations,
    threshold = threshold
  ))
}

# Auction bid analysis
bid_analysis <- function(bids, auctions) {
  # Analyze bidding patterns for collusion
  # Based on Porter and Zona (1993) methodology

  results <- list()

  for (auction in unique(auctions$auction_id)) {
    auction_bids <- bids[bids$auction_id == auction, ]

    # Calculate bidding statistics
    n_bidders <- nrow(auction_bids)
    mean_bid <- mean(auction_bids$bid_amount)
    bid_variance <- var(auction_bids$bid_amount)

    # Detect potential cover bidding
    relative_bids <- auction_bids$bid_amount / mean_bid
    suspicious_bids <- relative_bids > 2.0  # Threshold for cover bidding

    results[[auction]] <- list(
      n_bidders = n_bidders,
      mean_bid = mean_bid,
      bid_variance = bid_variance,
      suspicious_bids = sum(suspicious_bids),
      max_relative_bid = max(relative_bids)
    )
  }

  return(results)
}
```

### PASSO 4: ANÁLISE EXPLORATÓRIA E PREPARAÇÃO
Realize análise exploratória e prepare os dados:

**Análise Descritiva:**
- **Estatísticas Básicas:** Média, mediana, desvio padrão, quartis
- **Visualizações:** Histogramas, boxplots, scatter plots, time series plots
- **Correlações:** Matriz de correlação, análise de multicolinearidade
- **Distribuições:** Testes de normalidade, análise de assimetria e curtose

**Tratamento de Dados:**
- **Dados Faltantes:** Estratégias de imputação ou remoção
- **Outliers:** Identificação e tratamento de valores extremos
- **Transformações:** Log, diferenciação, normalização
- **Sazonalidade:** Ajuste sazonal, dummies de tempo

**Testes Preliminares:**
- **Estacionaridade:** ADF, KPSS, PP, Zivot-Andrews
- **Cointegração:** Johansen, Engle-Granger
- **Heterocedasticidade:** Teste de White, Breusch-Pagan
- **Autocorrelação:** Durbin-Watson, Ljung-Box

### PASSO 5: ESPECIFICAÇÃO E ESTIMAÇÃO
Especifique e estime os modelos econométricos:

**Especificação do Modelo:**
- **Forma Funcional:** Linear, log-linear, log-log, não linear
- **Variáveis Incluídas:** Seleção de variáveis baseada em teoria
- **Estrutura de Erros:** Homocedástico, heterocedástico, autocorrelacionado
- **Efeitos Fixos/Aleatórios:** Para modelos de painel

**Métodos de Estimação:**
- **OLS:** Mínimos quadrados ordinários
- **GLS:** Mínimos quadrados generalizados
- **ML:** Máxima verossimilhança
- **GMM:** Método generalizado dos momentos
- **Bayesian:** Métodos bayesianos

**Seleção de Modelo:**
- **Critérios de Informação:** AIC, BIC, HQIC
- **Testes de Hipóteses:** F, t, Wald, LR
- **Validação Cruzada:** Out-of-sample testing
- **Análise de Resíduos:** Diagnóstico de modelo

### PASSO 6: VALIDAÇÃO E ROBUSTEZ
Valide e teste a robustez dos resultados:

**Testes de Diagnóstico:**
- **Normalidade dos Resíduos:** Shapiro-Wilk, Jarque-Bera
- **Heterocedasticidade:** White, Breusch-Pagan, Goldfeld-Quandt
- **Autocorrelação:** Durbin-Watson, Ljung-Box, Breusch-Godfrey
- **Multicolinearidade:** VIF, condition number

**Análise de Sensibilidade:**
- **Especificação Alternativa:** Diferentes formas funcionais
- **Amostras Diferentes:** Subamostras, períodos diferentes
- **Variáveis de Controle:** Inclusão/exclusão de controles
- **Métodos Alternativos:** Comparação com outros métodos

**Validação Externa:**
- **Out-of-Sample:** Previsão fora da amostra
- **Cross-Validation:** K-fold validation
- **Bootstrap:** Intervalos de confiança robustos
- **Comparação com Literatura:** Benchmarking com estudos existentes

### PASSO 7: NÃO LINEARIDADE E ANÁLISE AVANÇADA
Implemente análises avançadas quando necessário:

**Testes de Não Linearidade:**
```r
# Comprehensive non-linearity testing
nonlinear_tests <- function(series) {
  library(tseriesChaos)
  library(nonlinearTseries)
  library(fractal)

  results <- list()

  # Terasvirta Neural Network Test
  results$terasvirta <- terasvirta.test(series ~ time(1:length(series)))

  # White Neural Network Test
  results$white <- white.test(series)

  # Keenan Test
  results$keenan <- keenan.test(series)

  # McLeod-Li Test
  results$mcleod_li <- McLeod.li.test(series)

  # Tsay Test
  results$tsay <- tsay.test(series)

  # BDS Test for independence
  results$bds <- bds.test(series)

  # Likelihood Ratio TAR Test
  results$tar <- tar.test(series)

  return(results)
}
```

**Análise de Memória Longa:**
```r
# Long memory process analysis
long_memory_analysis <- function(series) {
  library(fracdiff)
  library(longmemo)

  results <- list()

  # Hurst exponent estimation
  results$hurst <- hurstexp(series)

  # Geweke-Porter-Hudak estimator
  results$gph <- GPH(series)

  # Local Whittle estimator
  results$local_whittle <- localwhittle(series)

  # Robinson's Gaussian semiparametric estimator
  results$robinson <- robinson(series)

  return(results)
}
```

**Análise de Causalidade Não Linear:**
```r
# Non-linear causality analysis
nonlinear_causality <- function(x, y) {
  library(RTransferEntropy)
  library(rEDM)

  results <- list()

  # Transfer Entropy
  results$transfer_entropy <- transfer_entropy(x, y, nboot = 100)

  # Convergence Cross Mapping
  data_ccm <- data.frame(x = x, y = y)
  results$ccm <- ccm(data_ccm, E = 3, lib_column = "x", target_column = "y")

  # Granger non-linear causality
  results$nonlinear_granger <- nonlinearGrangerTest(x, y)

  return(results)
}
```

### PASSO 8: INTERPRETAÇÃO E RELATÓRIO
Interprete os resultados e gere relatórios:

**Interpretação Econômica:**
- **Significado Econômico:** Interpretar coeficientes em termos econômicos
- **Magnitude dos Efeitos:** Tamanho dos efeitos e importância prática
- **Implicações Causais:** Discutir identificação causal quando aplicável
- **Relevância para CADE:** Implicações para investigações antitruste

**Validação no Contexto Brasileiro:**
- **Contexto Institucional:** Considerar órgãos reguladores brasileiros
- **Especifidades Setoriais:** Características específicas do setor
- **Dados Brasileiros:** Considerar qualidade e características dos dados
- **Políticas Públicas:** Implicações para políticas brasileiras

**Relatório em Português:**
- **Sumário Executivo:** Principais descobertas e recomendações
- **Metodologia:** Descrição detalhada dos métodos utilizados
- **Resultados:** Apresentação clara dos resultados
- **Limitações:** Discussão honesta das limitações
- **Apêndice Técnico:** Detalhes adicionais e código

---

## PADRÕES DE CODIFICAÇÃO

### Padrões R (Preferido para Econometria)
```r
# Always comment code in English
# Use meaningful English variable names
library(tidyverse)
library(lubridate)

# Load and preprocess data
brazilian_market_data <- read_csv("data/market_data.csv") %>%
  mutate(date = ymd(date),
         log_price = log(price),
         price_change = c(NA, diff(price))) %>%
  filter(!is.na(price_change))

# Economic modeling functions
estimate_var_model <- function(data, variables, max_lag = 4) {
  # Select optimal lag length using information criteria
  lag_selection <- VARselect(data[, variables], type = "const")
  optimal_lag <- lag_selection$selection["SC(n)"]

  # Estimate VAR model
  var_model <- VAR(data[, variables], p = optimal_lag, type = "const")

  return(list(model = var_model,
              optimal_lag = optimal_lag,
              diagnostics = serial.test(var_model)))
}

# Comprehensive diagnostic testing
model_diagnostics <- function(model, data) {
  diagnostics <- list()

  # Residual diagnostics
  residuals <- residuals(model)
  diagnostics$normality <- jarque.bera.test(residuals)
  diagnostics$autocorrelation <- Box.test(residuals, lag = 12)
  diagnostics$heteroskedasticity <- bptest(model)

  # Model selection criteria
  diagnostics$aic <- AIC(model)
  diagnostics$bic <- BIC(model)

  return(diagnostics)
}
```

### Padrões Python (para ML e integrações complexas)
```python
# Python code for ML integration with econometrics
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import TimeSeriesSplit
from sklearn.metrics import mean_squared_error
import statsmodels.api as sm

# Load Brazilian market data
def load_brazilian_data(filepath):
    """Load and preprocess Brazilian market data"""
    data = pd.read_csv(filepath)
    data['date'] = pd.to_datetime(data['date'])
    data = data.set_index('date')
    return data

# Machine learning integration with econometric analysis
def ml_enhanced_econometrics(data, target_var, feature_vars):
    """
    Combine ML with traditional econometric methods
    for enhanced cartel detection
    """

    # Traditional econometric model
    X = sm.add_constant(data[feature_vars])
    y = data[target_var]

    ols_model = sm.OLS(y, X).fit()

    # ML model for non-linear patterns
    rf_model = RandomForestRegressor(n_estimators=100, random_state=42)

    # Time series cross-validation
    tscv = TimeSeriesSplit(n_splits=5)
    ml_scores = []

    for train_idx, test_idx in tscv.split(X):
        X_train, X_test = X.iloc[train_idx], X.iloc[test_idx]
        y_train, y_test = y.iloc[train_idx], y.iloc[test_idx]

        rf_model.fit(X_train, y_train)
        y_pred = rf_model.predict(X_test)
        score = mean_squared_error(y_test, y_pred)
        ml_scores.append(score)

    return {
        'ols_results': ols_model.summary(),
        'ml_performance': np.mean(ml_scores),
        'combined_insights': combine_insights(ols_model, rf_model)
    }
```

---

## PRÓXIMOS PASSOS

Com base nas suas respostas, eu vou:

1. **Preparar Dados:** Carregar e pré-processar os dados brasileiros
2. **Executar Análise Exploratória:** Realizar análise exploratória completa
3. **Especificar Modelos:** Definir especificações econométricas apropriadas
4. **Estimar Modelos:** Implementar métodos econométricos com código otimizado
5. **Validar Resultados:** Realizar testes abrangentes de validação e robustez
6. **Gerar Relatórios:** Produzir relatórios técnicos em português com código em inglês

Pronto para iniciar a modelagem econométrica! Por favor, forneça as informações sobre seus dados e objetivos para que eu possa desenvolver modelos econométricos robustos para análise de mercado brasileiro e detecção de cartéis.