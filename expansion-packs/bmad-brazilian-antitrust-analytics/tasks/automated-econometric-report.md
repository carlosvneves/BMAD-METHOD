<!-- Powered by BMAD™ Core -->

# automated-econometric-report

**Elicit:** true
**Interactive:** true

## GERAÇÃO AUTOMATIZADA DE RELATÓRIOS ECONOMÉTRICOS PARA ANÁLISE BRASILEIRA

Este processo guiado gera relatórios econométricos completos de forma automatizada, com foco em análise de mercado brasileiro e antitruste.

### PASSO 1: CONFIGURAÇÃO DO RELATÓRIO
Defina o tipo e escopo do relatório econométrico:

**Tipo de Relatório:**
- [ ] **Relatório de Modelo Econométrico:** Documentação completa de modelo econométrico
- [ ] **Relatório de Previsão Econômica:** Análise de previsão com validação
- [ ] **Relatório de Inferência Causal:** Documentação de estratégia causal
- [ ] **Relatório de Avaliação de Política:** Impacto de políticas brasileiras
- [ ] **Relatório de Detecção de Cartel:** Análise especializada para CADE
- [ ] **Relatório de Análise de Mercado:** Análise de estrutura e poder de mercado
- [ ] **Relatório de Simulação de Fusão:** Análise competitiva de operações
- [ ] **Relatório de Análise de Licitações:** Detecção de padrões suspeitos

**Nível de Detalhe:**
- [ ] **Resumido:** Sumário executivo com principais descobertas
- [ ] **Completo:** Análise completa com metodologia detalhada
- [ ] **Detalhado:** Inclui apêndices técnicos e código fonte
- [ ] **Customizado:** Personalizar seções específicas

**Público-Alvo:**
- [ ] **CADE/SEAE:** Autoridades de concorrência
- [ ] **Empresas:** Análise para tomada de decisão empresarial
- [ ] **Consultorias:** Relatórios técnicos para clientes
- [ ] **Acadêmicos:** Pesquisa e publicação
- [ ] **Poder Público:** Políticos e gestores públicos

### PASSO 2: DADOS E ANÁLISE
Configure os dados e tipo de análise:

**Fonte de Dados:**
- **Arquivo de Dados:** Caminho para arquivo (CSV, Excel, etc.)
- **Tipo de Análise:** Séries temporais, painel, cross-section, inferência causal
- **Variável Principal:** Variável dependente ou de interesse principal
- **Variáveis Explicativas:** Lista de variáveis independentes
- **Período de Análise:** Datas de início e fim

**Características da Análise:**
- **Modelos Utilizados:** Quais modelos econométricos foram aplicados?
- **Software Usado:** R, Python, Stata, ou outro
- **Metodologia Principal:** Abordagem metodológica principal
- **Testes Realizados:** Testes de diagnóstico e validação
- **Resultados Principais:** Descobertas e conclusões principais

### PASSO 3: ESTRUTURA DO RELATÓRIO
Selecione as seções do relatório:

**Seções Principais:**
- [ ] **Sumário Executivo:** Resumo conciso em português
- [ ] **Introdução e Contexto:** Contexto brasileiro e motivação
- [ ] **Metodologia:** Descrição detalhada dos métodos
- [ ] **Dados e Amostra:** Características e preprocessamento
- [ ] **Resultados Principais:** Análise e interpretação
- [ ] **Análise de Robustez:** Testes de sensibilidade
- [ ] **Implicações para Políticas:** Recomendações para CADE/SEAE
- [ ] **Limitações:** Discussão honesta das limitações
- [ ] **Conclusões:** Síntese e próximos passos
- [ ] **Apêndice Técnico:** Detalhes adicionais
- [ ] **Código Fonte:** Scripts de análise

**Elementos Visuais:**
- [ ] **Gráficos e Tabelas:** Visualizações dos principais resultados
- [ ] **Mapas:** Análises geográficas quando aplicável
- [ ] **Diagramas Causais:** DAGs e fluxos causais
- [ ] **Resultados Estatísticos:** Tabelas detalhadas com estatísticas

