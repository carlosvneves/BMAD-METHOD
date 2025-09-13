# Checklist de Metodologia de Análise de Políticas
# Para Avaliação de Impacto de Políticas Antitruste no Brasil

checklist_info:
  name: "Policy Analysis Methodology Checklist"
  version: "1.0"
  author: "Econometrician Agent - Brazilian Antitrust Analytics"
  description: "Checklist abrangente para metodologia de análise de políticas com foco em antitruste e contexto brasileiro"
  language: "pt-BR"
  created_date: "2024-01-01"
  last_updated: "2024-01-01"

# Seção 1: Definição e Escopo da Política
policy_definition:
  problem_identification:
    - item: "Definir claramente o problema de mercado a ser abordado"
      required: true
      weight: 1.0
      notes: "Clear market problem definition"

    - item: "Documentar evidências de falha de mercado"
      required: true
      weight: 1.0
      notes: "Market failure evidence documentation"

    - item: "Identificar stakeholders afetados pela política"
      required: true
      weight: 1.0
      notes: "Affected stakeholders identification"

  policy_objectives:
    - item: "Estabelecer objetivos claros e mensuráveis"
      required: true
      weight: 1.0
      notes: "Clear and measurable objectives"

    - item: "Definir métricas de sucesso da política"
      required: true
      weight: 1.0
      notes: "Policy success metrics"

    - item: "Estabelecer linha de base para comparação"
      required: true
      weight: 1.0
      notes: "Baseline establishment"

  scope_definition:
    - item: "Definir cobertura geográfica da política"
      required: true
      weight: 1.0
      notes: "Geographic coverage definition"

    - item: "Especificar setores e mercados relevantes"
      required: true
      weight: 1.0
      notes: "Relevant sectors and markets"

    - item: "Determinar período de análise e implementação"
      required: true
      weight: 1.0
      notes: "Analysis and implementation period"

# Seção 2: Análise de Mercado e Contexto
market_analysis:
  market_structure:
    - item: "Analisar estrutura de mercado relevante"
      required: true
      weight: 1.0
      r_code: "# Market structure analysis
    concentration_metrics <- calculate_hhi(market_shares)
    market_power_metrics <- estimate_lerner_index(prices, costs)"

    - item: "Calcular índices de concentração (HHI, CR4, etc.)"
      required: true
      weight: 1.0
      r_code: "# Concentration indices
    hhi <- sum(market_shares^2)
    cr4 <- sum(head(sort(market_shares, decreasing = TRUE), 4))"

    - item: "Identificar barreiras à entrada e saída"
      required: true
      weight: 1.0
      notes: "Entry and exit barriers identification"

  competitive_landscape:
    - item: "Mapear concorrentes e participação de mercado"
      required: true
      weight: 1.0
      r_code: "# Competitor mapping
    competitors_data <- data.frame(
      firm = firm_names,
      market_share = market_shares,
      revenue = firm_revenues
    )"

    - item: "Analisar condições competitivas atuais"
      required: true
      weight: 1.0
      notes: "Current competitive conditions analysis"

    - item: "Identificar práticas anticompetitivas existentes"
      required: true
      weight: 1.0
      notes: "Existing anticompetitive practices"

  institutional_context:
    - item: "Analisar framework regulatório brasileiro"
      required: true
      weight: 1.0
      notes: "Brazilian regulatory framework analysis"

    - item: "Considerar jurisdição do CADE e SEAE"
      required: true
      weight: 1.0
      notes: "CADE and SEAE jurisdiction consideration"

    - item: "Documentar histórico de políticas similares"
      required: true
      weight: 0.9
      notes: "Similar policies history documentation"

