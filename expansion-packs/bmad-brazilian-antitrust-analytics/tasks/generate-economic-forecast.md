<!-- Powered by BMAD™ Core -->

# generate-economic-forecast

**Elicit:** true
**Interactive:** true

## GERAÇÃO DE PREVISÕES ECONÔMICAS PARA O MERCADO BRASILEIRO

Este processo guiado gera previsões econômicas avançadas com foco em indicadores brasileiros e análise de mercado.

### PASSO 1: OBJETIVO DA PREVISÃO
Defina o objetivo da sua previsão econômica:

**Tipo de Previsão:**
- [ ] **Previsão de Séries Temporais:** Prever valores futuros de variáveis econômicas
- [ ] **Nowcasting:** Estimação em tempo real de indicadores correntes
- [ ] **Previsão Condicional:** Previsão sob cenários específicos
- [ ] **Previsão de Probabilidades:** Probabilidade de eventos econômicos
- [ ] **Previsão de Setor:** Previsões específicas para setores brasileiros
- [ ] **Previsão de Mercado:** Previsões para mercados específicos

**Variável Alvo:**
- **Qual variável você quer prever?** (ex: PIB, inflação, taxa de juros, preços)
- **Horizonte de Previsão:** Quantos períodos à frente?
- **Frequência:** Diária, semanal, mensal, trimestral, anual
- **Unidade:** Qual a unidade de medida (R$, %, índice, etc.)?

### PASSO 2: DADOS E CONTEXTO
Configure os dados e contexto da previsão:

**Fontes de Dados Brasileiros:**
- **IBGE:** PIB, IPCA, emprego, produção industrial
- **BACEN:** Selic, câmbio, inflação, agregados monetários
- **IPEA:** Indicadores sociais e econômicos
- **FGV:** IGP-M, IGP-10, expectativas de mercado
- **ANBIMA:** Taxas de títulos, índices de bolsa
- **Setoriais:** Dados específicos do setor analisado

**Características dos Dados:**
- **Período Histórico:** Quantos períodos de dados históricos?
- **Qualidade:** Dados completos, com valores faltantes, revisões?
- **Sazonalidade:** Padrões sazonais identificados?
- **Eventos Especiais:** Crises, mudanças políticas, eventos extremos?
- **Tendências:** Tendências de longo prazo identificadas?

**Contexto Econômico Atual:**
- **Cenário Macroeconômico:** Situação atual da economia brasileira
- **Política Monetária:** Posição atual do Copom e expectativas
- **Política Fiscal:** Situação fiscal e expectativas
- **Contexto Global:** Influências internacionais relevantes
- **Riscos Principais:** Principais riscos para a previsão

### PASSO 3: METODOLOGIA DE PREVISÃO
Selecione a abordagem metodológica:

**Linguagem Preferida:**
- [ ] **R (Recomendado para séries temporais)** - Melhor para econometria tradicional
- [ ] **Python** - Para ML e integrações complexas
- [ ] **Ambos** - R para métodos tradicionais, Python para ML quando necessário

