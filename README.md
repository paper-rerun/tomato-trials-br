# Tomato management trials in Brazil

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23649648.svg)](https://doi.org/10.5281/zenodo.23649648)

2,068 trial results from 35 trials on fresh-market and processing tomato (*Solanum lycopersicum*) in Brazil, taken from the tables of 20 open-access Embrapa publications: pests (tomato leafminer *Tuta absoluta*, whitefly *Bemisia tabaci* and begomovirus, fruit borers, tomato russet mite, thrips), diseases (late blight and powdery mildew, bacterial spot, bacterial wilt, root-knot nematodes, Fusarium wilt), cultivars and hybrids, grafting, fertilization and irrigation. Each row is one number exactly as printed, with the page and table it came from, and points to the trial it belongs to.

Tomato is grown across Brazil, staked for the fresh market and on the ground for processing. Much of the trial evidence on its pests, diseases and management sits in research bulletins and journal articles, some of them scanned. This dataset makes 20 of them searchable and comparable, from Amazonas and Rondônia to Rio Grande do Sul.

Part of [Paper Rerun](https://paper-rerun.github.io), in its Open Tables line. The work was done by AI agents on a home computer and reviewed by a human before publication.

## What is in it

| | |
|---|---|
| Publications | 20 (Alice and Infoteca-e, Embrapa's open repositories), 1986 to 2021 |
| Trials | 35 (29 field, 3 protected cultivation, 2 greenhouse pots, 1 laboratory), in the Federal District, Goiás, Pernambuco, Bahia, Sergipe, São Paulo, Minas Gerais, Rondônia, Amazonas and Rio Grande do Sul |
| Records | 2,068 (yield, fruit grading and defects, disease scores, pest counts, control efficacy, fruit number and mass, plant traits, soil chemistry and temperature, economics and others) |

Files:
- `data/tomato_trials.csv`: the dataset, one record per printed value, with `doc_id` and `trial_id`. Columns are described in `method/schema.json`. Where a record takes a value from its table column or from the whole table (unit, statistic, target, assessment time), the value is filled in and `inherited` says where it came from.
- `data/tomato_trials_context.csv`: one row per trial (place, season, planting date, harvest period, market segment, growing system, cultivar, pest or disease pressure, application method and timing, spacing, design). Join on `doc_id` + `trial_id`; an empty `application_stages` or `n_applications` in a record means the trial's value applies.
- `data/sources.csv`: the 20 publications, with authors, record link and licence notice.
- `qa/decisions.csv`: every disagreement between the two independent transcriptions and how it was settled against the PDF (2,446 decisions, with page).
- `method/`: the schema, the comparison script and the merge script.

## How it was made

1. **Collection.** An agent searched Embrapa's open repositories for publications with quantitative trial tables on this topic and downloaded 20, recording every candidate it did not take, with the reason.
2. **Two independent transcriptions.** Two agents per publication transcribed the trial context, the labels that apply to a whole table or to one column (which statistic, which test, what the letters mean, when the crop was assessed) and every table cell on the topic into a fixed schema, without seeing each other's work, copying numbers exactly as printed and accounting for every data cell.
3. **Comparison and adjudication.** A script paired the two transcriptions, trial by trial, table by table and record by record, and listed every disagreement and every value present in only one of them. Each one was settled by checking the PDF, with the page as evidence.

## How accurate it is

- **Numbers:** no number, significance letter or dispersion value was misread in either transcription. All 2,055 paired records matched as printed.
- **Context and labels:** one transcription alone was fully correct on 94.6% and 92.3% of records. Most errors came from reading the structure of a table: a treatment factor transcribed as separate columns, an accession code put in the cultivar field, a whole cell of doses copied to a spray strategy that used only one of them, or an inoculation recorded as a spray. All 2,446 decisions were settled against the PDF, so the dataset does not depend on either transcription alone.
- **Known limits:** values shown only in charts without printed numbers were not read. The 18 records taken from a figure are the labelled bars in souza2010, where the number is printed on each bar; both transcriptions read the same values. haji1986 is a scan whose table page has no usable text layer, and charchar1998 has a noisy one; both tables were read from the rendered page. One document, utumi1999 (cultivars under protected cultivation in Rondônia), holds 442 records, 21% of the dataset. The disease progress curve coefficients in lage2011 are regression parameters, not trial results, and were left out.

## Licence and citation

- The dataset (the CSV files, schema and scripts) is released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Please cite the original publications listed in `data/sources.csv` as well as this dataset.
- The publications are not redistributed. Embrapa's repositories show a CC BY-NC-ND 4.0 notice; journal articles and the one symposium paper keep their own terms, and four Embrapa bulletins (michereff2016, quezado2021, berni2019 and lage2011) print "Todos os direitos reservados" (all rights reserved). Only numbers, which are facts, and short citations are used here.

Errors or missing trials: please [open an issue](https://github.com/paper-rerun/tomato-trials-br/issues).

## How this dataset was made

This dataset was built by AI agents (Claude, by Anthropic) on a home computer and reviewed by a human before publication. No value was estimated or typed by hand: each one was transcribed from the source PDF twice, independently, and every disagreement was checked against the page. The method and scripts are in `method/`. How the wider project works: https://github.com/paper-rerun/paper-rerun/blob/main/METHOD.md

Contact: Filipe Tessaro, via issues on this repository.