### PASSO 4: CONFIGURAÇÃO TÉCNICA
Configure aspectos técnicos do relatório:

**Linguagem e Formato:**
- **Idioma do Relatório:** Português brasileiro (padrão)
- **Código Fonte:** Comentários em inglês (padrão)
- **Formato de Saída:** PDF, HTML, Markdown, ou Word
- **Estilo Visual:** Template profissional ou institucional

**Nível de Automação:**
- [ ] **Totalmente Automatizado:** Geração completa sem intervenção
- [ ] **Semi-automatizado:** Revisão manual de seções específicas
- [ ] **Guiado:** Processo interativo com validação humana
- [ ] **Template-based:** Usar template existente como base

**Integrações:**
- [ ] **R Markdown:** Relatórios dinâmicos com R
- [ ] **Python Jupyter:** Notebooks interativos
- [ ] **LaTeX:** Documentos profissionais em PDF
- [ ] **Web Dashboard:** Visualizações interativas online

### PASSO 5: CONTEÚDO CONTEXTUAL BRASILEIRO
Configure conteúdo específico para o contexto brasileiro:

**Contexto Institucional:**
- **Órgãos Relevantes:** CADE, SEAE, Bacen, IBGE, etc.
- **Marco Legal:** Lei 12.529/2011, legislação pertinente
- **Políticas Setoriais:** Políticas específicas do setor analisado
- **Precedentes:** Casos relevantes e jurisprudência

**Considerações de Mercado:**
- **Estrutura de Mercado:** Concentração, competição, barreiras
- **Dados Brasileiros:** Particularidades dos dados disponíveis
- **Aspectos Regionais:** Diferenças entre regiões brasileiras
- **Sazonalidade:** Padrões específicos do mercado brasileiro

---

## FRAMEWORK DE GERAÇÃO AUTOMATIZADA

