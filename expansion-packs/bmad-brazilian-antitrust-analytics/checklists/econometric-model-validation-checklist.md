# Checklist de Validação de Modelos Econométricos
# Para Análise de Mercado Brasileiro e Detecção de Cartéis

checklist_info:
  name: "Econometric Model Validation Checklist"
  version: "1.0"
  author: "Econometrician Agent - Brazilian Antitrust Analytics"
  description: "Checklist abrangente para validação de modelos econométricos com foco em contexto brasileiro"
  language: "pt-BR"
  created_date: "2024-01-01"
  last_updated: "2024-01-01"

# Seção 1: Validação de Dados
data_validation:
  data_sources_verification:
    - item: "Verificar consistência das fontes de dados brasileiras (IBGE, BACEN, IPEA, FGV)"
      required: true
      weight: 1.0
      notes: "Cross-reference multiple sources when possible"

    - item: "Confirmar periodicidade e cobertura temporal dos dados"
      required: true
      weight: 1.0
      notes: "Check for gaps and irregular frequencies"

    - item: "Validar unidades de medida e conversões necessárias"
      required: true
      weight: 1.0
      notes: "Ensure consistent currency units and time periods"

  data_quality_checks:
    - item: "Identificar e documentar valores missing (NA)"
      required: true
      weight: 1.0
      notes: "Document percentage and pattern of missing data"

    - item: "Detecção de outliers e valores extremos"
      required: true
      weight: 1.0
      notes: "Use IQR, Z-score, and visual inspection"

    - item: "Verificar consistência geográfica e setorial"
      required: true
      weight: 1.0
      notes: "Check for changes in regional classifications"

  data_preprocessing:
    - item: "Aplicar transformações necessárias (log, diferenças, etc.)"
      required: true
      weight: 1.0
      notes: "Document all transformations applied"

    - item: "Tratar sazonalidade e efeitos de calendário brasileiros"
      required: true
      weight: 0.8
      notes: "Carnival, elections, holidays"

    - item: "Construir variáveis dummy para eventos específicos"
      required: false
      weight: 0.6
      notes: "Policy changes, crises, structural breaks"

# Seção 2: Análise de Séries Temporais
time_series_analysis:
  stationarity_testing:
    - item: "Realizar teste ADF (Augmented Dickey-Fuller)"
      required: true
      weight: 1.0
      r_code: "adf_test <- ur.df(ts_data, type = 'drift', selectlags = 'AIC')"

    - item: "Realizar teste KPSS (Kwiatkowski-Phillips-Schmidt-Shin)"
      required: true
      weight: 1.0
      r_code: "kpss_test <- ur.kpss(ts_data, type = 'mu')"

    - item: "Realizar teste PP (Phillips-Perron)"
      required: true
      weight: 1.0
      r_code: "pp_test <- ur.pp(ts_data)"

    - item: "Realizar teste Zivot-Andrews para quebras estruturais"
      required: true
      weight: 0.9
      r_code: "za_test <- ur.za(ts_data, model = 'both', lag = 2)"

    - item: "Documentar conclusões sobre estacionaridade"
      required: true
      weight: 1.0
      notes: "Synthesize results from all tests"

  seasonality_analysis:
    - item: "Analisar padrões sazonais com decomposição STL"
      required: true
      weight: 1.0
      r_code: "decomp <- stl(ts_data, s.window = 'periodic')"

    - item: "Testar significância sazonal com testes apropriados"
      required: true
      weight: 0.9
      notes: "Use seasonal dummy tests or spectral analysis"

    - item: "Identificar efeitos de calendário específicos do Brasil"
      required: true
      weight: 0.8
      notes: "Carnival, regional holidays, election cycles"

  nonlinearity_testing:
    - item: "Realizar teste Terasvirta Neural Network"
      required: true
      weight: 0.9
      r_code: "terasvirta_test <- terasvirta.test(ts_data)"

    - item: "Realizar teste White Neural Network"
      required: true
      weight: 0.9
      r_code: "white_test <- white.test(ts_data)"

    - item: "Realizar teste Keenan"
      required: true
      weight: 0.9
      r_code: "keenan_test <- keenan.test(ts_data)"

    - item: "Realizar teste McLeod-Li"
      required: true
      weight: 0.9
      r_code: "mcleod_li_test <- McLeod.li.test(ts_data)"

    - item: "Estimar dimensão de embedding e parâmetro tau"
      required: true
      weight: 0.8
      r_code: "embed_dim <- estimate_embedding_dimension(ts_data)"

