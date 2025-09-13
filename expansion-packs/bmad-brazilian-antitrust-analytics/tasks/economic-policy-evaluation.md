<!-- Powered by BMAD™ Core -->

# economic-policy-evaluation

**Elicit:** true
**Interactive:** true

## AVALIAÇÃO DE IMPACTO DE POLÍTICAS ECONÔMICAS BRASILEIRAS

Este processo guiado realiza avaliação rigorosa do impacto de políticas econômicas no contexto brasileiro.

### PASSO 1: POLÍTICA A SER AVALIADA
Defina a política econômica que será avaliada:

**Tipo de Política:**
- [ ] **Política Monetária:** Decisões do Copom, taxa Selic, política cambial
- [ ] **Política Fiscal:** Política tributária, gastos públicos, resultado fiscal
- [ ] **Política Regulatória:** Regulamentação setorial, regras de concorrência
- [ ] **Política Comercial:** Tarifas, acordos comerciais, câmbio
- [ ] **Política Industrial:** Incentivos fiscais, subsídios, desenvolvimento setorial
- [ ] **Política Social:** Programas sociais, transferências, previdência
- [ ] **Política de Concorrência:** Decisões CADE, medidas antitruste
- [ ] **Política Setorial:** Políticas específicas para setores (energia, telecom, etc.)

**Detalhes da Política:**
- **Nome/Descrição:** Qual é o nome e descrição da política?
- **Período de Implementação:** Quando foi implementada?
- **Abrangência Geográfica:** Nacional, regional, estadual, municipal?
- **Alvo Principal:** Quem é o público-alvo da política?
- **Objetivo Declarado:** Quais são os objetivos oficiais da política?

### PASSO 2: QUESTÃO DE AVALIAÇÃO
Formule a clara questão de avaliação:

**Questão Principal:**
- Qual é o impacto causal da [política] no [outcome]?

**Dimensões da Avaliação:**
- **Eficácia:** A política alcançou seus objetivos declarados?
- **Eficiência:** Os benefícios superam os custos?
- **Equidade:** A política impactou diferentes grupos de forma justa?
- **Sustentabilidade:** Os impactos são sustentáveis a longo prazo?
- **Efeitos Não Intencionais:** Quais foram os efeitos inesperados?

**Outcomes de Interesse:**
- **Outcomes Primários:** Principais variáveis de resultado
- **Outcomes Secundários:** Outras variáveis afetadas
- **Mecanismos de Transmissão:** Como a política afeta os outcomes?
- **Canais Indiretos:** Efeitos através de outros canais
- **Efeitos de Longo Prazo:** Impactos sustentados no tempo

### PASSO 3: ESTRATÉGIA DE IDENTIFICAÇÃO
Selecione a estratégia de identificação causal:

**Desenho da Avaliação:**
- [ ] **Experimento Aleatório:** Randomização da intervenção
- [ ] **Experimento Natural:** Variação exógena na implementação
- [ ] **Diferenças-em-Diferenças:** Comparação antes/depois entre grupos
- [ ] **Regressão Descontínua:** Exploração de regras de elegibilidade
- [ ] **Variáveis Instrumentais:** Uso de instrumentos para endogeneidade
- [ ] **Matching:** Comparação com grupos de controle similares
- [ ] **Controle Sintético:** Construção de contrafactual sintético
- [ ] **Série Temporal Interrupta:** Análise de interrupção em séries temporais

**Validade do Desenho:**
- **Validade Interna:** O desenho identifica o efeito causal verdadeiro?
- **Validade Externa:** Os resultados podem ser generalizados?
- **Suposições Chave:** Quais suposições são necessárias?
- **Ameaças à Identificação:** Quais são as principais ameaças?

**Contexto Brasileiro Específico:**
- **Instituições:** Como as instituições brasileiras afetam a política?
- **Implementação:** Como a política foi implementada no Brasil?
- **Dados:** Qual é a disponibilidade e qualidade de dados brasileiros?
- **Condicionalidades:** Fatores específicos do contexto brasileiro

### PASSO 4: DADOS E MEDIDAS
Configure os dados e medidas de impacto:

**Dados de Resultado:**
- **Fontes de Dados:** IBGE, BACEN, IPEA, FGV, dados setoriais
- **Unidade de Análise:** Empresas, municípios, estados, indivíduos
- **Frequência:** Mensal, trimestral, anual
- **Período:** Pré e pós-política

**Variáveis de Interesse:**
- **Variável de Tratamento:** Indicador de exposição à política
- **Variáveis de Outcome:** Medidas dos impactos esperados
- **Variáveis de Controle:** Fatores que precisam ser controlados
- **Variáveis de Heterogeneidade:** Fatores que podem modificar o efeito

**Construção de Variáveis:**
```r
# Data preparation for Brazilian policy evaluation
prepare_brazilian_data <- function(raw_data, policy_info) {
  library(tidyverse)
  library(lubridate)

  # Create treatment indicator
  data <- raw_data %>%
    mutate(
      treatment_date = as.Date(policy_info$implementation_date),
      post_policy = as.Date(date) >= treatment_date,
      treatment = if_else(unit_id %in% policy_info$treated_units, 1, 0),
      treated_post = treatment * post_policy
    )

  # Create time relative to policy
  data <- data %>%
    group_by(unit_id) %>%
    mutate(
      time_to_treatment = as.numeric(date - treatment_date) / 30,  # in months
      time_period = if_else(time_to_treatment >= 0, time_to_treatment, NA_real_),
      pre_period = time_to_treatment < 0 & time_to_treatment >= -36,  # 3 years pre
      post_period = time_to_treatment >= 0 & time_to_treatment <= 36  # 3 years post
    ) %>%
    ungroup()

  # Add Brazilian-specific controls
  data <- data %>%
    mutate(
      # Macroeconomic controls
      gdp_growth = get_gdp_growth(date),
      inflation_rate = get_inflation(date),
      selic_rate = get_selic_rate(date),
      exchange_rate = get_exchange_rate(date),

      # Regional controls
      region = get_region(unit_id),
      state_gdp = get_state_gdp(unit_id, date),

      # Time controls
      year = year(date),
      quarter = quarter(date),
      month = month(date),
      # Brazilian calendar effects
      is_carnival = is_carnival_period(date),
      is_election_year = is_election_year(date)
    )

  return(data)
}

# Get Brazilian economic indicators
get_gdp_growth <- function(date) {
  # Simplified - in practice, use real data from IBGE/IPEA
  return(rnorm(1, mean = 0.02, sd = 0.01))
}

get_inflation <- function(date) {
  # Get IPCA from IBGE
  return(rnorm(1, mean = 0.005, sd = 0.002))
}

get_selic_rate <- function(date) {
  # Get Selic rate from BACEN
  return(runif(1, min = 0.02, max = 0.14))
}

get_exchange_rate <- function(date) {
  # Get USD/BRL exchange rate
  return(rnorm(1, mean = 5.0, sd = 0.5))
}

get_region <- function(unit_id) {
  # Get Brazilian region for municipality/state
  regions <- c("Norte", "Nordeste", "Sudeste", "Sul", "Centro-Oeste")
  return(sample(regions, 1))
}

# Brazilian calendar functions
is_carnival_period <- function(date) {
  # Simplified carnival detection
  year <- year(date)
  carnival_dates <- c(as.Date(paste0(year, "-02-01")), as.Date(paste0(year, "-03-01")))
  return(date %in% carnival_dates)
}

is_election_year <- function(date) {
  # Brazilian presidential elections every 4 years
  return(year(date) %% 4 == 2)
}
```

### PASSO 5: METODOLOGIA DE AVALIAÇÃO
Implemente os métodos de avaliação selecionados:

**Implementação de Métodos:**
```r
# Policy evaluation methods for Brazilian context
policy_evaluation_methods <- function(data, outcome_var, treatment_var, unit_var, time_var) {
  library(did)
  library(synth)
  library(fixest)
  library(rdrobust)
  library(AER)

  results <- list()

  # 1. Difference-in-Differences (primary method for Brazilian policies)
  if (length(unique(data[[unit_var]])) > 1) {
    results$did <- did_analysis_brazilian(data, outcome_var, treatment_var, unit_var, time_var)
  }

  # 2. Fixed Effects with time trends
  results$fixed_effects <- fixed_effects_analysis(data, outcome_var, treatment_var, unit_var, time_var)

  # 3. Event Study for dynamic effects
  results$event_study <- event_study_analysis(data, outcome_var, treatment_var, unit_var, time_var)

  # 4. Synthetic Control (for cases with few treated units)
  if (sum(data[[treatment_var]] == 1) <= 5) {
    results$synthetic_control <- synthetic_control_analysis(data, outcome_var, treatment_var, unit_var, time_var)
  }

  # 5. Regression Discontinuity (for eligibility threshold policies)
  if ("running_var" %in% names(data)) {
    results$regression_discontinuity <- rd_analysis(data, outcome_var, treatment_var, "running_var")
  }

  # 6. Heterogeneity analysis
  results$heterogeneity <- heterogeneity_analysis(data, outcome_var, treatment_var)

  return(results)
}

# Brazilian-specific DiD implementation
did_analysis_brazilian <- function(data, outcome, treatment, unit, time) {
  # Check parallel trends with Brazilian macro controls
  parallel_trend_data <- data[data[[time]] < policy_date, ]
  parallel_trend_model <- feols(
    as.formula(paste(outcome, "~", treatment, ":factor(", time, ") + factor(", time, ") + factor(", unit, ") + gdp_growth + inflation_rate + selic_rate")),
    data = parallel_trend_data
  )

  # Main DiD estimation with cluster-robust standard errors
  did_model <- feols(
    as.formula(paste(outcome, "~", treatment, " + gdp_growth + inflation_rate + selic_rate + exchange_rate |", unit, "+", time)),
    data = data,
    vcov = ~unit
  )

  # Event study with Brazilian-specific time periods
  event_data <- data %>%
    mutate(
      time_to_event = as.numeric(as.Date(time) - policy_date) / 30,
      event_bin = case_when(
        time_to_event < -36 ~ "-36+",
        time_to_event < -24 ~ "-36 to -24",
        time_to_event < -12 ~ "-24 to -12",
        time_to_event < 0 ~ "-12 to 0",
        time_to_event < 12 ~ "0 to 12",
        time_to_event < 24 ~ "12 to 24",
        time_to_event >= 24 ~ "24+"
      )
    )

  event_model <- feols(
    as.formula(paste(outcome, "~ factor(event_bin) + gdp_growth + inflation_rate |", unit, "+ time")),
    data = event_data,
    vcov = ~unit
  )

  return(list(
    parallel_trends = summary(parallel_trend_model),
    main_estimate = summary(did_model),
    event_study = summary(event_model),
    treatment_effect = coef(did_model)[treatment]
  ))
}

# Fixed effects with Brazilian context
fixed_effects_analysis <- function(data, outcome, treatment, unit, time) {
  # Include Brazilian-specific fixed effects
  fe_model <- feols(
    as.formula(paste(outcome, "~", treatment, " + gdp_growth + inflation_rate + selic_rate + exchange_rate + region |", unit, "+ year + quarter + month")),
    data = data,
    vcov = ~unit
  )

  # Model with state-specific time trends
  fe_state_trends <- feols(
    as.formula(paste(outcome, "~", treatment, " + gdp_growth + inflation_rate + selic_rate + exchange_rate |", unit, "+ year + quarter + month + state:year")),
    data = data,
    vcov = ~unit
  )

  return(list(
    baseline_fe = summary(fe_model),
    state_trends_fe = summary(fe_state_trends),
    robustness_check = compare_models(fe_model, fe_state_trends)
  ))
}

# Event study for dynamic effects
event_study_analysis <- function(data, outcome, treatment, unit, time) {
  # Create leads and lags
  event_data <- data %>%
    group_by(unit) %>%
    mutate(
      time_to_treatment = as.numeric(as.Date(time) - policy_date) / 30,
      # Create leads and lags
      lag12 = ifelse(time_to_treatment == -12, 1, 0),
      lag6 = ifelse(time_to_treatment == -6, 1, 0),
      lag3 = ifelse(time_to_treatment == -3, 1, 0),
      lead0 = ifelse(time_to_treatment == 0, 1, 0),
      lead3 = ifelse(time_to_treatment == 3, 1, 0),
      lead6 = ifelse(time_to_treatment == 6, 1, 0),
      lead12 = ifelse(time_to_treatment == 12, 1, 0)
    ) %>%
    ungroup()

  # Event study regression
  event_model <- feols(
    as.formula(paste(outcome, "~ lag12 + lag6 + lag3 + lead0 + lead3 + lead6 + lead12 + gdp_growth + inflation_rate |", unit, "+ time")),
    data = event_data,
    vcov = ~unit
  )

  # Test pre-trends
  pre_trend_test <- linearHypothesis(event_model,
                                   c("lag12 = 0", "lag6 = 0", "lag3 = 0"))

  return(list(
    event_study_results = summary(event_model),
    pre_trends_test = pre_trend_test,
    dynamic_effects = extract_dynamic_effects(event_model)
  ))
}

# Synthetic control for Brazilian case studies
synthetic_control_analysis <- function(data, outcome, treatment, unit, time) {
  # Prepare data for synthetic control
  treated_units <- unique(data[data[[treatment]] == 1, ][[unit]])
  donor_pool <- unique(data[data[[treatment]] == 0, ][[unit]])

  # For each treated unit, create synthetic control
  synth_results <- list()

  for (treated_unit in treated_units) {
    # Data preparation
    dataprep_out <- dataprep(
      foo = data,
      dependent = outcome,
      unit.variable = unit,
      time.variable = time,
      special.predictors = list(
        list("gdp_growth", 1, "mean"),
        list("inflation_rate", 1, "mean"),
        list("selic_rate", 1, "mean")
      ),
      treatment.identifier = treated_unit,
      controls.identifier = donor_pool,
      time.optimize.ssr = pre_period_dates,
      time.plot = all_dates
    )

    # Run synthetic control
    synth_out <- synth(dataprep_out)

    # Calculate treatment effects
    path_effects <- synth.tables(doop = dataprep_out, synth = synth_out)

    synth_results[[as.character(treated_unit)]] <- list(
      synthetic_control = synth_out,
      path_effects = path_effects,
      unit_treated = treated_unit
    )
  }

  return(synth_results)
}
```

