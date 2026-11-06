# Relatório de qualidade dos dados — Entrega 5

**TPAE II (PSAE00320) · Turma 01 · 2026.2 · Equipe 03** — Thiago Fernandes Ferreira de Oliveira, Matheus Lima Fonseca, Matheus Guilherme da Silva Lima
**Projeto:** Planejamento de Exportação para PMEs na Paraíba (demandante: Corecon-PB)
**Processado em:** 06/11/2026 00:34 · pandas 3.0.6

## 1. Fontes
| base | arquivo | data_extracao_dt_arquivo | ano_inicial | ano_final | linhas_declaradas_detalhamento | linhas_lidas_resultado | tipo_consulta | fluxo | detalhes_(dimensoes) | valores_(medidas) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | H_EXPORTACAO_POR_MUNICIPIO_2021-01_2025-12_DT20261022.xlsx | 2026-10-22 | 2021 | 2025 | 436 | 436 | Por Município | Exportação | UF do Município / Município / Posição (SH4) | Valor FOB / Quilograma Líquido |
| 2 | H_EXPORTACAO_POR_MUNICIPIO_2026-01_2026-12_DT20261022.xlsx | 2026-10-22 | 2026 | 2026 | 154 | 154 | Por Município | Exportação | UF do Município / Município / País | Valor FOB / Quilograma Líquido |

Arquivos originais preservados em `dados/brutos/` (SHA-256 em `registro_fontes.csv`; verificados antes e depois do tratamento).

## 2. Produtos gerados (`dados/tratados/`)
| Arquivo | Grão | Linhas |
|---|---|---|
| exportacoes_municipio_produto_2021_2025.csv | município x SH4 x ano | 829 |
| exportacoes_municipio_pais_2026.csv | município x país (2026, possivelmente parcial) | 154 |
| exportacoes_municipio_consolidado.csv | município | 48 |
| exportacoes_municipio_ano.csv | município x ano | 180 |
| dicionario_dados.csv | variável | 64 |

