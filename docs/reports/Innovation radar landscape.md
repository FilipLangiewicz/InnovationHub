# Turn hidden public innovations into reuse

This research found no tool in Poland, the EU or elsewhere that does all six functions of the PCI task for schools, universities and JST. Polish channels are separate catalogues and award programmes. Since 2017, Polish schools have had no duty to report innovations, so no national register exists. EU platforms either score only EU-funded research (the Innovation Radar) or validate cases by hand (OPSI, Interreg Europe, SIM). The gap matters, because most public-sector innovation is adoption: in Denmark, **73% of public innovations were inspired by or copied from others** ([Apolitical](https://apolitical.co/en/articles/denmark-releases-worlds-first-survey-government-innovation)). Pure listing sites fail without an active broker. Thus the strongest concept for PCI is a **broker console, not one more catalogue**. The console turns a short submission into a structured "paszport innowacji", scores it with a two-profile rubric, shows similar work and sends it to real instruments and adopters. Free open data without keys supports this today: RAD-on, the national FE 2021-2027 project list, Kohesio, NCBR success stories and the BZP procurement API. The **BZP demand signal** ("which institution buys a similar solution now") is the most original element. A CPU-only stack with Apache-2.0 or MIT models and SQLite fits EUPL-1.2. The legal design rule is short: score the innovation, not the person, keep a human decision, and label AI text.

## Catalogues list innovations, but only brokers move them

The current landscape has three layers: Polish channels, EU platforms and commercial tools. All three layers collect and publish records. None of them scores public-sector innovations with explicit criteria and sends them to adopters. The evidence on transfer shows that this missing step is the main opportunity.

### Poland has many small channels and no register for schools

For JST, the oldest source is the **Baza Dobrych Praktyk** of ZMP. It holds "ponad 500" archived standard descriptions from 2007 onward ([BDP history](https://www.dobrepraktyki.pl/strona/11,historia-bdp.html)). Its new SMUP list has about 50 records (an estimate from the pagination), with no visible search, scoring, export or API ([BDP SMUP list](https://www.dobrepraktyki.pl/projekt/smup/lista)). Competitions find more cases than databases. The PAP "Innowacyjny Samorząd" VII edition (May 2026) had **147 projects**, and all editions reviewed more than 1,200 ([wartowiedziec.pl](https://wartowiedziec.pl/serwis-glowny/aktualnosci/80775-znamy-laureatow-vii-edycji-konkursu-innowacyjny-samorzad)). Województwo Podkarpackie took 2nd place in its category with a regional traffic model. The FRDL competition accepts only implemented solutions and includes a self-assessment sheet ([FRDL 2026 rules](https://frdl.org.pl/static/upload/store/frdl/konkurs/2026_Regulamin_Konkursu.pdf)).

The closest Polish replication model is "Innowacje w samorządzie" of Fundacja Fundusz Współpracy. It offers a fixed catalogue of **30 tested social micro-innovations** and grants of **up to 300,000 PLN per JST** for adoption, with calls until 2027 and an end in March 2028 ([innowacjewsamorzadzie.pl](https://innowacjewsamorzadzie.pl/)). The catalogue does not discover new cases. GovTech@JST (PARP, 60 JST) and HackNation build capacity, but they keep no innovation database ([TED 427624-2026](https://ted.europa.eu/en/notice/427624-2026/pdf)).

Schools are the most hidden group. Since 1 September 2017, Prawo oświatowe makes innovation a normal school activity and removes the duty to notify the kurator ([CEN Suwałki guide](https://cen.suwalki.pl/pdf/A.Matusiewicz_Innowacje_pedagogiczne_formalnosci_krok_po_kroku.pdf)). Thus no national register exists. Do not cite a "2017 MEN regulation on pedagogical innovation" in jury material: this research found no such act, and the rules are in the statute ([portaloswiatowy.pl](https://www.portaloswiatowy.pl/organizacja-pracy-szkoly-inne-zagadnienia/zmiany-w-sprawie-innowacji-pedagogicznej-od-wrzesnia-2017-13909.html)). One region shows the hidden volume. A Kuratorium Oświaty we Wrocławiu survey collected **2,895 innovation records from 547 schools** in 2024 ([DODN report](https://dodn.dolnyslask.pl/wp-content/uploads/1.-Innowacje-edukacyjne-w-dolnoslaskich-szkolach.pdf)). The median innovation had 30 participants, and only **16.1% declared an evaluation**. The report recommends a "Karta innowacji" as a minimum standard, a common repository and transfer through leader schools. Its fields are a tested minimum schema for school intake:

- type (metodyczna, programowa, organizacyjna, mixed);
- subject area and school stage;
- scale (participants) and duration;
- evaluation (yes or no);
- RSPO number of the school.

The research side has the best data. The OPI PIB **RAD-on open API returns 48,853 research projects** with Polish and English abstracts and keywords, without a key ([RAD-on projects API](https://radon.nauka.gov.pl/opendata/polon/projects?resultNumbers=1)). The portal innowacje.opi.org.pl only links to other systems ([innowacje.opi.org.pl](https://innowacje.opi.org.pl/)). University technology offers, for example the Politechnika Rzeszowska catalogue, are PDF files ([PRz catalogue](https://przemysl.prz.edu.pl/fcp/mGBUKOQtTKlQhbx08SlkTVABQX2o8DAoHNiwFE1xVT31BG1gnBVcoFW8SBDRKHg/74/code_kE14VPxZcJBJhAAQqEk1UB0FcAyQ/obrazy/katalog.pdf)).

PCI itself runs the **60.1 mln PLN "PCI 2029" project** with a grant programme, commercialisation support and the PAPA academic accelerator ([PCI 2029](https://pcinn.org/projekty/pci-2029/)). It also runs Deeptech Akcelerator, InnoStart Hub Polska, ProtoLab and a directory of 24 laboratories ([pcinn.org](https://pcinn.org); [badania.pcinn.org](https://badania.pcinn.org/laboratoria)). PCI publishes a detailed point rubric for its R&D grants ([PCI criteria](https://projekty.pcinn.org/assets/Projekty/GrantN5/1.5.%20Za%C5%82.%20nr%203%20do%20Reg.%20-%20Kryteria%20wyboru%20projekt%C3%B3w.pdf)). This research found no public PCI pipeline database, CRM or similarity search. Thus a radar adds a front end to PCI. It does not replace a PCI system.

The nearest planned competitor is **Smart Radar 2.0** of MRiT. In 2026 the ministry asked for price offers for a system that monitors companies and smart specialisations. The plan includes a matchmaking "Platforma innowacji" and "Mapy innowacji" ([MRiT notice](https://www.gov.pl/attachment/4f4b8c85-a966-431a-925c-d00aa963ca5a)). It targets companies and KIS/RIS statistics, not schools or JST. Its status after April 2026 is not known. The radar can use the same KIS/RIS taxonomy and present itself as the public-sector complement.

### EU platforms score only research or validate by hand

The **EU Innovation Radar** is the only EU system with a published quantitative scoring method. Experts fill a questionnaire during EU project reviews. The JRC aggregates the answers into an Innovation Potential Indicator (readiness, management, market potential) and an Innovator Capacity Indicator ([JRC104870](https://publications.jrc.ec.europa.eu/repository/bitstream/JRC104870/jrc104870_innovation-radar_2_final.pdf)). The repository passed 10,000 innovations in January 2023 ([Digital Strategy](https://digital-strategy.ec.europa.eu/en/news/innovation-radar-european-commissions-repository-innovations-market-potential-hits-10000-milestone)). It covers only EU-funded research projects, and this research found no public API. Its own data supports the PCI problem: **28% of mature innovations were not exploited, and nearly 50% of high-potential innovations were not exploited** ([JRC104870](https://publications.jrc.ec.europa.eu/repository/bitstream/JRC104870/jrc104870_innovation-radar_2_final.pdf)).

The other EU sources are libraries with human validation:

- **OECD OPSI** case platform: filters and a submission form, licence CC BY-SA 3.0 IGO, no API ([OPSI](https://oecd-opsi.org/innovations/)).
- **JRC Public Sector Tech Watch**: 1,617 cases in 2024, more than 80% about AI, open dataset ([PSTW report](https://interoperable-europe.ec.europa.eu/collection/public-sector-tech-watch/document/public-sector-tech-watch-adoption-ai-blockchain-and-other-emerging-technologies-within-european); [JRC Data Catalogue](https://data.jrc.ec.europa.eu/dataset/e8e7bddd-8510-4936-9fa6-7e1b399cbd92)).
- **Interreg Europe good practices**: up to three validation steps; a common rejection reason is "evidence of success not clearly demonstrated" ([Interreg validation slides](https://WWW.INTERREGEUROPE.EU/sites/default/files/2023-10/TrainingDays2023_Day2_GP_submission_and_validation.pdf)).
- **ESF+ Social Innovation Match**: 150 to 200+ cases; manual browse, filter and contact; no algorithmic matching found ([SIM](https://european-social-fund-plus.ec.europa.eu/en/social-innovation-match)).
- **Erasmus+ Project Results Platform**: good-practice labels and a manual XLS export ([EC wiki](https://wikis.ec.europa.eu/spaces/NAITDOC/pages/121442219/Searching+in+Erasmus+Project+results+platform)).
- **EU Innovation Platform (EUIP)**: launched on 3-4 June 2026; it took the market-oriented results of the Horizon Results Platform ([EU4Digital](https://eufordigital.eu/european-commission-launches-eu-innovation-platform/)).
- **Enterprise Europe Network**: 5,148 to 5,709 active offer and request profiles in crawled snapshots ([EEN](https://int.een.ec.europa.eu/partnering-opportunities?page=35)).
- **EU Open Source Solutions Catalogue** (2025): its stated goal is to help public bodies avoid duplicate development ([data.europa.eu](https://www.data.europa.eu/en/news-events/news/discover-eu-open-source-solutions-catalogue-strengthening-collaboration-across)).

Public sources show no AI detection or semantic matching in these platforms. For the radar, they are exit routes for the recommended paths, not competitors.

### Commercial tools are expensive and blind to public institutions

Idea-management suites (Qmarkets, HYPE, Brightidea, IdeaScale) collect staff ideas and flag duplicates. Qmarkets documents that its similar-idea check is SOLR keyword relevance, "not generative AI" ([Qmarkets help](https://help.qmarkets.net/support/solutions/articles/44002796491-similar-idea-matchmaking-algorithm)). Scouting suites (ITONICS, Valuer, StartUs Insights, PatSnap, Dealroom, Crunchbase) mine startup, patent and news databases. Hidden public-sector innovations do not appear in these sources. Prices are a barrier for a regional agency: Dealroom Premium costs **€12,600 per year** ([Dealroom pricing](https://www.dealroom.co/pricing/)). A Vendr-based estimate gives a median Crunchbase API contract of about USD 20,000 per year ([Vendr](https://www.vendr.com/marketplace/crunchbase)).

In technology transfer, IN-PART connects 250+ universities with 6,000+ companies. Algorithms and in-house experts make the matches; universities pay and companies use the service free ([IN-PART](https://www.in-part.com/for-universities)). yet2 says that the pure online IP marketplace "would ultimately fail", and it moved to a service model ([yet2](https://www.yet2.com/?p=1221)). The mature open-source participation platforms (Decidim, CONSUL, Go Vocal free edition) all use AGPLv3 ([Decidim FAQ](https://decidim.org/faqs); [OSOR case study](https://interoperable-europe.ec.europa.eu/sites/default/files/inline-files/osor-case-study-of-consul-democracy.pdf)). This research found no maintained open-source tool for idea management or transfer matching.

**Table 1. Nearest existing solutions and what they lack**

| Solution | Scope | What it does | What it lacks |
|---|---|---|---|
| Baza Dobrych Praktyk (ZMP) | JST | 500+ archived descriptions, about 50 new | Search, scoring, export, API |
| "Innowacyjny Samorząd" (PAP) | JST | Award, 147 entries in 2026 | Public database |
| "Innowacje w samorządzie" (FFW) | JST, social | 30 ready innovations, grants | Discovery of new cases |
| KO Wrocław survey | Schools | 2,895 records from 547 schools | Open data, other regions |
| RAD-on API (OPI PIB) | Research | 48,853 projects, open API | Assessment, matching |
| PCI systems | Research, startups | Grant calls, lab directory, rubric | Pipeline view, similarity search |
| Smart Radar 2.0 (MRiT) | Companies, KIS/RIS | Planned matchmaking and maps | Schools and JST; not built yet |
| EU Innovation Radar | EU-funded research | 10,000+ scored innovations | Public sector, open API |
| OPSI, PSTW, Interreg, SIM | Public sector, EU | Case libraries | Scoring of potential, matching |
| IN-PART, Qmarkets, ITONICS | Universities, firms | Matching or idea management | Public institutions, low price |

### Transfer needs a broker and a record of adoption

Four findings shape the design. First, adaptation dominates. The Danish survey of 1,255 public workplaces (2013-2014) found that 60% of innovations were adapted and 13% were direct copies ([Apolitical](https://apolitical.co/en/articles/denmark-releases-worlds-first-survey-government-innovation)). The Danish centre COI counts spread only when a second workplace implements the solution ([OPSI](https://oecd-opsi.org/?p=18336)). Second, listings alone fail. A vendor study of company knowledge bases found that 76% of registered users never created a document ([Slite](https://slite.com/learn/why-knowledge-bases-fail)); treat this vendor number with caution. Third, brokers move innovations. Nesta writes: "Private markets don't assume new products and services will reach customers by themselves" ([Nesta](https://www.nesta.org.uk/blog/four-reflections-public-service-innovation/)). Nesta also found that innovations with clear referral routes through public services found demand more easily ([Nesta](https://nesta.org.uk/node/37012)). Fourth, evidence decides acceptance, as the Interreg rejection rule shows.

Nesta defined "hidden innovation" as innovation that R&D and patent statistics do not show, in sectors such as education ([Nesta: Hidden Innovation](https://nesta.org.uk/report/hidden-innovation)). Thus the radar must use other signals: project lists, competitions, procurement and the institutions' own pages. The evidence gives six design rules:

1. Show the date of the last confirmation of each record, and mark records without evidence.
2. Draft the record from a public source, so that the creator only confirms it.
3. Score evidence of effect, not only novelty.
4. Record adoption events, and show "adopted by N organisations".
5. Record the "core" and the "adaptable parts" of each innovation ([Nesta: Making it big](https://www.nesta.org.uk/report/making-it-big-strategies-for-scaling-social-innovations/)).
6. Keep a human broker (PCI staff) in the loop with a work queue.

## Six of eight task sources fail for automated use, but open substitutes exist

The task lists eight example sources. Tests and terms of use on 21 November 2026 showed that **six of the eight are blocked, unreachable or out of date for automated use**. Each of these sources with real value has an open substitute.

**Table 2. Status of the data sources that the task names**

| Task source | Status on 21 November 2026 | Usable substitute |
|---|---|---|
| UPRP e-Wyszukiwarka | Imperva bot protection; the API needs an e-Profil account, an API key and a JWT ([UPRP API guide](https://api.uprp.gov.pl/assets/instrukcje/PROD/e-Wyszukiwarka.pdf)) | UPRP "Lista Wynalazków" CSV on dane.gov.pl; RAD-on `products` for patents of science units |
| EPO Register | "Automated searches (robots) are not supported" ([EPO fair use](https://epo.org/en/service-support/ordering/fair-use)) | EPO Linked Open EP data (SPARQL, CC BY 4.0, no registration) |
| PBN | API needs an institution token (HTTP 403) ([PBN API](https://pbn.nauka.gov.pl/api/v3/api-docs)) | RAD-on `publications` (probably the same data, not confirmed) |
| PPM | No answer in 45-60 s; medical universities only ([OpenDOAR](https://opendoar.ac.uk/repository/10049)) | Skip |
| dLibra (czasopisma.pan.pl) | OAI-PMH 2.0 works, 101 sets ([PAN OAI](https://www.czasopisma.pan.pl/dlibra/oai-pmh-repository.xml?verb=Identify)) | Optional, with `sickle` (BSD) |
| BazTOL | Not updated since 1 January 2022, no API ([BazTOL](http://baztol.library.put.poznan.pl/baztol_czytelnik/baztol)) | Skip |
| interreg.gov.pl | No answer (connection timeout) | dane.gov.pl XLSX with 207 Interreg 2021-2027 projects and partners ([dane.gov.pl](https://api.dane.gov.pl/1.4/resources/487769)) |
| NCBR success search | Works; 316 entries in server HTML; gov.pl robots allow all ([NCBR](https://www.gov.pl/web/ncbr/wyszukiwarka-sukcesow-beneficjentow)) | Use directly |

### Five keyless sources cover supply and demand

The best supply source is the national **"Lista projektów FE 2021-2027"** on dane.gov.pl. It is one 41 MB XLSX file under **CC0 1.0**, with a quarterly update and about 36,500 rows ([dane.gov.pl resource 2912686](https://api.dane.gov.pl/1.4/resources/2912686)). Each row has the title, the description, the beneficiary, the programme, the value and the location down to gmina. The test found 2,353 Podkarpackie rows and 9,056 rows with "innowac". Some beneficiaries are natural persons, so the loader must hide them. **RAD-on** adds patents of science units (`products`, 19,032 records, with abstracts) and research projects, without a key ([RAD-on products](https://radon.nauka.gov.pl/opendata/polon/products?resultNumbers=1)). The dane.gov.pl copies of these RAD-on datasets carry CC0 1.0 ([dataset 294](https://api.dane.gov.pl/1.4/datasets/294)). The filter `institutionName` works: Politechnika Rzeszowska has 600 patent records, and Uniwersytet Rzeszowski has 125.

The best demand source is the **BZP notice API** on e-Zamówienia. The UZP rules say that the BZP web service is free and needs no access application ([UZP API rules](https://media.ezamowienia.gov.pl/pod/2023/01/Regulamin-korzystania-z-API.pdf)). The documented path returns only HTML. The working JSON endpoint is `https://ezamowienia.gov.pl/mo-board/api/v1/notice` ([BZP API](https://ezamowienia.gov.pl/mo-board/api/v1/notice?NoticeType=ContractNotice&PublicationDateFrom=2026-11-19&PublicationDateTo=2026-11-20&PageSize=2)). Each notice gives the buyer name, NIP, city, voivodeship code, CPV code and subject. Thus the radar can show that a given school or gmina bought a similar solution. Procurement plans are a stronger early signal by law, but the test found no open API path for them.

**Table 3. Data sources to use, ranked by value per hour**

| Rank | Source | Access | Licence | Limits and caveats | Use in the radar |
|---|---|---|---|---|---|
| 1 | FE 2021-2027 project list | One XLSX download (72 s), no key | CC0 1.0 | Title row first; no TERYT codes; natural persons | Supply corpus for all of Poland; similar projects; adopters |
| 2 | RAD-on open data | REST JSON, token pagination, no key | CC0 1.0 (dane.gov.pl copies) | Rate limits not known; `employees` is personal data | University patents and projects; partners |
| 3 | BZP notice API | REST JSON, no key | Reuse status confirmed only by third parties [VERIFY] | No published rate limit; no plans endpoint | Demand signal; adopter candidates |
| 4 | NCBR success search | One HTML page | Not stated; robots allow | 316 entries only | Seed set; calibration examples |
| 5 | Kohesio API | Undocumented REST JSON and CSV, no key | No explicit licence; CC BY 4.0 by EC default is an inference ([data.europa.eu](https://data.europa.eu/en/copyright-notice)) | API can change; EN CSV loses Polish letters | 2014-2020 projects; 2.28 million EU projects ([Kohesio](https://kohesio.ec.europa.eu/api/queries/statistics)) |
| 6 | Interreg 2021-2027 XLSX | One download | Not checked | 207 projects only | Cross-border partners |
| 7 | UPRP "Lista Wynalazków" CSV | One ZIP, no key | CC BY 4.0 | 139,381 rows; data stops at 2018-11-15 ([UPRP ZIP](https://www.dane.gov.pl/media/resources/20190409/2019_04_09_listaWynalazkow.zip)) | Older Polish inventions by city and IPC |
| 8 | EPO Linked Open EP data | SPARQL and JSON, no key | CC BY 4.0, "occasional use" ([EPO](https://www.epo.org/en/searching-for-patents/data/linked-open-data)) | Aggregate queries time out; no city in address | EP patents by exact applicant name |
| 9 | TED Search API v3 | Anonymous POST | Not confirmed on an official page | Above-threshold notices only ([TED API](https://docs.ted.europa.eu/api/latest/index.html)) | Large purchases |

Reference data also works without keys. GUS BDL allows about 10,000 anonymous requests per 7 days ([BDL API](https://bdl.stat.gov.pl/api/v1/units?level=2&format=json&page-size=2)). The KRS API answers by KRS number, and ROR finds universities with their identifiers ([KRS API](https://api-krs.ms.gov.pl/api/krs/OdpisAktualny/0000019193?rejestr=P&format=json); [ROR API](https://api.ror.org/v2/organizations?query=Rzesz%C3%B3w&filter=country.country_code:PL)). The BIP home page offers a ZIP list of all active BIP entities, the best seed for a later crawler ([gov.pl/web/bip](https://www.gov.pl/web/bip)). Science enrichers are OpenAlex (CC0), OpenAIRE (CC BY, 60 anonymous requests per hour) and CORDIS bulk files (CC BY 4.0) ([OpenAlex help](https://help.openalex.org/api/authentication/); [OpenAIRE terms](http://graph.openaire.eu/docs/apis/terms/); [CORDIS](https://cordis.europa.eu/llms.txt)). OpenAlex list queries need a free key in practice: the keyless budget was empty in the test.

### Schools and JST stay dark in open data

No open dataset lists school or JST innovations. The RSPO school register API now needs Basic Auth credentials, and the old open host does not resolve ([RSPO API doc](https://api.rspo.gov.pl/files/RSPO-API-doc.pdf)). All rights to BDP texts belong to ZMP, so the radar may store only titles, categories and links ([BDP rules](https://www.dobrepraktyki.pl/strona/5,regulamin.html)). The Obserwatorium Polityki Miejskiej publishes about 2,874 documents through its sitemaps, but no structured good-practice base ([OPM sitemap](https://obserwatorium.miasta.pl/sitemap_index.xml)). GDELT returned HTTP 429 and Common Crawl returned HTTP 504 in the test ([GDELT](https://api.gdeltproject.org/api/v2/doc/doc?query=innowacja%20sourcecountry:poland&mode=artlist&format=json&maxrecords=3); [Common Crawl](https://index.commoncrawl.org/collinfo.json)). Thus the hackathon version must detect candidates in open project data. A crawler of school and gmina pages comes later.

Do not use these sources in the hackathon:

- UPRP e-Wyszukiwarka API and web page (account, token, bot protection);
- EPO OPS: registration is necessary, and the terms forbid public redistribution of "the data as such" ([OPS terms](https://www.epo.org/en/service-support/ordering/ops-terms-and-conditions));
- PBN API (institution token), PPM, FBC and BazTOL;
- Semantic Scholar: HTTP 429, and the licence forbids redistribution ([S2 licence](https://www.semanticscholar.org/product/api/license));
- Lens.org and WIPO PATENTSCOPE (request process or paid);
- Baza Konkurencyjności (internal API only) and keep.eu API (key by e-mail).

The ORCID public API is "free for non-commercial use" ([ORCID FAQ](https://info.orcid.org/faq/what-are-the-api-limits/)). Check this term before PCI uses ORCID data in a service.

## Two maturity axes and gated path rules make the score defensible

The task asks for "ocenę potencjału według jasno określonych kryteriów", a repeatable process and a clear path after identification. Published frameworks give each element. The main design problem is that the common maturity scale, TRL, does not fit non-technical innovations.

### TRL for technology, SRL for the other types

TRL is the common language of Polish and EU funders. A Polish legal text with nine levels exists in the annex of Dz.U. 2020 poz. 1495, but its scope is defence R&D at NCBR ([ELI, Dz.U. 2020 poz. 1495](https://api.sejm.gov.pl/eli/acts/DU/2020/1495/text.html)). The TRL anchors use "technology" and "prototype", so they do not fit an educational or social solution. The **Societal Readiness Level (SRL 1-9)** of Innovation Fund Denmark fills this gap. It applies to "a social project, a technology, a product, a process, an intervention, or an innovation (whether social or technical)" ([IFD SRL](https://www.blogs.uni-mainz.de/research/files/2019/03/societal_readiness_levels_-_srl.pdf)). Thus the rubric uses TRL for profile T (technology) and SRL for profile N (non-technology). No published crosswalk between TRL and SRL exists, so the equal mapping to 0-5 is an assumption.

The KTH Innovation Readiness Level gives six dimensions on 1-9 scales: customer, technology, business model, IPR, team and funding ([DTU on KTH IRL](https://www.inside.dtu.dk/en/research/commercialisation-and-patenting/patents-and-software/kth-innovation-readiness-level)). It is the best model for a multi-dimension profile. But the KTH PDF says "© KTH Innovation – All rights reserved" ([KTH IRL PDF](https://www.energimyndigheten.se/49cd71/globalassets/utlysningar/iaif/bilaga--kth-innovation-readiness-level_compiled_c.pdf)). Thus the team must write its own anchor text and cite KTH only as the model.

### The Innovation Radar gives a simple, published aggregation rule

The JRC normalises each indicator as observed score divided by maximum score, times 100, and adds the indicators with equal weight. An innovation has "high potential" when its score is at least one standard deviation above the mean ([JRC104870](https://publications.jrc.ec.europa.eu/repository/bitstream/JRC104870/jrc104870_innovation-radar_2_final.pdf)). A JRC audit found the potential index statistically sound, with the most room to improve in market potential ([JRC110926](https://publications.jrc.ec.europa.eu/repository/handle/JRC110926)). For a small hackathon database, fixed thresholds are better than classes from the standard deviation.

### Non-technical criteria come from evidence and transfer frameworks

Four sources give the criteria that TRL does not cover:

- **Nesta Standards of Evidence** (levels 1-5): from a logical theory of change (level 1) to "manuals, systems and procedures to ensure consistent replication" (level 5) ([Nesta SoE](https://www.nesta.org.uk/feature/innovation-methods/standards-evidence/)).
- **Interreg Europe**: a good practice gives "tangible and measurable results" and is "of potential interest to other regions" ([Interreg Europe](http://www.interregeurope.eu/help-centre/expert-support-services/good-practices)).
- **Rogers, ExpandNet CORRECT and CFIR**: credible, observable, relevant, relative advantage, easy to install, compatible, testable ([PMC9338559](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9338559/)). The rubric turns these attributes into five yes or no items for transferability.
- **OECD OPSI facets**: enhancement, adaptive, mission and anticipatory innovation ([OPSI facets](https://oecd-opsi.org/blog/innovation-facets-different-tools-for-different-aims/)). Use the facet as a descriptive tag, not as a score.

### The PCI grant card is the rubric that the jury knows

PCI scores its V grant call (closed on 20 November 2026) in three layers ([PCI criteria](https://projekty.pcinn.org/assets/Projekty/GrantN5/1.5.%20Za%C5%82.%20nr%203%20do%20Reg.%20-%20Kryteria%20wyboru%20projekt%C3%B3w.pdf)):

1. Seven pass-or-fail merit criteria, for example TRL and fit to the Podkarpackie RIS.
2. Four quality criteria at 0, 2 or 4 points: competitive advantage, market need, benefit for the recipient and implementation path (minimum 8 of 16, and at least 2 for each).
3. Premium points (maximum 9): MVP 0 or 3, validation with a technology recipient 0 or 4, dual-use 0, 1 or 2.

A panel of three external experts scores each project by consensus on one card ([PCI procedure](https://projekty.pcinn.org/assets/Projekty/GrantN5/1.4.%20Za%C5%82.%20nr%202%20do%20Reg.%20-%20PROCEDURA%20OCENY%20PROJEKT%C3%93W.pdf)). The radar should map its criteria to this card, so that PCI jury members see their own logic. But the card serves university R&D. Schools and JST need criteria for evidence and transferability, and the Wrocław data shows that evidence is often missing.

Other funders use the same pattern. Horizon Europe scores each criterion 0-5 with a threshold of 3 **[UNVERIFIED]** ([HE General Annexes](https://ec.europa.eu/info/funding-tenders/opportunities/docs/2021-2027/horizon/wp-call/2023-2024/wp-13-general-annexes_horizon-2023-2024_en.pdf)). The EIC Accelerator uses GO or NO GO per criterion ([horizon-europe.gouv.fr](https://www.horizon-europe.gouv.fr/candidater-l-eic-accelerateur-le-processus-d-evaluation-37524)). PARP gives 0 points for innovativeness if a product is not new at least at national level ([PARP Annex 2](https://en.parp.gov.pl/storage/grants/documents/62/Zalacznik-nr-2-Kryteria-oceny-merytorycznej-pomyslu-przez-Ekspertw-na-e_28042020.pdf)). Thus the radar uses non-compensatory gates: a low value of a key criterion blocks a path, even with a high total.

### An LLM can propose a score, but it must not decide

Human reviewers of proposals agree little **[UNVERIFIED]** ([Pier et al. 2018](https://www.pnas.org/doi/10.1073/pnas.1714379115)). LLMs do not solve this problem. A 2026 study found that **a 0-5 scale gives the strongest human-LLM alignment** across six benchmarks ([Li et al. 2026](https://arxiv.org/abs/2601.03444)). But LLM ratings show low agreement between runs of the same prompt ([arXiv 2510.27106](https://arxiv.org/pdf/2510.27106.pdf)). Expert-LLM weighted kappa ranged from 0.17 to 0.86 across 21 dimensions ([arXiv 2605.16354](https://arxiv.org/pdf/2605.16354)). This research found no study that compares LLM and expert scores of public-sector innovations. Thus the LLM extracts facts and proposes a level with a quote. A rule engine computes the totals and the paths. A PCI analyst confirms or changes each value, and the system logs the reason.

Each criterion gets an evidence tag, which gives a confidence level for the total score:

- E0 "brak danych": no information (different from a score of 0);
- E1 "deklaracja": a statement of the author without detail;
- E2 "deklaracja z opisem": a statement with numbers, names or an internal document;
- E3 "źródło weryfikowalne": a public record (patent register, evaluation report, NCBR or Interreg database, BIP resolution).

### Paths must name real instruments and their measures

Each path needs a different profile, and the path engine must check the institution type. For example, the PCI Program Grantowy accepts only scientific units. Public buyers have a legal route when no product exists: "partnerstwo innowacyjne" applies when the need "nie może zostać zaspokojone przez zakup produktów, usług lub robót budowlanych już dostępnych na rynku" (art. 189 Pzp) ([lexlege.pl](https://lexlege.pl/zamow-publ/oddzial-6-partnerstwo-innowacyjne/13953/)). Its use is rare: a UZP report gives 53 notices in 2021, about 0.05% of all notices (search summary) ([gov.pl attachment](https://www.gov.pl/attachment/10669ed0-925e-4a9e-bd21-ef8acf4afcc4)). The EU platforms give exit routes: EUIP and EEN for commercialisation, SIM for social innovation and the Interreg Europe database for policy practice.

For effectiveness measures, the JRC and ASTP say that knowledge-transfer outputs "cannot be viewed in isolation from inputs" ([JRC120716](https://publications.jrc.ec.europa.eu/repository/handle/JRC120716)). RE-AIM gives post-adoption measures: reach, effectiveness, adoption, implementation and maintenance **[UNVERIFIED]** ([RE-AIM.org](https://re-aim.org/)). No standard KPI set for an innovation radar of schools or JST exists. The full rubric, the path rules and the measures per path are in the recommendations below.

## A CPU-only Polish stack fits EUPL-1.2 without Gemma, Llama or NC weights

Polish NLP has mature open models with permissive licences. The main trap is licences, not quality. Many recent Polish models with the best scores carry the Gemma licence, and some PLLuM models are non-commercial.

**Table 4. Recommended stack and licences**

| Function | First choice (licence) | Alternative (licence) | Avoid |
|---|---|---|---|
| Embeddings | `sdadas/mmlw-roberta-base`, 124M (Apache-2.0) or `mmlw-retrieval-roberta-large`, 435M (Apache-2.0) | `BAAI/bge-m3` (MIT); `Qwen3-Embedding-0.6B` (Apache-2.0) | sdadas v2 models and stella-pl-retrieval (Gemma); jina-embeddings-v3 (CC BY-NC 4.0); EmbeddingGemma |
| Reranker | `sdadas/polish-reranker-large-ranknet` (Apache-2.0) | `BAAI/bge-reranker-v2-m3` (Apache-2.0) | polish-reranker-roberta-v2 and v3 (Gemma) |
| LLM | Bielik-11B-v3.x-Instruct GGUF Q4_K_M, 6.7 GB (Apache-2.0) | Bielik-4.5B-v3.0, PLLuM-4B or 12B-2512, Qwen3.5-4B (Apache-2.0) | PLLuM "-nc" (CC BY-NC); Llama-PLLuM (Llama 3.1 licence); Gemma 3 |
| LLM runtime | Ollama with a JSON schema in `format` (MIT) | llama.cpp (MIT) | Hosted APIs with personal data |
| Store and search | SQLite FTS5 + `sqlite-vec` (MIT or Apache-2.0), fused with RRF | Qdrant (Apache-2.0) | Elasticsearch (AGPL, SSPL, ELv2); Typesense (GPL-3.0) |
| Polish lemmas | `simplemma` (MIT) or Morfeusz 2 (BSD-2) | `pystempel` (Apache-style) | spaCy Polish models (GPL-3.0) |
| Duplicates | `datasketch` MinHash (MIT) + embedding cosine | NeMo-style semantic dedup | Keyword-only checks |
| Institutions | `rapidfuzz` (MIT) + ROR dump (CC0) | `splink` (MIT) | — |
| Web text and PDF | `httpx`, `protego` (BSD-3), `trafilatura` (Apache-2.0), `pypdf` (BSD-3) | `docling` (MIT) | PyMuPDF (AGPL-3.0) |
| Web layer | FastAPI (MIT), Jinja2, htmx (0BSD), Alpine.js (MIT), Leaflet (BSD-2), Chart.js (MIT) | Streamlit (Apache-2.0) | A build step |
| Graph | NetworkX (BSD-3) in memory | — | Neo4j Community (GPL-3.0); Kuzu (archived) |

Sources for Table 4: [PL-MTEB v2](https://arxiv.org/html/2405.10138); [HF sdadas models](https://huggingface.co/api/models?author=sdadas); [HF bge-m3](https://huggingface.co/BAAI/bge-m3); [HF polish-reranker-roberta-v3 card](https://huggingface.co/sdadas/polish-reranker-roberta-v3); [Bielik v3 report](https://arxiv.org/pdf/2601.11579v1); [Ollama Bielik](https://ollama.com/SpeakLeash/bielik-11b-v3.1-instruct); [HF PLLuM](https://huggingface.co/CYFRAGOVPL/PLLuM-12B-chat); [PyPI sqlite-vec](https://pypi.org/project/sqlite-vec/); [spaCy pl model](https://github.com/explosion/spacy-models/releases/tag/pl_core_news_md-3.8.0); [Morfeusz licence](http://morfeusz.sgjp.pl/doc/license/); [trafilatura evaluation](https://trafilatura.readthedocs.io/en/latest/evaluation.html); [PyPI pymupdf](https://pypi.org/project/pymupdf/); [GitHub htmx](https://github.com/bigskysoftware/htmx).

### Expected quality and speed

On PL-MTEB v2, retrieval scores with Apache models are **53.60 for mmlw-roberta-base and 57.23 for mmlw-retrieval-roberta-large** ([PL-MTEB v2](https://arxiv.org/html/2405.10138)). The base model is about 3.5 times smaller, so it is the safer choice for CPU queries. On Polish public-administration data, `polish-reranker-large-ranknet` reaches NDCG@10 of 85.0, 78.2 and 79.9 on three sets ([HF card](https://huggingface.co/sdadas/polish-reranker-roberta-v3)). For the LLM, Bielik-11B-v3.0 reaches an Open PL LLM average of **65.93**, close to Llama-3.3-70B at 66.40 (self-reported by the Bielik team) ([Bielik v3 report](https://arxiv.org/pdf/2601.11579v1)). Bielik-4.5B-v3.0 reaches 56.13.

Speed is the main risk. This research found no reliable laptop measurement. An estimate for an 11B model at Q4_K_M on a laptop CPU is 3-8 tokens per second. Thus one JSON record of about 300 tokens can take 40-100 seconds. This breaks the 30-second rule of the repository. Thus the design must:

1. Extract the demo records in advance.
2. Let the LLM only pre-fill fields, so that the rule engine scores without it.
3. Measure the speed of the team machine in the first hour.

Extraction must be checkable. Define the record as a Pydantic model and pass its JSON schema to Ollama `format` ([Ollama docs](https://docs.ollama.com/capabilities/structured-outputs)). For each field, store the value, a short evidence quote and a confidence. Check in code that each quote is a substring of the source text, and set the field to null if the check fails. Use closed lists for type, sector, institution type and voivodeship, so that the model cannot invent a category.

### Duplicate detection and matching need two levels and visible reasons

Use two levels for duplicates. MinHash with a Jaccard threshold of about 0.8 finds copied text ([arXiv 2512.18834](https://arxiv.org/pdf/2512.18834)). Embedding cosine finds the same idea in different words. NVIDIA NeMo Curator uses a cosine of 0.93 in its text examples and tells users to check the distribution first ([NeMo SemDeDup](https://docs.nvidia.com/nemo/curator/main/curate-text/process-data/deduplication/semdedup.md)). No published threshold for Polish near-duplicates with mmlw models exists, so the team must calibrate it on a few labelled pairs. Hybrid search (FTS5 BM25 plus vectors, fused with Reciprocal Rank Fusion, k = 60) is simple in one SQLite file ([Simon Willison](https://simonwillison.net/2024/Oct/4/hybrid-full-text-search-and-vector-search-with-sqlite/)). The PIRB benchmark reports that hybrid retrieval improves on dense retrieval alone for Polish ([PIRB](https://arxiv.org/abs/2402.13350)).

For matching, use one explainable weighted sum per pair, for example 0.5 × cosine + 0.2 × sector match + 0.15 × maturity fit + 0.15 × region match. This formula is an engineering proposal without validation. Show each component and the shared key terms next to each match. Let the LLM write only a short reason from the computed fields.

### EUPL-1.2 accepts permissive code, but weight licences bind the operator

The European Commission compatibility matrix says that MIT, BSD and Apache code can be incorporated into an EUPL work. For GPL and AGPL code, incorporation is "NO (exceptions exist)", but dynamic linking is "OK" ([EC EUPL matrix](https://interoperable-europe.ec.europa.eu/collection/eupl/matrix-eupl-compatible-open-source-licences)). The EUPL appendix lists GPL, AGPL, LGPL, MPL-2.0, EPL-1.0 and other licences as compatible. But a combination can move the whole work to the other licence ([EUPL-1.2 text](https://interoperable-europe.ec.europa.eu/sites/default/files/distribution/access_url/2024-10/e5101c2c-40b6-428e-a16e-a6ac9ade8a01/EUPL_v1.2.pdf)). The application downloads model weights at run time, so the weight licence does not change the code licence. But PCI must obey the weight licence when it runs the system. The Gemma terms let Google "restrict (remotely or otherwise) usage" ([Gemma Terms](https://gemma.ai/resources/terms/)). Thus the deliverable should use only Apache-2.0 or MIT weights. Gemma 4 is an exception, because it has Apache-2.0 ([Google Open Source Blog](https://opensource.googleblog.com/2026/03/gemma-4-expanding-the-gemmaverse-with-apache-20.html)).

## The open gap is demand, follow-up and non-technical scoring

The problem is real in official data. Poland reaches **65.9% of the EU innovation average** and is an "Emerging Innovator" ([RIS 2025 Poland profile](https://ec.europa.eu/assets/rtd/ris/2025/ec_rtd_ris-regional-profile-pl.pdf)). Podkarpackie reaches 65.5% and ranks 193rd of the regions. The region shows a clear "hidden innovation" signal: creativity is strong, but conversion into use is weak.

| Podkarpackie indicator (RIS 2025, EU = 100) | Value | Rank |
|---|---|---|
| Design applications | 148.0 | 16 |
| Sales of new-to-market and new-to-firm innovations | 21.5 | 230 |
| Public-private co-publications | 51.5 | 223 |
| Innovative SMEs that collaborate (change since 2023) | +53.1 points | — |

Source: [RIS 2025 Poland profile, p. 32-33](https://ec.europa.eu/assets/rtd/ris/2025/ec_rtd_ris-regional-profile-pl.pdf). The total number of regions is 241 or 246, as sources differ **[VERIFY]**.

The classic research-to-business channel is also weak. NIK found in 2018 that special-purpose companies ("spółki celowe") were not an effective way to transfer research results ([money.pl on NIK](https://www.money.pl/gielda/nik-spolki-celowe-maja-slabe-efekty-w-transferze-wynikow-badan-do-gospodarki-6337330316613249a.html)). GUS measures innovation only in enterprises, and this research found no official statistic for schools or JST ([GUS](https://stat.gov.pl/obszary-tematyczne/nauka-i-technika-spoleczenstwo-informacyjne/nauka-i-technika/dzialalnosc-innowacyjna-przedsiebiorstw-w-latach-2022-2024,2,23.html)). The population of sources is large. Poland has **2,809 JST** (16 voivodeships, 314 powiaty and 2,479 gminy on 1 January 2026) ([MSWiA](https://www.gov.pl/web/mswia/baza-jst)). It has about 14,000 primary and 7,000 post-primary schools with 511,000 teachers ([GUS](https://stat.gov.pl/dla-mediow/informacje-prasowe/polska-szkola-w-liczbach-jak-wyglada-edukacja-w-roku-szkolnym-20242025,36,1.html)). Manual collection of good practices cannot cover this population.

### Seven gaps that no found tool fills

1. **Sector silos.** Each database covers one source: EU research, Interreg projects, emerging-technology cases or social incubators.
2. **Manual entry.** Experts or project owners write each record. This does not scale to about 24,000 schools and JST.
3. **Technology bias.** Current scores measure readiness and market potential. Transferability, cost to adopt and evidence of effect matter more for schools and JST.
4. **Supply side only.** Catalogues list solutions, but they do not show which institutions need them now.
5. **No follow-up.** No found database records what happened after the entry: adoption elsewhere, scale-up or stop. OPSI says that diffusion must be "a practice designed into all phases of the innovation lifecycle" ([OPSI](https://oecd-opsi.org/wp-content/uploads/2019/02/Diffusing_Lessons_LifecycleReport6_FINAL.pdf)).
6. **Keyword duplicate checks.** The documented market baseline is keyword search, not Polish semantic similarity.
7. **No permissive open tool.** Open platforms use AGPL, and scouting tools are closed and expensive.

### What to build so that the solution is original, useful and deployable

Each differentiator below answers one gap and one jury criterion:

- **Demand radar** (gap 4, originality): match BZP buyers to recorded innovations. Example output: "3 gminy bought a similar app in the last 12 months".
- **Duplicate alert before a new project** (gap 6, fit to the challenge): show similar records with a score and shared terms, and offer the path "Połącz z istniejącym rozwiązaniem".
- **"Adapt" as a first-class path** (fit): the Danish data shows that adaptation is the normal case, so replication is a result of equal rank with commercialisation.
- **Paszport innowacji with a status history** (gap 5, deployment): identified, assessed, matched, piloted, adopted elsewhere. This answers task section 9: "co dzieje się z innowacją po jej zidentyfikowaniu".
- **Two-profile rubric with evidence tags** (gap 3, usability): every score shows its source, its confidence and the person who confirmed it.
- **Analyst queue for PCI** (deployment): the broker role from IN-PART and Interreg, as a work list with next steps and dates.
- **Configuration per region** (transferability): region, RIS list, instruments and partners live in data, not in code.
- **Exit-route export** (transferability): an EEN-style offer or request profile and links to EUIP, SIM or Interreg Europe.

The strongest objection is that the jury can see "one more database". The answer is in the output. The main result of the console is a routed next step with an owner, a date and expected measures, not a list entry. HackNation 2025 juries picked the solutions with "największy potencjał do wdrożenia w administracji publicznej" ([gov.pl](https://www.gov.pl/web/cyfryzacja/tysiace-pomyslow-24-godziny-i-jeden-cel--usprawnic-panstwo-w-bydgoszczy-zakonczyl-sie-hacknation-2025)). The same source lists a team "CombAIns" as the 2025 winner of "Zobacz to, czego nie widać" (JSW IT Systems). If this is the same team, use it as a credibility point in the pitch.

## Score the innovation, not the person, to stay out of high-risk AI

The legal frame is manageable if the design makes four choices early. Score the innovation, not the creator. Collect institution-level data from automatic sources. Keep a human decision on every path. Label generated text. This section is a design checklist, not legal advice.

### GDPR: minimum data, separate contacts, care with minors

The GDPR lets a child of 16 or more give consent for an online service, and Poland kept the age of 16 **[VERIFY: secondary sources from 2018]** ([GDPR Art. 8](https://gdpr-text.com/pl/read/article-8/); [podatki.biz](https://www.podatki.biz/artykuly/rodo-w-praktyce-zbieranie-zgody-od-dzieci_34_38423.htm)). PCI supports creators aged 16-30, so some creators are minors. Data from public registers still triggers the Article 14 information duty. In 2019 UODO fined Bisnode Polska **more than PLN 940,000** for register data without notice to the persons ([Rzeczpospolita](https://pro.rp.pl/abc-firmy/art39132691-kara-od-uodo-dla-firmy-za-przetwarzanie-danych-z-rejestrow-publicznych)). The NSA said that the "disproportionate effort" exception must be read narrowly **[VERIFY: sources differ on the WSA outcome]** ([UODO](https://uodo.gov.pl/en/553/1572)). The EDPB lists "evaluation or scoring", vulnerable data subjects and new technology as DPIA criteria; two or more usually mean that a DPIA is necessary ([WP248](https://ec.europa.eu/newsroom/article29/items/611236)). The radar meets at least two. The GDPR part of the Digital Omnibus is still a proposal in November 2026 **[VERIFY]** ([eudigitalpolicy.eu](https://eudigitalpolicy.eu/instruments/data-omnibus/)), so the design must not rely on it.

### AI Act: preparatory scoring of projects is outside Annex III

Annex III makes a system high-risk when it decides access to education, selects persons for jobs or decides eligibility for public benefits ([AI Act Annex III](https://artificialintelligenceact.eu/annex/3/)). Scoring of innovations is not in this list. Article 6(3) excludes Annex III systems that do a preparatory task, but a system "shall always be considered to be high-risk where the AI system performs profiling of natural persons" ([AI Act Art. 6](https://artificialintelligenceact.eu/article/6/)). Thus the radar must not compute features about the creator, and it must not use a score alone to admit young creators to PCI programmes. Article 50 transparency duties apply from 2 August 2026: users must know when they interact with an AI system, and generated text needs a label or human editorial control ([AI Act Art. 50](https://artificialintelligenceact.eu/article/50/)). The Digital Omnibus on AI, Regulation (EU) 2026/1744, moves Annex III high-risk duties to 2 December 2027 **[VERIFY: dates from secondary sources]** ([artificialintelligenceact.eu](https://artificialintelligenceact.eu/ai-act-explorer/digital-omnibus/)). Poland published its AI act as Dz.U. 2026 poz. 1003 **[VERIFY: dates from search summary]** ([ELI](https://eli.gov.pl/eli/DU/2026/1003)).

### Copyright, TDM and open data: links and short extracts, not copies

Poland implemented the DSM text and data mining exceptions in Dz.U. 2024 poz. 1254, in force since 20 September 2024 ([ELI](https://eli.gov.pl/eli/DU/2024/1254)). Article 26³ allows copies of published works for TDM "chyba że uprawniony zastrzegł inaczej". For online works, the reservation must be machine-readable, and copies may stay only as long as necessary ([act text](https://eli.gov.pl/api/acts/DU/2024/1254/text.html)). Whether the final text excludes generative-AI training is not confirmed **[VERIFY]**. Public-sector information falls under the open data act, Dz.U. 2021 poz. 1641 ([ELI](https://eli.gov.pl/eli/DU/2021/1641/ogl)). dane.gov.pl gives the licence of each dataset in machine-readable fields ([dane.gov.pl dataset 13939](https://api.dane.gov.pl/1.4/datasets/13939)). Third-party texts need care: ZMP owns the BDP texts, and reused OPSI text must keep CC BY-SA 3.0 IGO.

### EUPL-1.2, accessibility and security

**Table 5. Legal rules and design responses**

| Area | Rule | Design response |
|---|---|---|
| Personal data | GDPR minimisation; Art. 14 duty for register data | Separate creator contact table; contact shown only after consent; no person names from automatic sources; hide RAD-on inventor names and natural-person FE beneficiaries |
| Minors | GDPR Art. 8; age 16 in Poland [VERIFY] | Year of birth only; no public name or contact under 18; parent consent under 16 |
| DPIA | EDPB criteria (scoring, vulnerable persons, new technology) | One-page DPIA outline in `README.md` |
| AI Act | Annex III, Art. 6(3), Art. 50 | Score the innovation; analyst confirms; "wygenerowane przez AI" label; one paragraph with the Art. 6(3) assessment |
| TDM | Art. 26³ (Dz.U. 2024 poz. 1254) | Check `robots.txt` and `tdm-reservation`; delete raw HTML after extraction; store summary and link |
| Data licences | OPS no redistribution; ORCID non-commercial; ZMP rights | Keep these sources out of the core; titles and links only for BDP |
| EUPL-1.2 | Keep notices; give licence and source; network use counts as distribution; no trademark rights ([EUPL page](https://interoperable-europe.ec.europa.eu/licence/european-union-public-licence-version-12-eupl)) | `LICENSE`, SPDX header `EUPL-1.2`, third-party notices with Apache NOTICE texts; no PCI logo as a grant |
| Accessibility | Dz.U. 2019 poz. 848; WCAG 2.1 AA through EN 301 549; "deklaracja dostępności" ([gov.pl](https://www.gov.pl/web/baza-wiedzy/wcag-na-skroty)) | Semantic HTML, keyboard use, contrast 4.5:1, text alternatives for charts; automatic check before the demo |
| Interoperability | KRI, Dz.U. 2024 poz. 773 ([ELI](https://eli.gov.pl/eli/DU/2024/773)) | JSON and CSV export, documented REST API, UTF-8 |
| Cybersecurity | KSC amendment (NIS2) in force since April 2026; compliance by 3 April 2027 [VERIFY] ([gov.pl](https://www.gov.pl/web/cyfryzacja/nowelizacja-ustawy-o-krajowym-systemie-cyberbezpieczenstwa-zaczyna-obowiazywac)) | Roles, audit log of score changes, secrets only in `.env`, EU hosting, no personal data to external LLM APIs |

## Build a broker console with two traced use cases in 15 hours

### Core product concept

"Radar innowacji" turns a short submission or an open-data signal into a scored "paszport innowacji" with a next step, a named recipient and a date. The product has two faces. Creators and institutions use a short Polish intake form. PCI analysts use a console with a queue, the assessment and the matches. The flow follows the six steps of the repository rules:

1. **Identification.** A person submits a form or pastes a text. A "Radar feed" also proposes candidates from open project data (RAD-on, FE 2021-2027, NCBR).
2. **Record.** The LLM pre-fills the paszport fields with evidence quotes. The creator or the analyst confirms them.
3. **Assessment.** The rule engine scores nine criteria in the T or N profile, with evidence tags, a confidence level and a score range. The similarity panel shows related and duplicate work first.
4. **Path and matches.** The gates and the rules give the top two paths, the instruments, the adopters (BZP buyers, FE beneficiaries) and the partners (RAD-on, Interreg, FENG beneficiaries).
5. **Justification.** A Polish template, filled by data, shows the reasons and the expected measures.
6. **Follow-up.** The status history records each next step and "adopted by N organisations".

### Minimum feature set, in priority order

**P0, necessary for the demo:**

1. Intake form with institution type, innovation type and profile T or N.
2. Paszport view with field values, evidence quotes, sources and dates.
3. Similar-solutions panel with score, shared terms and the duplicate gate.
4. Assessment page: nine criteria, editable values, LLM proposals marked, evidence tags, confidence, score range, criteria version.
5. Path page: top two paths, instruments, adopters and partners with match reasons.
6. Justification block with the measures of the path.
7. Status timeline and a simple analyst queue (roles creator, analyst, partner without a full login).
8. Two seeded use cases that run end to end.

**P1, if time remains:** the Radar feed from open data, a voivodeship map with a static GeoJSON file, JSON and CSV export, an EEN-style profile export and a sensitivity check (does the path change if a weight changes by ±5 points?).

**P2, only after P0 and P1:** URL import with `robots.txt` and TDM checks, and a reranker.

### Use case 1: a university technology (profile T)

Take a real patent record of Politechnika Rzeszowska from RAD-on `products` ([RAD-on products, PRz](https://radon.nauka.gov.pl/opendata/polon/products?resultNumbers=100&institutionName=Politechnika%20Rzeszowska%20im.%20Ignacego%20%C5%81ukasiewicza)). Show the institution, not the inventor names. The demo shows these steps:

1. The record enters through the Radar feed.
2. The LLM proposes the TRL from the text, with a quote.
3. The similar-solutions panel shows related patents and projects of other institutions in RAD-on.
4. The match list shows FENG beneficiaries in the FE list with similar project text as candidate licensees or partners.
5. The RIS tag (for example "Lotnictwo i kosmonautyka") selects PCI instruments.
6. The path is R1 (Komercjalizacja) if the team criterion is 3 or more, else R2 (licence).

### Use case 2: a school innovation (profile N)

Use a fictional "innowacja pedagogiczna (metodyczna)" of a Podkarpackie primary school, as the repository rules require fictional data for this case. It has pre and post results in two classes and materials under CC BY. The demo shows these steps:

1. A teacher submits the short form.
2. The rubric uses SRL, evidence (Nesta level 2) and the five transferability items.
3. The similar-solutions panel shows school projects from the FE list and Kohesio.
4. The adopter list shows schools and gminy in other voivodeships that bought education services (CPV 80000000) or ran similar FE projects. This shows the national reach.
5. The path is R4 (Replikacja) if evidence is 2 or more, else R5 (Pilotaż z ewaluacją).

An optional third case is a JST need without a market product. When two or more JST show the same need in BZP, rule R6 proposes "Zamówienie innowacyjne".

### Data sources to integrate first

1. **FE 2021-2027 list:** download once, cache in `data/cache/`, parse the location, hide natural persons, keep a subset for the demo (Podkarpackie rows plus rows with "innowac" or "szkoł").
2. **RAD-on `products` and `projects`:** Podkarpackie science institutions, 1-2 hours.
3. **BZP notices:** a recent date window and four CPV groups (48000000 software, 72000000 IT services, 80000000 education, 79400000 consulting); drop `htmlBody`.
4. **NCBR success page:** 316 curated records as a seed and a calibration set.
5. **Kohesio:** live search with a timeout and a cache, only if time remains.
6. **Interreg 2021-2027 XLSX:** small and clean; a quick source of cross-border partners.

Each source gets its own module with one function that returns the data and its metadata (source, data date, download date, licence). The effort for RAD-on comes from tests. The other effort values are estimates.

### Scoring rubric draft (criteria version v0.1)

The rubric uses a 0-5 scale for each criterion. "Brak danych" (E0) is a separate value, not 0. The total is S = Σ (weight × value / 5), on a 0-100 scale, as in the Innovation Radar rule. The score range uses 0 and 5 for each unknown criterion. Confidence is the share of weight with tag E2 or E3: high from 70%, medium from 40%, low below 40%.

| ID | Criterion (UI label) | Level 1 | Level 3 | Level 5 | Weight T | Weight N | Source |
|---|---|---|---|---|---|---|---|
| K1 | Potrzeba / problem | Problem stated by the author only | Need confirmed by 3 or more users or institutions | Need in a public strategy, plus demand signals | 10 | 15 | KTH CRL; IR market potential |
| K2 | Nowość | New to the unit only | New in Poland | New to the world | 15 | 10 | Oslo Manual levels [UNVERIFIED]; PARP national minimum |
| K3 | Dojrzałość (T: TRL, N: SRL) | Level 2 | Level 5-6 | Level 9 | 15 | 10 | Dz.U. 2020 poz. 1495; IFD SRL |
| K4 | Dowody efektu | Nesta 1: theory of change | Nesta 3: comparison group | Nesta 5: replicated at 2+ sites with manuals | 5 | 15 | Nesta SoE |
| K5 | Przenaszalność | 1 of 5 items | 3 of 5 items | 5 of 5 items | 5 | 20 | CORRECT, Rogers, CFIR |
| K6 | Zespół i zaangażowanie | One champion, no time assigned | Team with roles and support of the head | Record of transfer and assigned resources | 15 | 10 | KTH TMRL; IR management |
| K7 | Odbiorcy / rynek | Hypothetical users | 3 or more named adopters show interest | Paying customers or 2+ adoptions elsewhere | 15 | 10 | KTH CRL; PCI "walidacja z odbiorcą" |
| K8 | Prawa i otwartość | Ownership clear | T: application filed; N: part under open licence | T: rights support the business; N: open licence, public repository, GDPR and WCAG checked | 10 | 5 | KTH IPRL (own text) |
| K9 | Zasoby i finansowanie | Funding options listed | First funding secured | Sustainable funding or revenue | 10 | 5 | KTH FRL (own text) |

The five transferability items for K5 are:

1. Documentation exists that another institution can follow.
2. The cost to adopt is known and fits a typical school or gmina budget.
3. No special infrastructure or rare skills are necessary.
4. The solution fits the law and the standard processes of the target institution type.
5. A small pilot is possible, and its result is visible in a short time.

Add two tags without points. The RIS tag selects regional instruments from the configuration. The OPSI facet helps the match to JST goals. Map K2, K3, K7 and K9 to the PCI card fields, so that PCI analysts can compare.

**Gates (checked first):**

- G1 Need: K1 below 2 gives only "Doprecyzuj problem".
- G2 Legal: a critical legal flag gives "Konsultacja prawna przed dalszymi krokami".
- G3 Duplicate: K2 = 0 and a more mature similar record give "Połącz z istniejącym rozwiązaniem", with a link.
- G4 Confidence: low confidence gives "Uzupełnij dane", with a path marked "wstępna".

**Table 6. Path rules, instruments and expected measures**

| Rule | Path | Conditions | Instruments (examples) | Expected measures (12 months) |
|---|---|---|---|---|
| R1 | Komercjalizacja – spółka / startup | T; K3, K2, K7, K6 ≥ 3; K8 ≥ 2 | PCI Komercjalizacja; Deeptech Akcelerator; PARP Platformy startowe; EIC Accelerator if K2 and K3 ≥ 4 | Company created; funding (PLN); first customers; K7 +2 |
| R2 | Transfer technologii – licencja | T; K3 ≥ 3; K8 ≥ 3; K6 ≤ 2 | University CTT; EEN technology offer; contract research | Licence talks; licence signed (12-18 months); income |
| R3 | Rozwój B+R / prototyp | K2 ≥ 3; K3 ≤ 2; K1 ≥ 3 | PCI Program Grantowy (scientific units only); ProtoLab; InnoStart Hub | TRL or SRL +1; grant application and result |
| R4 | Replikacja w innej instytucji | K3 ≥ 3; K4 ≥ 2; K5 ≥ 3; K8 ≥ 3 | Replication package; Interreg Europe; Erasmus+ KA2 [UNVERIFIED]; FERS scaling calls; JST networks | Adopter institutions; people reached; effect at the new site; use after 12 months |
| R5 | Pilotaż z ewaluacją | K1 ≥ 3; K4 ≤ 1; K3 ≥ 2 | Pilot in 1-2 partner institutions; FERS social innovation incubators | Pilot started; baseline measured; Nesta level 2 or 3 |
| R6 | Zamówienie innowacyjne | K1 ≥ 4 in 2+ JST; K2 ≥ 3; K3 ≤ 3 | Market consultation; PCP; partnerstwo innowacyjne (art. 189 Pzp) | JST that confirm the need; consultation done; procedure published |
| R7 | Obserwuj i oceń ponownie | No rule matches, or S < 40 | Request for missing data | Data completed; re-assessment in 6 months |

The engine checks all rules and shows the top two with their conditions. The tie-break is the mean margin above the thresholds. All weights and thresholds are a design proposal without validation. Show them as "wersja kryteriów v0.1" and plan a calibration with PCI experts.

For the pitch, use five system KPIs: time to first match, match acceptance rate, replications completed, duplicate projects avoided and share of innovations with TRL or SRL +1 in 12 months.

### Stack cuts for 15 hours

Use the stack in Table 4 with these cuts:

- Use `mmlw-roberta-base` for embeddings, because it is fast on CPU. Change to the large model only if the speed test allows it.
- Pre-extract the two use cases with Bielik-11B. For live intake, test a 4B Apache-2.0 model (Bielik-4.5B, PLLuM-4B-2512 or Qwen3.5-4B) on three records. Keep the fastest model that returns valid JSON.
- Use one SQLite file with FTS5, `sqlite-vec` and RRF. Skip the reranker.
- Use FastAPI, Jinja2 and htmx without a build step.

### A 15-hour plan for two persons

| Hours | Person A: data and search | Person B: rubric, UI and pitch |
|---|---|---|
| 0-1 | Freeze the scope; measure CPU speed of the embedding model and the LLM | Freeze the scope; make the criteria module skeleton |
| 1-5 | Loaders for FE list, RAD-on, BZP and NCBR; SQLite schema; cache | Rubric, gates, rules and measures; intake form; paszport view |
| 5-9 | Embeddings, FTS5, hybrid search, duplicate check, match score | Assessment page, path page, justification template, status queue |
| 9-12 | LLM pre-fill with the quote check; tests with recorded responses | Seed use cases 1 and 2; Polish UI text; accessibility check |
| 12-15 | Integration; slow test; `README.md` (sources, licences, limits, scale-up, security) | Slides (10 or fewer); video (3 minutes or less); demo rehearsal |

### Risks and mitigations

| Risk | Effect | Mitigation |
|---|---|---|
| LLM too slow on CPU | Breaks the 30-second rule and the live demo | Pre-extract; LLM only pre-fills; form fallback |
| Source outage (BZP, Kohesio, RAD-on) | Empty panels during the demo | Cache in `data/cache/`; recorded responses; show "źródło niedostępne" |
| Undocumented Kohesio API | Silent change | Optional source; cache |
| Uncalibrated weights and thresholds | Jury doubts the score | Show the version, the sensitivity check and the calibration plan |
| Personal data in RAD-on and the FE list | GDPR breach | Hide inventor names and natural-person beneficiaries |
| Wrong model licence | Transfer under EUPL blocked | Licence table in `README.md`; Apache-2.0 or MIT weights only |
| Scope creep (crawler, chat, map) | Core flow not finished | P0 first; crawler after the hackathon |
| "One more database" impression | Low originality score | Start the demo with the duplicate alert, the BZP demand match and the next step |
| Wrong duplicate threshold | False gate | Label "podobne" by default; "duplikat" only above a strict threshold |

### Leave for after the hackathon

- A crawler of BIP, school and gmina pages, with `robots.txt`, TDM reservation and institution-level data only.
- RSPO API access through PCI, for school identifiers.
- OpenAlex with a free key, EPO Linked Open EP data, UPRP and CORDIS for expert and patent profiles.
- Erasmus+ project results and procurement plans as more signals.
- A calibration study: two or more PCI experts score 10-20 innovations; compute ICC or Krippendorff's alpha; set weights with AHP.
- A full DPIA, a WCAG audit and a "deklaracja dostępności".
- Full authentication, roles and an audit log for a real deployment.
- One tenant for each region or innovation centre.
- Exports to EEN and EUIP, and alignment of the taxonomy with KIS/RIS and Smart Radar 2.0.
- Reminders for freshness and a measurement of reuse.
- A listing in the EU Open Source Solutions Catalogue after publication under EUPL-1.2.
- "Positive deviance" detection: find institutions with outlier results in BDL statistics, then look for the practice behind them ([THIS Institute](https://eprints.whiterose.ac.uk/193204/1/Baxter%202022%20-%20THIS%20Institute%20The-positive-deviance-approach.pdf)).

## Conclusion

The value of a radar is not the number of records it finds. Its value is the number of records that become a routed and tracked next step. The Danish definition gives the right unit of success: an innovation spreads only when a second organisation implements it. No found Polish or EU database counts this event. Thus a radar that records "adopted by N organisations" is more than a PCI tool. It can become the first Polish measure of public-sector reuse, and it can give regional strategies the indicator that GUS does not collect.

Two combinations make the concept original with modest code. First, "who built it" (FE 2021-2027 and RAD-on) plus "who buys it" (BZP) gives an explainable match that no catalogue offers. Second, the PCI grant card extended with SRL, evidence and transferability turns a familiar R&D rubric into one that also fits a school or a gmina. The legal limits support the same design: a tool that scores innovations and leaves decisions to a person stays deployable in public bodies. A tool that profiles creators does not.