# Seção 3: Análise Causal e Identificação
causal_analysis:
  identification_strategy:
    - item: "Selecionar método de identificação apropriado"
      required: true
      weight: 1.0
      notes: "Appropriate identification method selection"

    - item: "Justificar suposições de identificação"
      required: true
      weight: 1.0
      notes: "Identification assumptions justification"

    - item: "Documentar potenciais viéses e limitações"
      required: true
      weight: 1.0
      notes: "Potential biases and limitations documentation"

  counterfactual_analysis:
    - item: "Construir cenário contrafactual plausível"
      required: true
      weight: 1.0
      r_code: "# Counterfactual construction
    synthetic_control <- Synth::synth(
      dataprep.out = dataprep_object,
      method.opt = 'BFGS'
    )"

    - item: "Validar suposições de tendências paralelas"
      required: true
      weight: 1.0
      r_code: "# Parallel trends validation
    parallel_trends_test <- lm(
      outcome ~ treatment*time_period + controls + fixed_effects,
      data = pre_treatment_data
    )"

    - item: "Realizar testes de placebo e falsificação"
      required: true
      weight: 1.0
      notes: "Placebo and falsification tests"

  treatment_effects:
    - item: "Estimar efeitos de tratamento heterogêneos"
      required: true
      weight: 1.0
      r_code: "# Heterogeneous treatment effects
    library(grf)
    causal_forest <- causal_forest(
      X = covariates,
      Y = outcomes,
      W = treatment,
      num.trees = 2000
    )"

    - item: "Analisar efeitos de curto e longo prazo"
      required: true
      weight: 1.0
      r_code: "# Short and long-term effects
    event_study <- feols(
      outcome ~ lead_lag_treatment + controls + fixed_effects,
      data = event_study_data
    )"

    - item: "Quantificar efeitos de equilíbrio geral"
      required: true
      weight: 0.9
      notes: "General equilibrium effects quantification"

# Seção 4: Análise de Custo-Benefício
cost_benefit_analysis:
  benefit_quantification:
    - item: "Quantificar benefícios econômicos diretos"
      required: true
      weight: 1.0
      r_code: "# Direct economic benefits
    consumer_surplus <- calculate_consumer_surplus(
      demand_curve, price_change, quantity_change
    )
    producer_surplus <- calculate_producer_surplus(
      supply_curve, price_change, quantity_change
    )"

    - item: "Estimar benefícios sociais indiretos"
      required: true
      weight: 1.0
      r_code: "# Indirect social benefits
    employment_effects <- estimate_employment_effects(
      output_change, employment_elasticity
    )
    innovation_effects <- estimate_innovation_spillovers(
      rdd_investment, knowledge_spillover_rate
    )"

    - item: "Calcular benefícios dinâmicos e de longo prazo"
      required: true
      weight: 0.9
      r_code: "# Long-term dynamic benefits
    present_value_benefits <- npv(
      cash_flows = benefits_stream,
      discount_rate = social_discount_rate,
      time_period = analysis_horizon
    )"

  cost_quantification:
    - item: "Estimar custos diretos de implementação"
      required: true
      weight: 1.0
      r_code: "# Direct implementation costs
    implementation_costs <- data.frame(
      administrative_costs = admin_budget,
      enforcement_costs = enforcement_budget,
      monitoring_costs = monitoring_budget
    )
    total_direct_costs <- sum(implementation_costs)"

    - item: "Quantificar custos de conformidade para empresas"
      required: true
      weight: 1.0
      r_code: "# Compliance costs for firms
    compliance_costs <- estimate_compliance_costs(
      firms_data,
      regulatory_requirements,
      compliance_rates
    )"

    - item: "Calcular custos de oportunidade e eficiência"
      required: true
      weight: 1.0
      r_code: "# Opportunity and efficiency costs
    deadweight_loss <- calculate_deadweight_loss(
      market_equilibrium, regulated_equilibrium
    )
    efficiency_costs <- estimate_efficiency_losses(
      production_function, regulatory_constraints
    )"

  discounting_and_time_horizon:
    - item: "Selecionar taxa de desconto social apropriada"
      required: true
      weight: 1.0
      r_code: "# Social discount rate selection
    social_discount_rate <- 0.05  # 5% real discount rate for Brazil
    discount_factors <- 1 / (1 + social_discount_rate) ^ (1:time_horizon)"

    - item: "Definir horizonte temporal de análise"
      required: true
      weight: 1.0
      notes: "Analysis time horizon definition"

    - item: "Realizar análise de sensibilidade de desconto"
      required: true
      weight: 0.9
      r_code: "# Discount rate sensitivity
    discount_scenarios <- c(0.03, 0.05, 0.07)
    npv_results <- sapply(discount_scenarios, function(rate) {
      npv(benefits - costs, discount_rate = rate)
    })"

