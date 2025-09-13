<!-- Powered by BMAD™ Core -->

# causal-inference-analysis

**Elicit:** true
**Interactive:** true

## ANÁLISE DE INFERÊNCIA CAUSAL PARA POLÍTICAS BRASILEIRAS

Este processo guiado conduz análise de inferência causal com foco em políticas econômicas brasileiras e impactos de mercado.

### PASSO 1: PERGUNTA DE PESQUISA CAUSAL
Defina a pergunta de pesquisa causal específica:

**Tipo de Pergunta Causal:**
- [ ] **Efeito de Política:** Impacto de uma política específica (ex: política monetária, regulatória)
- [ ] **Efeito de Intervenção:** Impacto de intervenção de mercado (ex: entrada/saída de empresa)
- [ ] **Efeito de Shocks:** Impacto de choques econômicos (ex: crise, mudança regulatória)
- [ ] **Efeito de Comportamento:** Relação causal entre comportamentos de mercado
- [ ] **Efeito Estrutural:** Impacto de mudanças estruturais no mercado

**Pergunta Específica:**
- Qual é o tratamento/intervenção que você está estudando?
- Qual é o resultado/outcome de interesse?
- Qual é a unidade de análise (empresas, mercados, consumidores)?
- Qual é o contexto geográfico e temporal?

### PASSO 2: IDENTIFICAÇÃO CAUSAL
Defina a estratégia de identificação causal:

**Desafios de Identificação:**
- **Endogeneidade:** Relação bidirecional entre tratamento e resultado
- **Variáveis Omitidas:** Fatores não observados que afetam ambos
- **Viés de Seleção:** Não alocação aleatória do tratamento
- **Contaminação:** Spillover effects entre unidades

**Estratégias de Identificação:**
- [ ] **Variáveis Instrumentais (IV):** Resolver endogeneidade com instrumentos válidos
- [ ] **Diferenças-em-Diferenças (DiD):** Comparar mudanças antes/depois entre grupos
- [ ] **Regressão Descontínua (RD):** Explorar descontinuidades em regras de alocação
- [ ] **Matching:** Comparar unidades tratadas com não tratadas similares
- [ ] **Controle Sintético:** Construir contrafactual sintético
- [ ] **Regressão com Controles:** Controle por variáveis observáveis
- [ ] **Experimentos Naturais:** Explorar variação exógena

**Validade da Identificação:**
- **Validade Interna:** A estimativa captura o efeito causal verdadeiro?
- **Validade Externa:** Os resultados podem ser generalizados?
- **Validade de Construção:** As variáveis medem os conceitos corretamente?
- **Validade Estatística:** Os métodos estatísticos são apropriados?

### PASSO 3: DADOS E VARIÁVEIS
Configure os dados para análise causal:

**Estrutura de Dados:**
- **Tipo de Dados:** Painel, cross-section, séries temporais
- **Unidade de Análise:** Empresas, mercados, regiões, indivíduos
- **Período:** Período pré e pós-tratamento
- **Frequência:** Diária, mensal, trimestral, anual

**Variável de Tratamento (D):**
- **Definição:** Como o tratamento é definido e medido?
- **Timing:** Quando o tratamento ocorre para cada unidade?
- **Intensidade:** Nível ou intensidade do tratamento
- **Exogeneidade:** O tratamento é exógeno ou endógeno?

**Variável de Resultado (Y):**
- **Definição:** Outcome de interesse principal
- **Tipo:** Contínua, binária, contagem, ordinal
- **Medição:** Como é medida e qual a unidade?
- **Frequência:** Frequência de medição

**Variáveis de Controle (X):**
- **Controles Pré-tratamento:** Características antes do tratamento
- **Controles de Tempo:** Tendências temporais, efeitos fixos
- **Controles de Contexto:** Variáveis contextuais relevantes
- **Instrumentos (Z):** Variáveis instrumentais para IV

### PASSO 4: METODOLOGIA CAUSAL
Selecione e implemente métodos de inferência causal:

**Método Principal:**
```r
# Causal inference methods using R
library(AER)
library(ivreg)
library(did)
library(MatchIt)
library(synth)
library(lfe)
library(fixest)

# 1. Instrumental Variables Approach
# For endogeneity problems in Brazilian market analysis
iv_analysis <- function(data, outcome, treatment, instruments, controls) {
  formula <- as.formula(paste(outcome, "~", treatment, "+", paste(controls, collapse = "+"), "|",
                             paste(instruments, collapse = "+"), "+", paste(controls, collapse = "+")))

  iv_model <- ivreg(formula, data = data)

  # First stage regression
  first_stage <- lm(as.formula(paste(treatment, "~", paste(instruments, collapse = "+"), "+",
                                   paste(controls, collapse = "+"))), data = data)

  # Diagnostic tests
  weak_instrument_test <- waldtest(iv_model)
  endogeneity_test <- wu.hausman(iv_model)
  overid_test <- ivreg::sargan(iv_model)

  return(list(
    iv_results = summary(iv_model, diagnostics = TRUE),
    first_stage = summary(first_stage),
    weak_instrument = weak_instrument_test,
    endogeneity = endogeneity_test,
    overidentification = overid_test
  ))
}

# 2. Difference-in-Differences for policy evaluation
# Common in Brazilian policy analysis
did_analysis <- function(data, outcome, treatment_var, time_var, unit_var) {
  # Check parallel trends assumption
  pre_period <- data[data[[time_var]] < treatment_date, ]
  parallel_trend_test <- lm(as.formula(paste(outcome, "~", treatment_var, ":factor(", time_var, ") + factor(", time_var, ") + factor(", unit_var, ")")), data = pre_period)

  # Main DiD estimation
  did_model <- att_gt(
    yname = outcome,
    gname = treatment_var,
    tname = time_var,
    data = data,
    control_group = "notyettreated"
  )

  # Event study plot
  event_study <- aggte(did_model, type = "dynamic")

  return(list(
    did_results = did_model,
    parallel_trend = summary(parallel_trend_test),
    event_study = event_study,
    treatment_effects = summary(did_model)
  ))
}

# 3. Regression Discontinuity Design
# For threshold-based policies in Brazil
rd_analysis <- function(data, outcome, running_var, cutoff, bandwidth) {
  # Optimal bandwidth selection
  library(rdrobust)
  optimal_bandwidth <- rdbwselect(data[[outcome]], data[[running_var]], c = cutoff)

  # RD estimation
  rd_results <- rdrobust(data[[outcome]], data[[running_var]], c = cutoff,
                        h = bandwidth, p = 1)

  # RD plots
  rdplot <- rdplot(data[[outcome]], data[[running_var]], c = cutoff, h = bandwidth)

  # Placebo tests
  placebo_cutoffs <- c(cutoff + 0.1, cutoff - 0.1)
  placebo_results <- lapply(placebo_cutoffs, function(pc) {
    rdrobust(data[[outcome]], data[[running_var]], c = pc, h = bandwidth)
  })

  return(list(
    rd_estimation = rd_results,
    bandwidth = optimal_bandwidth,
    rd_plot = rdplot,
    placebo_tests = placebo_results
  ))
}

# 4. Matching Methods
# For observational studies in Brazilian markets
matching_analysis <- function(data, outcome, treatment, controls) {
  # Propensity score matching
  match_obj <- matchit(as.formula(paste(treatment, "~", paste(controls, collapse = "+"))),
                      data = data, method = "nearest", ratio = 1)

  # Check balance
  balance <- summary(match_obj)

  # Extract matched data
  matched_data <- match.data(match_obj)

  # Estimate treatment effect
  te_model <- lm(as.formula(paste(outcome, "~", treatment, "+", paste(controls, collapse = "+"))),
                 data = matched_data, weights = weights)

  return(list(
    matching_results = match_obj,
    balance_table = balance,
    matched_data = matched_data,
    treatment_effect = summary(te_model)
  ))
}

# 5. Synthetic Control Method
# For case studies with few treated units
synth_analysis <- function(data, outcome, unit_var, time_var, treated_units, donor_pool) {
  # Prepare data for synth
  dataprep_out <- dataprep(
    foo = data,
    dependent = outcome,
    unit.variable = unit_var,
    time.variable = time_var,
    special.predictors = list(),  # Add covariates if needed
    treatment.identifier = treated_units,
    controls.identifier = donor_pool,
    time.optimize.ssr = time_periods_for_optimization,
    time.plot = time_periods_for_plot
  )

  # Run synthetic control
  synth_out <- synth(dataprep_out)

  # Placebo tests
  placebo_results <- placebotest(synth_out, dataprep_out)

  # Calculate post-treatment effects
  path_effects <- synth.tables(doop = dataprep_out, synth = synth_out)

  return(list(
    synthetic_control = synth_out,
    data_preparation = dataprep_out,
    placebo_tests = placebo_results,
    treatment_effects = path_effects
  ))
}
```

