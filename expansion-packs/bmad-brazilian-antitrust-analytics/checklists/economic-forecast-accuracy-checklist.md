# Checklist de Precisão de Previsões Econômicas
# Para Mercados Brasileiros e Análise de Concorrência

checklist_info:
  name: "Economic Forecast Accuracy Checklist"
  version: "1.0"
  author: "Econometrician Agent - Brazilian Antitrust Analytics"
  description: "Checklist abrangente para validação de precisão de previsões econômicas com foco em indicadores brasileiros"
  language: "pt-BR"
  created_date: "2024-01-01"
  last_updated: "2024-01-01"

# Seção 1: Validação de Dados de Entrada
input_data_validation:
  data_quality_checks:
    - item: "Verificar qualidade e cobertura temporal dos dados"
      required: true
      weight: 1.0
      notes: "Data quality and temporal coverage"

    - item: "Identificar e tratar outliers e valores extremos"
      required: true
      weight: 1.0
      r_code: "# Outlier detection
    library(forecast)
    ts_outliers <- tsoutliers(ts_data)
    clean_data <- ts_data[-ts_outliers$index]"

    - item: "Validar consistência de fontes brasileiras (IBGE, BACEN, IPEA)"
      required: true
      weight: 1.0
      notes: "Brazilian data source consistency"

  data_preprocessing:
    - item: "Aplicar transformações necessárias (log, diferenças, etc.)"
      required: true
      weight: 1.0
      r_code: "# Data transformations
    log_data <- log(ts_data)
    diff_data <- diff(ts_data)
    seasonal_diff <- diff(ts_data, lag = frequency(ts_data))"

    - item: "Tratar sazonalidade e efeitos de calendário brasileiros"
      required: true
      weight: 1.0
      r_code: "# Seasonal adjustment
    library(seasonal)
    seas_adj <- seas(ts_data)"

    - item: "Imputar valores missing com métodos apropriados"
      required: true
      weight: 1.0
      r_code: "# Missing data imputation
    library(imputeTS)
    imputed_data <- na_kalman(ts_data)"

  frequency_consistency:
    - item: "Verificar consistência de frequência temporal"
      required: true
      weight: 1.0
      notes: "Temporal frequency consistency"

    - item: "Ajustar para frequência de previsão desejada"
      required: true
      weight: 1.0
      r_code: "# Frequency conversion
    library(xts)
    monthly_data <- to.monthly(daily_data, OHLC = FALSE)
    quarterly_data <- to.quarterly(monthly_data, OHLC = FALSE)"

    - item: "Documentar mudanças de frequência na série histórica"
      required: true
      weight: 0.9
      notes: "Historical frequency changes"

# Seção 2: Análise Exploratória e Diagnóstico
exploratory_analysis:
  descriptive_statistics:
    - item: "Calcular estatísticas descritivas completas"
      required: true
      weight: 1.0
      r_code: "# Descriptive statistics
    summary_stats <- summary(ts_data)
    sd_value <- sd(ts_data, na.rm = TRUE)
    skew_value <- skewness(ts_data, na.rm = TRUE)
    kurt_value <- kurtosis(ts_data, na.rm = TRUE)"

    - item: "Analisar padrões de tendência, sazonalidade e ciclicidade"
      required: true
      weight: 1.0
      r_code: "# Decomposition analysis
    decomp <- stl(ts_data, s.window = 'periodic')
    plot(decomp)"

    - item: "Identificar quebras estruturais e mudanças de regime"
      required: true
      weight: 1.0
      r_code: "# Structural break detection
    library(strucchange)
    breakpoints <- breakpoints(ts_data ~ 1)
    summary(breakpoints)"

  stationarity_analysis:
    - item: "Realizar testes de estacionaridade (ADF, KPSS, PP, ZA)"
      required: true
      weight: 1.0
      r_code: "# Stationarity tests
    library(urca)
    adf_test <- ur.df(ts_data, type = 'drift', selectlags = 'AIC')
    kpss_test <- ur.kpss(ts_data, type = 'mu')
    pp_test <- ur.pp(ts_data)
    za_test <- ur.za(ts_data, model = 'both', lag = 2)"

    - item: "Determinar ordem de integração da série"
      required: true
      weight: 1.0
      notes: "Integration order determination"

    - item: "Testar estacionaridade em diferentes subamostras"
      required: true
      weight: 0.9
      notes: "Subsample stationarity tests"

  nonlinearity_analysis:
    - item: "Realizar testes de não linearidade (Terasvirta, White, etc.)"
      required: true
      weight: 0.9
      r_code: "# Nonlinearity tests
    library(tseries)
    terasvirta_test <- terasvirta.test(ts_data)
    white_test <- white.test(ts_data)
    keenan_test <- keenan.test(ts_data)"

    - item: "Estimar parâmetros de atrator (dimensão de embedding, tau)"
      required: true
      weight: 0.8
      r_code: "# Attractor parameter estimation
    library(tseriesChaos)
    embed_dim <- estimateEmbeddingDimension(ts_data)
    tau_param <- estimateTimeDelay(ts_data)"

    - item: "Testar dependência de longo prazo"
      required: true
      weight: 0.8
      r_code: "# Long memory testing
    library(fracdiff)
    hurst_exp <- hurstexp(ts_data)"