# Seção 5: Análise de Distribuição e Equidade
distributional_analysis:
  welfare_distribution:
    - item: "Analisar distribuição de benefícios entre consumidores"
      required: true
      weight: 1.0
      r_code: "# Consumer welfare distribution
    consumer_welfare_changes <- calculate_welfare_changes_by_income(
      demand_elasticities_by_income,
      price_changes,
      income_groups
    )"

    - item: "Avaliar impactos sobre produtores e empregadores"
      required: true
      weight: 1.0
      r_code: "# Producer and employer impacts
    producer_impacts <- estimate_producer_surplus_changes(
      supply_curves_by_firm_size,
      market_structure_changes
    )
    employment_impacts <- estimate_employment_by_firm_type(
      output_changes, labor_intensity_by_firm
    )"

    - item: "Quantificar efeitos sobre diferentes regiões"
      required: true
      weight: 1.0
      r_code: "# Regional effects
    regional_impacts <- estimate_regional_effects(
      regional_data,
      policy_intensity_by_region,
      regional_elasticities
    )"

  equity_assessment:
    - item: "Avaliar impactos distributivos por grupo socioeconômico"
      required: true
      weight: 1.0
      r_code: "# Socioeconomic distributional impacts
    equity_analysis <- calculate_equity_metrics(
      welfare_changes_by_group,
      baseline_inequality_metrics,
      population_weights
    )"

    - item: "Considerar efeitos sobre PMEs vs grandes empresas"
      required: true
      weight: 1.0
      r_code: "# SME vs large firm effects
    sme_effects <- estimate_sme_specific_effects(
      sme_data,
      cost_structure_differences,
      market_access_constraints
    )"

    - item: "Analisar impactos sobre emprego formal vs informal"
      required: true
      weight: 0.9
      r_code: "# Formal vs informal employment
    employment_formality_effects <- estimate_formality_effects(
      labor_market_data,
      policy_impact_on_formality,
      informal_sector_elasticities
    )"

  progressive_assessment:
    - item: "Determinar progressividade/regressividade da política"
      required: true
      weight: 1.0
      r_code: "# Policy progressivity assessment
    progressivity_metrics <- calculate_progressivity_index(
      welfare_changes_by_decile,
      average_welfare_change,
      gini_coefficient
    )"

    - item: "Avaliar impactos sobre pobreza e desigualdade"
      required: true
      weight: 1.0
      r_code: "# Poverty and inequality impacts
    poverty_effects <- estimate_poverty_effects(
      household_income_changes,
      poverty_lines,
      inequality_measures
    )"

    - item: "Considerar efeitos intergeracionais"
      required: true
      weight: 0.8
      notes: "Intergenerational effects consideration"