**Abordagem Híbrida para Contexto Brasileiro:**
```r
# Hybrid approach combining multiple methods for robustness
hybrid_causal_analysis <- function(data, outcome, treatment, controls, context = "brazilian") {
  results <- list()

  # Method 1: Fixed Effects with Time Trends
  fe_model <- feols(as.formula(paste(outcome, "~", treatment, "+", paste(controls, collapse = "+"),
                                   "| unit_id + year + month")), data = data)
  results$fixed_effects <- summary(fe_model)

  # Method 2: IV if instruments available
  if (exists("instruments")) {
    iv_results <- iv_analysis(data, outcome, treatment, instruments, controls)
    results$instrumental_variables <- iv_results
  }

  # Method 3: Matching for robustness
  match_results <- matching_analysis(data, outcome, treatment, controls)
  results$matching <- match_results

  # Method 4: Synthetic Control if applicable
  if (nrow(unique(data$unit_id)) < 10) {  # Few units
    synth_results <- synth_analysis(data, outcome, "unit_id", "time_var",
                                   treated_units = c(1, 2), donor_pool = 3:10)
    results$synthetic_control <- synth_results
  }

  # Method 5: Event Study for dynamic effects
  if (context == "brazilian") {
    # Special considerations for Brazilian context
    # Add controls for macroeconomic conditions, policy changes
    brazil_controls <- c(controls, "gdp_growth", "inflation_rate", "exchange_rate")

    event_study <- did_analysis(data, outcome, treatment, "time_var", "unit_id")
    results$event_study <- event_study
  }

  # Synthesis of results
  results$summary <- synthesize_results(results)

  return(results)
}

synthesize_results <- function(results_list) {
  # Compare estimates across methods
  estimates <- c()
  methods <- names(results_list)

  for (method in methods) {
    if (method %in% c("fixed_effects", "matching")) {
      est <- coef(results_list[[method]])[treatment]
      se <- sqrt(diag(vcov(results_list[[method]]))[treatment])
      estimates <- rbind(estimates, data.frame(method = method, estimate = est, se = se))
    }
  }

  # Check consistency across methods
  consistency_check <- check_consistency(estimates)

  return(list(
    point_estimates = estimates,
    consistency = consistency_check,
    robust_conclusion = derive_conclusion(estimates, consistency_check)
  ))
}
```

### PASSO 5: VALIDAÇÃO DE ASSUNÇÕES
Valide as suposições dos métodos causais:

**Suposições por Método:**

**Variáveis Instrumentais:**
- [ ] **Relevância:** Instrumentos correlacionados com tratamento
- [ ] **Exogeneidade:** Instrumentos não correlacionados com erro
- [ ] **Exclusão:** Instrumentos afetam resultado apenas através do tratamento
- [ ] **Monotonicidade:** Efeito dos instrumentos no tratamento é unidirecional