# Seção 3: Validação de Modelos de Previsão
model_validation:
  model_selection_criteria:
    - item: "Usar critérios de informação para seleção de modelos"
      required: true
      weight: 1.0
      r_code: "# Model selection
    library(forecast)
    auto_arima <- auto.arima(ts_data, stepwise = FALSE, approximation = FALSE)
    ets_model <- ets(ts_data)"

    - item: "Comparar múltiplos modelos candidatos"
      required: true
      weight: 1.0
      r_code: "# Model comparison
    models <- list(
      arima = auto_arima,
      ets = ets_model,
      theta = thetaf(ts_data),
      naive = naive(ts_data)
    )"

    - item: "Realizar validação cruzada temporal"
      required: true
      weight: 1.0
      r_code: "# Time series cross-validation
    library(forecast)
    cv_results <- tsCV(ts_data, forecastfunction = auto.arima, h = 12)"

  diagnostic_testing:
    - item: "Realizar testes de diagnóstico residual"
      required: true
      weight: 1.0
      r_code: "# Residual diagnostics
    library(lmtest)
    bp_test <- bptest(model)  # Heteroskedasticity
    dw_test <- dwtest(model)  # Autocorrelation
    jb_test <- jarque.bera.test(residuals(model))  # Normality"

    - item: "Verificar não autocorrelação residual"
      required: true
      weight: 1.0
      r_code: "# Autocorrelation tests
    library(forecast)
    acf_res <- Acf(residuals(model))
    ljung_box <- Box.test(residuals(model), lag = 12, type = 'Ljung-Box')"

    - item: "Testar homocedasticidade condicional"
      required: true
      weight: 1.0
      r_code: "# Conditional heteroskedasticity
    library(fGarch)
    garch_test <- lbtest(residuals(model)^2, lag = 12)"

  parameter_stability:
    - item: "Testar estabilidade de parâmetros ao longo do tempo"
      required: true
      weight: 0.9
      r_code: "# Parameter stability tests
    library(strucchange)
    efp_test <- efp(ts_data ~ 1, type = 'Rec-CUSUM')"

    - item: "Verificar previsibilidade em diferentes regimes"
      required: true
      weight: 0.9
      notes: "Different regime predictability"

    - item: "Realizar análise rolling forecast"
      required: true
      weight: 1.0
      r_code: "# Rolling forecast analysis
    rolling_forecasts <- rollapply(ts_data, width = window_size,
                                  FUN = function(x) forecast(auto.arima(x), h = h)$mean,
                                  align = 'right')"