### Template Principal de Relatório
```r
# Automated econometric report generation for Brazilian market analysis
generate_econometric_report <- function(config) {
  library(rmarkdown)
  library(tidyverse)
  library(knitr)
  library(ggplot2)
  library(plotly)
  library(DT)

  # Load configuration
  report_config <- load_config(config)

  # Prepare data
  data <- load_and_preprocess_data(report_config$data_config)

  # Run analysis
  results <- run_econometric_analysis(data, report_config$analysis_config)

  # Generate report sections
  report_sections <- generate_report_sections(results, report_config)

  # Compile final report
  final_report <- compile_report(report_sections, report_config$output_config)

  return(final_report)
}

load_config <- function(config_path) {
  # Load configuration from YAML or similar
  config <- yaml::read_yaml(config_path)

  # Set defaults for Brazilian context
  defaults <- list(
    language = "portuguese",
    country = "brazil",
    currency = "BRL",
    date_format = "%d/%m/%Y",
    decimal_mark = ",",
    thousands_sep = "."
  )

  config <- modifyList(defaults, config)
  return(config)
}

generate_report_sections <- function(results, config) {
  sections <- list()

  # Executive Summary
  sections$executive_summary <- generate_executive_summary(results, config)

  # Introduction and Context
  sections$introduction <- generate_introduction(results, config)

  # Methodology
  sections$methodology <- generate_methodology(results, config)

  # Data and Sample
  sections$data <- generate_data_section(results, config)

  # Main Results
  sections$results <- generate_results_section(results, config)

  # Robustness Analysis
  sections$robustness <- generate_robustness_section(results, config)

  # Policy Implications
  sections$policy_implications <- generate_policy_implications(results, config)

  # Limitations
  sections$limitations <- generate_limitations(results, config)

  # Conclusions
  sections$conclusions <- generate_conclusions(results, config)

  # Technical Appendix
  sections$appendix <- generate_appendix(results, config)

  return(sections)
}

generate_executive_summary <- function(results, config) {
  summary_text <- paste0(
    "# Sumário Executivo\n\n",
    "## Contexto da Análise\n",
    results$context_description, "\n\n",
    "## Objetivo Principal\n",
    results$main_objective, "\n\n",
    "## Principal Descoberta\n",
    "**", results$main_finding, "**\n\n",
    "## Magnitude do Efeito\n",
    results$effect_magnitude, "\n\n",
    "## Significância Estatística\n",
    results$statistical_significance, "\n\n",
    "## Recomendação Principal\n",
    results$main_recommendation, "\n\n"
  )

  return(list(
    content = summary_text,
    visualizations = generate_executive_visualizations(results),
    key_metrics = extract_key_metrics(results)
  ))
}

generate_methodology <- function(results, config) {
  methodology_text <- paste0(
    "# Metodologia\n\n",
    "## Desenho da Pesquisa\n",
    results$research_design, "\n\n",
    "## Estratégia de Identificação Causal\n",
    results$identification_strategy, "\n\n",
    "## Métodos Econométricos\n\n"
  )

  # Add methods details
  for (method in results$methods_used) {
    methodology_text <- paste0(methodology_text,
                             "### ", method$name, "\n",
                             method$description, "\n",
                             "**Justificativa:** ", method$justification, "\n",
                             "**Suposições Principais:** ", method$assumptions, "\n\n")
  }

  methodology_text <- paste0(methodology_text,
                           "## Validação Metodológica\n",
                           results$validation_approach, "\n\n",
                           "## Limitações Metodológicas\n",
                           results$methodological_limitations, "\n\n")

  return(list(
    content = methodology_text,
    method_table = generate_method_table(results),
    methodology_diagram = generate_methodology_diagram(results)
  ))
}

generate_results_section <- function(results, config) {
  results_text <- paste0(
    "# Resultados Principais\n\n",
    "## Análise Descritiva\n\n",
    results$descriptive_analysis, "\n\n",
    "## Resultados Econométricos\n\n"
  )

  # Add main results
  for (result in results$main_results) {
    results_text <- paste0(results_text,
                          "### ", result$title, "\n\n",
                          result$description, "\n\n",
                          "**Estimativa Principal:** ", result$main_estimate, "\n",
          "**Erro Padrão:** ", result$standard_error, "\n",
          "**Valor-p:** ", result$p_value, "\n",
          "**Intervalo de Confiança 95%:** [", result$ci_lower, ", ", result$ci_upper, "]\n\n")
  }

  results_text <- paste0(results_text,
                       "## Análise de Heterogeneidade\n\n",
                       results$heterogeneity_analysis, "\n\n",
                       "## Efeitos Dinâmicos\n\n",
                       results$dynamic_effects, "\n\n")

  return(list(
    content = results_text,
    result_tables = generate_result_tables(results),
    effect_plots = generate_effect_plots(results),
    heterogeneity_plots = generate_heterogeneity_plots(results)
  ))
}

generate_policy_implications <- function(results, config) {
  implications_text <- paste0(
    "# Implicações para Políticas Brasileiras\n\n",
    "## Recomendações para CADE\n\n",
    results$cade_recommendations, "\n\n",
    "## Recomendações para SEAE\n\n",
    results$seae_recommendations, "\n\n",
    "## Implicações Setoriais\n\n"
  )

  for (sector in results$sector_implications) {
    implications_text <- paste0(implications_text,
                             "### ", sector$name, "\n",
                             sector$analysis, "\n",
                             "**Recomendação Específica:** ", sector$recommendation, "\n\n")
  }

  implications_text <- paste0(implications_text,
                           "## Impacto Econômico Estimado\n",
                           results$economic_impact, "\n\n",
                           "## Considerações de Implementação\n",
                           results$implementation_considerations, "\n\n")

  return(list(
    content = implications_text,
    impact_table = generate_impact_table(results),
    recommendation_matrix = generate_recommendation_matrix(results)
  ))
}
```

