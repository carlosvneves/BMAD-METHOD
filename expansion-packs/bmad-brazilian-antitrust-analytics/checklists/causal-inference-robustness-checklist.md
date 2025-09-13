# Checklist de Robustez em Inferência Causal
# Para Análise de Mercado Brasileiro e Políticas Públicas

checklist_info:
  name: "Causal Inference Robustness Checklist"
  version: "1.0"
  author: "Econometrician Agent - Brazilian Antitrust Analytics"
  description: "Checklist abrangente para validação de robustez em inferência causal com foco em políticas brasileiras"
  language: "pt-BR"
  created_date: "2024-01-01"
  last_updated: "2024-01-01"

# Seção 1: Validação da Estratégia de Identificação
identification_strategy_validation:
  research_design_clarity:
    - item: "Definir claramente a pergunta causal"
      required: true
      weight: 1.0
      notes: "Clear causal question specification"

    - item: "Justificar escolha do método de identificação"
      required: true
      weight: 1.0
      notes: "Method selection justification"

    - item: "Documentar todas as suposições de identificação"
      required: true
      weight: 1.0
      notes: "Complete identification assumptions"

  parallel_trends_validation:
    - item: "Testar suposição de tendências paralelas (para DiD)"
      required: true
      weight: 1.0
      r_code: "# Test parallel trends
    parallel_test <- lm(outcome ~ treatment*time_fe + controls + unit_fe + time_fe, data = pre_period)
    summary(parallel_test)"

    - item: "Visualizar tendências pré-tratamento"
      required: true
      weight: 1.0
      r_code: "# Plot pre-treatment trends
    ggplot(data, aes(x = time, y = outcome, color = treatment)) +
      geom_line() + geom_vline(xintercept = treatment_date, linetype = 'dashed')"

    - item: "Realizar teste de equilíbrio para grupos de tratamento/controle"
      required: true
      weight: 1.0
      r_code: "# Balance test
    balance_test <- lm(treatment ~ controls + unit_fe, data = pre_period)
    summary(balance_test)"

  exclusion_restriction_validation:
    - item: "Validar restrição de exclusão (para IV)"
      required: true
      weight: 1.0
      notes: "Instrument relevance and exogeneity"

    - item: "Testar relevância do instrumento (F-statistic > 10)"
      required: true
      weight: 1.0
      r_code: "# First stage F-test
    first_stage <- ivreg(endogenous ~ instrument + controls | controls, data = data)
    summary(first_stage, diagnostics = TRUE)"

    - item: "Realizar testes de sobreidentificação quando possível"
      required: true
      weight: 0.9
      notes: "Overidentification tests"

# Seção 2: Validação de Dados e Amostra
data_sample_validation:
  sample_construction:
    - item: "Documentar critérios de inclusão/exclusão"
      required: true
      weight: 1.0
      notes: "Sample construction criteria"

    - item: "Verificar representatividade da amostra"
      required: true
      weight: 1.0
      notes: "Sample representativeness"

    - item: "Analisar atrito (attrition) na amostra"
      required: true
      weight: 1.0
      r_code: "# Attrition analysis
    attrition_test <- lm(attrited ~ treatment + controls, data = baseline)
    summary(attrition_test)"

  treatment_assignment:
    - item: "Documentar mecanismo de atribuição do tratamento"
      required: true
      weight: 1.0
      notes: "Treatment assignment mechanism"

    - item: "Verificar não-manipulabilidade da atribuição"
      required: true
      weight: 1.0
      notes: "Non-manipulable assignment"

    - item: "Testar por contaminação entre grupos"
      required: true
      weight: 0.9
      notes: "Spillover contamination tests"

  covariate_balance:
    - item: "Testar balanceamento de covariadas pré-tratamento"
      required: true
      weight: 1.0
      r_code: "# Covariate balance test
    balance_table <- table.balance(data, treatment, covariates)
    print(balance_table)"

    - item: "Calcular standardized mean differences"
      required: true
      weight: 1.0
      r_code: "# Standardized mean differences
    smd <- calculate_smd(data, treatment, covariates)
    print(smd)"

    - item: "Realizar testes de equilíbrio para matching"
      required: true
      weight: 1.0
      notes: "Matching balance tests"

