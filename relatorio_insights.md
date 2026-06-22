# Relatório de Insights
## Impacto da Automação e IA no Emprego Formal no Brasil

**Projeto:** Analista de Dados — EBAC × Semantix  
**Data:** Junho de 2026  
**Fonte de dados:** CAGED 2020–2024 via Base dos Dados (BigQuery)  
**Ferramentas:** Python, pandas, scikit-learn, matplotlib, seaborn

---

## Resumo executivo

- Setores de **alto risco** (índice > 0,70): Serviços Financeiros (0,87), Indústria de Transformação (0,76) e Comércio Varejista (0,72)
- Correlação **negativa** confirmada entre risco de automação e salário médio — os empregos mais ameaçados são também os mais mal remunerados
- O saldo acumulado de empregos é **negativo** para os grupos de menor escolaridade e **positivo** para nível superior e pós-graduação
- **Tecnologia e Comunicação** e **Saúde** são os setores com saldo positivo consistente no período 2020–2024
- O clustering K-Means identificou **3 grupos** distintos de setores por perfil de vulnerabilidade

---

## Achado 1 — Polarização setorial acentuada

**O que os dados mostram:**  
Os dados reais do CAGED (2020–2024) mostram que Serviços Financeiros concentra o maior índice estimado de risco de automação (0,87) e apresenta saldo acumulado negativo no período. Em contraste, Tecnologia e Comunicação tem risco de apenas 0,14 e saldo acumulado positivo.

**Por que isso acontece:**  
A automação de processos bancários — caixas eletrônicos, aprovação de crédito por algoritmo, atendimento por chatbot — eliminou funções de back-office em escala. Em contraste, a demanda por desenvolvedores, analistas de dados e engenheiros de ML cresceu continuamente, sustentando o saldo positivo de Tecnologia.

**Relevância para o problema:**  
A polarização indica que a transformação tecnológica não elimina o trabalho — ela o reorganiza. O problema está na velocidade da mudança: mais rápida do que a capacidade espontânea de requalificação dos trabalhadores afetados.

**Recomendação:**  
Programas de requalificação (Senai, Pronatec, Sebrae Digital) devem priorizar os setores financeiro e industrial, especialmente nas regiões de maior concentração — São Paulo, Minas Gerais e Rio Grande do Sul.

---

## Achado 2 — Escolaridade determina o saldo de empregos

**O que os dados mostram:**  
A análise do saldo acumulado por nível de escolaridade revela uma relação direta e consistente: grupos com escolaridade mais baixa acumulam saldo negativo (mais demissões do que admissões), enquanto grupos com nível superior e pós-graduação acumulam saldo positivo.

**Por que isso acontece:**  
Ocupações que exigem menor escolaridade tendem a envolver tarefas rotineiras, repetitivas e facilmente codificáveis — o perfil mais suscetível à substituição por sistemas automatizados. Ocupações de alta escolaridade exigem raciocínio abstrato, criatividade e inteligência emocional, habilidades mais difíceis de automatizar.

**Relevância para o problema:**  
O Brasil tem aproximadamente 30% da força de trabalho formal com ensino fundamental incompleto ou completo — um contingente de dezenas de milhões de pessoas em posição de alta vulnerabilidade, com menor capacidade de adaptação espontânea.

**Recomendação:**  
Políticas de educação continuada e requalificação devem focar em trabalhadores adultos com escolaridade básica, com oferta de cursos técnicos de curta duração em áreas de baixo risco (saúde, educação, tecnologia de campo).

---

## Achado 3 — Correlação negativa entre risco e remuneração

**O que os dados mostram:**  
A análise de dispersão e a matriz de correlação (gráficos 5 e 6) revelam correlação negativa entre índice de risco de automação e salário médio por setor. Setores de alto risco têm remuneração abaixo da média, enquanto Tecnologia e Saúde — de baixo risco — concentram os maiores salários.

**Por que isso acontece:**  
Setores de alto risco geralmente concentram ocupações de menor qualificação, reduzindo o poder de barganha dos trabalhadores e comprimindo salários. O mercado já precifica, de forma implícita, a substituibilidade da função.

**Relevância para o problema:**  
Este achado agrava o quadro social: os trabalhadores mais ameaçados pela automação são os que menos têm recursos — financeiros e educacionais — para se adaptar. O impacto se concentra nos estratos mais vulneráveis da população, com risco de aprofundamento da desigualdade.

**Recomendação:**  
Políticas de renda mínima e seguro-desemprego devem ser dimensionadas considerando o risco de automação setorial de forma preditiva — antecipando picos de demanda em vez de apenas reagir a eles.

---

## Achado 4 — Clustering identifica três grupos de setores

A análise K-Means (k=3) com as variáveis risco médio, salário médio e saldo acumulado identificou três perfis distintos:

| Grupo | Setores | Característica | Ação recomendada |
|---|---|---|---|
| Alto risco / Baixo salário | Serviços Financeiros, Indústria de Transformação, Comércio Varejista | Maior perda de postos + menores salários | Intervenção imediata em requalificação |
| Risco médio | Construção Civil, Agronegócio, Serviços Administrativos | Saldo equilibrado, risco moderado | Monitoramento contínuo |
| Baixo risco / Alto salário | Tecnologia e Comunicação, Saúde, Educação | Crescimento de postos + maiores salários | Destino prioritário para requalificação |

O clustering confirma que os setores não estão distribuídos de forma contínua — há separação clara entre grupos, o que valida a segmentação como base para políticas diferenciadas.

---

## Limitações da análise

- O índice de risco de automação foi aplicado por setor (CNAE seção), não por ocupação (CBO). Uma análise por CBO seria mais granular e precisa.
- O período analisado (2020–2024) inclui os anos da pandemia de COVID-19, que gerou distorções atípicas nos saldos de 2020 e 2021.
- Os dados do CAGED cobrem apenas o emprego formal — o impacto sobre trabalhadores informais (~40% da força de trabalho) não está capturado.

---

## Conclusão geral

Os dados reais do CAGED confirmam o que a literatura econômica prevê: a automação e a IA estão reorganizando o mercado de trabalho formal brasileiro de forma estrutural. O impacto não é distribuído uniformemente — ele se concentra nos setores, escolaridades e perfis de trabalhadores que já partem de posição mais vulnerável.

A análise de dados permite **quantificar, mapear e priorizar** onde e como agir. Sem essa base empírica, políticas públicas e iniciativas empresariais de requalificação correm o risco de chegar tarde e no lugar errado.

O próximo passo natural é expandir a análise para o nível de ocupação (CBO) e município, aumentando a granularidade e permitindo recomendações ainda mais precisas para intervenções locais.

---

*Relatório gerado como parte do projeto de Analista de Dados — EBAC × Semantix.*
