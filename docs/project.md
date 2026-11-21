# Project plan: Radar innowacji

This file has the concept, the architecture and the data sources of the project. It follows the FAQ (`task/Radar Innowacji FAQ.pdf`). The research is in `docs/reports/Innovation radar landscape.md`.

Status: draft v0.1, 2026-11-21. The team must accept or change the decisions in the section "Open decisions".

## 1. Concept

Radar innowacji is one place to discover Polish innovations that already exist. It shows their real level of development and where they can be used.

The application does not collect submissions. It finds innovations in open sources, makes an innovation card for each one, and scores it with clear criteria. A user searches, filters and compares the cards.

Two results of the project (FAQ, section 8):

1. A user finds an existing solution for a need in less time.
2. More innovations that use Polish intellectual property (IP) are implemented.

### Originality

Other catalogues need manual entry, and each covers one sector. The research found no tool that does all of these items for Poland:

1. Automatic discovery from many Polish sources in one index (science, EU projects, administration, education).
2. A card that separates confirmed facts from estimates, with a source for each fact.
3. A transparent score of the implementation level and the potential, the same for all three areas.
4. A demand signal: the card shows public institutions that bought similar solutions (BZP notices). This shows the application potential with data, not with an opinion.
5. A duplicate check: "Czy to już istnieje?" A user describes a need in free text and gets similar existing solutions.

## 2. Scope

### Areas (FAQ, section 2)

| Area | Examples | Main sources |
|---|---|---|
| Technological | inventions, products, technical solutions | RAD-on (patents and projects), UPRP list, EPO linked data, NCBR, FE projects |
| Educational | methods of teaching, tools for schools and universities | FE projects, Erasmus+ results, RAD-on projects |
| Administrative | solutions for public administration and JST | FE projects, "Innowacyjny Samorząd", ZMP good practices, Kohesio |

### Definition of an innovation

The definition comes from the Oslo Manual 2018. An innovation is a new or improved solution that differs significantly from earlier solutions. It is available to users or implemented.

A record that is only an idea gets the implementation level "koncepcja". The application keeps it, but with a low score. Thus the user sees the difference between an implemented solution and a plan.

### Out of scope (FAQ, section 6)

- Submission of new ideas.
- A social portal for innovators.
- Crowdfunding and funding.
- A search for investors.

## 3. Users

The application has one search and a filter for the user group (FAQ, section 3). The filter changes the default sort and the visible fields of the card.

| User group | Typical question | Default sort and fields |
|---|---|---|
| Investors and companies | "Which Polish technologies are ready to use?" | TRL, implementation level, IP (patent) |
| Public institutions and JST | "Did another gmina already solve this problem?" | Administrative area, used in practice, region, BZP buyers |
| Universities and research units | "Who works on a similar topic?" | Similar projects, institutions, TRL |
| Schools | "Which teaching methods work in other schools?" | Educational area, credibility, region |
| Startups | "Is my idea new?" | Duplicate check, innovativeness |
| Individual innovators | "Does a similar invention exist?" | Patents, duplicate check |

## 4. Process

1. **Discovery.** Loaders download records from the sources (section 6). A filter keeps records that describe a solution.
2. **Record.** The application maps each record to one data schema. It removes personal data.
3. **Classification.** The application selects the area and the type. The first method is keywords and the source type. A language model proposes a value for unclear records.
4. **Assessment.** The rule module scores the five criteria (section 7).
5. **Card.** The web page shows the card (section 5).
6. **Use.** The user searches, filters and compares cards. The duplicate check finds similar cards for a free-text need.
7. **Update.** A scheduled job loads each source again. It adds new records, changes the status of old records and writes the date of the check.

## 5. Innovation card

The card has the items that the FAQ requires (section 5), plus the data for the comparison.

| Field | Example | Fact or estimate |
|---|---|---|
| Title | "System zarządzania ruchem w regionie" | Fact (source) |
| Description | Short summary of the solution | Fact if quoted; estimate if a model wrote it |
| Area and type | Administrative; process | Fact (source category) or estimate |
| Area of application | Transport, JST | Estimate with reason |
| Institution | Województwo Podkarpackie | Fact |
| Region | podkarpackie | Fact |
| Implementation status | Pilot implementation | Fact or estimate |
| Readiness level | TRL 6 (only if it applies) | Usually estimate |
| Assessment of the potential | 68/100, confidence: medium | Calculated |
| Sources | Links, date of the data, date of the last check, license | Fact |
| Similar innovations | 5 cards with the similarity value | Calculated |
| Demand signals | BZP notices for similar solutions | Fact (notice) |