# Seção 6: Análise de Riscos e Incertezas
risk_analysis:
  risk_identification:
    - item: "Identificar riscos de implementação da política"
      required: true
      weight: 1.0
      r_code: "# Implementation risk identification
    implementation_risks <- data.frame(
      risk = c('regulatory_capture', 'enforcement_challenges', 'compliance_costs'),
      probability = c(0.3, 0.5, 0.7),
      impact = c('high', 'medium', 'high'),
      mitigation = c('transparency_measures', 'capacity_building', 'phased_implementation')
    )"

    - item: "Avaliar riscos de distorções de mercado"
      required: true
      weight: 1.0
      notes: "Market distortion risks assessment"

    - item: "Considerar riscos legais e institucionais"
      required: true
      weight: 1.0
      notes: "Legal and institutional risks consideration"

  uncertainty_quantification:
    - item: "Realizar análise de sensibilidade de parâmetros"
      required: true
      weight: 1.0
      r_code: "# Parameter sensitivity analysis
    sensitivity_results <- sensitivity_analysis(
      model = policy_model,
      parameters = c(elasticity, discount_rate, market_growth),
      ranges = list(elasticity = c(-2, -0.5),
                   discount_rate = c(0.03, 0.08),
                   market_growth = c(0.01, 0.05))
    )"

    - item: "Calcular intervalos de confiança para benefícios líquidos"
      required: true
      weight: 1.0
      r_code: "# Net benefits confidence intervals
    bootstrap_results <- bootstrap_net_benefits(
      model_estimates,
      parameter_uncertainty,
      n_bootstrap = 1000
    )
    ci_95 <- quantile(bootstrap_results, c(0.025, 0.975))"

    - item: "Realizar análise de cenários alternativos"
      required: true
      weight: 1.0
      r_code: "# Alternative scenarios analysis
    scenarios <- list(
      baseline = list(growth = 0.03, elasticity = -1.2, compliance = 0.8),
      optimistic = list(growth = 0.05, elasticity = -1.5, compliance = 0.9),
      pessimistic = list(growth = 0.01, elasticity = -0.8, compliance = 0.6)
    )
    scenario_results <- lapply(scenarios, evaluate_policy_scenario)"

  robustness_testing:
    - item: "Testar robustez a diferentes metodologias"
      required: true
      weight: 1.0
      r_code: "# Methodological robustness
    methods_comparison <- compare_methods(
      methods = c('did', 'iv', 'rd', 'synthetic_control'),
      data = policy_data,
      outcome = outcome_variable,
      treatment = treatment_variable
    )"

    - item: "Validar com diferentes amostras e períodos"
      required: true
      weight: 1.0
      notes: "Different samples and periods validation"

    - item: "Realizar análise de valor presente líquido (VPL)"
      required: true
      weight: 0.9
      r_code: "# Net present value analysis
    npv_analysis <- calculate_npv(
      benefits_stream = projected_benefits,
      costs_stream = projected_costs,
      discount_rate = social_discount_rate,
      project_life = analysis_horizon
    )"

# Seção 7: Análise de Implementação e Viabilidade
implementation_analysis:
  institutional_feasibility:
    - item: "Avaliar capacidade institucional para implementação"
      required: true
      weight: 1.0
      notes: "Institutional capacity assessment"

    - item: "Identificar requisitos legais e regulatórios"
      required: true
      weight: 1.0
      notes: "Legal and regulatory requirements identification"

    - item: "Analisar coordenação entre agências governamentais"
      required: true
      weight: 1.0
      notes: "Government agency coordination analysis"

  operational_feasibility:
    - item: "Desenvolver plano de implementação detalhado"
      required: true
      weight: 1.0
      r_code: "# Detailed implementation plan
    implementation_plan <- data.frame(
      phase = c('preparation', 'pilot', 'full_implementation', 'monitoring'),
      timeline = c('6 months', '12 months', '24 months', 'ongoing'),
      responsible_agency = c('SEAE', 'CADE', 'SEAE+CADE', 'CADE'),
      budget_allocation = c(0.15, 0.25, 0.5, 0.1)
    )"

    - item: "Estimar custos operacionais e de monitoramento"
      required: true
      weight: 1.0
      r_code: "# Operational and monitoring costs
    operational_costs <- estimate_operational_costs(
      staff_requirements,
      technology_infrastructure,
      enforcement_activities
    )"

    - item: "Identificar necessidades de capacitação"
      required: true
      weight: 0.9
      notes: "Capacity building needs identification"

  political_feasibility:
    - item: "Analisar suporte político e stakeholder engagement"
      required: true
      weight: 1.0
      notes: "Political support and stakeholder engagement analysis"

    - item: "Avaliar oposição e barreiras políticas"
      required: true
      weight: 1.0
      notes: "Opposition and political barriers assessment"

    - item: "Desenvolver estratégia de comunicação"
      required: true
      weight: 0.9
      notes: "Communication strategy development"