**Diferenças-em-Diferenças:**
- [ ] **Tendências Paralelas:** Grupos teriam evoluções similares sem tratamento
- [ ] **Nenhuma Contaminação:** Tratamento de um grupo não afeta outro
- [ ] **Composição Estável:** Nenhuma mudança sistemática na composição dos grupos

**Regressão Descontínua:**
- [ ] **Alocação Contínua:** Unidades não podem manipular a variável de execução
- [ ] **Continuidade:** Função de resultado é contínua no cutoff
- [ ] **Nenhuma Contaminação:** Tratamento não afeta unidades longe do cutoff

**Matching:**
- [ ] **Condicional Independência:** Tratamento independente de outcomes dado controles
- [ ] **Suposição de Suporte:** Sobreposição de características entre tratados e controles
- [ ] **Medição Correta:** Todas as variáveis relevantes são medidas corretamente

**Controle Sintético:**
- [ ] **Interpolação:** Unidade tratada é combinação convexa de controles
- [ ] **Não Interferência:** Unidades não interferem umas nas outras
- [ ] **Estabilidade:** Relações entre variáveis são estáveis no tempo

**Testes de Validação:**
```r
# Validation tests for causal methods
validate_causal_assumptions <- function(data, method, ...) {
  validation_results <- list()

  if (method == "did") {
    # Parallel trends test
    pre_trend_data <- data[data$period < treatment_date, ]
    pre_trend_model <- lm(outcome ~ treatment:time_factor + time_factor + unit_factor,
                         data = pre_trend_data)
    validation_results$parallel_trends <- summary(pre_trend_model)

    # Placebo tests
    placebo_dates <- generate_placebo_dates(data)
    placebo_effects <- lapply(placebo_dates, function(pd) {
      fake_data <- data
      fake_data$treatment_date <- pd
      did_analysis(fake_data, ...)
    })

  } else if (method == "iv") {
    # First stage F-statistic
    first_stage_f <- extract_first_stage_f(iv_model)
    validation_results$first_stage_strength <- first_stage_f

    # Overidentification test
    validation_results$overid <- sargan_test(iv_model)

    # Endogeneity test
    validation_results$endogeneity <- wu_hausman_test(iv_model)

  } else if (method == "rd") {
    # Density test for manipulation
    density_test <- rddensity(data[[running_var]], c = cutoff)
    validation_results$density <- density_test

    # Covariate balance at cutoff
    cov_balance <- rdbinselect(data[[running_var]], data[[covariate]], c = cutoff)
    validation_results$covariate_balance <- cov_balance

  } else if (method == "matching") {
    # Balance statistics
    balance_stats <- calculate_balance_stats(matched_data)
    validation_results$balance <- balance_stats

    # Common support check
    support_check <- check_common_support(propensity_scores)
    validation_results$common_support <- support_check
  }

  return(validation_results)
}
```

### PASSO 6: ANÁLISE DE HETEROGENEIDADE
Explore heterogeneidade nos efeitos de tratamento:

**Dimensões de Heterogeneidade:**
- **Setorial:** Diferenças entre setores da economia brasileira
- **Regional:** Variações entre estados ou regiões
- **Temporal:** Efeitos dinâmicos ao longo do tempo
- **Tamanho:** Efeitos por tamanho de empresa ou mercado
- **Institucional:** Impacto de características institucionais