# Seção 4: Validação de Precisão de Previsão
forecast_accuracy_validation:
  accuracy_metrics:
    - item: "Calcular métricas de erro (MAE, RMSE, MAPE, MASE)"
      required: true
      weight: 1.0
      r_code: "# Accuracy metrics
    library(forecast)
    accuracy_values <- accuracy(forecast_model, actual_values)
    print(accuracy_values)"

    - item: "Calcular métricas direcionais (DA, Theil's U)"
      required: true
      weight: 0.9
      r_code: "# Directional accuracy
    directional_accuracy <- mean(sign(diff(forecast_values)) == sign(diff(actual_values)))
    theil_u <- theil(forecast_values, actual_values)"

    - item: "Testar significância estatística da precisão"
      required: true
      weight: 0.9
      r_code: "# Statistical significance
    dm_test <- dm.test(actual_values, forecast1_values, forecast2_values)"

  benchmark_comparison:
    - item: "Comparar com modelos benchmark simples"
      required: true
      weight: 1.0
      r_code: "# Benchmark comparison
    naive_forecast <- naive(ts_data, h = h)
    seasonal_naive <- snaive(ts_data, h = h)
    benchmark_accuracy <- accuracy(model_forecast, actual_values)"

    - item: "Testar superioridade sobre modelos ingênuos"
      required: true
      weight: 1.0
      notes: "Superiority over naive models"

    - item: "Comparar com previsões de consenso (quando disponível)"
      required: false
      weight: 0.8
      notes: "Consensus forecast comparison"

  out_of_sample_validation:
    - item: "Realizar validação hold-out sample"
      required: true
      weight: 1.0
      r_code: "# Hold-out validation
    train_data <- window(ts_data, end = end_train)
    test_data <- window(ts_data, start = start_test)
    model <- auto.arima(train_data)
    forecast <- forecast(model, h = length(test_data))"

    - item: "Realizar validação walk-forward"
      required: true
      weight: 1.0
      r_code: "# Walk-forward validation
    wf_errors <- walk_forward_validation(ts_data, window_size = 24, h = 6)"

    - item: "Testar diferentes horizontes de previsão"
      required: true
      weight: 1.0
      notes: "Different forecast horizons"

# Seção 5: Validação de Intervalos de Previsão
interval_validation:
  interval_coverage:
    - item: "Verificar cobertura dos intervalos de previsão"
      required: true
      weight: 1.0
      r_code: "# Interval coverage
    forecast_obj <- forecast(model, h = h, level = 95)
    actual_in_interval <- actual_values >= forecast_obj$lower & actual_values <= forecast_obj$upper
    coverage_rate <- mean(actual_in_interval)"

    - item: "Calcular largura média dos intervalos"
      required: true
      weight: 0.9
      r_code: "# Interval width
    avg_width <- mean(forecast_obj$upper - forecast_obj$lower)"

    - item: "Testar calibração probabilística"
      required: true
      weight: 0.9
      notes: "Probabilistic calibration"

  multiple_intervals:
    - item: "Validar múltiplos níveis de confiança"
      required: true
      weight: 0.9
      r_code: "# Multiple confidence levels
    forecast_80 <- forecast(model, h = h, level = 80)
    forecast_90 <- forecast(model, h = h, level = 90)
    forecast_95 <- forecast(model, h = h, level = 95)"

    - item: "Analisar trade-off entre cobertura e precisão"
      required: true
      weight: 0.9
      notes: "Coverage-precision trade-off"

    - item: "Testar intervalos assimétricos quando aplicável"
      required: false
      weight: 0.7
      notes: "Asymmetric intervals"

# Seção 6: Validação de Combinação de Previsões
forecast_combination:
  combination_methods:
    - item: "Testar métodos de combinação de previsões"
      required: true
      weight: 0.9
      r_code: "# Forecast combination
    library(forecastComb)
    combined_forecast <- forecast_comb(forecasts_matrix, method = 'median')"

    - item: "Calcular pesos ótimos para combinação"
      required: true
      weight: 0.9
      r_code: "# Optimal weights
    library(Metrics)
    weights <- optimal_weights(forecasts_matrix, actual_values)"

    - item: "Comparar com previsões individuais"
      required: true
      weight: 1.0
      notes: "Comparison with individual forecasts"

  machine_learning_integration:
    - item: "Usar ML para otimização de pesos"
      required: true
      weight: 0.8
      r_code: "# ML for weight optimization
    library(xgboost)
    xgb_model <- xgboost(data = forecasts_matrix, label = actual_values,
                       objective = 'reg:squarederror')"

    - item: "Testar métodos de ensemble learning"
      required: true
      weight: 0.8
      notes: "Ensemble learning methods"

    - item: "Validar com cross-validation"
      required: true
      weight: 0.8
      notes: "Cross-validation validation"