### Sistema de Templates Dinâmicos
```r
# Dynamic template system for different report types
get_report_template <- function(report_type, context = "brazilian") {
  templates <- list(
    "cartel_detection" = cartel_detection_template(),
    "market_analysis" = market_analysis_template(),
    "policy_evaluation" = policy_evaluation_template(),
    "merger_simulation" = merger_simulation_template(),
    "economic_forecast" = economic_forecast_template(),
    "causal_inference" = causal_inference_template()
  )

  base_template <- templates[[report_type]]

  # Add Brazilian context specific sections
  if (context == "brazilian") {
    base_template <- add_brazilian_context(base_template)
  }

  return(base_template)
}

cartel_detection_template <- function() {
  list(
    sections = c(
      "executive_summary",
      "introduction",
      "market_context",
      "data_sources",
      "methodology",
      "screening_results",
      "statistical_evidence",
      "economic_analysis",
      "legal_framework",
      "recommendations",
      "limitations",
      "appendix"
    ),
    required_elements = list(
      "cartel_indicators",
      "statistical_significance",
      "economic_damage",
      "legal_assessment",
      "evidence_strength"
    ),
    brazilian_additions = list(
      "cade_procedure",
      "brazilian_legal_framework",
      "leniency_program_analysis",
      "sectoral_regulations"
    )
  )
}

market_analysis_template <- function() {
  list(
    sections = c(
      "executive_summary",
      "market_overview",
      "competitive_structure",
      "market_definition",
      "market_power_analysis",
      "barriers_to_entry",
      "consumer_impact",
      "regulatory_assessment",
      "policy_recommendations",
      "conclusions"
    ),
    required_elements = list(
      "market_definition",
      "concentration_indices",
      "market_shares",
      "entry_conditions",
      "competitive_effects"
    ),
    brazilian_additions = list(
      "brazilian_market_characteristics",
      "regulatory_authority_role",
      "sectoral_policies",
      "regional_considerations"
    )
  )
}

policy_evaluation_template <- function() {
  list(
    sections = c(
      "executive_summary",
      "policy_background",
      "evaluation_objectives",
      "methodology",
      "data_description",
      "impact_analysis",
      "cost_benefit_analysis",
      "distributional_effects",
      "robustness_checks",
      "policy_recommendations",
      "implementation_guidelines"
    ),
    required_elements = list(
      "causal_estimates",
      "economic_benefits",
      "implementation_costs",
      "distributional_impact",
      "policy_effectiveness"
    ),
    brazilian_additions <- list(
      "institutional_capacity",
      "political_feasibility",
      "budgetary_constraints",
      "stakeholder_analysis"
    )
  )
}
```

### Sistema de Visualização Automatizada
```r
# Automated visualization system
generate_automated_visualizations <- function(results, config) {
  visualizations <- list()

  # Main effect visualization
  visualizations$main_effect <- plot_main_effect(results)

  # Heterogeneity analysis
  visualizations$heterogeneity <- plot_heterogeneity(results)

  # Time series analysis (if applicable)
  if (has_time_series(results)) {
    visualizations$time_series <- plot_time_series(results)
  }

  # Geographic analysis (if applicable)
  if (has_geographic_data(results)) {
    visualizations$geographic <- plot_geographic(results)
  }

  # Robustness checks
  visualizations$robustness <- plot_robustness(results)

  # Economic impact
  visualizations$economic_impact <- plot_economic_impact(results)

  return(visualizations)
}

plot_main_effect <- function(results) {
  library(ggplot2)
  library(plotly)

  # Extract main effect estimates
  estimates <- results$main_estimates

  p <- ggplot(estimates, aes(x = reorder(model, estimate), y = estimate,
                            ymin = ci_lower, ymax = ci_upper)) +
    geom_point(size = 3, color = "darkblue") +
    geom_errorbar(width = 0.3, color = "darkblue") +
    geom_hline(yintercept = 0, linetype = "dashed", color = "red") +
    coord_flip() +
    labs(title = "Efeitos Causais Principais",
         x = "Modelo/Especificação",
         y = "Efeito Estimado",
         caption = "Barras representam intervalos de confiança de 95%") +
    theme_minimal() +
    theme(axis.text.y = element_text(size = 10))

  return(ggplotly(p))
}

plot_heterogeneity <- function(results) {
  if (is.null(results$heterogeneity_results)) return(NULL)

  het_data <- results$heterogeneity_results

  p <- ggplot(het_data, aes(x = group, y = estimate, color = group)) +
    geom_point(size = 3) +
    geom_errorbar(aes(ymin = estimate - 1.96*se, ymax = estimate + 1.96*se),
                  width = 0.2) +
    labs(title = "Análise de Heterogeneidade",
         x = "Grupo",
         y = "Efeito Estimado",
         color = "Grupo") +
    theme_minimal() +
    theme(axis.text.x = element_text(angle = 45, hjust = 1))

  return(p)
}

plot_economic_impact <- function(results) {
  if (is.null(results$cost_benefit)) return(NULL)

  cb_data <- results$cost_benefit

  p <- ggplot(cb_data, aes(x = type, y = value, fill = type)) +
    geom_bar(stat = "identity", position = "dodge") +
    labs(title = "Análise Custo-Benefício",
         x = "Componente",
         y = "Valor (R$ milhões)",
         fill = "Tipo") +
    theme_minimal() +
    scale_fill_brewer(palette = "Set1")

  return(p)
}
```