# Seção 3: Especificação do Modelo
model_specification:
  functional_form:
    - item: "Justificar escolha da forma funcional (linear, log-linear, etc.)"
      required: true
      weight: 1.0
      notes: "Based on economic theory and data characteristics"

    - item: "Testar especificações alternativas"
      required: true
      weight: 0.9
      notes: "Compare different functional forms"

    - item: "Verificar linearidade vs não linearidade"
      required: true
      weight: 0.9
      notes: "Based on nonlinearity test results"

  variable_selection:
    - item: "Basear seleção de variáveis em teoria econômica"
      required: true
      weight: 1.0
      notes: "Economic justification for each variable"

    - item: "Realizar análise de correlação e multicolinearidade"
      required: true
      weight: 1.0
      r_code: "vif_values <- vif(model)"

    - item: "Considerar variáveis de controle relevantes"
      required: true
      weight: 1.0
      notes: "Include relevant institutional and policy controls"

  lag_selection:
    - item: "Usar critérios de informação para seleção de lags"
      required: true
      weight: 1.0
      r_code: "VARselect(data, type = 'const')"

    - item: "Verificar autocorrelação residual"
      required: true
      weight: 1.0
      r_code: "dw_test <- dwtest(model)"

    - item: "Testar diferentes ordens de defasagem"
      required: true
      weight: 0.9
      notes: "Compare models with different lag structures"

# Seção 4: Estimação e Diagnóstico
estimation_diagnostics:
  estimation_quality:
    - item: "Verificar convergência do algoritmo de estimação"
      required: true
      weight: 1.0
      notes: "Check optimization convergence"

    - item: "Examinar signo e magnitude dos coeficientes"
      required: true
      weight: 1.0
      notes: "Economic plausibility of estimates"

    - item: "Avaliar significância estatística dos coeficientes"
      required: true
      weight: 1.0
      notes: "Standard errors, t-statistics, p-values"

  residual_analysis:
    - item: "Testar normalidade dos resíduos (Jarque-Bera, Shapiro-Wilk)"
      required: true
      weight: 1.0
      r_code: "jarque_bera_test <- jarque.bera.test(residuals)"

    - item: "Testar heterocedasticidade (White, Breusch-Pagan)"
      required: true
      weight: 1.0
      r_code: "bp_test <- bptest(model)"

    - item: "Testar autocorrelação (Durbin-Watson, Ljung-Box)"
      required: true
      weight: 1.0
      r_code: "dw_test <- dwtest(model)"

    - item: "Analisar padrões de resíduos graficamente"
      required: true
      weight: 0.9
      notes: "Residual plots, ACF, PACF"

  model_fit:
    - item: "Calcular R² e R² ajustado"
      required: true
      weight: 1.0
      notes: "Goodness of fit measures"

    - item: "Calcular critérios de informação (AIC, BIC, HQIC)"
      required: true
      weight: 1.0
      notes: "Model selection criteria"

    - item: "Realizar análise de previsão out-of-sample"
      required: true
      weight: 0.9
      notes: "Out-of-sample forecasting performance"

# Seção 5: Validação de Robustez
robustness_validation:
  alternative_specifications:
    - item: "Estimar modelo com diferentes amostras"
      required: true
      weight: 1.0
      notes: "Subsample analysis, rolling windows"

    - item: "Testar diferentes métodos de estimação"
      required: true
      weight: 1.0
      notes: "OLS, IV, GMM, ML estimators"

    - item: "Incluir/excluir variáveis de controle"
      required: true
      weight: 1.0
      notes: "Sensitivity to control variables"

    - item: "Testar diferentes formas funcionais"
      required: true
      weight: 0.9
      notes: "Linear, log-linear, polynomial"

  external_validation:
    - item: "Comparar com resultados da literatura"
      required: true
      weight: 1.0
      notes: "Compare with existing studies"

    - item: "Validar com dados externos quando disponível"
      required: false
      weight: 0.8
      notes: "External data sources"

    - item: "Realizar validação cruzada"
      required: true
      weight: 0.9
      notes: "Cross-validation procedures"

  causal_inference_validation:
    - item: "Testar suposições de identificação"
      required: true
      weight: 1.0
      notes: "Identification assumptions"

    - item: "Realizar testes de placebo"
      required: true
      weight: 0.9
      notes: "Placebo tests"

    - item: "Verificar suposição de tendências paralelas"
      required: true
      weight: 0.9
      notes: "Parallel trends assumption"

# Seção 6: Validação Específica para Contexto Brasileiro
brazilian_context_validation:
  institutional_factors:
    - item: "Controlar por mudanças institucionais brasileiras"
      required: true
      weight: 1.0
      notes: "CADE reforms, regulatory changes"

    - item: "Considerar ciclos políticos e econômicos"
      required: true
      weight: 0.9
      notes: "Election cycles, economic crises"

    - item: "Incluir variáveis de contexto macroeconômico"
      required: true
      weight: 0.9
      notes: "Inflation, interest rates, exchange rates"

  data_specific_validation:
    - item: "Validar qualidade de dados brasileiros específicos"
      required: true
      weight: 1.0
      notes: "IBGE data quality, revisions"

    - item: "Considerar mudanças metodológicas nas fontes"
      required: true
      weight: 0.9
      notes: "Methodological changes in data sources"

    - item: "Verificar consistência com dados regionais"
      required: false
      weight: 0.8
      notes: "Regional data consistency"

  policy_validation:
    - item: "Validar impacto de políticas brasileiras específicas"
      required: true
      weight: 0.9
      notes: "Antitrust policies, price controls"

    - item: "Considerar efeitos de spillover entre regiões"
      required: true
      weight: 0.8
      notes: "Regional spillover effects"

    - item: "Testar heterogeneidade regional"
      required: true
      weight: 0.8
      notes: "Regional heterogeneity analysis"

