# Planejamento de Exportação para PMEs na Paraíba

**Disciplina:** PSAE00320 – Técnicas de Pesquisa e Análise Econômica II (TPAE II) | Turma 01 | 2026.2
**Demandante:** Conselho Regional de Economia da Paraíba (Corecon-PB)
**Entrega Atual:** 4

---

## Sobre o Projeto

Este repositório contém os dados, códigos e relatórios do projeto focado na inserção de Pequenas e Médias Empresas (PMEs) paraibanas no mercado externo. A economia da Paraíba apresenta atualmente um paradoxo: possui uma sólida gestão fiscal e nota de crédito máxima, mas ainda lida com desafios estruturais de produtividade e baixo desenvolvimento inclusivo. Este projeto visa fornecer um panorama empírico e um roteiro prático para auxiliar o Corecon-PB a direcionar ações de capacitação, mitigando obstáculos operacionais, informacionais e de custos fixos que limitam a diversificação e a exportação de bens de maior valor agregado no estado.

## Equipe (Nº 03)

* **Thiago Fernandes Ferreira de Oliveira** (Contato: tffo@academico.ufpb.br)
* **Matheus Lima Fonseca**
* **Matheus Guilherme da Silva Lima**

## Problema e Pergunta de Pesquisa

**Problema Central:** A inserção de PMEs paraibanas no mercado externo esbarra na ausência de um panorama empírico e sistematizado sobre a atual atividade exportadora da Paraíba e sobre os entraves operacionais documentados. Essa lacuna inviabiliza o direcionamento eficiente de políticas e ações institucionais de capacitação por parte do Corecon-PB.

**Pergunta de Pesquisa:** Quais são as características da atividade exportadora da Paraíba e de suas PMEs com potencial de comercialização externa, e quais barreiras operacionais e etapas documentadas devem compor o roteiro de preparação desses empresários?

## Objetivos

Descrever, com base em dados públicos e fontes documentais, a atividade exportadora da Paraíba e as condições de acesso das PMEs, produzindo um roteiro de apoio prático à preparação de empresários. O projeto adota uma abordagem descritiva e exploratória, sem pretensões de inferência causal.

O roteiro documental abrangerá conceitos fundamentais do comércio exterior exigidos para o planejamento logístico, como:

* **Documentação Básica:** Fatura Proforma, Fatura Comercial, Romaneio de Embarque (Packing List), e Conhecimento de Embarque (como B/L e AWB).


* **Logística e Seguros:** Estruturação de fretes internacionais (marítimo, aéreo, rodoviário) e coberturas de seguro internacional.


* **Termos de Comércio (Incoterms 2020):** Definição de responsabilidades de custos e riscos (como EXW, FOB, CIF, e DPU) essenciais para a formatação de preços competitivos.



## Metodologia e Fontes de Dados

O tratamento de dados estatísticos e a modelagem exploratória estão a ser conduzidos em ambientes **Google Colab**, utilizando **Python** para processamento analítico na pasta de códigos.

As bases de dados e fontes documentais utilizadas incluem:

* **Comex Stat (MDIC):** Exportações por município, produto e destino. Arquivos de histórico municipal já anexados à base (ex: `H_EXPORTACAO_POR_MUNICIPIO_2021-01_2025-12_DT20261022.xlsx` e `H_EXPORTACAO_POR_MUNICIPIO_2026-01_2026-12_DT20261022.xlsx`).
* **RAIS:** Dados sobre estabelecimentos, vínculos empregatícios e remuneração.
* **Cadastro de CNPJ (Receita Federal):** Dados abertos para identificação de porte e setor das empresas.
* **IBGE:** Indicadores de população, PIB (contas regionais) e cadastro de empresas.
* **Documentação Institucional:** Manuais do Sebrae, Apex-Brasil, MDIC e Portal Único Siscomex.

## Estrutura do Repositório

```text
.
├── README.md            # Visão geral do projeto e documentação principal
├── dados/               
│   ├── brutos/          # Bases originais sem tratamento (ex: planilhas Comex Stat)
│   └── tratados/        # Bases limpas e processadas prontas para análise
├── codigo/              # Notebooks (Google Colab/Jupyter) e scripts Python de processamento e visualização
├── relatorio/           # Análises, gráficos gerados e roteiro de apoio à exportação
└── docs/                # Arquivos de fundamentação (E03_equipe03_plano.pdf, manuais MRE, Incoterms, Corecon-PB)

```

## Situação Atual

**Fase:** Plano de pesquisa desenvolvido e em execução a partir da coleta de dados para aplicação (Entrega 4).

**Status dos Dados:** Coleta inicial realizada (histórico do Comex Stat de 2021 a 2026 carregado). O tratamento de dados em Python encontra-se em estágio inicial de estruturação das rotinas de limpeza e cruzamento com as tabelas de CNPJ e RAIS.
