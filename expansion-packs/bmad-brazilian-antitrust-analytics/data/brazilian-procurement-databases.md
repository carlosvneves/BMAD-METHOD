# Brazilian Public Procurement Databases for Cartel Investigation

## Federal Level Databases

### 1. Portal de Compras Governamentais (PCG)
**URL**: https://www.gov.br/compras/pt-br
**Authority**: Ministério da Economia/Secretaria de Gestão Corporativa
**Scope**: All federal government procurement
**Data Available**:
- Bidding notices and documents (avisos de licitação)
- Award notices (contratações)
- Contract values and winners
- Participating companies
- Pricing information
- Contract execution status
**Cartel Investigation Value**: High - Primary source for federal procurement patterns

### 2. Sistema de Gestão de Convênios e Contratos de Repasse (SICONV)
**URL**: https://siconv.gov.br
**Authority**: Ministério do Planejamento
**Scope**: Federal transfers to states/municipalities
**Data Available**:
- Transfer agreements
- Contract values
- Beneficiary entities
- Execution reports
**Cartel Investigation Value**: Medium - Regional collusion patterns

### 3. Portal da Transparência
**URL**: https://portaldatransparencia.gov.br
**Authority**: Controladoria-Geral da União (CGU)
**Scope**: All federal government spending
**Data Available**:
- Company payments by federal government
- Contract details
- Supplier information
- Payment history
**Cartel Investigation Value**: High - Comprehensive payment patterns

### 4. Páginas da Transparência (TCU)
**URL**: https://contasabertas.tcu.gov.br
**Authority**: Tribunal de Contas da União (TCU)
**Scope**: Federal public spending oversight
**Data Available**:
- Contract information
- Spending by category
- Supplier details
- Anomaly reports
**Cartel Investigation Value**: High - Audit findings and irregularities

### 5. Sistema Integrado de Administração de Serviços Gerais (SIASG)
**URL**: https://www.siasg.com.br
**Authority**: Ministério da Economia
**Scope**: Federal government contracts and purchases
**Data Available**:
- Purchase orders
- Contract management
- Supplier registration
- Price registration system
**Cartel Investigation Value**: High - Detailed transaction data

## State and Municipal Level Databases

### 6. Sistemas Estaduais de Licitações
**Examples**:
- **São Paulo**: Sistema de Licitações e Contratos do Estado de São Paulo
- **Rio de Janeiro**: Sistema de Compras do Estado do Rio de Janeiro
- **Minas Gerais**: Sistema de Compras do Estado de Minas Gerais
**Scope**: State-level procurement
**Cartel Investigation Value**: High - Regional market patterns

### 7. Portais de Transparência Estaduais e Municipais
**Coverage**: All 26 states + Federal District and major municipalities
**Data Available**:
- Local government contracts
- Municipal spending
- Supplier information
**Cartel Investigation Value**: Medium - Local cartel detection

## Specialized Databases

### 8. Cadastro Nacional de Fornecedores (CAN)
**Scope**: Registered federal government suppliers
**Data Available**:
- Company registration data
- Qualification certificates
- Sanctions history
**Cartel Investigation Value**: Medium - Supplier eligibility screening

### 9. Sistema de Cadastramento Unificado de Fornecedores (SICAF)
**Scope**: Federal supplier registration system
**Data Available**:
- Company information
- Technical capacity
- Financial capacity
**Cartel Investigation Value**: Medium - Company relationships

### 10. CEIS (Cadastro de Empresas Inidôneas e Suspensas)
**Authority**: CGU
**Scope**: Companies sanctioned for corruption
**Data Available**:
- Debarred companies
- Sanction reasons
- Sanction periods
**Cartel Investigation Value**: High - Repeat offender identification

## Sector-Specific Procurement Databases

### 11. Saúde (Health)
- **Sistema de Gerenciamento da Tabela de Procedimentos, Medicamentos e OPM do SUS (SIGTAP)**
- **Compras de medicamentos e equipamentos hospitalares**

### 12. Educação (Education)
- **Fundo Nacional de Desenvolvimento da Educação (FNDE)**
- **Programas de aquisição de materiais escolares**

### 13. Infraestrutura (Infrastructure)
- **Ministério da Infraestrutura - obras e serviços de engenharia**
- **PAC (Programa de Aceleração do Crescimento)** contracts

### 14. Defesa (Defense)
- **Ministério da Defesa - compras militares**
- **Sistema de Gestão de Material da Marinha, Exército e Aeronáutica**

## Data Integration for Cartel Investigation

### Key Data Points for Cartel Detection:
1. **Bid Submission Patterns** - Similar pricing across companies
2. **Winner Rotation** - Companies taking turns winning contracts
3. **Market Allocation** - Geographic or product specialization patterns
4. **Cover Bidding** - Companies submitting non-competitive bids
5. **Structural Changes** - Sudden changes in bidding behavior
6. **Subcontracting Patterns** - Unusual subcontracting relationships

### Integration Strategies:
1. **Cross-Database Analysis** - Link federal, state, and municipal data
2. **Time Series Analysis** - Track bidding patterns over time
3. **Network Analysis** - Identify company relationships and connections
4. **Geographic Analysis** - Detect regional collusion patterns
5. **Sector Analysis** - Focus on high-risk procurement categories

## Data Access and Technical Considerations

### API Access:
- Many portals offer REST APIs for programmatic access
- Some require authentication and special permissions
- Data formats vary (JSON, XML, CSV, PDF)
- Update frequencies differ by system

### Data Quality Challenges:
- Inconsistent data formats across systems
- Missing or incomplete information
- Varying levels of detail
- Historical data availability limitations

### Legal and Privacy Considerations:
- LGPD (Lei Geral de Proteção de Dados) compliance
- Public access restrictions for sensitive information
- Company confidentiality protections
- Legal requirements for data usage

## Recommended Investigation Approach

1. **Start with Federal Data** - PCG and Portal da Transparência provide comprehensive coverage
2. **Expand to State/Municipal** - Add regional data for complete market view
3. **Focus on High-Risk Sectors** - Construction, health, defense, infrastructure
4. **Monitor Over Time** - Track changes in bidding patterns and market structure
5. **Cross-Reference Intelligence** - Combine with other market data sources
6. **Use Advanced Analytics** - Apply machine learning for pattern detection

## Contact Points for Official Data Access

### Federal Level:
- **CGU**: Central authority for procurement transparency
- **TCU**: Court of accounts with audit powers
- **Ministério da Economia**: Managing procurement systems
- **CADE**: Competition authority with investigation powers

### State Level:
- **Tribunais de Contas Estaduais**: State courts of accounts
- **Controladorias Estaduais**: State comptroller offices
- **Secretarias de Fazenda**: State finance departments

This database ecosystem provides a comprehensive foundation for cartel detection and investigation in Brazilian public procurement markets.