### PASSO 6: ANÁLISE DE HETEROGENEIDADE
Explore efeitos diferenciados da política:

**Análise por Subgrupos:**
```r
# Heterogeneity analysis for Brazilian policies
heterogeneity_analysis <- function(data, outcome, treatment) {
  library(interactions)

  results <- list()

  # 1. Regional heterogeneity (important for Brazil)
  regional_model <- feols(
    as.formula(paste(outcome, "~", treatment, "* region + gdp_growth + inflation_rate | unit + time")),
    data = data,
    vcov = ~unit
  )

  # 2. Sectoral heterogeneity
  if ("sector" %in% names(data)) {
    sectoral_model <- feols(
      as.formula(paste(outcome, "~", treatment, "* sector + gdp_growth + inflation_rate | unit + time")),
      data = data,
      vcov = ~unit
    )
    results$sectoral <- summary(sectoral_model)
  }

  # 3. Size heterogeneity (firm size, municipality size)
  if ("size_category" %in% names(data)) {
    size_model <- feols(
      as.formula(paste(outcome, "~", treatment, "* size_category + gdp_growth + inflation_rate | unit + time")),
      data = data,
      vcov = ~unit
    )
    results$size <- summary(size_model)
  }

  # 4. Time-based heterogeneity (short vs long term)
  data_short_term <- data[data$time_to_treatment >= 0 & data$time_to_treatment <= 12, ]
  data_long_term <- data[data$time_to_treatment > 12, ]

  short_term_model <- feols(
    as.formula(paste(outcome, "~", treatment, "+ gdp_growth + inflation_rate | unit + time")),
    data = data_short_term,
    vcov = ~unit
  )

  long_term_model <- feols(
    as.formula(paste(outcome, "~", treatment, "+ gdp_growth + inflation_rate | unit + time")),
    data = data_long_term,
    vcov = ~unit
  )

  results$regional <- summary(regional_model)
  results$short_term <- summary(short_term_model)
  results$long_term <- summary(long_term_model)

  return(results)
}

# Distributional effects analysis
distributional_effects <- function(data, outcome, treatment, group_var) {
  # Quantile treatment effects
  library(qte)

  qte_results <- qte(
    as.formula(paste(outcome, "~", treatment)),
    data = data,
    se = TRUE,
    probs = seq(0.1, 0.9, 0.1)
  )

  # Distributional analysis
  treated <- data[data[[treatment]] == 1, ][[outcome]]
  control <- data[data[[treatment]] == 0, ][[outcome]]

  distribution_test <- ks.test(treated, control)

  return(list(
    quantile_effects = qte_results,
    distribution_test = distribution_test,
    mean_treated = mean(treated, na.rm = TRUE),
    mean_control = mean(control, na.rm = TRUE)
  ))
}
```