**Abordagem de Séries Temporais:**
```r
# R implementation for economic forecasting
library(forecast)
library(vars)
library(urca)
library(tseries)
library(fpp2)
library(prophet)
library(tsfeatures)
library(MLmetrics)

# Step 1: Exploratory analysis and testing
comprehensive_time_series_analysis <- function(ts_data) {
  results <- list()

  # Basic decomposition
  results$decomposition <- decompose(ts_data)
  results$stl <- stl(ts_data, s.window = "periodic")

  # Stationarity tests - Comprehensive approach
  results$stationarity <- list(
    adf = adf.test(ts_data),
    kpss = kpss.test(ts_data),
    pp = pp.test(ts_data),
    za = ur.za(ts_data, model = "both", lag = 2)
  )

  # Seasonality tests
  results$seasonality <- list(
    friedman = friedman.test(matrix(ts_data, ncol = frequency(ts_data))),
    qs = nsdiffs(ts_data),
    ocsb <- ocsb.test(ts_data)
  )

  # Feature extraction
  results$features <- tsfeatures(ts_data)

  return(results)
}

# Step 2: Model selection and estimation
forecast_models <- function(train_data, h = 12, include_ml = TRUE) {
  models <- list()

  # Traditional time series models
  models$arima <- auto.arima(train_data, stepwise = FALSE, approximation = FALSE)
  models$ets <- ets(train_data)
  models$tbats <- tbats(train_data)
  models$stl_ets <- stlf(train_data, h = h)

  # VAR models for multivariate forecasting
  if (is.matrix(train_data) || is.data.frame(train_data)) {
    var_select <- VARselect(train_data, type = "const")
    models$var <- VAR(train_data, p = var_select$selection["SC(n)"], type = "const")
  }

  # Advanced models
  models$prophet <- prophet(data.frame(ds = time(train_data), y = train_data))

  # Machine learning models (optional)
  if (include_ml) {
    models$ml <- ml_forecast_models(train_data, h)
  }

  return(models)
}

# Step 3: Machine learning integration
ml_forecast_models <- function(ts_data, h) {
  library(caret)
  library(xgboost)
  library(randomForest)

  # Create lagged features
  create_lagged_features <- function(ts, lags = 12) {
    n <- length(ts)
    features <- data.frame(y = ts)

    for (lag in 1:lags) {
      features[[paste0("lag_", lag)]] <- c(rep(NA, lag), ts[1:(n-lag)])
    }

    # Add calendar features
    features$month <- cycle(time(ts))
    features$quarter <- (cycle(time(ts)) - 1) %/% 3 + 1
    features$year <- as.numeric(format(time(ts), "%Y"))

    return(features)
  }

  # Prepare data
  features_df <- create_lagged_features(ts_data)
  features_df <- features_df[complete.cases(features_df), ]

  # Split data
  train_size <- floor(0.8 * nrow(features_df))
  train_df <- features_df[1:train_size, ]
  test_df <- features_df[(train_size + 1):nrow(features_df), ]

  # XGBoost model
  xgb_model <- xgboost(
    data = as.matrix(train_df[, -1]),
    label = train_df$y,
    nrounds = 100,
    objective = "reg:squarederror"
  )

  # Random Forest model
  rf_model <- randomForest(y ~ ., data = train_df, ntree = 500)

  return(list(xgb = xgb_model, rf = rf_model))
}

# Step 4: Model evaluation and selection
model_evaluation <- function(models, test_data, h) {
  evaluations <- list()

  # Generate forecasts
  forecasts <- list()
  for (model_name in names(models)) {
    if (model_name == "xgb" || model_name == "rf") {
      # Handle ML models differently
      forecasts[[model_name]] <- predict_ml_model(models[[model_name]], test_data, h)
    } else if (model_name == "prophet") {
      future <- make_future_dataframe(models[[model_name]], periods = h)
      forecasts[[model_name]] <- predict(models[[model_name]], future)$yhat[(nrow(test_data)+1):(nrow(test_data)+h)]
    } else {
      forecasts[[model_name]] <- forecast(models[[model_name]], h = h)$mean
    }
  }

  # Calculate accuracy metrics
  accuracy_metrics <- function(actual, predicted) {
    data.frame(
      MAE = MAE(actual, predicted),
      RMSE = RMSE(actual, predicted),
      MAPE = MAPE(actual, predicted),
      MASE = MASE(actual, predicted),
      Theil = theil(actual, predicted)
    )
  }

  for (model_name in names(forecasts)) {
    evaluations[[model_name]] <- accuracy_metrics(test_data, forecasts[[model_name]])
  }

  return(list(forecasts = forecasts, evaluations = evaluations))
}
```