# Seção 3: Análise de Sensibilidade
sensitivity_analysis:
  specification_checks:
    - item: "Testar diferentes especificações do modelo"
      required: true
      weight: 1.0
      r_code: "# Alternative specifications
    spec1 <- feols(y ~ treatment + controls | unit_fe + time_fe, data = data)
    spec2 <- feols(log(y) ~ treatment + controls | unit_fe + time_fe, data = data)
    spec3 <- feols(y ~ treatment*region + controls | unit_fe + time_fe, data = data)"

    - item: "Variar conjunto de controles"
      required: true
      weight: 1.0
      notes: "Different control sets"

    - item: "Testar diferentes períodos de amostra"
      required: true
      weight: 1.0
      notes: "Different sample periods"

  functional_form_sensitivity:
    - item: "Testar diferentes formas funcionais"
      required: true
      weight: 1.0
      r_code: "# Functional form tests
    linear <- lm(y ~ treatment + controls, data = data)
    log_linear <- lm(log(y) ~ treatment + controls, data = data)
    quadratic <- lm(y ~ treatment + I(treatment^2) + controls, data = data)"

    - item: "Testar transformações não-lineares"
      required: true
      weight: 0.9
      notes: "Non-linear transformations"

    - item: "Verificar outliers e influência"
      required: true
      weight: 1.0
      r_code: "# Outlier analysis
    influence_measures <- influence.measures(model)
    print(influence_measures)"

  sample_sensitivity:
    - item: "Realizar análise de leave-one-out"
      required: true
      weight: 0.9
      r_code: "# Leave-one-out analysis
    loo_results <- sapply(1:nrow(data), function(i) {
      model <- lm(y ~ treatment + controls, data = data[-i, ])
      coef(model)['treatment']
    })"

    - item: "Testar diferentes janelas de tempo"
      required: true
      weight: 1.0
      notes: "Different time windows"

    - item: "Analisar subamostras geográficas"
      required: true
      weight: 0.9
      notes: "Geographic subsamples"

# Seção 4: Validação por Placebo e Falsificação
placebo_validation:
  placebo_tests:
    - item: "Testar efeitos em períodos pré-tratamento"
      required: true
      weight: 1.0
      r_code: "# Placebo test - pre-treatment effects
    placebo_data <- data %>% mutate(time_to_treatment = time - treatment_date)
    placebo_model <- feols(y ~ placeb_treatment + controls | unit_fe + time_fe, data = placebo_data)
    summary(placebo_model)"

    - item: "Testar efeitos em grupos não-tratados"
      required: true
      weight: 1.0
      notes: "Placebo tests on control groups"

    - item: "Realizar testes de randomização de tratamento"
      required: true
      weight: 0.9
      notes: "Randomization tests"

  falsification_tests:
    - item: "Testar efeitos em outcomes que não deveriam ser afetados"
      required: true
      weight: 1.0
      notes: "Falsification tests on unaffected outcomes"

    - item: "Testar diferentes datas de implementação"
      required: true
      weight: 0.9
      notes: "Different implementation dates"

    - item: "Verificar consistência com teoria econômica"
      required: true
      weight: 1.0
      notes: "Economic theory consistency"

# Seção 5: Validação por Métodos Alternativos
alternative_methods_validation:
  cross_method_validation:
    - item: "Comparar resultados com métodos diferentes"
      required: true
      weight: 1.0
      r_code: "# Cross-method comparison
    did_result <- did2s::did2s(y ~ treatment, data = data, unit_fe = unit, time_fe = time)
    iv_result <- ivreg(y ~ treatment_endogenous | instrument + controls, data = data)
    rd_result <- rdrobust(y, running_score, cutoff = cutoff)
    matching_result <- Match(Y = y, Tr = treatment, X = covariates)"

    - item: "Verificar consistência dos efeitos estimados"
      required: true
      weight: 1.0
      notes: "Consistency across methods"

    - item: "Documentar diferenças entre métodos"
      required: true
      weight: 1.0
      notes: "Method differences documentation"

  synthetic_control_validation:
    - item: "Construir grupo controle sintético quando aplicável"
      required: true
      weight: 0.9
      r_code: "# Synthetic control
    library(Synth)
    dataprep_out <- dataprep(data,
      dependent = outcome,
      predictors = covariates,
      unit.variable = unit_id,
      time.variable = time,
      treatment.identifier = treated_unit,
      controls.identifier = control_units,
      time.optimize.ssr = pre_period)
    synth_out <- synth(dataprep_out)"

    - item: "Testar placebo synthetic control"
      required: true
      weight: 0.9
      notes: "Placebo synthetic control"

    - item: "Validar preços de otimização"
      required: true
      weight: 0.8
      notes: "Optimization weights validation"

  machine_learning_validation:
    - item: "Usar métodos de ML para heterogeneidade de tratamento"
      required: true
      weight: 0.8
      r_code: "# ML for treatment heterogeneity
    library(grf)
    cf <- causal_forest(X = covariates, Y = outcome, W = treatment, data = data)
    average_treatment(cf)"

    - item: "Validar com cross-validation causal"
      required: true
      weight: 0.8
      notes: "Causal cross-validation"

    - item: "Testar robustez a hiperparâmetros"
      required: true
      weight: 0.7
      notes: "Hyperparameter robustness"