# Seção 8: Análise de Monitoramento e Avaliação
monitoring_evaluation:
  indicator_development:
    - item: "Desenvolver indicadores de desempenho claros"
      required: true
      weight: 1.0
      r_code: "# Performance indicators development
    performance_indicators <- data.frame(
      indicator = c('market_concentration', 'consumer_prices', 'product_quality', 'innovation_index'),
      target = c('decrease_10%', 'decrease_5%', 'increase_15%', 'increase_20%'),
      measurement_method = c('HHI_calculation', 'price_index', 'quality_surveys', 'patent_applications'),
      frequency = c('quarterly', 'monthly', 'annual', 'annual')
    )"

    - item: "Estabelecer linha de base e metas"
      required: true
      weight: 1.0
      notes: "Baseline and targets establishment"

    - item: "Definir frequência de monitoramento"
      required: true
      weight: 1.0
      notes: "Monitoring frequency definition"

  data_collection_systems:
    - item: "Desenvolver sistemas de coleta de dados"
      required: true
      weight: 1.0
      r_code: "# Data collection systems
    data_collection_plan <- list(
      administrative_data = c('CADE_decisions', 'SEAE_investigations', 'market_registries'),
      survey_data = c('firm_surveys', 'consumer_surveys', 'expert_opinions'),
      market_data = c('price_data', 'quantity_data', 'quality_metrics')
    )"

    - item: "Garantir qualidade e consistência dos dados"
      required: true
      weight: 1.0
      notes: "Data quality and consistency assurance"

    - item: "Estabelecer protocolos de verificação"
      required: true
      weight: 0.9
      notes: "Verification protocols establishment"

  evaluation_framework:
    - item: "Desenvolver framework de avaliação ex-ante e ex-post"
      required: true
      weight: 1.0
      r_code: "# Evaluation framework
    evaluation_framework <- list(
      ex_ante_evaluation = list(
        methods = c('cost_benefit', 'impact_assessment', 'risk_analysis'),
        timeline = 'before_implementation',
        responsible = 'SEAE'
      ),
      ex_post_evaluation = list(
        methods = c('counterfactual_analysis', 'stakeholder_surveys', 'market_analysis'),
        timeline = '12_36_months_after',
        responsible = 'CADE'
      )
    )"

    - item: "Planejar avaliações de médio e longo prazo"
      required: true
      weight: 1.0
      notes: "Medium and long-term evaluation planning"

    - item: "Estabelecer mecanismos de feedback e ajuste"
      required: true
      weight: 0.9
      notes: "Feedback and adjustment mechanisms establishment"

# Seção 9: Análise Específica para Contexto Brasileiro
brazilian_context_analysis:
  institutional_specificities:
    - item: "Considerar estrutura federativa brasileira"
      required: true
      weight: 1.0
      r_code: "# Brazilian federal structure considerations
    federal_analysis <- analyze_federal_implications(
      policy_design = policy_specifications,
      state_responsibilities = state_implementation_costs,
      municipal_impacts = local_economic_effects
    )"

    - item: "Analisar coordenação entre esferas governamentais"
      required: true
      weight: 1.0
      notes: "Government spheres coordination analysis"

    - item: "Considerar capacidade de implementação estadual/municipal"
      required: true
      weight: 1.0
      notes: "State/municipal implementation capacity consideration"

  economic_context:
    - item: "Incorporar volatilidade econômica brasileira"
      required: true
      weight: 1.0
      r_code: "# Brazilian economic volatility incorporation
    economic_volatility <- analyze_macro_volatility(
      gdp_volatility = brazilian_gdp_volatility,
      inflation_volatility = brazilian_inflation_volatility,
      exchange_rate_volatility = brl_volatility
    )"

    - item: "Considerar ciclos de negócios brasileiros"
      required: true
      weight: 1.0
      notes: "Brazilian business cycles consideration"

    - item: "Analisar impacto em setores estratégicos"
      required: true
      weight: 1.0
      r_code: "# Strategic sectors impact analysis
    strategic_sectors <- c('oil_gas', 'mining', 'agribusiness', 'manufacturing', 'services')
    sectoral_impacts <- estimate_sectoral_policy_effects(
      policy_specifications,
      sectoral_linkages_matrix,
      employment_intensity_by_sector
    )"

  social_context:
    - item: "Considerar desigualdades regionais brasileiras"
      required: true
      weight: 1.0
      r_code: "# Brazilian regional inequalities consideration
    regional_analysis <- analyze_regional_disparities(
      regional_data = brazilian_regions_data,
      policy_sensitivity_by_region = regional_elasticities,
      development_levels = regional_development_indices
    )"

    - item: "Avaliar impactos sobre informalidade"
      required: true
      weight: 1.0
      r_code: "# Informality impacts assessment
    informality_effects <- estimate_informality_effects(
      informal_sector_size = brazilian_informality_rate,
      formalization_elasticities = sector_formalization_elasticities,
      policy_costs_on_informal = informal_sector_burden
    )"

    - item: "Considerar especificidades do mercado de trabalho"
      required: true
      weight: 1.0
      notes: "Labor market specificities consideration"