### Sistema de Geração de Tabelas Automatizadas
```r
# Automated table generation
generate_automated_tables <- function(results, config) {
  tables <- list()

  # Summary statistics
  tables$summary_stats <- generate_summary_table(results)

  # Main results
  tables$main_results <- generate_results_table(results)

  # Robustness checks
  tables$robustness <- generate_robustness_table(results)

  # Economic impact
  if (!is.null(results$economic_impact)) {
    tables$economic_impact <- generate_economic_impact_table(results)
  }

  # Policy recommendations
  if (!is.null(results$recommendations)) {
    tables$recommendations <- generate_recommendations_table(results)
  }

  return(tables)
}

generate_results_table <- function(results) {
  library(kableExtra)
  library(dplyr)

  # Extract main results
  main_results <- results$main_results %>%
    select(Model, Estimate, `Std. Error`, `t value`, `Pr(>|t|)`, `CI Lower`, `CI Upper`) %>%
    mutate(
      `Pr(>|t|)` = format.pval(`Pr(>|t|)`, digits = 3),
      Estimate = round(Estimate, 4),
      `Std. Error` = round(`Std. Error`, 4),
      `t value` = round(`t value`, 3),
      `CI Lower` = round(`CI Lower`, 4),
      `CI Upper` = round(`CI Upper`, 4)
    )

  table <- kable(main_results,
                 caption = "Resultados Principais da Análise Econométrica",
                 booktabs = TRUE,
                 format = "latex") %>%
    kable_styling(latex_options = c("striped", "hold_position"),
                  full_width = FALSE) %>%
    add_header_above(c(" " = 1, "Estimativas" = 4, "Intervalo de Confiança" = 2)) %>%
    row_spec(0, background = "lightgray", color = "black", bold = TRUE)

  return(table)
}

generate_economic_impact_table <- function(results) {
  economic_data <- results$economic_impact

  table <- kable(economic_data,
                 caption = "Impacto Econômico Estimado (R$ milhões)",
                 booktabs = TRUE,
                 format = "latex",
                 digits = 2,
                 col.names = c("Componente", "Valor Estimado", "Intervalo Inferior", "Intervalo Superior")) %>%
    kable_styling(latex_options = c("striped", "hold_position"),
                  full_width = FALSE) %>%
    column_spec(2:4, width = "3cm") %>%
    row_spec(0, background = "lightgray", color = "black", bold = TRUE)

  return(table)
}
```