**Abordagem de Nowcasting:**
```r
# Nowcasting for Brazilian economic indicators
nowcasting_models <- function(high_freq_data, low_freq_target) {
  library(midasr)
  library(dlm)
  library(nowcasting)

  # Mixed Data Sampling (MIDAS) approach
  midas_model <- midas_r(low_freq_target ~ mls(high_freq_data, 0:3, 12),
                         data = list(low_freq_target = low_freq_target,
                                    high_freq_data = high_freq_data),
                         start = list(0.5))

  # Dynamic Factor Model (DFM)
  dfm_model <- nowcast_dfm(data, target_var, factors = 3)

  # Bridge equations
  bridge_model <- bridge_equations(high_freq_data, low_freq_target)

  return(list(
    midas = midas_model,
    dfm = dfm_model,
    bridge = bridge_model
  ))
}

# Real-time updating with new information
update_forecast <- function(model, new_data, method = "bayesian") {
  if (method == "bayesian") {
    # Bayesian updating
    posterior <- bayesian_update(model, new_data)
    updated_forecast <- generate_forecast(posterior)
  } else if (method == "kalman") {
    # Kalman filter updating
    updated_model <- kalman_update(model, new_data)
    updated_forecast <- forecast(updated_model, h = 1)$mean
  }

  return(updated_forecast)
}
```

### PASSO 4: ANÁLISE DE SÉRIES TEMPORAIS
Realize análise completa das séries temporais:

**Análise Exploratória:**
- **Decomposição:** Tendência, sazonalidade, componente irregular
- **Autocorrelação:** ACF e PACF para identificar padrões
- **Estacionaridade:** Testes ADF, KPSS, PP, Zivot-Andrews
- **Heterocedasticidade:** Testes ARCH/GARCH
- **Não Linearidade:** Testes de não linearidade (Terasvirta, White)

**Características Específicas do Brasil:**
- **Sazonalidade Brasileira:** Padrões sazonais específicos (carnaval, natal, etc.)
- **Efeitos de Calendário:** Feriados, eventos esportivos, eleições
- **Volatilidade:** Padrões de volatilidade em mercados brasileiros
- **Efeitos de Política:** Impacto de decisões do Copom, políticas fiscais

**Análise de Relações:**
- **Correlação Cruzada:** Relações entre variáveis econômicas
- **Cointegração:** Relações de longo prazo entre variáveis
- **Causalidade:** Testes de causalidade de Granger
- **Transmissão de Choques:** Funções resposta ao impulso

### PASSO 5: MODELAGEM AVANÇADA
Implemente modelos avançados quando necessário:

**Modelos de Volatilidade:**
```r
# Volatility modeling for Brazilian financial data
volatility_models <- function(returns_data) {
  library(rugarch)
  library(fGarch)

  # GARCH family models
  garch_spec <- ugarchspec(
    variance.model = list(model = "sGARCH", garchOrder = c(1, 1)),
    mean.model = list(armaOrder = c(0, 0), include.mean = TRUE),
    distribution.model = "std"
  )

  garch_fit <- ugarchfit(garch_spec, returns_data)

  # EGARCH for asymmetric effects
  egarch_spec <- ugarchspec(
    variance.model = list(model = "eGARCH", garchOrder = c(1, 1)),
    mean.model = list(armaOrder = c(1, 1), include.mean = TRUE),
    distribution.model = "std"
  )

  egarch_fit <- ugarchfit(egarch_spec, returns_data)

  # GJR-GARCH for leverage effects
  gjr_spec <- ugarchspec(
    variance.model = list(model = "gjrGARCH", garchOrder = c(1, 1)),
    mean.model = list(armaOrder = c(0, 0), include.mean = TRUE),
    distribution.model = "std"
  )

  gjr_fit <- ugarchfit(gjr_spec, returns_data)

  return(list(garch = garch_fit, egarch = egarch_fit, gjr = gjr_fit))
}

# Forecast volatility
forecast_volatility <- function(vol_model, h = 10) {
  ugarchforecast(vol_model, n.ahead = h)
}
```