### Facts and estimates

Each field has these parts:

- `value`: the value;
- `kind`: `fact` or `estimate`;
- `source`: the link to the source and the date;
- `evidence`: a short quote from the source;
- `reason`: for an estimate, the reason in one sentence.

The code checks that each quote is a part of the source text. If the check fails, the field gets no value. The web page shows an estimate with the label "szacunek".

## 6. Data sources

Order of integration. All sources in the first group are free and need no key (tests of 2026-11-21, see the research report, section 2).

| Priority | Source | Content | Access | License | Area |
|---|---|---|---|---|---|
| 1 | Lista projektów FE 2021-2027 (dane.gov.pl) | About 36,500 projects with description, beneficiary, location | One XLSX | CC0 1.0 | All three |
| 2 | RAD-on open data (OPI PIB) | 19,032 patents of science units, 48,853 research projects | REST JSON | CC0 1.0 | Technological, educational |
| 3 | BZP notices (e-Zamówienia) | Purchases of public institutions | REST JSON | Free use; terms to check | Demand signal |
| 4 | NCBR "Wyszukiwarka sukcesów beneficjentów" | About 316 implemented results | One HTML page | Not stated; robots allow | Technological |
| 5 | Kohesio | EU projects 2014-2020 in Poland | Undocumented REST JSON | Probably CC BY 4.0 | All three |
| 6 | UPRP "Lista Wynalazków" (dane.gov.pl) | 139,381 Polish patent applications to 2018 | CSV | CC BY 4.0 | Technological |
| 7 | Interreg 2021-2027 (dane.gov.pl) | 207 projects with partners | XLSX | To check | Administrative |
| 8 | "Innowacyjny Samorząd", ZMP good practices | Administrative innovations | Web page | Rights of the owner: store only title, link and own summary | Administrative |

Later sources: EPO linked open data, CORDIS, OpenAlex (free key), Erasmus+ results, a crawler for BIP, school and JST pages.

Sources not to use in the hackathon: UPRP e-Wyszukiwarka (account, bot protection), EPO OPS (no public redistribution), PBN API (token), Google Scholar (no API, no robots).

Each source has its own module in `src/sources/`. Each module has one function that returns the records and the metadata: source, date of the data, date of the download, license.

## 7. Assessment

The criteria come from the FAQ (section 5). The scales are version v0.1. They need a calibration with the PCI mentors.

| ID | Criterion | Scale | Weight | Reference |
|---|---|---|---|---|
| K1 | Poziom wdrożenia | 1 koncepcja, 2 prototyp, 3 testy, 4 wdrożenie pilotażowe, 5 stosowane w praktyce | 30 | FAQ; Oslo Manual 2018 |
| K2 | Dojrzałość technologiczna | TRL 1-9; "nie dotyczy" for a non-technological innovation | 15 (0 if "nie dotyczy") | EU Horizon TRL definition |
| K3 | Innowacyjność | 1 new in the institution, 2 new in the region, 3 new in Poland, 4 new in Europe, 5 new in the world | 20 | Oslo Manual 2018 (degree of novelty) |
| K4 | Potencjał zastosowania | 0-5 points: documentation for others, known cost, no special infrastructure, use in more than one sector, demand signals (BZP) | 20 | Interreg Europe good practice; Rogers (diffusion attributes) |
| K5 | Wiarygodność | 0 no data, 1 own statement, 2 description with numbers, 3 one verifiable source, 4 two independent sources, 5 public confirmation of the implementation | 15 | FAQ; Nesta Standards of Evidence |

Rules:

1. Each value has a scale from 0 to 5. TRL changes to this scale: TRL 1-2 is 1, TRL 3-4 is 2, TRL 5-6 is 3, TRL 7-8 is 4, TRL 9 is 5.
2. If K2 does not apply, the other weights increase in proportion.
3. Potential = Σ weight × value / 5, on a scale of 0-100.
4. "Brak danych" is not 0. The score shows a range: the lowest value with 0 for unknown criteria, and the highest value with 5.
5. Confidence: the share of the weight with facts. High: 70% or more. Medium: 40% to 69%. Low: below 40%.
6. Each score shows its version of the criteria (for example "kryteria v0.1").
7. The rules and the weights are in one module: `src/criteria.py`.