# Seção 7: Validação de Cenários e Análise de Risco
scenario_validation:
  scenario_analysis:
    - item: "Desenvolver e validar múltiplos cenários"
      required: true
      weight: 0.9
      r_code: "# Scenario analysis
    scenarios <- data.frame(
      optimistic = forecast_optimistic,
      baseline = forecast_baseline,
      pessimistic = forecast_pessimistic
    )"

    - item: "Atribuir probabilidades a diferentes cenários"
      required: true
      weight: 0.9
      notes: "Scenario probabilities"

    - item: "Testar sensibilidade a suposições de cenário"
      required: true
      weight: 0.8
      notes: "Scenario assumption sensitivity"

  risk_assessment:
    - item: "Quantificar risco de previsão"
      required: true
      weight: 0.9
      r_code: "# Risk quantification
    risk_metrics <- calculate_risk_metrics(forecast_distribution, actual_values)"

    - item: "Realizar análise de estresse de cenários"
      required: true
      weight: 0.8
      notes: "Stress testing scenarios"

    - item: "Identificar principais fontes de incerteza"
      required: true
      weight: 0.9
      notes: "Uncertainty sources identification"

# Seção 8: Validação Específica para Contexto Brasileiro
brazilian_context_validation:
  institutional_factors:
    - item: "Incorporar eventos institucionais brasileiros"
      required: true
      weight: 1.0
      r_code: "# Brazilian institutional events
    events <- data.frame(
      date = as.Date(c('2020-03-01', '2021-01-01')),  # COVID, policy changes
      type = c('crisis', 'policy_change'),
      impact = c(-0.1, 0.05)
    )"

    - item: "Considerar ciclos políticos e eleitorais"
      required: true
      weight: 1.0
      notes: "Political and electoral cycles"

    - item: "Validar com mudanças regulatórias do CADE"
      required: true
      weight: 0.9
      notes: "CADE regulatory changes"

  economic_context:
    - item: "Controlar por volatilidade cambial brasileira"
      required: true
      weight: 1.0
      r_code: "# Exchange rate volatility
    usd_brl <- getFX('USD/BRL')
    exchange_vol <- volatility(returns(usd_brl))"

    - item: "Considerar inflação e política monetária"
      required: true
      weight: 1.0
      notes: "Inflation and monetary policy"

    - item: "Incorporar índices setoriais brasileiros"
      required: true
      weight: 0.9
      notes: "Brazilian sectoral indices"

  data_specific_validation:
    - item: "Validar com revisões de dados brasileiros"
      required: true
      weight: 1.0
      notes: "Brazilian data revisions"

    - item: "Considerar mudanças metodológicas do IBGE"
      required: true
      weight: 0.9
      notes: "IBGE methodological changes"

    - item: "Testar consistência com dados regionais"
      required: true
      weight: 0.8
      notes: "Regional data consistency"

# Seção 9: Validação de Nowcasting e High-Frequency
nowcasting_validation:
  high_frequency_data:
    - item: "Validar nowcasting com dados de alta frequência"
      required: true
      weight: 0.8
      r_code: "# Nowcasting with high-frequency data
    library(nowcasting)
    nowcast_model <- nowcasting(target_variable, high_freq_indicators)"

    - item: "Testar diferentes frequências de atualização"
      required: true
      weight: 0.8
      notes: "Different update frequencies"

    - item: "Comparar com previsões tradicionais"
      required: true
      weight: 0.8
      notes: "Traditional forecast comparison"

  mixed_frequency_validation:
    - item: "Validar modelos de frequência mista"
      required: true
      weight: 0.8
      r_code: "# Mixed-frequency models
    library(midasr)
    midas_model <- midas_r(target ~ mls(low_freq, 0:12, 12), data = data)"

    - item: "Testar diferentes métodos de agregação"
      required: true
      weight: 0.7
      notes: "Different aggregation methods"

    - item: "Verificar ganhos de informação de alta frequência"
      required: true
      weight: 0.8
      notes: "High-frequency information gains"