# Seção 6: Validação Específica para Contexto Brasileiro
brazilian_context_validation:
  institutional_validation:
    - item: "Controlar por mudanças institucionais brasileiras"
      required: true
      weight: 1.0
      notes: "Brazilian institutional changes"

    - item: "Considerar contexto regulatório do CADE/SEAE"
      required: true
      weight: 1.0
      notes: "CADE/SEAE regulatory context"

    - item: "Validar com marcos temporais institucionais"
      required: true
      weight: 0.9
      notes: "Institutional timeline validation"

  economic_context_validation:
    - item: "Controlar por ciclos econômicos brasileiros"
      required: true
      weight: 1.0
      notes: "Brazilian economic cycles"

    - item: "Considerar efeitos de políticas macroeconômicas"
      required: true
      weight: 1.0
      notes: "Macroeconomic policy effects"

    - item: "Testar heterogeneidade regional brasileira"
      required: true
      weight: 1.0
      r_code: "# Regional heterogeneity
    regional_het <- feols(y ~ treatment*region + controls | unit_fe + time_fe, data = data)
    summary(regional_het)"

  policy_validation:
    - item: "Validar políticas antitruste brasileiras específicas"
      required: true
      weight: 1.0
      notes: "Brazilian antitrust policy validation"

    - item: "Considerar efeitos de spillover entre estados/regiões"
      required: true
      weight: 0.9
      notes: "Regional spillover effects"

    - item: "Testar interações com políticas federais/estaduais"
      required: true
      weight: 0.9
      notes: "Federal/state policy interactions"

# Seção 7: Validação de Não Linearidade e Complexidade
nonlinear_validation:
  nonlinearity_testing:
    - item: "Testar efeitos de tratamento não-lineares"
      required: true
      weight: 0.9
      r_code: "# Non-linear treatment effects
    nonlinear_model <- lm(y ~ poly(treatment, 2) + controls, data = data)
    summary(nonlinear_model)"

    - item: "Testar heterogeneidade de tratamento"
      required: true
      weight: 1.0
      r_code: "# Treatment heterogeneity
    het_model <- feols(y ~ treatment*covariate + controls | unit_fe + time_fe, data = data)
    summary(het_model)"

    - item: "Verificar efeitos de longo prazo vs curto prazo"
      required: true
      weight: 0.9
      notes: "Long-term vs short-term effects"

  dynamic_effects_validation:
    - item: "Estimar efeitos dinâmicos do tratamento"
      required: true
      weight: 1.0
      r_code: "# Dynamic treatment effects
    event_study_data <- data %>% mutate(
      rel_time = time - treatment_date,
      rel_time = ifelse(rel_time < -5, -5, rel_time),
      rel_time = ifelse(rel_time > 5, 5, rel_time)
    )
    event_study <- feols(y ~ factor(rel_time) + controls | unit_fe + time_fe, data = event_study_data)"

    - item: "Testar persistência dos efeitos"
      required: true
      weight: 0.9
      notes: "Effect persistence tests"

    - item: "Verificar reversão ou adaptação"
      required: true
      weight: 0.8
      notes: "Reversal or adaptation effects"

# Seção 8: Validação Externa e Generalização
external_validation:
  external_generalization:
    - item: "Testar generalização para outros contextos"
      required: true
      weight: 0.8
      notes: "External generalization tests"

    - item: "Comparar com literatura internacional"
      required: true
      weight: 0.9
      notes: "International literature comparison"

    - item: "Validar com evidência qualitativa"
      required: false
      weight: 0.7
      notes: "Qualitative evidence validation"

  transportability_validation:
    - item: "Avaliar transportabilidade para outros mercados"
      required: true
      weight: 0.8
      notes: "Market transportability"

    - item: "Testar em diferentes períodos temporais"
      required: true
      weight: 0.8
      notes: "Time period transportability"

    - item: "Verificar aplicabilidade para diferentes setores"
      required: true
      weight: 0.7
      notes: "Sectoral applicability"

