# Collaboration Protocols
# For Econometrician and Data Scientist Integration

## Overview
These protocols define how the Econometrician and Data Scientist agents collaborate to deliver comprehensive Brazilian antitrust analytics while avoiding duplication and leveraging complementary strengths.

## Collaboration Principles

### 1. Clear Role Separation
- **Econometrician**: Economic theory, causal inference, policy impact, market power assessment
- **Data Scientist**: Data engineering, ML infrastructure, network analysis, automated processing

### 2. Handoff Protocols
- **Data Preparation**: Data Scientist → Econometrician
- **Economic Analysis**: Econometrician → Data Scientist
- **Joint Validation**: Both agents collaborate
- **Final Reporting**: Integrated effort

### 3. Shared Infrastructure
- Common Brazilian data sources
- Unified quality standards
- Consistent documentation
- Integrated workflows

## Specific Collaboration Scenarios

### Scenario 1: Cartel Detection Analysis
**Trigger**: `*collaborate cartel-detection`

```r
# Data Scientist Responsibilities
data_scientist_tasks <- list(
  "Extract and clean procurement data from PCG/Portal da Transparência",
  "Build company relationship networks from bidding patterns",
  "Apply ML clustering to identify suspicious bidding groups",
  "Create interactive network visualizations"
)

# Econometrician Responsibilities
econometrician_tasks <- list(
  "Apply economic screening algorithms to identified clusters",
  "Calculate potential economic damages from collusion",
  "Assess market power and competitive effects",
  "Provide policy recommendations for CADE investigations"
)

# Joint Deliverables
joint_output <- list(
  "Comprehensive cartel detection report",
  "Economic damage assessment",
  "Network analysis dashboard",
  "Policy recommendations"
)
```

### Scenario 2: Brazilian Market Analysis
**Trigger**: `*collaborate market-analysis`

```r
# Data Scientist Responsibilities
data_scientist_tasks <- list(
  "Process IBGE and BACEN economic data",
  "Create market structure visualizations",
  "Build automated data pipelines for monitoring",
  "Develop interactive dashboards"
)

# Econometrician Responsibilities
econometrician_tasks <- list(
  "Conduct market definition analysis",
  "Calculate concentration indices (HHI, CR4)",
  "Assess market power and competitive dynamics",
  "Evaluate potential antitrust concerns"
)

# Joint Deliverables
joint_output <- list(
  "Market structure analysis report",
  "Concentration metrics dashboard",
  "Competitive assessment",
  "Monitoring recommendations"
)
```

### Scenario 3: Policy Impact Evaluation
**Trigger**: `*collaborate policy-impact`

```r
# Data Scientist Responsibilities
data_scientist_tasks <- list(
  "Process policy implementation data",
  "Build counterfactual models",
  "Create treatment vs control group comparisons",
  "Develop automated monitoring systems"
)

# Econometrician Responsibilities
econometrician_tasks <- list(
  "Design causal inference strategy",
  "Conduct difference-in-differences analysis",
  "Calculate welfare effects and economic impacts",
  "Provide policy recommendations"
)

# Joint Deliverables
joint_output <- list(
  "Policy impact evaluation report",
  "Causal analysis results",
  "Welfare assessment",
  "Policy recommendations"
)
```

## Communication Protocol

### Initiation
1. Either agent can initiate collaboration with `*collaborate {scenario}`
2. System automatically identifies required tasks
3. Agents negotiate分工 and timeline

### Data Handoff Standards
- **Format**: Clean, structured data with metadata
- **Documentation**: Data dictionary and processing notes
- **Quality**: Validated against shared quality standards
- **Versioning**: Clear version control and reproducibility

### Validation Process
1. **Cross-validation**: Both agents review each other's work
2. **Sensitivity analysis**: Test robustness of findings
3. **Peer review**: Technical review of methods and results
4. **Integration**: Combine insights into final deliverables

## Quality Assurance

### Shared Quality Standards
```yaml
data_quality:
  completeness: ">95%"
  accuracy: "validated against sources"
  timeliness: "most recent available"
  consistency: "cross-validated"

methodological_quality:
  reproducibility: "fully documented code"
  validation: "multiple testing approaches"
  robustness: "sensitivity analysis performed"
  transparency: "clear assumptions documented"
```

### Review Protocol
1. **Peer Review**: Each agent reviews the other's contributions
2. **Technical Validation**: Methods and code verification
3. **Impact Assessment**: Business relevance and actionability
4. **Final Sign-off**: Joint approval before delivery

## Technical Integration

### Shared Dependencies
```r
# Common Brazilian data processing
shared_brazilian_data <- function(source, type) {
  # Data Scientist: Raw data extraction and cleaning
  clean_data <- data_scientist::wrangle_brazilian_data(source, type)

  # Econometrician: Economic validation and market context
  validated_data <- econometrician::validate_economic_context(clean_data)

  return(validated_data)
}

# Joint cartel detection workflow
detect_cartels <- function(procurement_data) {
  # Data Scientist: Network construction and ML analysis
  network_data <- data_scientist::build_procurement_network(procurement_data)
  suspicious_clusters <- data_scientist::detect_anomalous_clusters(network_data)

  # Econometrician: Economic screening and damage assessment
  economic_evidence <- econometrician::economic_cartel_screening(suspicious_clusters)
  damage_estimate <- econometrician::calculate_economic_damages(economic_evidence)

  return(list(
    network_analysis = network_data,
    suspicious_patterns = suspicious_clusters,
    economic_evidence = economic_evidence,
    damage_assessment = damage_estimate
  ))
}
```

## Conflict Resolution

### Decision Making
- **Technical Disagreements**: Based on empirical evidence and best practices
- **Methodological Choices**: Econometrician has final say on economic methods
- **Data Processing**: Data Scientist has final say on data engineering
- **Joint Decisions**: Collaborative approach with documented rationale

### Escalation Protocol
1. **Peer Discussion**: Direct negotiation between agents
2. **Evidence Review**: Present supporting evidence
3. **Best Practice Reference**: Consult established methodologies
4. **Documented Decision**: Record final decision and rationale

## Success Metrics

### Collaboration Effectiveness
- **Reduction in Duplication**: >70% reduction in overlapping work
- **Improved Quality**: Enhanced validation and cross-checking
- **Faster Delivery**: Streamlined workflows and handoffs
- **Better Outcomes**: More comprehensive and actionable insights

### Performance Indicators
- **Time to Delivery**: Reduced project completion time
- **Quality Scores**: Improved validation and accuracy
- **Stakeholder Satisfaction**: Enhanced user experience
- **Innovation**: New approaches from cross-pollination

## Continuous Improvement

### Feedback Loop
1. **Post-Project Review**: Lessons learned and improvement areas
2. **Protocol Updates**: Refine collaboration based on experience
3. **Tool Enhancement**: Improve shared tools and infrastructure
4. **Training**: Cross-training on complementary skills

### Innovation Opportunities
- **New Methods**: Joint development of innovative approaches
- **Tool Integration**: Enhanced technical integration
- **Process Optimization**: Streamlined workflows
- **Knowledge Sharing**: Cross-domain expertise exchange