**Métodos de Análise de Heterogeneidade:**
```r
# Heterogeneity analysis
heterogeneity_analysis <- function(data, outcome, treatment, heterogeneity_vars) {
  results <- list()

  for (var in heterogeneity_vars) {
    # Interaction effects
    interaction_model <- feols(as.formula(paste(outcome, "~", treatment, "*", var, "+ controls | unit + time")),
                              data = data)

    # Subgroup analysis
    subgroups <- unique(data[[var]])
    subgroup_effects <- list()

    for (subgroup in subgroups) {
      subgroup_data <- data[data[[var]] == subgroup, ]
      subgroup_model <- feols(as.formula(paste(outcome, "~", treatment, "+ controls | unit + time")),
                             data = subgroup_data)
      subgroup_effects[[as.character(subgroup)]] <- coef(subgroup_model)[treatment]
    }

    results[[var]] <- list(
      interaction_effect = summary(interaction_model),
      subgroup_effects = subgroup_effects,
      heterogeneity_test = waldtest(interaction_model)
    )
  }

  return(results)
}

# Event study for dynamic effects
dynamic_effects_analysis <- function(data, outcome, treatment, time_var, unit_var) {
  # Create leads and lags
  data <- data %>%
    group_by(unit_var) %>%
    mutate(
      time_to_treatment = as.numeric(time_var - treatment_date),
      lead3 = ifelse(time_to_treatment == -3, 1, 0),
      lead2 = ifelse(time_to_treatment == -2, 1, 0),
      lead1 = ifelse(time_to_treatment == -1, 1, 0),
      lag0 = ifelse(time_to_treatment == 0, 1, 0),
      lag1 = ifelse(time_to_treatment == 1, 1, 0),
      lag2 = ifelse(time_to_treatment == 2, 1, 0),
      lag3 = ifelse(time_to_treatment == 3, 1, 0)
    )

  # Event study regression
  event_model <- feols(as.formula(paste(outcome, "~ lead3 + lead2 + lead1 + lag0 + lag1 + lag2 + lag3 + controls | unit + time")),
                      data = data)

  # Plot event study coefficients
  event_study_plot <- plot_event_study(coef(event_model), vcov(event_model))

  return(list(
    event_study_results = summary(event_model),
    event_plot = event_study_plot,
    pre_trends_test = test_pre_trends(coef(event_model)[c("lead3", "lead2", "lead1")])
  ))
}
```

### PASSO 7: SENSIBILIDADE E ROBUSTEZ
Realize análise de sensibilidade completa:

**Análise de Sensibilidade:**
```r
# Sensitivity analysis for causal estimates
sensitivity_analysis <- function(main_results, data, ...) {
  sensitivity_results <- list()

  # 1. Different model specifications
  alternative_specs <- list(
    "linear" = feols(y ~ d + x1 + x2 | unit + time, data = data),
    "log_linear" = feols(log(y) ~ d + x1 + x2 | unit + time, data = data),
    "quadratic" = feols(y ~ d + I(d^2) + x1 + x2 | unit + time, data = data)
  )

  sensitivity_results$specification <- compare_estimates(alternative_specs)

  # 2. Different time periods
  time_periods <- list(
    "full_sample" = data,
    "pre_crisis" = data[data$year < 2008, ],
    "post_crisis" = data[data$year >= 2008, ]
  )

  period_effects <- lapply(time_periods, function(df) {
    feols(y ~ d + x1 + x2 | unit + time, data = df)
  })
  sensitivity_results$time_periods <- compare_estimates(period_effects)

  # 3. Different control sets
  control_sets <- list(
    "minimal" = c("x1"),
    "full" = c("x1", "x2", "x3"),
    "economic" = c("x1", "x2", "gdp_growth", "inflation")
  )

  control_effects <- lapply(control_sets, function(controls) {
    formula <- as.formula(paste("y ~ d +", paste(controls, collapse = "+"), "| unit + time"))
    feols(formula, data = data)
  })
  sensitivity_results$controls <- compare_estimates(control_effects)

  # 4. Placebo tests
  placebo_treatments <- generate_placebo_treatments(data)
  placebo_effects <- lapply(placebo_treatments, function(placebo_data) {
    feols(y ~ placebo_treatment + x1 + x2 | unit + time, data = placebo_data)
  })
  sensitivity_results$placebo <- placebo_effects

  return(sensitivity_results)
}

# Rosenbaum bounds for sensitivity to unobserved confounding
rosenbaum_bounds <- function(estimate, se, gamma_values = seq(1, 3, 0.1)) {
  bounds <- data.frame(gamma = gamma_values)

  for (gamma in gamma_values) {
    # Calculate bounds under different levels of hidden bias
    lower_bound <- estimate - qnorm(0.975) * se * sqrt(gamma)
    upper_bound <- estimate + qnorm(0.975) * se * sqrt(gamma)

    bounds[bounds$gamma == gamma, "lower"] <- lower_bound
    bounds[bounds$gamma == gamma, "upper"] <- upper_bound
    bounds[bounds$gamma == gamma, "significant"] <- abs(lower_bound) > 0 | abs(upper_bound) > 0
  }

  return(bounds)
}
```