# Seção 9: Validação de Incerteza e Erro
uncertainty_validation:
  statistical_uncertainty:
    - item: "Calcular erros padrão robustos"
      required: true
      weight: 1.0
      r_code: "# Robust standard errors
    robust_model <- feols(y ~ treatment + controls | unit_fe + time_fe, data = data, vcov = ~unit_id)"

    - item: "Realizar bootstrap para inferência"
      required: true
      weight: 0.9
      r_code: "# Bootstrap inference
    boot_results <- boot(data, function(data, indices) {
      sample_data <- data[indices, ]
      model <- lm(y ~ treatment + controls, data = sample_data)
      return(coef(model)['treatment'])
    }, R = 1000)"

    - item: "Calcular intervalos de confiança"
      required: true
      weight: 1.0
      notes: "Confidence intervals"

  model_uncertainty:
    - item: "Realizar análise de sensibilidade a modelos"
      required: true
      weight: 0.9
      notes: "Model sensitivity analysis"

    - item: "Testar diferentes métodos de inferência"
      required: true
      weight: 0.9
      notes: "Different inference methods"

    - item: "Quantificar incerteza de especificação"
      required: true
      weight: 0.8
      notes: "Specification uncertainty"

# Seção 10: Documentação e Transparência
documentation_validation:
  transparency_documentation:
    - item: "Documentar todas as decisões de pré-registro"
      required: true
      weight: 1.0
      notes: "Pre-registration documentation"

    - item: "Fornecer código reprodutível completo"
      required: true
      weight: 1.0
      notes: "Complete reproducible code"

    - item: "Incluir dados de análise e código de limpeza"
      required: true
      weight: 1.0
      notes: "Analysis data and cleaning code"

  results_documentation:
    - item: "Apresentar todos os resultados (inclusive não-significantes)"
      required: true
      weight: 1.0
      notes: "Complete results presentation"

    - item: "Documentar limitações e restrições"
      required: true
      weight: 1.0
      notes: "Limitations documentation"

    - item: "Discutir implicações para políticas brasileiras"
      required: true
      weight: 0.9
      notes: "Brazilian policy implications"

# Seção 11: Critérios de Aprovação
approval_criteria:
  mandatory_validation:
    - item: "Todos os testes obrigatórios devem ser completados"
      required: true
      weight: 1.0
      notes: "Mandatory tests completion"

    - item: "Score mínimo de 85% na validação de robustez"
      required: true
      weight: 1.0
      notes: "85% minimum robustness score"

    - item: "Consistente com teoria econômica e evidência empírica"
      required: true
      weight: 1.0
      notes: "Economic theory consistency"

  quality_thresholds:
    - item: "Resultados devem ser robustos a múltiplas especificações"
      required: true
      weight: 1.0
      notes: "Robust to multiple specifications"

    - item: "Placebo tests devem mostrar efeitos nulos pré-tratamento"
      required: true
      weight: 1.0
      notes: "Null pre-treatment placebo effects"

    - item: "Efeitos devem ser economicamente significativos"
      required: true
      weight: 0.9
      notes: "Economic significance"

  brazilian_specific_criteria:
    - item: "Análise deve considerar contexto institucional brasileiro"
      required: true
      weight: 1.0
      notes: "Brazilian institutional context"

    - item: "Resultados devem ser relevantes para políticas antitruste"
      required: true
      weight: 1.0
      notes: "Antitrust policy relevance"

    - item: "Deve usar dados e contexto brasileiros apropriados"
      required: true
      weight: 1.0
      notes: "Appropriate Brazilian data context"

---
## Instruções de Uso

### Como Utilizar Este Checklist

1. **Executar cada item de validação** e documentar resultados
2. **Calcular score ponderado** baseado nos pesos
3. **Priorizar validações obrigatórias** (required = true)
4. **Usar R preferencialmente** com comentários em inglês
5. **Considerar contexto brasileiro** em todas as análises

### Níveis de Evidência

- **Forte evidência:** Passa em todos os testes principais e robustez
- **Evidência moderada:** Passa na maioria dos testes com algumas preocupações
- **Evidência fraca:** Falha em testes importantes ou alta sensibilidade

### Critérios de Qualidade

- **Reprodutibilidade:** Código completo e dados disponíveis
- **Robustez:** Resultados estáveis a diferentes especificações
- **Relevância:** Importante para políticas antitruste brasileiras
- **Transparência:** Todas as decisões documentadas

### Padrões Técnicos

- **Linguagem:** Português para documentação, inglês para código
- **Software:** R preferencialmente para inferência causal
- **Validação:** Múltiplos métodos complementares
- **Contexto:** Análise contextualizada para o Brasil