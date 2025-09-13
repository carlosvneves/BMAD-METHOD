# Shared Brazilian Data Infrastructure
# For Econometrician and Data Scientist Collaboration

## Purpose
This shared infrastructure provides common Brazilian data processing capabilities, reducing duplication between agents while enabling seamless collaboration.

## Brazilian Data Sources Processing

### Primary Economic Data Sources
- **IBGE** (Brazilian Institute of Geography and Statistics)
- **BACEN** (Central Bank of Brazil)
- **SEAE** (Secretariat of Economic Monitoring)
- **CADE** (Brazilian Competition Authority)
- **IPEA** (Institute for Applied Economic Research)
- **FGV** (Getulio Vargas Foundation)

### Public Procurement Data Sources
- **PCG** (Portal de Compras Governamentais)
- **Portal da Transparência**
- **SIASG** (Federal Contract Management System)
- **TCU** (Court of Accounts) Audit Data
- **SICONV** (Federal Transfer Agreements)

## Shared Dependencies Structure

### Core Shared Tasks
```yaml
shared_tasks:
  - "shared-brazilian-data-processing.md"
  - "shared-collaboration-protocols.md"
  - "shared-quality-standards.md"
```

### Agent-Specific Tasks

#### Econometrician (Economic Analysis Focus)
```yaml
econometrician_tasks:
  - "econometric-modeling.md"
  - "causal-inference-analysis.md"
  - "generate-economic-forecast.md"
  - "antitrust-market-analysis.md"
  - "policy-impact-evaluation.md"
```

#### Data Scientist (Data Engineering & ML Focus)
```yaml
data_scientist_tasks:
  - "data-wrangling-workflows.md"
  - "ml-pipeline-development.md"
  - "network-analysis-cartels.md"
  - "procurement-data-processing.md"
  - "automated-reporting.md"
```

## Collaboration Workflow

### Data Processing Handoff
```
Data Scientist: Raw Data → Clean & Structure → Feature Engineering
↓
Econometrician: Economic Modeling → Causal Analysis → Policy Insights
```

### Joint Analysis Projects
```
Collaboration Trigger: *collaborate {cartel-detection | market-analysis | policy-evaluation}

1. Data Scientist prepares data infrastructure
2. Econometrician performs economic analysis
3. Combined results and validation
4. Joint reporting and recommendations
```

## Quality Standards

### Data Quality Standards
- **Completeness**: >95% data coverage for key variables
- **Accuracy**: Validated against official sources
- **Timeliness**: Most recent data available
- **Consistency**: Cross-validated across multiple sources

### Methodological Standards
- **Reproducibility**: All code and methods documented
- **Validation**: Multiple testing approaches
- **Robustness**: Sensitivity analysis performed
- **Transparency**: Clear documentation of assumptions

## Integration Protocols

### File Naming Conventions
- Shared files: `shared-{purpose}.md`
- Econometrician: `econometric-{purpose}.md`
- Data Scientist: `data-{purpose}.md`

### Collaboration Commands
```yaml
collaboration_commands:
  econometrician:
    - "*collaborate data-prep" - Request data preparation
    - "*collaborate network-analysis" - Joint network analysis
    - "*collaborate model-validation" - Cross-validate findings

  data_scientist:
    - "*collaborate economic-analysis" - Request economic insights
    - "*collaborate policy-impact" - Joint policy evaluation
    - "*collaborate reporting" - Combined report generation
```

## Brazilian Context Integration

### Institutional Knowledge
- CADE investigation processes and methodologies
- Brazilian regulatory framework and legal requirements
- Economic data collection and reporting standards
- Public procurement laws and procedures

### Regional Considerations
- State-level vs federal data sources
- Regional economic disparities
- Local procurement practices
- Geographic market definitions