# Seção 10: Documentação e Relatório
documentation_validation:
  forecast_documentation:
    - item: "Documentar todas as suposições e limitações"
      required: true
      weight: 1.0
      notes: "Assumptions and limitations documentation"

    - item: "Fornecer código reprodutível completo"
      required: true
      weight: 1.0
      notes: "Complete reproducible code"

    - item: "Incluir visualizações comparativas"
      required: true
      weight: 0.9
      r_code: "# Comparative visualizations
    library(ggplot2)
    plot_data <- data.frame(
      date = time(actual_values),
      actual = as.numeric(actual_values),
      forecast = as.numeric(forecast_values),
      lower = forecast_intervals$lower,
      upper = forecast_intervals$upper
    )
    ggplot(plot_data, aes(x = date)) +
      geom_line(aes(y = actual, color = 'Actual')) +
      geom_line(aes(y = forecast, color = 'Forecast')) +
      geom_ribbon(aes(ymin = lower, ymax = upper), alpha = 0.2)"

  performance_documentation:
    - item: "Apresentar métricas de precisão completas"
      required: true
      weight: 1.0
      notes: "Complete accuracy metrics"

    - item: "Incluir análise de incerteza e risco"
      required: true
      weight: 0.9
      notes: "Uncertainty and risk analysis"

    - item: "Documentar melhorias iterativas"
      required: true
      weight: 0.8
      notes: "Iterative improvements documentation"

# Seção 11: Critérios de Aprovação
approval_criteria:
  mandatory_requirements:
    - item: "Todos os testes obrigatórios devem ser completados"
      required: true
      weight: 1.0
      notes: "Mandatory tests completion"

    - item: "Score mínimo de 80% em precisão de previsão"
      required: true
      weight: 1.0
      notes: "80% minimum accuracy score"

    - item: "Modelo deve superar benchmarks relevantes"
      required: true
      weight: 1.0
      notes: "Relevant benchmark superiority"

  quality_thresholds:
    - item: "MAPE < 10% para previsões de curto prazo"
      required: true
      weight: 0.9
      notes: "Short-term forecast MAPE threshold"

    - item: "Cobertura de intervalos > 90%"
      required: true
      weight: 0.9
      notes: "Interval coverage threshold"

    - item: "Precisão direcional > 60%"
      required: true
      weight: 0.8
      notes: "Directional accuracy threshold"

  brazilian_specific_criteria:
    - item: "Deve incorporar contexto econômico brasileiro"
      required: true
      weight: 1.0
      notes: "Brazilian economic context"

    - item: "Resultados devem ser úteis para políticas antitruste"
      required: true
      weight: 0.9
      notes: "Antitrust policy usefulness"

    - item: "Deve usar dados brasileiros apropriados"
      required: true
      weight: 1.0
      notes: "Appropriate Brazilian data"

---
## Instruções de Uso

### Como Utilizar Este Checklist

1. **Executar cada item de validação** e documentar resultados
2. **Calcular score ponderado** baseado nos pesos
3. **Priorizar validações obrigatórias** (required = true)
4. **Usar R preferencialmente** com comentários em inglês
5. **Considerar contexto brasileiro** em todas as análises

### Níveis de Qualidade de Previsão

- **Excelente:** MAPE < 5%, supera todos benchmarks, intervalos bem calibrados
- **Bom:** MAPE 5-10%, supera benchmarks principais, intervalos razoáveis
- **Aceitável:** MAPE 10-15%, equivalente a benchmarks, intervalos amplos
- **Precisa melhorar:** MAPE > 15%, inferior a benchmarks, intervalos muito amplos

### Prioridades de Validação

- **Crítico:** Testes de diagnóstico, validação out-of-sample, comparação com benchmarks
- **Importante:** Análise de sensibilidade, validação de intervalos, contexto brasileiro
- **Recomendado:** Combinação de modelos, análise de cenários, nowcasting

### Padrões Técnicos

- **Linguagem:** Português para documentação, inglês para código
- **Software:** R preferencialmente para previsão econométrica
- **Validação:** Abordagem múltipla com diferentes horizontes e métricas
- **Contexto:** Análise contextualizada para o mercado brasileiro