# Seção 10: Análise de Alternativas e Recomendações
alternatives_analysis:
  policy_alternatives:
    - item: "Desenvolver e comparar alternativas de política"
      required: true
      weight: 1.0
      r_code: "# Policy alternatives development and comparison
    policy_alternatives <- list(
      status_quo = list(description = 'Maintain current regulations', costs = 0, benefits = baseline_benefits),
      alternative_1 = list(description = 'Light regulation approach', costs = cost_alt1, benefits = benefit_alt1),
      alternative_2 = list(description = 'Strict regulation approach', costs = cost_alt2, benefits = benefit_alt2),
      market_based = list(description = 'Market-based incentives', costs = cost_mb, benefits = benefit_mb)
    )
    alternatives_comparison <- compare_alternatives(policy_alternatives)"

    - item: "Realizar análise de custo-benefício comparativa"
      required: true
      weight: 1.0
      notes: "Comparative cost-benefit analysis"

    - item: "Avaliar trade-offs entre alternativas"
      required: true
      weight: 1.0
      notes: "Alternatives trade-offs assessment"

  recommendation_development:
    - item: "Formular recomendações baseadas em evidências"
      required: true
      weight: 1.0
      r_code: "# Evidence-based recommendations formulation
    recommendations <- formulate_recommendations(
      analysis_results = comprehensive_analysis_results,
      cost_benefit_comparison = alternatives_comparison,
      risk_assessment = risk_analysis_results,
      stakeholder_considerations = stakeholder_analysis
    )"

    - item: "Priorizar ações de curto, médio e longo prazo"
      required: true
      weight: 1.0
      r_code: "# Short, medium, and long-term action prioritization
    action_priorities <- data.frame(
      action = c('capacity_building', 'pilot_program', 'full_implementation', 'monitoring_system'),
      timeframe = c('0-6_months', '6-18_months', '18-36_months', 'ongoing'),
      priority_level = c('high', 'medium', 'high', 'medium'),
      estimated_impact = c('medium', 'high', 'high', 'medium')
    )"

    - item: "Desenvolver estratégia de mitigação de riscos"
      required: true
      weight: 1.0
      notes: "Risk mitigation strategy development"

  implementation_roadmap:
    - item: "Criar roadmap detalhado de implementação"
      required: true
      weight: 1.0
      r_code: "# Detailed implementation roadmap
    implementation_roadmap <- create_implementation_roadmap(
      policy_recommendations = final_recommendations,
      institutional_responsibilities = agency_responsibilities,
      resource_requirements = budget_and_staff_needs,
      timeline = implementation_schedule,
      milestones = key_implementation_milestones
    )"

    - item: "Definir marcos e indicadores de progresso"
      required: true
      weight: 1.0
      notes: "Milestones and progress indicators definition"

    - item: "Estabelecer mecanismos de ajuste de curso"
      required: true
      weight: 0.9
      notes: "Course adjustment mechanisms establishment"