### PASSO 7: ANÁLISE DE CUSTO-BENEFÍCIO
Realize análise econômica completa:

**Análise de Custos e Benefícios:**
```r
# Cost-benefit analysis for Brazilian policies
cost_benefit_analysis <- function(policy_effects, costs_info, discount_rate = 0.05) {
  # Calculate monetary benefits
  benefits <- calculate_monetary_benefits(policy_effects)

  # Calculate costs
  costs <- calculate_policy_costs(costs_info)

  # Net present value calculation
  npv <- calculate_npv(benefits, costs, discount_rate)

  # Benefit-cost ratio
  bcr <- sum(benefits) / sum(costs)

  # Internal rate of return
  irr <- calculate_irr(benefits, costs)

  # Sensitivity analysis
  sensitivity <- sensitivity_analysis(benefits, costs, discount_rate)

  return(list(
    total_benefits = sum(benefits),
    total_costs = sum(costs),
    net_present_value = npv,
    benefit_cost_ratio = bcr,
    internal_rate_of_return = irr,
    sensitivity_analysis = sensitivity
  ))
}

calculate_monetary_benefits <- function(effects) {
  # Convert statistical effects to monetary values
  # This is highly context-specific and requires detailed assumptions

  # Example: employment effects to wage gains
  employment_effect <- effects$employment_effect
  average_wage <- effects$average_wage
  time_horizon <- effects$time_horizon

  wage_benefits <- employment_effect * average_wage * 12 * time_horizon

  # Example: productivity effects to GDP gains
  productivity_effect <- effects$productivity_effect
  gdp_multiplier <- effects$gdp_multiplier

  productivity_benefits <- productivity_effect * gdp_multiplier * time_horizon

  # Total benefits
  total_benefits <- wage_benefits + productivity_benefits

  return(total_benefits)
}

calculate_policy_costs <- function(costs_info) {
  # Direct implementation costs
  direct_costs <- costs_info$direct_costs

  # Administrative costs
  admin_costs <- costs_info$administrative_costs

  # Opportunity costs
  opportunity_costs <- costs_info$opportunity_costs

  total_costs <- direct_costs + admin_costs + opportunity_costs

  return(total_costs)
}

sensitivity_analysis <- function(benefits, costs, discount_rate) {
  # Test different scenarios
  scenarios <- list(
    baseline = list(benefits = benefits, costs = costs, discount_rate = discount_rate),
    optimistic = list(benefits = benefits * 1.2, costs = costs * 0.8, discount_rate = discount_rate * 0.8),
    pessimistic = list(benefits = benefits * 0.8, costs = costs * 1.2, discount_rate = discount_rate * 1.2)
  )

  results <- list()
  for (scenario_name in names(scenarios)) {
    scenario <- scenarios[[scenario_name]]
    npv <- calculate_npv(scenario$benefits, scenario$costs, scenario$discount_rate)
    bcr <- sum(scenario$benefits) / sum(scenario$costs)
    results[[scenario_name]] <- list(npv = npv, bcr = bcr)
  }

  return(results)
}
```

### PASSO 8: VALIDAÇÃO E ROBUSTEZ
Valide os resultados com múltiplos testes:

**Análise de Robustez:**
```r
# Robustness analysis for policy evaluation
robustness_analysis <- function(main_results, data, outcome, treatment) {
  robustness_checks <- list()

  # 1. Different model specifications
  specs <- list(
    baseline = feols(as.formula(paste(outcome, "~", treatment, "+ controls | unit + time")), data = data),
    log_linear = feols(as.formula(paste("log(", outcome, ") ~", treatment, "+ controls | unit + time")), data = data),
    quadratic = feols(as.formula(paste(outcome, "~", treatment, "+ I(", treatment, "^2) + controls | unit + time")), data = data)
  )

  robustness_checks$specifications <- compare_estimates(specs)

  # 2. Different time periods
  time_periods <- list(
    full_sample = data,
    pre_crisis = data[data$year < 2008, ],
    post_crisis = data[data$year >= 2008, ],
    recent = data[data$year >= 2015, ]
  )

  period_estimates <- lapply(time_periods, function(df) {
    feols(as.formula(paste(outcome, "~", treatment, "+ controls | unit + time")), data = df)
  })

  robustness_checks$time_periods <- compare_estimates(period_estimates)

  # 3. Placebo tests
  placebo_dates <- generate_placebo_dates(data)
  placebo_effects <- lapply(placebo_dates, function(pd) {
    fake_data <- data
    fake_data$fake_treatment <- ifelse(data$date >= pd & data$treatment == 1, 1, 0)
    feols(as.formula(paste(outcome, "~ fake_treatment + controls | unit + time")), data = fake_data)
  })

  robustness_checks$placebo <- placebo_effects

  # 4. Different control groups
  control_groups <- list(
    all_controls = data,
    geographic_controls = data[data$region == treated_region, ],
    sectoral_controls = data[data$sector == treated_sector, ]
  )

  group_estimates <- lapply(control_groups, function(df) {
    feols(as.formula(paste(outcome, "~", treatment, "+ controls | unit + time")), data = df)
  })

  robustness_checks$control_groups <- compare_estimates(group_estimates)

  return(robustness_checks)
}

# Generate placebo tests
generate_placebo_dates <- function(data) {
  actual_policy_date <- min(data[data$treatment == 1, ]$date)

  # Generate placebo dates before actual implementation
  placebo_dates <- seq(actual_policy_date - years(2), actual_policy_date - months(3), by = "3 months")

  return(placebo_dates)
}
```

### PASSO 9: IMPLICAÇÕES PARA POLÍTICAS BRASILEIRAS
Interprete os resultados no contexto institucional brasileiro:

**Análise Institucional:**
```r
# Institutional analysis for Brazilian context
institutional_analysis <- function(evaluation_results, policy_info) {
  analysis <- list()

  # 1. Alignment with Brazilian policy goals
  analysis$alignment <- assess_policy_alignment(evaluation_results, policy_info)

  # 2. Implementation considerations
  analysis$implementation <- implementation_considerations(policy_info)

  # 3. Distributional implications
  analysis$distributional <- distributional_implications(evaluation_results)

  # 4. Political feasibility
  analysis$political_feasibility <- political_feasibility_assessment(policy_info)

  # 5. Recommendations for CADE/SEAE
  analysis$agency_recommendations <- agency_recommendations(evaluation_results)

  return(analysis)
}

assess_policy_alignment <- function(results, policy) {
  # Check if results align with stated policy goals
  goals <- policy$stated_goals
  outcomes <- results$treatment_effects

  alignment <- data.frame(
    goal = goals,
    outcome = names(outcomes),
    effect = outcomes,
    aligned = sapply(names(outcomes), function(outcome) {
      # Simplified alignment assessment
      ifelse(outcomes[outcome] > 0, "Yes", "No")
    })
  )

  return(alignment)
}

implementation_considerations <- function(policy) {
  considerations <- list(
    institutional_capacity = assess_institutional_capacity(policy),
    data_availability = assess_data_availability(policy),
    stakeholder_support = assess_stakeholder_support(policy),
    budget_constraints = assess_budget_constraints(policy)
  )

  return(considerations)
}

agency_recommendations <- function(results) {
  recommendations <- list()

  if (results$treatment_effect > 0 & results$significant) {
    recommendations$positive <- list(
      description = "Policy shows positive significant effects",
      action = "Consider expanding or continuing the policy",
      monitoring = "Continue monitoring for long-term effects"
    )
  } else if (results$treatment_effect < 0 & results$significant) {
    recommendations$negative <- list(
      description = "Policy shows negative significant effects",
      action = "Consider modifying or terminating the policy",
      investigation = "Investigate channels of negative effects"
    )
  } else {
    recommendations$neutral <- list(
      description = "Policy shows no significant effects",
      action = "Consider redesign or alternative approaches",
      evaluation = "Review implementation and targeting"
    )
  }

  return(recommendations)
}
```