**Modelos de Estado-Espaço:**
```r
# State-space models for Brazilian economic indicators
state_space_models <- function(ts_data) {
  library(dlm)
  library(KFAS)

  # Local level model
  local_level <- dlmModPoly(order = 1) + dlmModSeas(12)

  # Local linear trend model
  local_trend <- dlmModPoly(order = 2)

  # Structural time series model
  structural_model <- dlmModPoly(order = 1) +
    dlmModSeas(12) +
    dlmModReg(cycle(time(ts_data)), addInt = FALSE)

  # Bayesian structural time series
  bsts_model <- bsts(ts_data ~ .,
                    state.specification = AddLocalLinearTrend(list(),
                    AddSeasonal(list(), nseasons = 12)),
                    niter = 1000)

  return(list(
    local_level = local_level,
    local_trend = local_trend,
    structural = structural_model,
    bsts = bsts_model
  ))
}
```

**Modelos de Machine Learning:**
```python
# Python implementation for ML forecasting
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestRegressor, GradientBoostingRegressor
from sklearn.neural_network import MLPRegressor
from sklearn.model_selection import TimeSeriesSplit
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import mean_absolute_error, mean_squared_error
import xgboost as xgb
import lightgbm as lgb

class BrazilianEconomicForecaster:
    def __init__(self):
        self.models = {}
        self.scalers = {}
        self.feature_names = []

    def create_features(self, data, lags=12):
        """Create features for Brazilian economic forecasting"""
        features = pd.DataFrame(index=data.index)
        target = data.values

        # Lag features
        for lag in range(1, lags + 1):
            features[f'lag_{lag}'] = data.shift(lag)

        # Rolling statistics
        for window in [3, 6, 12]:
            features[f'rolling_mean_{window}'] = data.rolling(window=window).mean()
            features[f'rolling_std_{window}'] = data.rolling(window=window).std()

        # Calendar features for Brazilian context
        features['month'] = data.index.month
        features['quarter'] = data.index.quarter
        features['year'] = data.index.year
        features['is_carnival'] = self.is_carnival_period(data.index)
        features['is_election_year'] = data.index.year % 4 == 2  # Brazilian elections

        # Macroeconomic features (if available)
        if hasattr(self, 'macro_data'):
            features = pd.concat([features, self.macro_data.reindex(data.index)], axis=1)

        return features, target

    def is_carnival_period(self, dates):
        """Identify carnival period in Brazilian calendar"""
        carnival = pd.DatetimeIndex(dates.year.map(lambda x: self.get_carnival_dates(x)))
        return dates.isin(carnival)

    def get_carnival_dates(self, year):
        """Get carnival dates for a given year (simplified)"""
        # This is a simplified version - in practice, use proper calculation
        easter = pd.to_datetime(f"{year}-04-{self.get_easter_sunday(year)}")
        carnival = easter - pd.Timedelta(days=47)
        return carnival

    def get_easter_sunday(self, year):
        """Calculate Easter Sunday (simplified)"""
        # Implement proper Easter calculation
        return "01"  # Placeholder

    def train_models(self, train_data, validation_split=0.2):
        """Train multiple ML models for forecasting"""
        features, target = self.create_features(train_data)

        # Remove rows with NaN values
        mask = features.notna().all(axis=1)
        features = features[mask]
        target = target[mask]

        # Split data
        split_idx = int(len(features) * (1 - validation_split))
        X_train, X_val = features.iloc[:split_idx], features.iloc[split_idx:]
        y_train, y_val = target[:split_idx], target[split_idx:]

        # Scale features
        scaler = StandardScaler()
        X_train_scaled = scaler.fit_transform(X_train)
        X_val_scaled = scaler.transform(X_val)

        self.feature_names = features.columns.tolist()
        self.scalers['feature_scaler'] = scaler

        # Train models
        models_config = {
            'xgboost': xgb.XGBRegressor(n_estimators=100, random_state=42),
            'lightgbm': lgb.LGBMRegressor(n_estimators=100, random_state=42),
            'random_forest': RandomForestRegressor(n_estimators=100, random_state=42),
            'gradient_boosting': GradientBoostingRegressor(n_estimators=100, random_state=42),
            'neural_network': MLPRegressor(hidden_layer_sizes=(50, 25), random_state=42)
        }

        for name, model in models_config.items():
            print(f"Training {name}...")
            model.fit(X_train_scaled, y_train)
            self.models[name] = model

        # Validate models
        validation_results = {}
        for name, model in self.models.items():
            val_pred = model.predict(X_val_scaled)
            mae = mean_absolute_error(y_val, val_pred)
            rmse = np.sqrt(mean_squared_error(y_val, val_pred))
            validation_results[name] = {'MAE': mae, 'RMSE': rmse}

        return validation_results

    def forecast(self, last_data, h=12):
        """Generate forecasts using trained models"""
        forecasts = {}

        for name, model in self.models.items():
            # Create features recursively
            forecasts[name] = []
            current_data = last_data.copy()

            for step in range(h):
                features, _ = self.create_features(pd.Series(current_data))
                features_scaled = self.scalers['feature_scaler'].transform(features.tail(1))

                pred = model.predict(features_scaled)[0]
                forecasts[name].append(pred)

                # Update data for next prediction
                current_data = np.append(current_data[1:], pred)

        return forecasts
```

