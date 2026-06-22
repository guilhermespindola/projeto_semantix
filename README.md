# Impacto da Automação e IA no Emprego Formal no Brasil

**Projeto de Analista de Dados — EBAC × Semantix**  
**Autor:** Guilherme Spindola Bernardo da Silva  
**Data:** Junho de 2026

---

## Sobre o projeto

Este projeto analisa o impacto da automação e da Inteligência Artificial no mercado de trabalho formal brasileiro, identificando quais setores e perfis de trabalhadores estão mais expostos ao risco de substituição tecnológica.

A análise utiliza dados reais do CAGED (Cadastro Geral de Empregados e Desempregados) coletados via Base dos Dados e BigQuery, combinados com índices de automabilidade por setor, produzindo insights acionáveis para políticas públicas e estratégias de requalificação profissional.

---

## Estrutura do repositório

```
projeto-semantix/
├── README.md                        ← este arquivo
├── relatorio_insights.md            ← relatório de insights (Etapa 4)
├── notebooks/
│   └── 03_analise_eda.ipynb         ← análise exploratória completa
├── dados/
│   ├── caged_agregado.csv           ← dados reais do CAGED (BigQuery)
│   ├── looker_resumo_setor.csv      ← exportado pelo notebook
│   ├── looker_evolucao_temporal.csv ← exportado pelo notebook
│   ├── looker_escolaridade.csv      ← exportado pelo notebook
│   └── looker_por_uf.csv            ← exportado pelo notebook
└── outputs/
    ├── grafico1_risco_por_setor.png
    ├── grafico2_saldo_por_setor.png
    ├── grafico3_evolucao_temporal.png
    ├── grafico4_saldo_escolaridade.png
    ├── grafico5_dispersao_risco_salario.png
    ├── grafico6_correlacao.png
    └── grafico7_clusters.png
```

---

## O problema

A automação de processos industriais e cognitivos, acelerada pelo avanço da IA e da robótica, está transformando a estrutura do mercado de trabalho formal no Brasil. Estudos do IPEA e da FGV estimam que entre 50% e 54% dos empregos formais possuem alto grau de suscetibilidade à automação.

O impacto é desigual: concentra-se nos setores de maior rotinização de tarefas (serviços financeiros, indústria de transformação, comércio varejista) e nos trabalhadores com menor escolaridade — justamente os que têm menos recursos para se adaptar.

**Como dados ajudam:**  
Com os microdados do CAGED é possível mapear quais setores já perdem postos, identificar perfis de risco e detectar tendências reais — fornecendo base empírica para decisões de política pública e estratégias empresariais de requalificação.

---

## Coleta de dados

### Fonte utilizada

| Fonte | Órgão | Tipo | Período | Acesso | CAGED (Novo CAGED) | Ministério do Trabalho | Agregado via BigQuery | 2020–2024 | Base dos Dados |

### Como reproduzir a coleta