### Sistema de Validação e Qualidade
```r
# Quality assurance system
validate_report_quality <- function(report_config, report_content) {
  quality_checks <- list()

  # Structural validation
  quality_checks$structure <- validate_report_structure(report_config, report_content)

  # Content validation
  quality_checks$content <- validate_report_content(report_config, report_content)

  # Methodological validation
  quality_checks$methodology <- validate_methodology(report_config, report_content)

  # Language validation
  quality_checks$language <- validate_language(report_content)

  # Visualization validation
  quality_checks$visualizations <- validate_visualizations(report_content)

  return(quality_checks)
}

validate_report_structure <- function(config, content) {
  required_sections <- config$template$sections
  actual_sections <- names(content$sections)

  missing_sections <- setdiff(required_sections, actual_sections)
  extra_sections <- setdiff(actual_sections, required_sections)

  validation_result <- list(
    structure_valid = length(missing_sections) == 0,
    missing_sections = missing_sections,
    extra_sections = extra_sections,
    section_order = check_section_order(required_sections, actual_sections)
  )

  return(validation_result)
}

validate_methodology <- function(config, content) {
  methodology_checks <- list()

  # Check if methods are properly documented
  methodology_checks$methods_documented <- check_methods_documentation(content)

  # Check if assumptions are stated
  methodology_checks$assumptions_stated <- check_assumptions(content)

  # Check if limitations are discussed
  methodology_checks$limitations_discussed <- check_limitations(content)

  # Check if robustness checks are included
  methodology_checks$robustness_included <- check_robustness(content)

  return(methodology_checks)
}

validate_language <- function(content) {
  language_checks <- list()

  # Check if report is in Portuguese
  language_checks$portuguese_used <- check_portuguese_language(content)

  # Check for proper Brazilian Portuguese terminology
  language_checks$brazilian_terminology <- check_brazilian_terminology(content)

  # Check for consistent formatting
  language_checks$formatting_consistent <- check_formatting(content)

  return(language_checks)
}
```

### Sistema de Exportação e Formatação
```r
# Export and formatting system
export_report <- function(report_content, config) {
  output_format <- config$output$format

  if (output_format == "pdf") {
    return(export_pdf(report_content, config))
  } else if (output_format == "html") {
    return(export_html(report_content, config))
  } else if (output_format == "word") {
    return(export_word(report_content, config))
  } else if (output_format == "markdown") {
    return(export_markdown(report_content, config))
  }
}

export_pdf <- function(content, config) {
  library(rmarkdown)

  # Create R Markdown document
  rmd_content <- create_rmd_document(content, config)

  # Render to PDF
  output_file <- tempfile(fileext = ".pdf")

  rmarkdown::render(
    input = rmd_content,
    output_file = output_file,
    output_format = "pdf_document",
    params = list(config = config)
  )

  return(output_file)
}

create_rmd_document <- function(content, config) {
  rmd_content <- c(
    "---",
    "title: '", config$report$title, "'",
    "author: '", config$report$author, "'",
    "date: '", format(Sys.Date(), "%d/%m/%Y"), "'",
    "lang: pt-BR",
    "output:",
    "  pdf_document:",
    "    latex_engine: xelatex",
    "    includes:",
    "      in_header: 'brazilian_style.tex'",
    "---",
    "",
    content$content
  )

  return(paste(rmd_content, collapse = "\n"))
}
```

---

## PRÓXIMOS PASSOS

Com base nas suas respostas, eu vou:

1. **Configurar Relatório:** Definir estrutura e conteúdo do relatório econométrico
2. **Processar Dados:** Carregar e preprocessar dados brasileiros para análise
3. **Executar Análise:** Rodar métodos econométricos com validação robusta
4. **Gerar Conteúdo:** Produzir seções do relatório automaticamente
5. **Criar Visualizações:** Gerar gráficos e tabelas profissionalmente formatados
6. **Validar Qualidade:** Realizar verificações de qualidade e consistência
7. **Exportar Relatório:** Gerar documento final no formato desejado

Pronto para gerar seu relatório econométrico automatizado! Por favor, forneça os detalhes da sua análise, dados e preferências de formatação para que eu possa criar um relatório completo e profissional para análise de mercado brasileiro e antitruste.