### PASSO 6: COMBINAÇÃO DE MODELOS
Combine múltiplos modelos para previsões robustas:

**Métodos de Combinação:**
```r
# Model combination methods
model_combination <- function(forecasts, actuals, method = "weighted_average") {
  if (method == "simple_average") {
    combined_forecast <- rowMeans(do.call(cbind, forecasts))
  } else if (method == "weighted_average") {
    # Calculate weights based on historical performance
    weights <- calculate_performance_weights(forecasts, actuals)
    combined_forecast <- as.matrix(do.call(cbind, forecasts)) %*% weights
  } else if (method == "bayesian_model_averaging") {
    combined_forecast <- bayesian_model_averaging(forecasts, actuals)
  } else if (method == "stacking") {
    combined_forecast <- stacking_ensemble(forecasts, actuals)
  }

  return(combined_forecast)
}

# Performance-based weighting
calculate_performance_weights <- function(forecasts, actuals, metric = "RMSE") {
  n_models <- length(forecasts)
  errors <- numeric(n_models)

  for (i in 1:n_models) {
    if (metric == "RMSE") {
      errors[i] <- sqrt(mean((forecasts[[i]] - actuals)^2))
    } else if (metric == "MAE") {
      errors[i] <- mean(abs(forecasts[[i]] - actuals))
    } else if (metric == "MAPE") {
      errors[i] <- mean(abs((forecasts[[i]] - actuals) / actuals)) * 100
    }
  }

  # Inverse error weighting
  weights <- (1 / errors) / sum(1 / errors)
  return(weights)
}

# Bayesian Model Averaging
bayesian_model_averaging <- function(forecasts, actuals) {
  # Simplified BMA implementation
  n_models <- length(forecasts)
  model_performance <- numeric(n_models)

  for (i in 1:n_models) {
    # Calculate BIC as approximation to marginal likelihood
    residuals <- forecasts[[i]] - actuals
    n_params <- 2  # Simplified assumption
    bic <- length(actuals) * log(mean(residuals^2)) + n_params * log(length(actuals))
    model_performance[i] <- -bic
  }

  # Calculate posterior model probabilities
  model_probs <- exp(model_performance - max(model_performance))
  model_probs <- model_probs / sum(model_probs)

  # Weighted average
  combined_forecast <- as.matrix(do.call(cbind, forecasts)) %*% model_probs
  return(combined_forecast)
}
```

### PASSO 7: AVALIAÇÃO E VALIDAÇÃO
Avalie a qualidade das previsões:

