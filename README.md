```markdown
# 🏙️ Smart City IoT ETL Pipeline | Pentaho Data Integration (PDI)

[![Pentaho Kettle](https://img.shields.io/badge/ETL-Pentaho%20Kettle-blue)](https://www.hitachivantara.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Repo Status](https://img.shields.io/badge/Status-Completed-brightgreen)]()

Pipeline de ETL (Extract, Transform, Load) automatizado e resiliente desenvolvido no **Pentaho Data Integration (Spoon)** para integração de dados heterogéneos de sensores IoT urbanos (Qualidade do Ar e Tráfego).

Este projeto foi desenvolvido no âmbito da unidade curricular de **Integração de Sistemas de Informação (ISI)** da Licenciatura em Engenharia de Sistemas Informáticos no **Instituto Politécnico do Cávado e do Ave (IPCA)**.

---

## 🎯 Objetivos do Projeto

- **Ingestão Multiformato:** Ingerir e harmonizar dados provenientes de origens heterogéneas (CSV, JSON e XML).
- **Validação & Limpeza:** Garantir a qualidade dos dados através de sanitização de schemas e validação de padrões com **Expressões Regulares (Regex)**.
- **Transformação & Análise:** Executar cruzamentos de fluxos (*Merge Joins*, *Stream Lookups*) e agregações estatísticas em memória.
- **Orquestração Automatizada:** Controlar o ciclo de vida do fluxo através de um **Job** com verificação de pré-requisitos, tratamento gracioso de exceções e alertas.

---

## 📐 Arquitetura da Solução


```

[ manutencoes.csv ] ────┐
[ sensores_meta.json ] ─┼──> [ 01_extracao_dados.ktr ] ──> [ sensores_processados.json ]
[ limites_alertas.xml ] ┘       (Transformações & Regex)   ──> [ resumo_estatistico.json ]
▲
│ (Orquestração)
[ 00_pipeline_main.kjb ]

```

### 1. Extração ("E")
- **`manutencoes.csv`**: Histórico operacional de manutenções e revisões técnicas.
- **`sensores_meta.json`**: Metadados de localização e data de instalação das estações.
- **`limites_alertas.xml`**: Regras de negócio com os limiares críticos de alerta por tipo de sensor.

### 2. Transformação ("T")
- **`Sort rows` + `Merge join`**: Unificação dos dados do CSV e JSON através do identificador `id_sensor`.
- **`Regex evaluation`**: Aplicação da expressão regular `^SNSR-\d{3}$` para validação e auditoria da nomenclatura dos sensores.
- **`Stream lookup`**: Cruzamento dinâmico com o ficheiro XML para associação do limite máximo de alerta.
- **`Select values`**: Higienização e remoção de colunas redundantes do schema.
- **`Group by`**: Agregação estatística para cálculo do total de sensores operacionais e média dos limites de alerta por tipo.

### 3. Carga e Exportação ("L")
- **`sensores_processados.json`**: Dataset consolidado, higienizado e validado.
- **`resumo_estatistico.json`**: Dataset estatístico sumarizado para suporte à decisão e dashboards.

### 4. Orquestração e Resiliência (Job)
- **`File exists`**: Validação da presença dos ficheiros de entrada antes do disparo da transformação.
- **`Write to log` + `Mail` + `Abort job`**: Circuito de contingência para registo de erros, envio de alerta ao administrador e encerramento seguro em caso de falha.

---

## 📂 Estrutura do Repositório

```bash
.
├── dados_entrada/
│   ├── limites_alertas.xml       # Regras de negócio XML
│   ├── manutencoes.csv           # Dados operacionais CSV
│   └── sensores_meta.json        # Metadados JSON
├── dados_saida/
│   ├── resumo_estatistico.json   # Dataset executivo gerado
│   └── sensores_processados.json # Dataset granular final
├── docs/
│   └── Relatorio_ISI_TP01.pdf    # Relatório académico formal
├── etl_kettle/
│   ├── 00_pipeline_main.kjb      # Job de orquestração
│   └── 01_extracao_dados.ktr     # Transformação principal
└── README.md

```

---

## 🚀 Como Executar o Projeto

### Pré-requisitos

* **Java Runtime Environment (JRE) 8** ou superior.
* **Pentaho Data Integration (PDI / Spoon)** v9.x ou v10.x.

### Passos:

1. Clona este repositório:
```bash
git clone [https://github.com/teu-usuario/teu-repositorio.git](https://github.com/teu-usuario/teu-repositorio.git)
cd teu-repositorio

```


2. Abre o **Pentaho Spoon**.
3. Navega até à pasta `etl_kettle/` e abre o Job `00_pipeline_main.kjb`.
4. Executa o Job clicando no botão **Play ▶**.
5. Verifica os resultados gerados na pasta `dados_saida/`.

---

## 👨‍💻 Autor

* **Lucas Lima Magalhães** - *Engenharia de Sistemas Informáticos (IPCA)*

---

*Trabalho Prático I desenvolvido para a Unidade Curricular de Integração de Sistemas de Informação (2026/2027).*

```

```
