<!-- Powered by BMAD™ Core -->

# create-deep-research-prompt

**Elicit:** true
**Interactive:** true

## CRIAÇÃO DE PROMPT DE PESQUISA APROFUNDADA PARA ECONOMETRIA

Este processo guiado ajuda a criar prompts detalhados para pesquisa econométrica focada em análise de mercado brasileiro e detecção de cartéis.

### PASSO 1: OBJETIVO DA PESQUISA
Defina o objetivo principal da sua pesquisa econométrica:

**Categoria de Pesquisa:**
- [ ] **Análise de Cartel:** Detecção e evidências de cartéis em mercados brasileiros
- [ ] **Poder de Mercado:** Avaliação de poder de mercado para investigações CADE
- [ ] **Simulação de Fusões:** Análise de efeitos competitivos de operações de concentração
- [ ] **Análise de Licitações:** Detecção de colusão em licitações públicas brasileiras
- [ ] **Previsão Econômica:** Modelagem de séries temporais para o mercado brasileiro
- [ ] **Avaliação de Política:** Impacto de políticas econômicas e regulatórias
- [ ] **Inferência Causal:** Identificação de relações causais em dados brasileiros
- [ ] **Machine Learning Aplicado:** Métodos de ML para detecção de padrões anticompetitivos

**Foco Específico:**
- Qual é o problema específico que você está investigando?
- Qual é o setor econômico ou mercado em análise?
- Qual é o período geográfico e temporal de interesse?
- Quais são as questões principais que precisam ser respondidas?

### PASSO 2: CONTEXTO ECONÔMICO
Forneça detalhes sobre o contexto econômico brasileiro:

**Setor Econômico:**
- **Características do Mercado:** Estrutura de mercado, número de empresas, barreiras à entrada
- **Regulamentação:** Órgãos reguladores, legislação relevante, políticas setoriais
- **Dinâmica Competitiva:** Nível de concorrência, inovação, substitutos
- **Dados Disponíveis:** Fontes de dados, frequência, qualidade, acessibilidade

**Contexto Institucional:**
- **CADE:** Histórico de investigações, precedentes, metodologias
- **SEAE:** Estudos de mercado, análises setoriais, recomendações
- **Outros Órgãos:** ANATEL, ANEEL, ANP, Bacen, etc.
- **Legislação:** Lei 12.529/2011, legislação concorrencial, políticas de defesa da concorrência

### PASSO 3: DADOS E VARIÁVEIS
Especifique os dados e variáveis relevantes:

**Fontes de Dados:**
- **Dados Primários:** Dados coletados diretamente da indústria ou empresas
- **Dados Secundários:** IBGE, Bacen, IPEA, FGV, B3, etc.
- **Dados de Mercado:** Preços, quantidades, participação de mercado
- **Dados de Licitações:** Portal da Transparência, compras governamentais
- **Dados Alternativos:** Web scraping, dados digitais, redes sociais

**Variáveis Chave:**
- **Variável Dependente:** O que você está tentando explicar ou prever?
- **Variáveis Independentes:** Fatores que influenciam a variável dependente
- **Variáveis de Controle:** Fatores que precisam ser controlados
- **Variáveis Instrumentais:** Para resolver problemas de endogeneidade
- **Variáveis Dummy:** Para efeitos fixos, sazonalidade, eventos

**Estrutura dos Dados:**
- **Tipo de Dados:** Cross-section, painel, séries temporais, dados não estruturados
- **Frequência:** Diária, semanal, mensal, trimestral, anual
- **Amostra:** Tamanho da amostra, período coberto, representatividade
- **Qualidade:** Dados faltantes, outliers, erros de medição

### PASSO 4: ABORDAGEM METODOLÓGICA
Defina a abordagem metodológica detalhada:

**Métodos Econométricos:**
- **Séries Temporais:**
  ```r
  # Preferência por R para séries temporais
  library(forecast)
  library(vars)
  library(urca)
  library(tseries)
  library(nlts)

  # Testes de estacionaridade
  adf.test(data$variable)
  kpss.test(data$variable)
  pp.test(data$variable)

  # Modelagem inicial VAR
  var_model <- VAR(data, p = optimal_lag)

  # Modelagem avançada se necessário
  library(dynlm)
  library(ARDL)
  ```

- **Análise de Painel:**
  ```r
  library(plm)
  library(lme4)
  library(fixest)

  # Modelos de efeitos fixos e aleatórios
  fe_model <- plm(y ~ x1 + x2, data = data, model = "within")
  re_model <- plm(y ~ x1 + x2, data = data, model = "random")
  ```

- **Inferência Causal:**
  ```r
  library(AER)
  library(ivreg)
  library(MatchIt)
  library(did)

  # Variáveis instrumentais
  iv_model <- ivreg(y ~ x1 + x2 | z1 + z2 + x2)

  # Diferenças-em-Diferenças
  did_model <- att_gt(y ~ treat + post, data = data)
  ```

**Testes Específicos:**
- **Estacionaridade:** ADF, KPSS, PP, Zivot-Andrews
- **Cointegração:** Johansen, Engle-Granger
- **Não Linearidade:** Terasvirta, White, Keenan, McLeod-Li
- **Causalidade:** Transfer Entropy, Convergence Cross Mapping
- **Robustez:** Testes de sensibilidade, validação cruzada

### PASSO 5: HIPÓTESES E EXPECTATIVAS
Formule hipóteses testáveis:

**Hipóteses Principais:**
1. Hipótese sobre a relação entre variáveis principais
2. Hipótese sobre mecanismos causais
3. Hipótese sobre heterogeneidade de efeitos
4. Hipótese sobre dinâmica temporal
5. Hipótese sobre interações com políticas