# Seção 7: Validação de Previsão (se aplicável)
forecast_validation:
  forecast_accuracy:
    - item: "Calcular métricas de erro de previsão (MAE, RMSE, MAPE)"
      required: true
      weight: 1.0
      r_code: "forecast_accuracy <- accuracy(forecast, actual)"

    - item: "Realizar validação cruzada temporal"
      required: true
      weight: 0.9
      notes: "Time series cross-validation"

    - item: "Comparar com modelos benchmark"
      required: true
      weight: 0.9
      notes: "Benchmark model comparison"

  uncertainty_quantification:
    - item: "Calcular intervalos de previsão"
      required: true
      weight: 1.0
      notes: "Prediction intervals"

    - item: "Realizar análise de cenários"
      required: false
      weight: 0.8
      notes: "Scenario analysis"

    - item: "Quantificar incerteza paramétrica"
      required: true
      weight: 0.9
      notes: "Parameter uncertainty"

# Seção 8: Documentação e Relatório
documentation:
  model_documentation:
    - item: "Documentar todas as suposições do modelo"
      required: true
      weight: 1.0
      notes: "Complete model documentation"

    - item: "Fornece código reprodutível com comentários em inglês"
      required: true
      weight: 1.0
      notes: "Reproducible code with English comments"

    - item: "Documentar limitações e restrições"
      required: true
      weight: 1.0
      notes: "Model limitations and constraints"

  results_documentation:
    - item: "Apresentar resultados em formato claro e acessível"
      required: true
      weight: 1.0
      notes: "Clear results presentation"

    - item: "Incluir visualizações apropriadas"
      required: true
      weight: 0.9
      notes: "Appropriate visualizations"

    - item: "Discutir implicações para políticas brasileiras"
      required: true
      weight: 0.9
      notes: "Policy implications for Brazil"

  technical_appendix:
    - item: "Incluir detalhes técnicos completos"
      required: true
      weight: 1.0
      notes: "Complete technical details"

    - item: "Fornece tabelas de resultados robustas"
      required: true
      weight: 1.0
      notes: "Robust results tables"

    - item: "Documentar todas as transformações de dados"
      required: true
      weight: 1.0
      notes: "Data transformations documentation"

# Seção 9: Critérios de Aprovação
approval_criteria:
  mandatory_requirements:
    - item: "Todos os testes obrigatórios (required = true) devem ser completados"
      required: true
      weight: 1.0
      notes: "All required tests must be completed"

    - item: "Score mínimo de 80% nos itens com peso 1.0"
      required: true
      weight: 1.0
      notes: "Minimum 80% score on weighted items"

    - item: "Documentação completa e reprodutível"
      required: true
      weight: 1.0
      notes: "Complete and reproducible documentation"

  quality_thresholds:
    - item: "Resíduos não devem mostrar autocorrelação significativa"
      required: true
      weight: 1.0
      notes: "No significant residual autocorrelation"

    - item: "Coeficientes devem ser economicamente plausíveis"
      required: true
      weight: 1.0
      notes: "Economically plausible coefficients"

    - item: "Modelo deve superar benchmarks simples"
      required: true
      weight: 0.9
      notes: "Outperform simple benchmarks"

  brazilian_specific_criteria:
    - item: "Análise deve considerar contexto institucional brasileiro"
      required: true
      weight: 1.0
      notes: "Brazilian institutional context"

    - item: "Resultados devem ser relevantes para políticas antitruste"
      required: true
      weight: 0.9
      notes: "Relevant for antitrust policies"

    - item: "Deve usar fontes de dados brasileiras apropriadas"
      required: true
      weight: 1.0
      notes: "Appropriate Brazilian data sources"

---
## Instruções de Uso

### Como Utilizar Este Checklist

1. **Preencher cada item** com status (completo/incompleto)
2. **Calcular score ponderado** baseado nos pesos
3. **Documentar todas as decisões** tomadas durante a validação
4. **Usar R preferencialmente** com comentários em inglês
5. **Considerar contexto brasileiro** em todas as análises

### Cálculo de Score

- Para cada item completo: multiplicar pelo peso
- Score total = soma dos scores ponderados
- Score mínimo para aprovação: 80%

### Prioridades

- **Alta prioridade:** Itens com `required: true` e `weight: 1.0`
- **Média prioridade:** Itens com `weight: 0.8-0.9`
- **Baixa prioridade:** Itens com `required: false`

### Padrões Técnicos

- **Linguagem:** Português para documentação, inglês para código
- **Software:** R preferencialmente para econométrica
- **Validação:** Abordagem múltipla com testes complementares
- **Contexto:** Sempre considerar especificidades brasileiras