Os dados foram coletados via [Base dos Dados](https://basedosdados.org) usando a tabela pública `basedosdados.br_me_caged.microdados_movimentacao` no BigQuery. A query abaixo reproduz exatamente a coleta:

```sql
SELECT
    ano,
    sigla_uf,
    cnae_2_secao,
    grau_instrucao,
    sexo,
    SUM(saldo_movimentacao)  AS saldo_total,
    AVG(salario_mensal)      AS salario_medio,
    COUNT(*)                 AS total_movimentacoes
FROM `basedosdados.br_me_caged.microdados_movimentacao`
WHERE ano BETWEEN 2020 AND 2024
  AND salario_mensal > 0
GROUP BY ano, sigla_uf, cnae_2_secao, grau_instrucao, sexo
```

**Pré-requisitos para rodar:**
1. Conta gratuita no [Google Cloud](https://console.cloud.google.com) com BigQuery ativado
2. Pacote Python: `pip install basedosdados`
3. Autenticação via `bd.read_sql(query=query, billing_project_id="SEU_PROJETO_GCP")`

### Variáveis utilizadas

| Variável original | Renomeada para | Descrição |
|---|---|---|
| `sigla_uf` | `uf` | Unidade federativa |
| `cnae_2_secao` | `setor_codigo` | Código de seção CNAE (A–U) |
| `grau_instrucao` | `escolaridade` | Nível de escolaridade (código 1–9) |
| `saldo_total` | `saldo` | Admissões menos demissões |
| `salario_medio` | `salario` | Média salarial agregada |
| `total_movimentacoes` | `total_mov` | Total de registros no grupo |

---

## Modelagem

### Ferramentas

- **Python 3.11** com pandas, numpy, matplotlib, seaborn, scikit-learn
- **Google Colab** para execução do notebook
- **Base dos Dados + BigQuery** para coleta
- **Looker Studio** para visualização final

### Etapas da análise

**1. Limpeza e pré-processamento**
- Renomeação das colunas para nomes legíveis
- Mapeamento de códigos CNAE (A–U) para nomes de setores
- Mapeamento de códigos de escolaridade (1–9) para descrições
- Adição do índice de risco de automação por setor (Frey & Osborne adaptado)
- Remoção de registros com salário abaixo do mínimo vigente (R$ 1.320)
- Criação de variável `faixa_risco` (Baixo / Médio / Alto)

**2. Análise descritiva**
- Risco médio de automação por setor (gráfico 1)
- Saldo acumulado de empregos formais por setor 2020–2024 (gráfico 2)
- Evolução temporal do saldo por setor selecionado (gráfico 3)
- Saldo acumulado por nível de escolaridade (gráfico 4)

**3. Análise correlacional**
- Dispersão: risco de automação × salário médio por setor (gráfico 5)
- Matriz de correlação: risco × salário × saldo × total de movimentações (gráfico 6)

**4. Segmentação — K-Means (k=3)**
- Agrupamento de setores por: risco médio, salário médio e saldo acumulado
- Padronização com StandardScaler antes do clustering
- Resultado: 3 grupos — alto risco/baixo salário, risco médio, baixo risco/alto salário (gráfico 7)

## Conclusões

**1. Polarização setorial acentuada**  
Serviços Financeiros (risco 0,87), Indústria de Transformação (0,76) e Comércio Varejista (0,72) concentram as maiores perdas de emprego formal no período 2020–2024. Tecnologia e Comunicação e Saúde apresentam saldo positivo e baixo risco de automação.

**2. Escolaridade é o principal fator de proteção**  
O saldo acumulado de empregos é negativo para os grupos de menor escolaridade e positivo para nível superior e pós-graduação. Trabalhadores com ensino fundamental incompleto têm risco estimado de 0,80 — quatro vezes maior que os com nível superior (0,20).

**3. Correlação negativa entre risco e remuneração**  
Os empregos mais ameaçados são também os mais mal remunerados, concentrando o impacto nos estratos socioeconômicos mais vulneráveis e aprofundando a desigualdade.

**4. Três perfis de setor identificados pelo clustering**  
O K-Means separou claramente os setores em: (a) alto risco e baixo salário — maior urgência de intervenção; (b) risco médio — monitoramento contínuo; (c) baixo risco e alto salário — setores de destino para requalificação.

**Recomendações principais:**
- Priorizar requalificação nos setores financeiro e industrial, nas regiões SP, MG e RS
- Ampliar acesso a cursos técnicos de curta duração para trabalhadores com escolaridade básica
- Dimensionar políticas de seguro-desemprego considerando o risco de automação setorial de forma preditiva, não reativa

---

## Dashboard

Visualizações interativas disponíveis no Looker Studio:  
🔗 [(https://datastudio.google.com/reporting/aa180135-4752-4d59-ad6d-1b19ee5d945f)]

**Gráficos incluídos no dashboard:**
- Scorecard: total de movimentações e saldo geral
- Risco de automação por setor (barras horizontais)
- Variação acumulada de empregos por setor (barras)
- Evolução temporal do saldo por setor (série temporal)
- Saldo por nível de escolaridade (barras)
- Dispersão risco × salário por setor (scatter)
- Mapa de calor por UF e setor

---

## Referências

- Frey, C. B., & Osborne, M. A. (2013). *The Future of Employment*. University of Oxford.
- IPEA (2020). *Automação e Mercado de Trabalho no Brasil*. Brasília.
- McKinsey Global Institute (2017). *A Future that Works: Automation, Employment, and Productivity*.
- Base dos Dados. CAGED — Microdados de Movimentação. https://basedosdados.org
- Ministério do Trabalho e Emprego. Novo CAGED. https://www.gov.br/trabalho-e-emprego

---