## 8. Search and comparison

- Hybrid search: full-text search (SQLite FTS5, BM25) plus vector search (embeddings), joined with Reciprocal Rank Fusion.
- Filters: area, type, region, implementation level, TRL, source, date.
- Comparison: a table of 2-4 cards, with the criteria in rows.
- Duplicate check: the user writes a need. The application shows similar cards with the similarity value and the common terms.
- Similar cards: the 5 nearest cards by the embedding.

## 9. Architecture

```
src/
  sources/        one module for each data source
  pipeline.py     discovery, record, classification, deduplication
  criteria.py     criteria, scales, weights, rules (one module)
  search.py       full-text and vector search, similar cards
  app.py          FastAPI: HTML pages and JSON API
web/              templates, CSS, htmx (no build step)
data/cache/       downloads, SQLite database, embeddings (not in git)
tests/            cases.json, recorded responses, tests
```

- One SQLite database with FTS5 and `sqlite-vec`.
- The application makes the index in advance. A search does not need the sources online.
- The update job is a command (`python -m src.pipeline update`). A cron entry or a scheduler starts it on a server.
- The JSON API and the CSV export let other systems use the data.

### Stack

| Function | Component | License |
|---|---|---|
| Backend | FastAPI, Jinja2, httpx | MIT, BSD-3 |
| Web page | htmx, Alpine.js, Chart.js, Leaflet | 0BSD, MIT, MIT, BSD-2 |
| Store and search | SQLite FTS5, sqlite-vec | Public domain, MIT/Apache-2.0 |
| Embeddings | `sdadas/mmlw-roberta-base` | Apache-2.0 |
| Polish lemmas | simplemma | MIT |
| Language model (option) | Bielik v3 (Ollama), only for unclear records, made in advance | Apache-2.0 |
| Web text | trafilatura, protego | Apache-2.0, BSD-3 |

Do not use model weights with the Gemma, Llama or CC BY-NC license. See `AGENTS.md`.

## 10. Demo

Use case 1, technological: a patent of Politechnika Rzeszowska from RAD-on.

1. The investor filter shows the patent with TRL and implementation level.
2. The card separates the facts (patent, institution, dates) from the estimates (TRL).
3. Similar cards show related work of other institutions.
4. Demand signals show public buyers of similar solutions.

Use case 2, administrative: a JST user writes a need, for example "system rezerwacji wizyt w urzędzie".

1. The duplicate check shows existing solutions from FE projects and "Innowacyjny Samorząd".
2. The comparison table shows two or three solutions with the criteria.
3. The card of the best solution shows the implementation level "stosowane w praktyce" and the sources.

Use case 3, educational (if time remains): a teaching method from an FE project for schools.

## 11. Measures of effectiveness

- The time to find an existing solution for a need (demo: below 1 minute).
- The number of cards and sources, and the share of cards with high confidence.
- The share of cards with the implementation level 4 or 5.
- After the hackathon: the number of contacts and implementations that started from a card.

## 12. Work plan

| Step | Person A: data and search | Person B: assessment, web page, presentation |
|---|---|---|
| 1 | Measure the speed of the embedding model; SQLite schema | `src/criteria.py`; card template |
| 2 | Loaders: FE list, RAD-on, BZP, NCBR | Search page, filters, card page |
| 3 | Classification, deduplication, embeddings, hybrid search | Comparison page, duplicate check page, user-group filter |
| 4 | Update job; tests with recorded responses | Demo use cases; Polish text; accessibility check |
| 5 | `README.md`: sources, licenses, limits, scale-up, security | Presentation (10 slides or fewer); video (3 minutes or less) |

## 13. Risks

| Risk | Action |
|---|---|
| A source fails during the demo | The index is made in advance; the card shows "źródło niedostępne" |
| A language model is too slow on the CPU | Use rules first; run the model in advance on Kaggle or not at all |
| Wrong classification | Show it as an estimate with its reason |
| Personal data in sources | Remove names of natural persons in the loader |
| Weights without calibration | Show "kryteria v0.1" and the plan for calibration |

## 14. Open decisions

1. Accept the five criteria and the weights of section 7, or change them after a talk with the PCI mentors.
2. Select the regions for the demo: all of Poland, or all of Poland with Podkarpackie as the first view.
3. Decide on the language model: no model, or Bielik made in advance.
4. Decide on the optional usage analytics (FAQ, section 7).
