# Desafio Final — Workshop de Dados 2026.2
## ClínicaCare — Sistema de Gestão e Análise de Dados

**Autor(a):** [Seu nome aqui]
**Squad Líder:** Gabriel Lucas de Araujo Bandeira
**Área:** Dados

---

## 📋 Sobre o projeto

Este repositório contém a solução completa do Desafio Final do Workshop de Dados 2026.2: um sistema integrado de gestão para a **ClínicaCare**, cobrindo modelagem de dados, SQL, Python/Machine Learning e Power BI, com foco especial na **previsão de no-shows** (pacientes que faltam às consultas agendadas).

## 🗂️ Estrutura do repositório

```
Desafio_Final_WKS_26.2/
├── 1_Modelagem/
│   ├── Modelo_Conceitual_ER.mermaid   # Diagrama ER (notação Mermaid)
│   └── Modelo_Logico.txt              # Modelo lógico descritivo (tabelas, tipos, PK/FK)
├── 2_SQL/
│   ├── clinica_care.sql               # Script completo: DDL + DML + DQL
│   └── Analise_Consultas.docx         # Análise das 5 agregações + 4 JOINs
├── 3_Python/
│   ├── analise_clinica.ipynb          # Notebook: ETL, EDA, visualizações e modelo de ML
│   └── dados_limpos.csv               # Dataset tratado usado no modelo e no Power BI
├── 4_Power_BI/
│   ├── Dashboard_ClinicaCare.pbix     # Dashboard interativo
│   ├── dados_limpos.csv               # Mesma fonte de dados do Módulo 3
│   └── Insights_Dashboard.docx        # Análise dos 5 painéis + recomendações
└── README.md
```

## 🎯 Módulo 1 — Modelagem de Dados

Modelo E-R com **8 entidades** (7 principais + 1 tabela associativa), resolvendo o relacionamento N:N entre Médicos e Especialidades através da tabela `medico_especialidade`.

**Entidades:** Pacientes, Médicos, Especialidades, Medico_Especialidade, Consultas, Prontuários, Prescrições, Pagamentos.

## 🗄️ Módulo 2 — SQL

Banco `clinica_care` implementado em MySQL:
- **DDL:** 8 tabelas com PK, FK, constraints (`NOT NULL`, `UNIQUE`, `CHECK`) e regras de integridade referencial (`ON DELETE RESTRICT`/`CASCADE`)
- **DML:** 131 registros distribuídos entre as 8 tabelas (12-20 por tabela) + 4 operações `UPDATE`
- **DQL:** 5 consultas de agregação (`AVG`, `SUM`, `COUNT`, `MAX`/`MIN`) e 4 operações de `JOIN` (`INNER`, `LEFT`, `RIGHT`), documentadas com insights em `Analise_Consultas.docx`

**Principais achados:** taxa combinada de cancelamento + no-show de ~20% no recorte real do sistema; Neurologia e Cardiologia com os maiores tickets médios de consulta.

## 🐍 Módulo 3 — Python + Machine Learning

**Tema escolhido:** Previsão de No-Shows

- **Extração:** consultas exportadas do banco `clinica_care` (14 registros reais) + histórico simulado (188 registros), representando os anos de atendimento em papel anteriores à digitalização — documentado explicitamente no notebook como premissa metodológica
- **Transformação:** engenharia de atributos (`dias_antecedencia`, `historico_faltas_paciente`, `idade_paciente`, `dia_semana`, codificação de `especialidade`/`tipo_plano`)
- **Visualizações:** taxa de no-show por especialidade, proporção realizada vs. faltou, evolução temporal
- **Modelo:** classificação (Regressão Logística) para prever risco de no-show, com avaliação por acurácia, matriz de confusão e interpretação de coeficientes

**Principal achado:** o histórico de faltas do próprio paciente é o preditor mais forte — pacientes com 1 falta anterior têm taxa de no-show ~47% maior que pacientes sem faltas registradas.

## 📊 Módulo 4 — Power BI

Dashboard `Dashboard_ClinicaCare.pbix` com:
- 3 cartões de KPI (Total de Pacientes, Taxa de No-Show, Faturamento Total)
- Gráfico de barras (Consultas por Especialidade)
- Gráfico de colunas (Faturamento por Mês)
- Gráfico de pizza (Pacientes por Tipo de Plano)
- Gráfico de linha (Evolução da Taxa de No-Show)
- Tabela com formatação condicional (escala verde→vermelho) destacando risco de no-show por especialidade
- 3 filtros interativos (especialidade, tipo de plano, fonte dos dados)

**Principal achado:** Endocrinologia, Psiquiatria e Pediatria concentram as maiores taxas de no-show (45-50%), sendo as especialidades prioritárias para ações de confirmação de consulta.

## 🛠️ Tecnologias utilizadas

- **Modelagem:** Mermaid (diagrama E-R)
- **Banco de dados:** MySQL
- **Análise de dados:** Python (pandas, numpy, matplotlib, scikit-learn)
- **Business Intelligence:** Power BI Desktop

## ⚠️ Nota metodológica

Parte da base de dados usada nos Módulos 3 e 4 é **histórico simulado**, criado para viabilizar treino e avaliação estatística do modelo de Machine Learning (o banco real do Módulo 2 possui apenas 20 consultas, insuficiente para esse fim). Essa premissa está documentada em todos os arquivos onde se aplica, e o dashboard permite isolar apenas os dados reais através do filtro "fonte".