## 3. Problemas encontrados, decisões e impacto
| Variável/base | Problema encontrado | Decisão aplicada | Impacto/validação |
|---|---|---|---|
| Código SH4 (base 1) | Tipo misto na planilha (412 células inteiras e 24 de texto). A leitura automática do pandas converte tudo para inteiro e remove o zero à esquerda (24 códigos afetados, ex.: '0807' viraria 807). | Leitura como texto (dtype=str) e padronização em 4 dígitos; criado capitulo_sh2; sh4 exportado como texto (README e dicionário orientam a leitura com dtype=str). | 0 códigos inválidos; 259 SH4 distintos; 0 códigos com descrição conflitante; chave (município, SH4) sem duplicatas. |
| Descrição SH4 (base 1) | 78 linhas (49 códigos SH4) com espaço não separável (U+00A0, ex.: 'posição 0105') nas descrições; 78 delas por esse motivo. Cada código tem uma única descrição. | Espaços normalizados para espaço simples em descricao_sh4; conteúdo textual mantido como na fonte (os originais permanecem intactos). | 0 códigos com descrições conflitantes; a chave (SH4) não é afetada. |
| Município (bases 1 e 2) | Nome e UF juntos na mesma coluna ('Campina Grande - PB'); planilhas sem código IBGE do município. | Separação em municipio_nome e uf_sigla; criação de municipio_chave (sem acento, minúsculo, espaços normalizados + UF) como identificador comum. Nenhum código foi inventado. | 0 nomes fora do padrão; 0 UF incoerentes; 0 nomes com espaços anômalos; 0 colisões de chave; 0 grafias suspeitas entre as bases. |
| Valores ausentes (ambas) | Nenhuma célula ausente na aba Resultado (nulos: base 1 = 0; base 2 = 0). | Nada a imputar. Ausências que surgem na consolidação (município sem registro em um período) permanecem vazias, nunca preenchidas com 0. | Teste: nulos remanescentes limitados a preço indefinido (kg=0) e municípios sem registro no período; todos explicados. |
| Duplicidades (ambas) | Linhas totalmente duplicadas: base 1 = 0, base 2 = 0. Chaves repetidas (município+SH4 / município+país): 0 / 0; após a padronização: 0 / 0. | Nenhuma exclusão necessária. | Teste de unicidade das chaves naturais aprovado em todas as tabelas. |
| Valor FOB e Quilograma Líquido (tipos) | Valores numéricos lidos como texto (dtype=str) para evitar coerção silenciosa. Falhas de conversão: 0; negativos: 0. | Conversão para int64 (valores inteiros em US$ e kg); nenhum valor alterado. | Totais por ano idênticos aos dos originais (teste 6.3). |
| Zeros no formato largo (base 1) | 1.351 de 2.180 células município x produto x ano têm FOB=0 e kg=0: no Comex Stat significam ausência de registro naquele ano, não 'exportação zero' nem dado faltante. | Não imputados. Omitidos ao passar para o formato longo (uma observação por linha); permanecem nos dados originais. | Formato longo: 829 observações (= células com registro). Soma das células omitidas = 0; totais anuais de FOB e kg idênticos ao original. |
| Linha totalmente zerada (base 1) | 1 linha(s) município x produto sem valor em nenhum ano: João Pessoa - PB / SH4 4822. | Mantida em limpo_A (e nos originais); sem observações no formato longo por não ter registro. | Nenhum valor de exportação perdido; o município segue presente por outros produtos. |
| FOB > 0 com kg = 0 | Valor positivo com peso zero: base 1 = 52 observações; base 2 = 3. O preço por kg é indefinido (pode refletir peso arredondado para zero ou mercadoria sem peso relevante). | Valores originais mantidos. preco_medio_usd_kg = vazio (NaN), nunca 0 nem infinito. | Teste: preço vazio exatamente onde kg=0 (52 obs. na base 1; 3 na base 2). |
| FOB = 0 com kg > 0 | Peso positivo com valor zero: base 1 = 1 observação(ões); base 2 = 0. | Mantido como na fonte (preço = 0). Participações tratam o valor 0 normalmente. | Não afeta totais de FOB; apenas peso. |
| Preço médio por kg (valores extremos) | Preços implícitos de US$ 0,0021 a US$ 10.593 por kg (base 1; maior: João Pessoa. SH4 6204. 2025: US$ 10.593 para 1 kg) e até US$ 203.333/kg na base 2 (Salgadinho x Hong Kong: US$ 610.000 para 3 kg). Os extremos vêm de pesos muito pequenos (1 a 3 kg) ou de mercadorias de valor unitário muito diferente; produtos distintos não são comparáveis em US$/kg. | Valores extremos diagnosticados e mantidos (sem winsorização ou exclusão, pois não há critério objetivo de erro). Preço de agregados = razão dos totais. Comparar preços apenas dentro do mesmo SH4; tratar pontos como o da base 2 com cautela antes de usá-los em análises. | Tabelas de maiores/menores preços na Etapa 5.5; totais de FOB e kg não são afetados. |
| Granularidade diferente entre as bases | Base 1 = município x produto; base 2 = município x país. Um merge direto por município geraria 7.009 linhas (vs. 393 + 154) e inflaria o FOB de 2025 em 23,3x, além de criar combinações produto x país inexistentes. | Junção direta rejeitada. Três tabelas (município x produto; município x país; consolidada por município) + dim_municipio. A consolidada resulta de agregar cada base ao município antes de juntar (merge um-para-um com validate=). | Totais antes e depois conferem (2021-2025 e 2026); nenhuma tabela contém produto e país juntos; chaves únicas. |
| Chaves sem correspondência entre as bases | 48 municípios na base 1; 25 na base 2; 25 em ambas; 23 somente na base 1; 0 somente na base 2. | Todos os municípios mantidos (outer join de chaves); coluna presenca_bases identifica a situação. Para o período sem registro os valores ficam vazios (ausência de registro não implica exportação zero). | União = 48 municípios = linhas do consolidado; nenhum município descartado; nenhuma chave órfã. |
| Período 2026 (base 2) | Consulta identificada como jan-dez/2026, mas extraída em 22/10/2026: não representa necessariamente o ano completo e as planilhas não informam os meses efetivamente cobertos. | Marcado com periodo_possivelmente_parcial=True. Nenhuma variação ou taxa de crescimento 2026 x 2025 é calculada; gráficos e tabelas trazem ressalva. | Limitação permanece: 2026 só pode ser comparado a anos completos com ressalva ou em base comparável (mesmos meses), que exige dados mensais. |
| UF e filtros (Detalhamento) | A aba Detalhamento mostra consulta 'Por Município', fluxo Exportação, filtro UF = 25 (Paraíba). | Parâmetros lidos e registrados em registro_fontes.csv (inclui SHA-256, data da extração e versão do pandas). | Testes: linhas declaradas = linhas lidas; anos conferem; todas as linhas com UF = PB. |
| País de destino (base 2) | Países sem código padronizado (ex.: 'Países Baixos (Holanda)', 'Taiwan (Formosa)'). Países distintos: 64; grafias conflitantes: 0. | Grafias mantidas como na fonte; apenas espaços normalizados. | Soma dos países = FOB total de 2026 (teste). |
| Unidades e valores monetários | Valores em US$ FOB correntes (nominais, sem deflator) e peso em quilograma líquido; as planilhas não informam outra unidade. | Sem conversão de unidade. Nomes das colunas explicitam a unidade (valor_fob_usd, kg_liquido). | Limitação: comparações entre anos refletem valores nominais, não poder de compra. |
| Limitações que permanecem | Sem informação de empresa, porte (PME) ou CNPJ; sem relação produto x país; atribuição ao município segue a metodologia do Comex Stat (em geral, domicílio fiscal do exportador), podendo diferir do local de produção; sem código IBGE. | Documentadas. Próximas entregas poderão ligar a chave municipal ao código IBGE (DTB) e às bases RAIS/CNPJ. | 57/57 testes de integridade aprovados. |

## 4. Validação
57 de 57 testes aprovados.

| grupo | testes | aprovados |
|---|---|---|
| 6.1 Detalhamento | 7 | 7 |
| 6.1 Preservação | 2 | 2 |
| 6.2 Contagens | 7 | 7 |
| 6.3 Totais | 11 | 11 |
| 6.4 Chaves | 8 | 8 |
| 6.5 Consistência | 1 | 1 |
| 6.5 Participações | 6 | 6 |
| 6.5 Valores | 7 | 7 |
| 7.1 Dicionário | 4 | 4 |
| 7.2 Exportação | 4 | 4 |

## 5. Totais de referência (US$ FOB nominais)
- 2021–2025 (soma, anos completos): US$ 1.021.021.321
- 2026 (período possivelmente parcial, extração em 22/10/2026): US$ 88.177.849 — **não comparável diretamente a anos completos**.