# Seção 11: Documentação e Transparência
documentation_standards:
  transparency_requirements:
    - item: "Documentar todas as suposições e limitações"
      required: true
      weight: 1.0
      notes: "All assumptions and limitations documentation"

    - item: "Fornecer código e dados reprodutíveis"
      required: true
      weight: 1.0
      notes: "Reproducible code and data provision"

    - item: "Incluir análise de sensibilidade completa"
      required: true
      weight: 1.0
      notes: "Complete sensitivity analysis inclusion"

  reporting_standards:
    - item: "Seguir padrões internacionais de análise de políticas"
      required: true
      weight: 1.0
      notes: "International policy analysis standards compliance"

    - item: "Preparar resumo executivo para tomadores de decisão"
      required: true
      weight: 1.0
      notes: "Executive summary for decision makers preparation"

    - item: "Desenvolver materiais de comunicação para públicos diversos"
      required: true
      weight: 0.9
      notes: "Communication materials for diverse audiences development"

  peer_review:
    - item: "Submeter análise para revisão por pares"
      required: true
      weight: 1.0
      notes: "Peer review submission"

    - item: "Incorporar feedback de especialistas"
      required: true
      weight: 1.0
      notes: "Expert feedback incorporation"

    - item: "Realizar validação externa independente"
      required: true
      weight: 0.9
      notes: "Independent external validation"

# Seção 12: Critérios de Aprovação
approval_criteria:
  methodological_rigor:
    - item: "Todos os componentes obrigatórios devem estar completos"
      required: true
      weight: 1.0
      notes: "All mandatory components completion"

    - item: "Análise deve seguir padrões metodológicos aceitos"
      required: true
      weight: 1.0
      notes: "Accepted methodological standards compliance"

    - item: "Resultados devem ser estatisticamente significativos"
      required: true
      weight: 1.0
      notes: "Statistically significant results"

  policy_relevance:
    - item: "Análise deve ser relevante para políticas antitruste"
      required: true
      weight: 1.0
      notes: "Antitrust policy relevance"

    - item: "Recomendações devem ser factíveis e implementáveis"
      required: true
      weight: 1.0
      notes: "Feasible and implementable recommendations"

    - item: "Análise deve considerar contexto brasileiro específico"
      required: true
      weight: 1.0
      notes: "Specific Brazilian context consideration"

  evidence_quality:
    - item: "Evidências devem ser robustas e replicáveis"
      required: true
      weight: 1.0
      notes: "Robust and replicable evidence"

    - item: "Análise deve superar testes de sensibilidade"
      required: true
      weight: 1.0
      notes: "Sensitivity tests passing"

    - item: "Conclusões devem ser suportadas por dados e análise"
      required: true
      weight: 1.0
      notes: "Data and analysis supported conclusions"

---
## Instruções de Uso

### Como Utilizar Este Checklist

1. **Executar cada componente de análise** e documentar resultados
2. **Calcular score ponderado** baseado nos pesos e completude
3. **Priorizar componentes obrigatórios** (required = true)
4. **Usar R preferencialmente** com comentários em inglês
5. **Considerar contexto brasileiro** em todas as análises

### Níveis de Qualidade da Análise

- **Excelente:** Todos os componentes completos, análise robusta, recomendações claras e implementáveis
- **Bom:** Maioria dos componentes completos, análise sólida com algumas limitações
- **Aceitável:** Componentes principais completos, análise básica com limitações identificadas
- **Precisa melhorar:** Componentes críticos faltando, análise fraca ou incompleta

### Prioridades de Análise

- **Crítico:** Identificação de problema, análise causal, custo-benefício, contexto brasileiro
- **Importante:** Análise de distribuição, riscos, implementação, monitoramento
- **Recomendado:** Análise de alternativas, documentação, revisão por pares

### Padrões Técnicos

- **Linguagem:** Português para documentação, inglês para código
- **Software:** R preferencialmente para análise econométrica
- **Metodologia:** Abordagem rigorosa com validação e sensibilidade
- **Contexto:** Análise contextualizada para o ambiente institucional brasileiro