### PASSO 10: RELATÓRIO E RECOMENDAÇÕES
Gere relatório completo com recomendações:

**Estrutura de Relatório de Avaliação:**
```markdown
# Avaliação de Impacto da Política: [Nome da Política]

## Sumário Executivo
- **Política Avaliada:** [Breve descrição]
- **Principal Descoberta:** [Efeito principal e significância]
- **Recomendação:** [Ação recomendada]
- **Impacto Orçamentário:** [Custos e benefícios estimados]

## Contexto e Objetivos
- **Motivação da Política:** Razões para implementação
- **Objetivos Declarados:** Metas oficiais da política
- **Marco Institucional:** Base legal e implementação
- **Público-Alvo:** Grupos beneficiados pela política

## Metodologia de Avaliação
- **Desenho da Avaliação:** Método de identificação causal
- **Dados Utilizados:** Fontes e período de análise
- **Estratégia de Identificação:** Como o efeito causal foi identificado
- **Suposições e Limitações:** Pressupostos metodológicos

## Resultados Principais
- **Efeito Causal Principal:** [Magnitude e significância]
- **Efeitos Dinâmicos:** Evolução dos efeitos no tempo
- **Análise de Heterogeneidade:** Efeitos diferenciados por grupos
- **Mecanismos de Transmissão:** Canais principais de impacto

## Análise de Robustez
- **Testes de Sensibilidade:** Diferentes especificações e amostras
- **Testes Placebo:** Resultados com datas de tratamento falsas
- **Análise de Subgrupos:** Consistência entre diferentes grupos
- **Validação de Suposições:** Verificação das suposições metodológicas

## Análise Custo-Benefício
- **Custos Estimados:** [Detalhamento dos custos]
- **Benefícios Estimados:** [Detalhamento dos benefícios]
- **Relação Custo-Benefício:** [RCB calculado]
- **Análise de Sensibilidade:** [Variação sob diferentes cenários]

## Implicações para Políticas Brasileiras
- **Recomendações para CADE:** Implicações antitruste
- **Recomendações para SEAE:** Análise de mercado
- **Recomendações Setoriais:** Impactos específicos por setor
- **Recomendações Regionais:** Considerações regionais

## Limitações e Pesquisa Futura
- **Limitações Metodológicas:** Restrições da análise
- **Limitações de Dados:** Problemas com dados brasileiros
- **Direções para Pesquisa:** Áreas para investigação futura
- **Recomendações de Dados:** Melhorias na coleta de dados

## Apêndice Técnico
- **Código Fonte:** Scripts de análise em R/Python
- **Tabelas Detalhadas:** Resultados completos
- **Robustness Checks:** Testes adicionais
- **Mapas e Visualizações:** Análises geográficas
```

---

## PRÓXIMOS PASSOS

Com base nas suas respostas, eu vou:

1. **Estruturar Avaliação:** Definir desenho de avaliação apropriado para a política
2. **Preparar Dados:** Processar dados brasileiros com controles contextuais
3. **Implementar Métodos:** Executar múltiplos métodos de avaliação causal
4. **Analisar Heterogeneidade:** Explorar efeitos diferenciados por subgrupos
5. **Realizar Custo-Benefício:** Calcular benefícios econômicos e custos
6. **Validar Resultados:** Testar robustez com múltiplas especificações
7. **Gerar Recomendações:** Produzir recomendações contextualizadas para o Brasil

Pronto para avaliar o impacto da política econômica! Por favor, forneça os detalhes da política que você deseja avaliar e os dados disponíveis para que eu possa desenvolver uma avaliação rigorosa e relevante para o contexto brasileiro.