### PASSO 8: INTERPRETAÇÃO E IMPLICAÇÕES
Interprete os resultados no contexto brasileiro:

**Interpretação Econômica:**
- **Magnitude do Efeito:** Qual é o tamanho do efeito econômico?
- **Significância Prática:** O efeito é economicamente significativo?
- **Mecanismos:** Quais são os canais causais?
- **Generalização:** Os resultados podem ser generalizados para outros contextos?

**Implicações para Políticas Brasileiras:**
- **CADE:** Como os resultados informam investigações antitruste?
- **SEAE:** Quais implicações para análises de mercado?
- **Políticas Setoriais:** Recomendações para políticas específicas
- **Reforma Regulatória:** Sugestões para mudanças regulatórias

**Limitações e Cuidados:**
- **Limitações de Dados:** Qualidade e disponibilidade de dados brasileiros
- **Contexto Institucional:** Particularidades do ambiente regulatório brasileiro
- **Validade Externa:** Limitações na generalização dos resultados
- **Incerteza:** Intervalos de confiança e margens de erro

---

## PADRÕES DE DOCUMENTAÇÃO

### Estrutura de Relatório Causal
```markdown
# Relatório de Análise Causal - [Título]

## Resumo Executivo
- Questão de pesquisa e contexto brasileiro
- Principal descoberta e magnitude do efeito
- Implicações para políticas públicas

## Contexto e Motivação
- Descrição do problema de pesquisa
- Relevância para o mercado brasileiro
- Lacunas na literatura existente

## Estratégia de Identificação
- Método causal selecionado
- Justificativa para escolha do método
- Diagrama causal (DAG) quando aplicável
- Validação das suposições

## Dados e Amostra
- Fontes de dados brasileiras
- Construção de variáveis
- Estatísticas descritivas
- Testes de balanceamento

## Resultados Principais
- Estimativas do efeito causal
- Intervalos de confiança
- Testes de hipóteses
- Análise de heterogeneidade

## Análise de Robustez
- Especificações alternativas
- Testes de sensibilidade
- Análise de subgrupos
- Testes placebo

## Validação de Suposições
- Testes diagnósticos
- Verificação de suposições
- Limitações metodológicas
- Discussão de incerteza

## Implicações para Políticas
- Recomendações para CADE/SEAE
- Impacto econômico estimado
- Considerações de implementação
- Direções para pesquisa futura

## Apêndice Técnico
- Código fonte em R/Python
- Tabelas detalhadas de resultados
- Visualizações adicionais
- Robustness checks adicionais
```

---

## PRÓXIMOS PASSOS

Com base nas suas respostas, eu vou:

1. **Estruturar Análise:** Definir estratégia de identificação causal apropriada
2. **Preparar Dados:** Processar dados brasileiros para análise causal
3. **Implementar Métodos:** Executar métodos de inferência causal com validação robusta
4. **Validar Suposições:** Realizar testes abrangentes de suposições causais
5. **Analisar Heterogeneidade:** Explorar efeitos diferenciados por dimensões relevantes
6. **Testar Sensibilidade:** Garantir robustez dos resultados através de múltiplos testes
7. **Gerar Relatório:** Produzir relatório completo em português com código em inglês

Pronto para realizar análise de inferência causal para políticas brasileiras! Por favor, forneça os detalhes da sua pergunta de pesquisa causal e contexto específico para que eu possa desenvolver uma análise rigorosa e contextualizada para o mercado brasileiro.