**Expectativas Teóricas:**
- **Teoria Econômica:** Qual teoria econômica fundamenta suas hipóteses?
- **Evidência Prévia:** O que a literatura existente diz sobre este problema?
- **Contexto Brasileiro:** Como o contexto institucional brasileiro afeta as expectativas?
- **Expectativas Empíricas:** Quais resultados você espera encontrar?

**Implicações de Políticas:**
- **Para CADE:** Como os resultados podem informar investigações?
- **Para Empresas:** Quais implicações para estratégias empresariais?
- **Para Política:** Como os resultados podem guiar políticas públicas?
- **Para Sociedade:** Qual o impacto potencial para consumidores e bem-estar?

### PASSO 6: DESAFIOS E LIMITAÇÕES
Identifique desafios potenciais:

**Desafios Metodológicos:**
- **Problemas de Identificação:** Endogeneidade, variáveis omitidas
- **Limitações de Dados:** Qualidade, disponibilidade, representatividade
- **Viés de Seleção:** Amostra não representativa, viés de sobrevivência
- **Heterogeneidade:** Diferenças não observadas entre unidades

**Desafios Contextuais:**
- **Institucionais:** Mudanças regulatórias, políticas governamentais
- **Econômicos:** Crises, ciclos econômicos, choques externos
- **Tecnológicos:** Inovação, digitalização, mudanças estruturais
- **Sociais:** Mudanças de preferências, comportamento do consumidor

**Estratégias de Mitigação:**
- **Metodológicas:** Uso de métodos robustos, testes de sensibilidade
- **De Dados:** Imputação, coleta adicional, fontes alternativas
- **Analíticas:** Análise de subgrupos, diferentes especificações
- **De Comunicação:** Transparência sobre limitações, interpretação cautelosa

### PASSO 7: ENTREGÁVEIS ESPERADOS
Defina o que você espera como resultado:

**Análise e Resultados:**
- **Relatório Principal:** Análise completa em português brasileiro
- **Código Fonte:** Scripts em R/Python bem documentados em inglês
- **Visualizações:** Gráficos e tabelas dos principais resultados
- **Diagnósticos:** Testes de validação e robustez
- **Apêndices:** Detalhes metodológicos e resultados adicionais

**Resultados Quantitativos:**
- **Estimativas Principais:** Coeficientes, erros padrão, p-valores
- **Intervalos de Confiança:** Para estimativas principais
- **Testes de Hipóteses:** Resultados de testes estatísticos
- **Medidas de Ajuste:** R², AIC, BIC, outras métricas
- **Análise de Sensibilidade:** Variação de resultados sob diferentes especificações

**Recomendações:**
- **Para CADE:** Recomendações específicas para investigações
- **Metodológicas:** Sugestões para melhorar métodos existentes
- **De Políticas:** Implicações para políticas de concorrência
- **De Pesquisa:** Direções para pesquisa futura

---

## TEMPLATE DE PESQUISA ESTRUTURADO

### Prompt Estruturado para LLMs

```
Contexto: Você é um economista especializado em análise de concorrência para o mercado brasileiro, com expertise em métodos econométricos avançados.

Objetivo: [Descrição detalhada do objetivo da pesquisa]

Setor e Mercado: [Descrição do setor, estrutura de mercado, contexto institucional]

Dados e Variáveis:
- Fontes: [Lista de fontes de dados]
- Variável Dependente: [Definição e medição]
- Variáveis Independentes: [Lista e definições]
- Período e Amostra: [Detalhes temporais e amostrais]

Abordagem Metodológica:
1. Análise Exploratória: [Estatísticas descritivas, visualizações]
2. Modelagem Principal: [Método econométrico principal]
3. Testes de Validação: [Testes de diagnóstico e robustez]
4. Análise de Sensibilidade: [Estratégias para verificar robustez]

Hipóteses:
H1: [Hipótese principal com base teórica]
H2: [Hipótese secundária]
H3: [Hipótese sobre heterogeneidade]

Contexto Brasileiro:
- Regulamentação: [Detalhes regulatórios relevantes]
- Dados Específicos: [Particularidades dos dados brasileiros]
- Desafios Metodológicos: [Desafios específicos do contexto]

Expectativas de Resultados:
- Quais padrões você espera encontrar nos dados?
- Como os resultados podem informar políticas antitruste?
- Quais são as implicações para o mercado brasileiro?

Requisitos de Análise:
- Use R como linguagem primária para econometria
- Comente todo o código em inglês
- Forneça relatórios de análise em português brasileiro
- Inclua testes abrangentes de estacionaridade e não linearidade
- Considere métodos avançados como Transfer Entropy quando aplicável
- Valide todos os resultados com múltiplos testes de robustez
```

---

## PRÓXIMOS PASSOS

Com base nas suas respostas, eu vou criar um prompt de pesquisa detalhado que inclui:

1. **Estrutura Clara:** Organização lógica com todos os elementos necessários
2. **Contexto Brasileiro:** Consideração das particularidades do mercado brasileiro
3. **Metodologia Específica:** Detalhes dos métodos econométricos apropriados
4. **Código de Exemplo:** Exemplos de código R para implementação
5. **Orientações Práticas:** Instruções para execução da pesquisa

Pronto para criar seu prompt de pesquisa econométrica especializada! Por favor, forneça as informações solicitadas para que eu possa desenvolver um prompt completo e detalhado para sua análise de mercado brasileiro e detecção de cartéis.