**Métricas de Avaliação:**
```r
# Comprehensive forecast evaluation
forecast_evaluation <- function(forecasts, actuals) {
  evaluations <- list()

  for (model_name in names(forecasts)) {
    pred <- forecasts[[model_name]]
    actual <- actuals

    # Basic metrics
    evaluations[[model_name]] <- list(
      MAE = mean(abs(pred - actual)),
      RMSE = sqrt(mean((pred - actual)^2)),
      MAPE = mean(abs((pred - actual) / actual)) * 100,
      MASE = mean(abs(pred - actual)) / mean(abs(diff(actual))),
      Theil_U = sqrt(mean((pred - actual)^2)) / sqrt(mean(actual^2)),
      R_squared = 1 - sum((pred - actual)^2) / sum((actual - mean(actual))^2)
    )

    # Directional accuracy
    evaluations[[model_name]]$directional_accuracy <- mean(
      sign(diff(pred)) == sign(diff(actual))
    )

    # Economic value (for financial forecasts)
    evaluations[[model_name]]$economic_value <- calculate_economic_value(pred, actual)
  }

  return(evaluations)
}

# Diebold-Mariano test for forecast comparison
diebold_mariano_test <- function(forecast1, forecast2, actual) {
  loss_diff <- (forecast1 - actual)^2 - (forecast2 - actual)^2
  dm_stat <- mean(loss_diff) / (sd(loss_diff) / sqrt(length(loss_diff)))
  p_value <- 2 * pt(abs(dm_stat), df = length(loss_diff) - 1)

  return(list(statistic = dm_stat, p_value = p_value))
}
```

**Validação Cruzada:**
```r
# Time series cross-validation
time_series_cv <- function(data, forecast_func, h = 12, initial_window = 60) {
  n <- length(data)
  cv_errors <- list()

  for (i in seq(initial_window, n - h, by = h)) {
    train <- data[1:i]
    test <- data[(i + 1):(i + h)]

    forecast <- forecast_func(train, h = h)
    error <- forecast - test

    cv_errors[[length(cv_errors) + 1]] <- error
  }

  return(cv_errors)
}
```

### PASSO 8: INTERVALOS DE CONFIANÇA E INCERTEZA
Estime intervalos de confiança e incerteza:

**Métodos de Intervalos:**
```r
# Confidence interval estimation
confidence_intervals <- function(forecasts, model, method = "analytical", h = 12) {
  if (method == "analytical") {
    # Analytical confidence intervals
    se <- sqrt(predict(model, h = h, se.fit = TRUE)$se.fit^2)
    lower <- forecasts - qnorm(0.975) * se
    upper <- forecasts + qnorm(0.975) * se

  } else if (method == "bootstrap") {
    # Bootstrap confidence intervals
    boot_results <- bootstrap_forecast(model, h = h, n_boot = 1000)
    lower <- apply(boot_results, 1, quantile, probs = 0.025)
    upper <- apply(boot_results, 1, quantile, probs = 0.975)

  } else if (method == "simulation") {
    # Simulation-based intervals
    sim_results <- simulate_forecast_paths(model, h = h, n_sims = 1000)
    lower <- apply(sim_results, 1, quantile, probs = 0.025)
    upper <- apply(sim_results, 1, quantile, probs = 0.975)
  }

  return(list(
    forecast = forecasts,
    lower = lower,
    upper = upper,
    method = method
  ))
}

# Bootstrap forecasting
bootstrap_forecast <- function(model, h, n_boot = 1000) {
  boot_forecasts <- matrix(NA, nrow = h, ncol = n_boot)

  for (i in 1:n_boot) {
    # Resample residuals
    residuals <- residuals(model)
    boot_residuals <- sample(residuals, size = h, replace = TRUE)

    # Generate bootstrap forecast
    boot_forecast <- forecast(model, h = h)$mean + boot_residuals
    boot_forecasts[, i] <- boot_forecast
  }

  return(boot_forecasts)
}
```

### PASSO 9: ANÁLISE DE CENÁRIOS
Desenvolva previsões sob diferentes cenários:

**Análise de Cenários:**
```r
# Scenario analysis for Brazilian economic forecasting
scenario_analysis <- function(baseline_forecast, scenarios) {
  results <- list()

  for (scenario_name in names(scenarios)) {
    scenario <- scenarios[[scenario_name]]

    # Apply scenario adjustments
    adjusted_forecast <- baseline_forecast

    for (adjustment in scenario$adjustments) {
      if (adjustment$type == "multiplicative") {
        adjusted_forecast <- adjusted_forecast * adjustment$factor
      } else if (adjustment$type == "additive") {
        adjusted_forecast <- adjusted_forecast + adjustment$value
      }
    }

    results[[scenario_name]] <- list(
      forecast = adjusted_forecast,
      description = scenario$description,
      probability = scenario$probability,
      key_assumptions = scenario$assumptions
    )
  }

  return(results)
}

# Define Brazilian economic scenarios
brazilian_scenarios <- function() {
  list(
    baseline = list(
      description = "Cenário base com políticas atuais",
      probability = 0.5,
      assumptions = c("Selic estável", "Política fiscal consistente", "Câmbio ordem"),
      adjustments = list()
    ),
    optimistic = list(
      description = "Cenário otimista com reformas e crescimento",
      probability = 0.2,
      assumptions = c("Reformas aprovadas", "Confiança empresarial alta", "Investimento forte"),
      adjustments = list(
        list(type = "multiplicative", factor = 1.1),
        list(type = "additive", value = 0.5)
      )
    ),
    pessimistic = list(
      description = "Cenário pessimista com instabilidade",
      probability = 0.3,
      assumptions = c("Instabilidade política", "Risco fiscal elevado", "Juros altos"),
      adjustments = list(
        list(type = "multiplicative", factor = 0.9),
        list(type = "additive", value = -0.3)
      )
    )
  )
}
```

### PASSO 10: RELATÓRIO E VISUALIZAÇÃO
Gere relatórios e visualizações:

**Visualizações de Previsão:**
```r
# Visualization of Brazilian economic forecasts
visualize_forecasts <- function(forecasts, actuals, conf_intervals = NULL) {
  library(ggplot2)
  library(plotly)

  # Create forecast plot
  plot_data <- data.frame(
    date = time(actuals),
    actual = as.numeric(actuals)
  )

  # Add forecasts
  for (model_name in names(forecasts)) {
    forecast_dates <- seq(max(time(actuals)) + 1/frequency(actuals),
                        length.out = length(forecasts[[model_name]]),
                        by = 1/frequency(actuals))

    forecast_df <- data.frame(
      date = forecast_dates,
      forecast = forecasts[[model_name]],
      model = model_name
    )

    plot_data <- rbind(plot_data, forecast_df)
  }

  # Create ggplot
  p <- ggplot(plot_data, aes(x = date)) +
    geom_line(aes(y = actual, color = "Actual"), size = 1) +
    geom_line(aes(y = forecast, color = model), size = 1, linetype = "dashed") +
    labs(title = "Previsões Econômicas - Brasil",
         x = "Data", y = "Valor",
         color = "Série") +
    theme_minimal() +
    scale_color_manual(values = c("Actual" = "black",
                                 "ARIMA" = "blue",
                                 "ETS" = "red",
                                 "Prophet" = "green"))

  # Add confidence intervals if available
  if (!is.null(conf_intervals)) {
    ci_data <- data.frame(
      date = conf_intervals$dates,
      lower = conf_intervals$lower,
      upper = conf_intervals$upper
    )

    p <- p + geom_ribbon(data = ci_data,
                        aes(ymin = lower, ymax = upper),
                        alpha = 0.2, fill = "gray")
  }

  return(p)
}
```

---

## PRÓXIMOS PASSOS

Com base nas suas respostas, eu vou:

1. **Analisar Dados:** Realizar análise completa das séries temporais brasileiras
2. **Estimar Modelos:** Implementar múltiplos modelos de previsão (tradicionais e ML)
3. **Combinar Previsões:** Usar métodos de combinação para previsões robustas
4. **Calcular Intervalos:** Estimar intervalos de confiança e medidas de incerteza
5. **Analisar Cenários:** Desenvolver previsões sob diferentes cenários econômicos
6. **Validar Resultados:** Realizar validação cruzada e testes de robustez
7. **Gerar Relatório:** Produzir relatório completo em português com visualizações

Pronto para gerar previsões econômicas avançadas para o mercado brasileiro! Por favor, forneça os detalhes da variável que você deseja prever e os dados disponíveis para que eu possa desenvolver um sistema de previsão robusto e contextualizado.