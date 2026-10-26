# Ensaios de manejo do tomateiro no Brasil

English version: [README.md](README.md)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23649648.svg)](https://doi.org/10.5281/zenodo.23649648)

2.068 resultados de 35 ensaios com tomate de mesa e para processamento industrial (*Solanum lycopersicum*) no Brasil, tirados das tabelas de 20 publicações de acesso aberto da Embrapa: pragas (traça-do-tomateiro *Tuta absoluta*, mosca-branca *Bemisia tabaci* e begomovírus, brocas-dos-frutos, ácaro-do-bronzeamento, tripes), doenças (requeima e oídio, mancha-bacteriana, murcha-bacteriana, nematoides-das-galhas, murcha-de-fusário), cultivares e híbridos, enxertia, adubação e irrigação. Cada linha é um número exatamente como foi impresso, com a página e a tabela de onde saiu, e aponta para o ensaio a que pertence.

O tomate é cultivado em todo o Brasil, tutorado para o mercado de mesa e rasteiro para a indústria. Boa parte dos dados de ensaios sobre suas pragas, doenças e manejo está em boletins de pesquisa e artigos de periódicos, alguns deles escaneados. Este dataset torna 20 deles pesquisáveis e comparáveis, do Amazonas e de Rondônia ao Rio Grande do Sul.

Faz parte do [Paper Rerun](https://paper-rerun.github.io), na linha Open Tables. O trabalho foi feito por agentes de IA num computador doméstico e revisado por uma pessoa antes da publicação.

## O que tem

| | |
|---|---|
| Publicações | 20 (Alice e Infoteca-e, os repositórios abertos da Embrapa), 1986 a 2021 |
| Ensaios | 35 (29 de campo, 3 em cultivo protegido, 2 em vasos em casa de vegetação, 1 em laboratório), no Distrito Federal, Goiás, Pernambuco, Bahia, Sergipe, São Paulo, Minas Gerais, Rondônia, Amazonas e Rio Grande do Sul |
| Registros | 2.068 (produtividade, classificação e defeitos de frutos, notas de doenças, contagens de pragas, eficiência de controle, número e massa de frutos, características da planta, química e temperatura do solo, dados econômicos e outros) |

Arquivos:
- `data/tomato_trials.csv`: o dataset, um registro por valor impresso, com `doc_id` e `trial_id`. As colunas estão descritas em `method/schema.json`. Quando um registro herda um valor da sua coluna na tabela ou da tabela inteira (unidade, estatística, alvo, momento da avaliação), o valor vem preenchido e `inherited` diz de onde ele veio.
- `data/tomato_trials_context.csv`: uma linha por ensaio (local, safra, data de plantio, período de colheita, segmento de mercado, sistema de cultivo, cultivar, pressão de pragas ou doenças, método e momento de aplicação, espaçamento, delineamento). Junte os dois arquivos por `doc_id` + `trial_id`; `application_stages` ou `n_applications` vazios num registro significam que vale o valor do ensaio.
- `data/sources.csv`: as 20 publicações, com autores, link do registro e aviso de licença.
- `qa/decisions.csv`: cada divergência entre as duas transcrições independentes e como ela foi resolvida conferindo o PDF (2.446 decisões, com a página).
- `method/`: o schema, o script de comparação e o script de junção.

### Principais colunas

O arquivo está em UTF-8, com vírgula separando as colunas. Local, safra, segmento de mercado, sistema de cultivo, cultivar e delineamento de cada ensaio estão em `data/tomato_trials_context.csv`.

- `doc_id`: identificador da publicação de origem. Liga o registro a `data/sources.csv` e é também o nome do PDF, sem a extensão.
- `trial_id`: o ensaio a que o valor pertence, dentro da publicação. Com `doc_id`, liga o registro à sua linha no arquivo de contexto.
- `page`: página do PDF contada a partir de 1 (a primeira página do arquivo é 1), e não o número impresso na página. O rótulo da tabela está em `table_or_figure`, como impresso ("Tabela 3", "Quadro 1", "Figura 2"), ou `text` quando o número está no corpo do texto.
- `treatment`: rótulo da linha exatamente como impresso (cultivar, código, produto, dose, "Testemunha", "Média"). Numa célula de fatorial, os níveis vêm unidos por " x ", na ordem da tabela.
- `biotic_target`: a praga, doença ou distúrbio a que a variável se refere, como `bacterial_wilt` (murcha-bacteriana), `bacterial_spot` (mancha-bacteriana), `whitefly` (mosca-branca), `begomovirus`, `thrips` (tripes), `tuta_absoluta` (traça-do-tomateiro), `fusarium_wilt` (murcha-de-fusário), `root_knot_nematode` (nematoide-das-galhas), `late_blight` (requeima) ou `physiological_disorder` (distúrbio fisiológico). É `multiple` quando um valor cobre vários.
- `variable`: variável medida, como impressa no cabeçalho da coluna ou na legenda. Quando o cabeçalho tem mais de um nível, os níveis vêm separados por " / " ("Ca / (cmolc dm-3)").
- `variable_standardized`: a mesma variável numa lista fechada. Alguns exemplos: `marketable_yield_area` (produção comercial por área), `total_yield_area` (produção total por área), `nonmarketable_yield` (refugo, frutos não comerciais), `yield_unspecified` (produtividade quando o documento não diz se é comercial ou total), `fruit_class_share_pct` (% dos frutos ou da produção numa classe de tamanho ou qualidade), `disease_score` (nota de doença), `control_efficacy_pct` (% de controle impresso pelos autores) e `soil_chemistry` (análise de solo ao fim do ensaio). A lista completa está em `method/schema.json`.
- `value_as_printed`: a célula exatamente como impressa, sem letras, asteriscos, marcas de rodapé ou termo de dispersão ("63.750", "33,3", "MR"). Quando `value_kind` é `class` ou `illegible`, `value` fica vazio.
- `value`: o mesmo número pronto para cálculo, com ponto decimal e sem separador de milhar ("63.750" vira 63750, "33,3" vira 33.3). Nada foi calculado, convertido ou arredondado.
- `unit`: unidade como impressa ou informada na legenda ou no cabeçalho ("t/ha", "kg/ha", "%", "cmolc dm-3"). Vazia quando não há unidade impressa, como em notas e contagens.
- `statistic`: o que o número é: `mean` (média), `relative_to_check` (calculado pelos autores em relação à testemunha), `total`, `median`, `single_value` (uma parcela ou uma observação), `percentage_of_total` ou `other`.
- `significance_letter`: letra(s) do teste de médias exatamente como impressas ("a", "ab", "a A"). Vazia quando não há. O teste e o significado das letras estão em `significance_test` e `letters_meaning`.

## Como foi feito

1. **Coleta.** Um agente buscou nos repositórios abertos da Embrapa publicações com tabelas de ensaios quantitativos sobre o tema e baixou 20, registrando cada candidata que não entrou, com o motivo.
2. **Duas transcrições independentes.** Dois agentes por publicação transcreveram o contexto dos ensaios, os rótulos que valem para uma tabela inteira ou para uma coluna (qual estatística, qual teste, o que as letras significam, quando a cultura foi avaliada) e cada célula de tabela sobre o tema num schema fixo, sem ver o trabalho um do outro, copiando os números exatamente como impressos e prestando contas de cada célula de dados.
3. **Comparação e arbitragem.** Um script pareou as duas transcrições, ensaio por ensaio, tabela por tabela e registro por registro, e listou cada divergência e cada valor presente em só uma delas. Cada caso foi resolvido conferindo o PDF, com a página como evidência.

## Qual a precisão

- **Números:** nenhum número, letra de significância ou valor de dispersão foi lido errado em nenhuma das duas transcrições. Os 2.055 registros pareados bateram com o impresso.
- **Contexto e rótulos:** sozinha, uma transcrição estava totalmente correta em 94,6% dos registros, e a outra em 92,3%. A maioria dos erros veio da leitura da estrutura da tabela: um fator de tratamento transcrito como colunas separadas, um código de acesso posto no campo de cultivar, uma célula inteira de doses copiada para uma estratégia de pulverização que usou só uma delas, ou uma inoculação registrada como pulverização. As 2.446 decisões foram resolvidas conferindo o PDF, então o dataset não depende de nenhuma das duas transcrições sozinha.
- **Limites conhecidos:** valores mostrados só em gráficos, sem número impresso, não foram lidos. Os 18 registros tirados de uma figura são as barras rotuladas de souza2010, em que o número vem impresso em cada barra; as duas transcrições leram os mesmos valores. haji1986 é um documento escaneado cuja página da tabela não tem camada de texto aproveitável, e a de charchar1998 tem muito ruído; as duas tabelas foram lidas a partir da imagem da página. Um documento, utumi1999 (cultivares em cultivo protegido em Rondônia), tem 442 registros, 21% do dataset. Os coeficientes das curvas de progresso da doença em lage2011 são parâmetros de regressão, não resultados de ensaio, e ficaram de fora.

## Licença e citação

- O dataset (os arquivos CSV, o schema e os scripts) é publicado sob a licença [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Por favor, cite as publicações originais listadas em `data/sources.csv` e também este dataset.
- As publicações não são redistribuídas. Os repositórios da Embrapa exibem um aviso CC BY-NC-ND 4.0; os artigos de periódico e o único trabalho de simpósio mantêm os próprios termos, e quatro boletins da Embrapa (michereff2016, quezado2021, berni2019 e lage2011) trazem "Todos os direitos reservados". Aqui são usados só os números, que são fatos, e citações curtas.

Erros ou ensaios que faltam: por favor, [abra uma issue](https://github.com/paper-rerun/tomato-trials-br/issues).

## Como este dataset foi feito

Este dataset foi montado por agentes de IA (Claude, da Anthropic) num computador doméstico e revisado por uma pessoa antes da publicação. Nenhum valor foi estimado ou digitado à mão: cada um foi transcrito do PDF original duas vezes, de forma independente, e cada divergência foi conferida na página. O método e os scripts estão em `method/`. Como funciona o projeto como um todo (em inglês): https://github.com/paper-rerun/paper-rerun/blob/main/METHOD.md

Contato: Filipe Tessaro, pelas issues deste repositório.
