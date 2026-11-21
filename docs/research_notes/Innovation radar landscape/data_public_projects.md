# Data sources for the innovation radar: public projects, public institutions, good practices and public demand in Poland

Test conditions: all requests ran on 21 November 2026 (evening, Europe/Warsaw) with curl and Python from a Polish home connection. The User-Agent was generic (`Mozilla/5.0 (compatible; research-client/1.0)`). No account, key or form was used. "TESTED" means a real request returned the result in the card. "NOT TESTED" means the claim comes only from documentation or a search result.

## 1. EU-funded and nationally funded projects (supply of innovations)

### Takeaway
The best single source is the national "Lista projektów realizowanych z Funduszy Europejskich w Polsce w latach 2021-2027" on dane.gov.pl: one 41 MB XLSX file, CC0 1.0, updated quarterly, about 36 000 projects with title, description, beneficiary and location down to gmina. Kohesio adds 2014-2020 and 2021-2027 projects through an undocumented but open JSON/CSV API and a public SPARQL endpoint. The NCBR success search is one server-rendered HTML page with about 316 curated success stories, easy to scrape. interreg.gov.pl and mapadotacji.gov.pl did not answer during the test, but the Interreg list is also on dane.gov.pl as XLSX.

### Cited Findings

#### Card 1.1 — Lista projektów FE 2021-2027 (national list of projects) — TESTED
- Content: projects of all 2021-2027 programmes managed in Poland (FENG, FERS, FEPW, FERC, FEnIKS, 16 regional programmes, FEdR fisheries, some Interreg, home affairs funds). — [dane.gov.pl resource 2912686](https://api.dane.gov.pl/1.4/resources/2912686)
- Access: direct file download, no key. File URL: `https://api.dane.gov.pl/media/resources/20261102/Lista_projektow_FE_2021_2027_01112026.xlsx`. Format XLSX, 41 169 972 bytes, data date 2026-11-02, openness score 2. — [dane.gov.pl resource 2912686](https://api.dane.gov.pl/1.4/resources/2912686)
- Licence: CC0 1.0. Update frequency: "Kwartalnie" (quarterly). Dataset title "Lista beneficjentów (projektów) Funduszy Europejskich 2021-2027". — [dane.gov.pl dataset 13939](https://api.dane.gov.pl/1.4/datasets/13939)
- Test result: the download took 72 s. One sheet "Lista projektów 2021-2027". About 36 500 rows (with header rows). The first row is a title row, so the parser must skip it. — [file](https://api.dane.gov.pl/media/resources/20261102/Lista_projektow_FE_2021_2027_01112026.xlsx)
- Columns seen in the data (order as in the file): project title; project description; project number (for example `FEDR.02.03-IP.01-0176/25`); beneficiary name; fund; specific objective; programme; priority; action; total value; co-financing rate; EU co-financing amount; location as text `WOJ.: LUBELSKIE, POW.: janowski, GM.: Potok Wielki`; start date and end date (Excel serial numbers); intervention field. — [file](https://api.dane.gov.pl/media/resources/20261102/Lista_projektow_FE_2021_2027_01112026.xlsx)
- Rows per programme (count of the programme column): FE dla Rybactwa 3 791; FE dla Śląskiego 3 398; FENG 2 811; FE dla Dolnego Śląska 2 308; FE dla Łódzkiego 2 293; FE dla Polski Wschodniej 1 571; FE dla Podkarpacia 1 520; FERS 1 247; FEnIKS 1 242; FERC 134; Interreg NEXT Poland-Ukraine 82; Interreg Polska-Słowacja 80. — [file](https://api.dane.gov.pl/media/resources/20261102/Lista_projektow_FE_2021_2027_01112026.xlsx)
- Podkarpackie coverage: 2 353 rows contain "PODKARPACKIE" in the location; 1 520 rows belong to the regional programme "Fundusze Europejskie dla Podkarpacia 2021-2027". Thus a separate FEP list is not necessary for the prototype. — [file](https://api.dane.gov.pl/media/resources/20261102/Lista_projektow_FE_2021_2027_01112026.xlsx)
- Keyword counts in the full row text: 9 056 rows contain "innowac"; 16 703 rows contain "szko" (this includes "szkolenia", so it overcounts schools). — [file](https://api.dane.gov.pl/media/resources/20261102/Lista_projektow_FE_2021_2027_01112026.xlsx)
- Personal data: some beneficiaries are natural persons (for example fishery beneficiaries with a first and last name in the beneficiary column). — [file](https://api.dane.gov.pl/media/resources/20261102/Lista_projektow_FE_2021_2027_01112026.xlsx)
- A 2014-2020 version exists on gov.pl ("Lista projektów realizowanych z funduszy europejskich 2014-2020 – 30 czerwca 2026 roku"), XLSX of about 59 MB, with title, short description, beneficiary, fund, programme, priority, value, EU co-financing and dates (NOT TESTED, from a search result). — [gov.pl](https://www.gov.pl/web/fundusze-regiony/lista-projektow-realizowanych-z-funduszy-europejskich-2014-2020---30-czerwca-2026-roku)

#### Card 1.2 — Kohesio (European Commission, DG REGIO) — TESTED
- Coverage: EU-wide cohesion policy projects. Live statistics endpoint returned `numberProjects: 2 277 418`, `numberBeneficiaries: 825 181`. — [Kohesio statistics API](https://kohesio.ec.europa.eu/api/queries/statistics)
- Kohesio covers 2014-2020 and adds 2021-2027 projects progressively. — [data.europa.eu data story](https://data.europa.eu/en/publications/datastories/linking-data-kohesio-platform)
- Bulk: the services page says per-country exports in CSV/XLSX and RDF exist and are free to reuse (NOT TESTED, the page is a JavaScript app). — [Kohesio services (DA)](https://kohesio.ec.europa.eu/da/tjenester)
- REST API (undocumented, found in the Angular bundle `main.*.js`, base `/api`): `/api/projects`, `/api/projects/{id}`, `/api/projects/download/{format}`, `/api/beneficiaries`, `/api/queries/{filter}`, `/api/map/search`. No key. — [Kohesio front end](https://kohesio.ec.europa.eu/en/projects)
- Test 1: `GET https://kohesio.ec.europa.eu/api/projects?language=pl&keywords=innowacje%20szko%C5%82a&country=https%3A%2F%2Flinkedopendata.eu%2Fentity%2FQ13&offset=0&limit=2` → HTTP 200 JSON in 1.5 s, `numberResults: 22`. Fields: `link, item, labels, originalLabels, descriptions, originalDescriptions, startTimes, endTimes, euBudgets, totalBudgets, images, copyrightImages, coordinates, objectiveIds, countrycode`. First hit: "Rozwój infrastruktury edukacyjnej Zespołu Szkół Ponadgimnazjalnych nr 5 w Łopusznie…". — [Kohesio API](https://kohesio.ec.europa.eu/api/projects?language=pl&keywords=innowacje%20szko%C5%82a&country=https%3A%2F%2Flinkedopendata.eu%2Fentity%2FQ13&offset=0&limit=2)
- Test 2: `GET https://kohesio.ec.europa.eu/api/projects/download/csv?language=en&keywords=innovation%20school&country=https%3A%2F%2Flinkedopendata.eu%2Fentity%2FQ13` → HTTP 200 `text/csv`, 2.17 MB, 953 rows in 5.8 s. Columns: `ID, PROJECT NAME, TOTAL BUDGET, AMOUNT EU SUPPORT, START DATE, END DATE, COUNTRY, SUMMARY, HIGHLIGHTED`. Polish letters came out as "?" in this English export (example "Gda?sk"). — [Kohesio CSV export](https://kohesio.ec.europa.eu/api/projects/download/csv?language=en&keywords=innovation%20school&country=https%3A%2F%2Flinkedopendata.eu%2Fentity%2FQ13)
- Test 3: `/api/beneficiaries?...&keywords=gmina%20Rzesz%C3%B3w` returned the largest Polish beneficiaries (GDDKiA, BGK) with `numberResults: 57 768`. The keyword did not seem to filter. Fields: `budget, cofinancingRate, euBudget, id, label, numberProjects, link (Wikidata)`. — [Kohesio API](https://kohesio.ec.europa.eu/api/beneficiaries?language=pl&keywords=gmina%20Rzesz%C3%B3w&country=https%3A%2F%2Flinkedopendata.eu%2Fentity%2FQ13&offset=0&limit=2)
- SPARQL: `https://query.linkedopendata.eu/bigdata/namespace/wdq/sparql` (Blazegraph, Wikibase model). Entity Q13 has the English label "Poland" (TESTED). A count of items with `P35 = Q9934` and `P32 = Q13` returned 120 980 in 12.9 s. My reading of P35 as "instance of" and Q9934 as "Kohesio project" is an assumption. — [EU Knowledge Graph SPARQL](https://query.linkedopendata.eu/bigdata/namespace/wdq/sparql)
- Socrata copy of 2014-2020 data: dataset "2014-2020 Kohesio projects", id `557j-pmg8`, CSV/JSON/RDF at `https://cohesiondata.ec.europa.eu/resource/557j-pmg8.csv` (found through the data.europa.eu search API; file NOT downloaded). — [data.europa.eu search API](https://data.europa.eu/api/hub/search/search?q=kohesio&filter=dataset&limit=5)
- Licence: I found no explicit Kohesio data licence. EU-owned content on Commission sites is generally CC BY 4.0 under Commission Decision 2011/833/EU; the application of this rule to Kohesio is an inference. — [data.europa.eu copyright notice](https://data.europa.eu/en/copyright-notice)

#### Card 1.3 — Interreg 2021-2027 projects managed by Poland — TESTED
- `https://interreg.gov.pl/lista-projektow/` did not answer: curl connect timeout after 21 s (HTTP 000), and the WebFetch service got `ECONNREFUSED 89.31.65.142:443`. The site can be down or can block some networks. — [interreg.gov.pl](https://interreg.gov.pl/lista-projektow/)
- Alternative: "Wykaz projektów realizowanych w programach Interreg 2021-2027 zarządzanych przez Polskę", XLSX 356 647 bytes, data date 2025-11-05. File: `https://api.dane.gov.pl/media/resources/20251105/Wykaz_projektow_Interreg_2021_2027_art_49_CPR_05112025.xlsx`. — [dane.gov.pl resource 487769](https://api.dane.gov.pl/1.4/resources/487769)
- Content: 207 projects. Bilingual columns: programme, project number, title, description, start, end, total cost EUR, EU rate, fund, specific objective, lead partner name, postal code, country, voivodeship/region, powiat, gmina, city, lead partner and partners names, intervention field, strategic project flag, above 5 mln EUR flag. Example: PLSK.01.01-IP.01-0002/23 "Od Beskidu Niskiego po Solinę", lead partner Województwo Podkarpackie. — [file](https://api.dane.gov.pl/media/resources/20251105/Wykaz_projektow_Interreg_2021_2027_art_49_CPR_05112025.xlsx)

#### Card 1.4 — keep.eu (Interact, all Interreg) — NOT TESTED
- Manual export: users filter projects, partners or programmes on the web site and export to Excel/CSV. — [keep.eu FAQ](https://keep.eu/faq/api-how-to-access-keep_eu-data-in-open-data-format/)
- API: a key is given by e-mail on request to keep.support@interact.eu, case by case. Levels: programmes, projects, partnerships. Filters: programme IDs, funding period, call status. — [keep.eu FAQ](https://keep.eu/faq/api-how-to-access-keep_eu-data-in-open-data-format/)

#### Card 1.5 — mapadotacji.gov.pl — TESTED (failed)
- No answer from the host during the test (curl HTTP 000 after 21 s; WebFetch `ECONNREFUSED 91.231.242.109:443`). — [mapadotacji.gov.pl](https://mapadotacji.gov.pl/)

#### Card 1.6 — NCBR "Wyszukiwarka sukcesów beneficjentów" — TESTED
- `GET https://www.gov.pl/web/ncbr/wyszukiwarka-sukcesow-beneficjentow` → HTTP 200, 934 KB HTML, 0.26 s. All entries are in the server HTML; no JavaScript call is necessary. — [NCBR success search](https://www.gov.pl/web/ncbr/wyszukiwarka-sukcesow-beneficjentow)
- Count: 316 "Tytuł projektu" labels, 315 "Region:" labels, 170 "Słowa Kluczowe" labels. Labelled fields per entry: Tytuł projektu, Nazwa Beneficjenta/Beneficjentów, Nazwa programu, Konkurs, Wartość projektu, Wartość dofinansowania, Okres realizacji projektu, Region, Słowa Kluczowe, link "Dowiedz się więcej" to a gov.pl detail page. — [NCBR success search](https://www.gov.pl/web/ncbr/wyszukiwarka-sukcesow-beneficjentow)
- Example entries: "Theraply VR – … terapii dzieci SPE z wykorzystaniem technologii wirtualnej rzeczywistości"; "Dostosowanie oferty edukacyjnej Wyższej Szkoły Finansów i Prawa w Bielsku-Białej…" (POWER, Zintegrowane Programy Uczelni, region śląskie). Thus the list contains university and education projects, not only firms. — [NCBR success search](https://www.gov.pl/web/ncbr/wyszukiwarka-sukcesow-beneficjentow)
- gov.pl robots.txt: `User-Agent: * / Allow: /`. — [gov.pl robots.txt](https://www.gov.pl/robots.txt)
- NCBR has a page "Ponowne wykorzystywanie informacji sektora publicznego" in its menu (content NOT read). — [NCBR success search](https://www.gov.pl/web/ncbr/wyszukiwarka-sukcesow-beneficjentow)

#### Card 1.7 — NCBR and PARP lists of funded projects
- The national FE 2021-2027 list (card 1.1) includes FENG (2 811 rows) and FEPW (1 571 rows), the main NCBR and PARP programmes. — [file](https://api.dane.gov.pl/media/resources/20261102/Lista_projektow_FE_2021_2027_01112026.xlsx)
- Per-call ranking lists of FENG are gov.pl attachments (PDF or XLSX), with evaluation scores and recommended funding. — [search result: gov.pl attachments](https://www.gov.pl/attachment/4c194aab-3459-461c-91e5-7e66eb74d395)

#### Card 1.8 — Erasmus+ Project Results Platform — NOT TESTED
- The platform covers all projects of Erasmus+ and Creative Europe 2021-2027 (also earlier periods). Users can download search results as XLS with the "Download results (XLS)" button. — [EC wiki: Searching in Erasmus+ Project results platform](https://wikis.ec.europa.eu/spaces/NAITDOC/pages/121442219/Searching+in+Erasmus+Project+results+platform)
- Public data includes title and project description from the application form. — [Erasmus+ PRP guide for beneficiaries](https://www.erasmusplus.nl/sites/default/files/assets/Downloads/Promotietoolbox/Erasmus%2BProject%20Results%20Platform%20Guide%20for%20Beneficiaries.pdf)
- A request to the search page with a Poland filter returned HTTP 301 (redirect); I did not follow it. I found no documented API. — [Erasmus+ projects](https://erasmus-plus.ec.europa.eu/projects/faqs)

#### Card 1.9 — CORDIS (brief)
- Covered by another researcher. Not tested here.

### Inferences
- For a 20-hour prototype, the FE 2021-2027 XLSX gives national coverage with one download and a CC0 licence. Convert it once to Parquet or SQLite in `data/cache/`. The location string is easy to parse into województwo, powiat and gmina. TERYT codes are not in the file, so join on names.
- Kohesio is the best second source because it adds 2014-2020 projects, multilingual descriptions and coordinates. Use the JSON API for live "similar project" search and the CSV export for bulk. Because the API is undocumented, cache the results and keep a fallback.
- The NCBR page gives a small, curated, high-quality "success" set for demos and for scoring calibration.
- Beneficiary type (school, JST, university) is not a column in the FE list. A simple rule on the beneficiary name ("Gmina", "Powiat", "Szkoła", "Uniwersytet", "Politechnika") plus a KRS/REGON/RSPO join can classify it.
- Treat beneficiaries who are natural persons as personal data: filter them out or show no names.

### Gaps
- I did not confirm a formal licence for Kohesio data or for the Kohesio API.
- interreg.gov.pl and mapadotacji.gov.pl were not reachable; I cannot describe their current export functions.
- I did not find a stable API for the Erasmus+ Project Results Platform. The XLS export needs a manual browser step.
- I did not verify the exact header row of the FE 2021-2027 XLSX (the first row is a title; my column list comes from the data rows).
- I did not test PARP-specific lists or FE dla Podkarpacia regional pages, because the national list already covers them.

## 2. Open government data about institutions (reference data)

### Takeaway
dane.gov.pl, GUS BDL, KRS, RAD-on and ROR answer without any key. RSPO (register of schools) moved to `api.rspo.gov.pl` and now needs Basic Auth credentials; the old open host `api-rspo.men.gov.pl` did not resolve. REGON BIR and the TERYT web service need registration; TERYT files are a free alternative. CEIDG was not confirmed.

### Cited Findings

#### Card 2.1 — dane.gov.pl API — TESTED
- `GET https://api.dane.gov.pl/1.4/datasets?q=projekty%20unijne&per_page=2` → HTTP 200, `application/vnd.api+json` (JSON:API), 0.31 s. `GET https://api.dane.gov.pl/1.4/search?q=innowacje&per_page=2` → HTTP 200 (search over datasets and resources). — [dane.gov.pl API](https://api.dane.gov.pl/1.4/search?q=innowacje&per_page=2)
- Each dataset carries licence fields: `license_name`, `license_condition_default_cc40`, `license_condition_personal_data`, `license_condition_db_or_copyrighted`, `license_condition_custom_description`. Example: "Nauka w Polsce" has `license_name: CC BY 4.0`, `license_condition_default_cc40: true`; dataset 13939 has `CC0 1.0`. — [dataset 13939](https://api.dane.gov.pl/1.4/datasets/13939)
- Resource objects give `file_url`, `download_url`, `format`, `file_size`, `data_date`, `openness_score`. — [resource 2912686](https://api.dane.gov.pl/1.4/resources/2912686)
- Useful hit: "Zestawienie akredytowanych ośrodków innowacji" (2019). — [dane.gov.pl search](https://api.dane.gov.pl/1.4/search?q=innowacje&per_page=2)
- Search "rejestr szkół i placówek" returned 5 377 hits. The RSPO resource on dane.gov.pl is only a link to `https://rspo.gov.pl/` (type website, modified 2023-10-04). — [dane.gov.pl resource 51521](https://api.dane.gov.pl/1.4/resources/51521)

#### Card 2.2 — RSPO (Rejestr Szkół i Placówek Oświatowych) — TESTED
- `https://api-rspo.men.gov.pl/api/placowki/?page=1` → no connection (HTTP 000; the name did not resolve to an address). — [old RSPO API host](https://api-rspo.men.gov.pl/api/placowki/?page=1)
- `GET https://api.rspo.gov.pl/api/placowki/?page=1` with `Accept: application/json` → HTTP 401 `{"status":401,"error":"Unauthorized","path":"/api/placowki/"}`. — [new RSPO API](https://api.rspo.gov.pl/api/placowki/?page=1)
- The API documentation (PDF dated July 2026) sets "AUTHORIZATION Basic Auth" with username and password for the whole collection. Resources include `/api/placowki/:id`, `/api/etap_edukacyjny/`, `/api/gminy/` (filters `powiat.kodTeryt`, `nazwa`, `kodTeryt`), `/api/gminy_rodzaje/`, plus `powiaty`, `miejscowosci`, `status_publiczno_prawny`. Paging: `page`, `pageSize` (default 100). — [RSPO API doc](https://api.rspo.gov.pl/files/RSPO-API-doc.pdf)
- The public search web site `https://rspo.gov.pl/` works (HTTP 200, Angular app). — [RSPO search](https://rspo.gov.pl/)
- RSPO holds identification data of schools (name, type, founding date) and gives each school an RSPO number, by law. — [ustawa o SIO art. 7 (lexlege)](https://lexlege.pl/ustawa-o-systemie-informacji-oswiatowej/art-7/)

#### Card 2.3 — GUS BDL API (Bank Danych Lokalnych) — TESTED
- `GET https://bdl.stat.gov.pl/api/v1/units?level=2&format=json&page-size=2` → HTTP 200 JSON, 16 voivodeships (`totalRecords: 16`), 0.5 s, no key. — [BDL API](https://bdl.stat.gov.pl/api/v1/units?level=2&format=json&page-size=2)
- Anonymous limit seen in response headers: `X-Rate-Limit-Limit: 7d`, `X-Rate-Limit-Remaining: 9992` after a few calls, so about 10 000 requests per 7 days per anonymous client. Shorter windows also exist in the documentation. — [BDL API](https://bdl.stat.gov.pl/api/v1/subjects?format=json&page-size=1)
- A key (header `X-ClientId`) raises the limits; registration needs only an e-mail address. — [GUS API portal: BDL](https://api.stat.gov.pl/Home/BdlApi?lang=en)

#### Card 2.4 — KRS open API (Ministry of Justice) — TESTED
- `GET https://api-krs.ms.gov.pl/api/krs/OdpisAktualny/0000019193?rejestr=P&format=json` → HTTP 200 JSON, 51 KB, 0.23 s, no key. Response contains `odpis.naglowekA` (register, KRS number, date of last entry 08.10.2026) and `dane`. — [KRS API](https://api-krs.ms.gov.pl/api/krs/OdpisAktualny/0000019193?rejestr=P&format=json)
- The API needs a known KRS number; it has no name search (based on the tested method only).

#### Card 2.5 — REGON BIR1 (GUS) — NOT TESTED
- BIR1 is a web service (SOAP) with REGON data. The portal has sections "Rejestracja" and "Instrukcja"; a user key is necessary. — [GUS API portal: REGON](https://api.stat.gov.pl/Home/RegonApi)

#### Card 2.6 — TERYT (GUS) — NOT TESTED
- The TERYT ws1 web service gives TERYT data to public administration and commercial users, free of charge, in XML; the portal has a "Rejestracja" section. — [GUS API portal: TERYT](https://api.stat.gov.pl/Home/TerytApi)
- The RSPO API also exposes TERYT gminy and powiaty, but behind Basic Auth. — [RSPO API doc](https://api.rspo.gov.pl/files/RSPO-API-doc.pdf)

#### Card 2.7 — CEIDG — TESTED (inconclusive)
- `GET https://dane.biznes.gov.pl/api/ceidg/v2/firmy?nip=…` → HTTP 404 "No context-path matches the request URI." The path is not current. — [CEIDG host](https://dane.biznes.gov.pl/api/ceidg/v2/firmy?nip=1234567890)

#### Card 2.8 — ROR (Research Organization Registry) — TESTED
- `GET https://api.ror.org/v2/organizations?query=Rzesz%C3%B3w&filter=country.country_code:PL` → HTTP 200 JSON, 5 results, 0.3 s, no key. First hit: Uniwersytet Rzeszowski, domain `ur.edu.pl`, established 2001, external IDs FundRef, GRID, ISNI, Wikidata Q660907. — [ROR API](https://api.ror.org/v2/organizations?query=Rzesz%C3%B3w&filter=country.country_code:PL)

#### Card 2.9 — RAD-on / POL-on open data (OPI) — TESTED
- `GET https://radon.nauka.gov.pl/opendata/polon/institutions?resultNumbers=1` → HTTP 200 JSON, no key. Fields include `addresses` (voivodeship, city, street), `regon`, `institutionUuid`, `supervisingInstitutions`. — [RAD-on open data](https://radon.nauka.gov.pl/opendata/polon/institutions?resultNumbers=1)

### Inferences
- Use BDL units (or TERYT files) as the backbone for regions and gminy, KRS + RAD-on + ROR for universities and organisations, and the BIP entity list (section 5) for all public bodies.
- RSPO needs credentials now. For a 20-hour build, ask the organisers (PCI) whether they have RSPO API access, or use the RSPO web search manually for a demo. Do not plan the core flow on RSPO.
- BDL gives context data for matching demand (population, budget per gmina), with no key at low volume.

### Gaps
- I did not find how to get RSPO Basic Auth credentials (who is eligible, what the terms are).
- I did not confirm CEIDG API v2/v3 paths or token rules.
- I did not confirm the ROR data licence in this session (ROR states CC0 in its documentation, from prior knowledge; verify).
- I did not test the REGON BIR test environment.

## 3. Public demand signals (who needs a solution)

### Takeaway
The BZP notice API on e-Zamówienia is open (no key, JSON) and gives each contracting authority's name, NIP, city, voivodeship code, CPV codes and subject. TED Search API v3 is anonymous and gives above-threshold notices. Together they show which schools, gminy and other bodies buy a given type of solution. Procurement plans (plany postępowań) are a strong early signal by law, but I did not find an open API path for them. Baza Konkurencyjności has only an undocumented internal API.

### Cited Findings

#### Card 3.1 — BZP (Biuletyn Zamówień Publicznych) on e-Zamówienia — TESTED
- The API rules (Regulamin korzystania z API, UZP order 20/2022) say: the BZP WebService to download national notices published in BZP needs no access application, is free, and is at `https://ezamowienia.gov.pl/mo-client-board/api/notices/`. Other API services (sending notices, plans, CRD) need an application and OAuth 2.0 Client Credentials. — [Regulamin korzystania z API](https://media.ezamowienia.gov.pl/pod/2023/01/Regulamin-korzystania-z-API.pdf)
- The documented address returned only the HTML shell of the board app. The working JSON endpoint is `https://ezamowienia.gov.pl/mo-board/api/v1/notice`. — [BZP board config](https://ezamowienia.gov.pl/mo-board/api/v1/Config)
- Test: `GET https://ezamowienia.gov.pl/mo-board/api/v1/notice?NoticeType=ContractNotice&PublicationDateFrom=2026-11-19&PublicationDateTo=2026-11-20&PageSize=2` → HTTP 200 JSON array, 2 items, 0.74 s (84 KB because each item includes the full HTML body). — [BZP API](https://ezamowienia.gov.pl/mo-board/api/v1/notice?NoticeType=ContractNotice&PublicationDateFrom=2026-11-19&PublicationDateTo=2026-11-20&PageSize=2)
- Fields: `clientType, orderType, tenderType, noticeType, noticeNumber, bzpNumber, isTenderAmountBelowEU, publicationDate, orderObject, cpvCode, submittingOffersDate, procedureResult, organizationName, organizationCity, organizationProvince (e.g. PL12), organizationCountry, organizationNationalId (NIP), organizationId, tenderId (OCDS id), contractors, objectId, htmlBody`. First item: "Sukcesywna dostawa artykułów żywnościowych dla Szkoły Podstawowej nr 2 … w Krzeszowicach", buyer Szkoła Podstawowa Nr 2 w Krzeszowicach. — [BZP API](https://ezamowienia.gov.pl/mo-board/api/v1/notice?NoticeType=ContractNotice&PublicationDateFrom=2026-11-19&PublicationDateTo=2026-11-20&PageSize=2)
- `NoticeType=TenderResultNotice` also works (results with contractors). `NoticeType=TenderPlanNotice` → HTTP 400 "ma zakres wartości, który nie obejmuje TenderPlanNotice". — [BZP API](https://ezamowienia.gov.pl/mo-board/api/v1/notice?NoticeType=TenderResultNotice&PublicationDateFrom=2026-11-19&PublicationDateTo=2026-11-19&PageSize=1)
- Notice types found in the front end include ContractNotice, SmallContractNotice, TenderResultNotice, CompetitionNotice, AgreementIntentionNotice, ContractPerformingNotice, NoticeUpdateNotice, TenderPlanNotice. — [BZP front end](https://ezamowienia.gov.pl/mo-client-board/)
- Since 1 January 2021, contracting authorities must publish their procurement plans in BZP and on their own web site. — [UZP: Plan postępowań w nowej ustawie Pzp](https://www.uzp.gov.pl/__data/assets/pdf_file/0013/50341/Plan_postepowan_okreslony_w_nowej_ustawie_Pzp.pdf)
- The BZP search covers all notices and procurement plans, with filters such as CPV code and buyer type (manual, user guide). — [BZP user guide 5.1](https://media.ezamowienia.gov.pl/pod/2020/12/BZP-5.1.pdf)
- No rate limit is published for the WebService. The rules allow the administrator to block access that loads the system and threatens security (§3.3). — [Regulamin korzystania z API](https://media.ezamowienia.gov.pl/pod/2023/01/Regulamin-korzystania-z-API.pdf)

#### Card 3.2 — TED Search API v3 — TESTED
- `POST https://api.ted.europa.eu/v3/notices/search` with JSON `{"query":"buyer-country=POL AND classification-cpv=72000000 AND publication-date>=20261101","fields":["publication-number","notice-title","buyer-name","publication-date"],"limit":2,"page":1}` → HTTP 200 in 0.21 s, `totalNoticeCount: 410`. Hits: Agencja Mienia Wojskowego (web site design service); Zakład Gospodarki Komunalnej ("Dostawa i wdrożenie oprogramowania… w ramach realizacji projektu 'Cyberbezpieczny wod…'"). — [TED API](https://api.ted.europa.eu/v3/notices/search)
- The Search API needs no key; only operations on unpublished notices need a key. — [TED API docs](https://docs.ted.europa.eu/api/latest/index.html); [TED developers' corner](https://ted.europa.eu/en/simap/developers-corner-for-reusers)
- No numeric rate limit is published; the Publications Office tells users to respect the limits. — [OP Q&A](https://op.europa.eu/en/web/ted-reusers-workshops/questions-and-answers2)
- Licence: only third-party pages claim CC BY 4.0 for content and CC0 for metadata; not confirmed on an official page. — [Apify TED scraper](https://apify.com/ivladimir/ted-eu-tenders.md)

#### Card 3.3 — Baza Konkurencyjności (EU-funds beneficiaries' purchases) — TESTED (no public API)
- The site is a React app with Keycloak login (`KEYCLOAK_URL: https://id.funduszeeuropejskie.gov.pl`, client `public`). — [BK config.js](https://bazakonkurencyjnosci.funduszeeuropejskie.gov.pl/config.js)
- The bundle uses internal paths such as `/api/announcements/search`, `/api/dictionaries/cpv`, `/api/dictionaries/teryt`. `GET /api/announcements/search?page=0&limit=1` → HTTP 500; `POST` with `{}` → HTTP 405. No documentation found. — [Baza Konkurencyjności](https://bazakonkurencyjnosci.funduszeeuropejskie.gov.pl/)

#### Card 3.4 — JST strategy documents
- I found no central register of JST development strategies. The Obserwatorium Polityki Miejskiej publishes about 2 874 documents (sitemaps `opm_document-sitemap*.xml` with 1 001 + 1 000 + 873 entries) with document types such as "raport" and "raporty-i-analizy". — [OPM sitemap index](https://obserwatorium.miasta.pl/sitemap_index.xml)

### Inferences
- BZP is the most practical demand signal: filter by CPV groups (for example 48000000 software, 72000000 IT services, 80000000 education services, 79400000 consulting) and by buyer type, then match the buyer NIP or name to institutions. A school or gmina that buys a similar thing is a candidate "user" of an innovation.
- For matching, the `orderObject` text plus CPV gives enough material for embeddings. Drop `htmlBody` to keep storage small.
- TED adds only large purchases; for schools and small gminy, BZP matters more.
- Procurement plans would give earlier signals, but without an open API they need manual export or a scrape of the board UI. Leave them for after the hackathon.

### Gaps
- I did not find the API path for procurement plans (plany postępowań).
- I did not find full parameter documentation for `/mo-board/api/v1/notice` (CPV filter, buyer filter, maximum page size). The swagger attachment on gov.pl was not read because of a session limit.
- I did not confirm a TED data licence on an official page.

## 4. Good-practice and social-innovation sources

### Takeaway
The ZMP "Baza Dobrych Praktyk" (dobrepraktyki.pl) is the best curated source of JST good practices; it is easy to scrape (robots allow all) but all rights belong to ZMP. The Obserwatorium Polityki Miejskiej has thousands of documents but no structured good-practice database. I found no central, exportable database of social innovations from the POWER/FERS incubators, no national register of "innowacje pedagogiczne", and no structured GovTech Polska results database.

### Cited Findings

#### Card 4.1 — ZMP Baza Dobrych Praktyk (dobrepraktyki.pl) — TESTED
- `https://www.dobrepraktyki.pl/` → HTTP 200, redirects to `/projekt/smup/` ("System Monitorowania Usług Publicznych"). Practice pages follow `https://www.dobrepraktyki.pl/praktyka/{id},{slug}.html` (ids up to at least 401 seen). Lists: `/projekt/smup/lista?ability={id}`. Archive: `https://archiwum.dobrepraktyki.pl` (returned HTTP 200 with an empty body). — [dobrepraktyki.pl](https://www.dobrepraktyki.pl/)
- Example: "Podatkowy robot" — automation of property tax information IN-1 from ePUAP into the domain system. Categories in the menu: Finanse JST, Edukacja, Kultura i rekreacja, Polityka społeczna, Ochrona środowiska, Transport, Zarządzanie danymi and others. — [practice 401](https://www.dobrepraktyki.pl/praktyka/401,podatkowy-robot.html)
- robots.txt: `User-agent: * / Disallow:` (all allowed). — [robots.txt](https://www.dobrepraktyki.pl/robots.txt)
- Terms: ZMP (Poznań) runs the service; the author's economic rights to materials put in by the administrator belong to ZMP; no open licence is given. — [Regulamin](https://www.dobrepraktyki.pl/strona/5,regulamin.html)
- A 2022 document describes it as the "Samorządowa Baza Dobrych Praktyk", run by ZMP and national organisations of gminy and powiaty. — [search result: gov.pl attachment](https://www.gov.pl/attachment/809e2e64-a3c9-4fc9-8fa4-3c0dd130c858)

#### Card 4.2 — Obserwatorium Polityki Miejskiej (IRMiR) — TESTED
- WordPress site; robots.txt allows all and lists `sitemap_index.xml`. About 2 874 `opm_document` entries in three sitemaps. — [OPM sitemap index](https://obserwatorium.miasta.pl/sitemap_index.xml)
- OPM publishes good-practice reports such as "Guide to good Urban Lab ideas". — [OPM](https://obserwatorium.miasta.pl/guide-to-good-urban-lab-ideas/)

#### Card 4.3 — Social-innovation incubators (POWER 2014-2020, FERS 2021-2027) — NOT TESTED
- In February 2024 the ministry signed contracts with seven beneficiaries for social-innovation incubators in FERS, about 70 mln zł; POWER had four calls for incubators with more than 114 mln zł. — [gov.pl: Wspieramy innowacje społeczne](https://www.gov.pl/web/fundusze-regiony/wspieramy-innowacje-spoleczne---uruchamiamy-inkubatory-innowacji-spolecznych)
- EPALE describes examples of innovations from Polish incubators, but as blog text, not as a dataset. — [EPALE](https://epale.ec.europa.eu/pl/blog/innowacje-spoleczne-w-polsce-od-pomyslu-do-zmiany)
- The old PO KL product base `www.produktyefs.pl` did not answer (HTTP 000). — [produktyefs.pl](https://www.produktyefs.pl/)
- Incubator projects themselves appear in the FE 2021-2027 list (programme FERS, 1 247 rows). — [FE list](https://api.dane.gov.pl/media/resources/20261102/Lista_projektow_FE_2021_2027_01112026.xlsx)

#### Card 4.4 — Innowacje pedagogiczne — NOT TESTED
- I found no national register. The rules changed in September 2017. — [Portal Oświatowy](https://www.portaloswiatowy.pl/organizacja-pracy-szkoly-inne-zagadnienia/zmiany-w-sprawie-innowacji-pedagogicznej-od-wrzesnia-2017-13909.html)
- A Łódź catalogue lists innovations submitted to the Łódź kurator, partly from before 1 September 2017. — [WCKP Łódź catalogue](https://www.wckp.lodz.pl/sites/default/files/Katalog%20dobrych%20praktyk%20zeszyt%2017.pdf)
- The Ministry publishes a list of voivodeship coordinators for innovation in education. — [gov.pl attachment](https://www.gov.pl/attachment/43dc37a5-0605-4b38-9980-abcf01c9b35d)
- Single schools publish their own "wykaz innowacji pedagogicznych" on their web sites or BIP. — [search result summary; example kuratorium PDF](https://www.kuratorium.waw.pl/pl/kuratorium/patronaty-i-rekomendac/wykaz/r58522,3288-Innowacje-pedagogiczne-na-zajeciach-komputerowych.pdf)

#### Card 4.5 — GovTech Polska — TESTED (no dataset)
- `https://www.gov.pl/web/govtech` → HTTP 200 (gov.pl page with "O nas", "Aktualności"). I found no structured list or export of challenge results. — [GovTech Polska](https://www.gov.pl/web/govtech)

### Inferences
- Use dobrepraktyki.pl as a seed set of "known JST innovations" for similarity search and scoring examples. Store the title, category, URL and a short self-written summary; do not republish full texts without ZMP consent.
- "Hidden" school innovations are mostly on school web sites and BIP pages, not in any database. This supports the "submission + automatic detection" design of the task.

### Gaps
- No exportable, structured source of social innovations from incubators was found.
- No national or Podkarpackie kuratorium list of innowacje pedagogiczne was found; Kuratorium Oświaty w Rzeszowie was not checked directly.
- GovTech challenge results: no dataset found.

## 5. Automatic detection on the open web

### Takeaway
The central BIP home page on gov.pl gives the list of all bodies that must run a BIP, with their BIP addresses, and offers a ZIP download (under 1 MB) of active entities. This list is the best seed for a crawler of public institutions. GDELT DOC API returned only HTTP 429 during the test, and the Common Crawl index returned HTTP 504, so both are unreliable for a 20-hour build.

### Cited Findings
- The BIP home page task is to give the BIP addresses of all obliged entities, their basic contact data and data on BIP editors. It has "Wyszukiwarka oraz spis podmiotów BIP" and "Pobieranie danych": "Informacje o aktywnych podmiotach w BIP – Pobierz plik ZIP, rozmiar: poniżej 1 MB" and an archive ZIP. — [gov.pl/web/bip](https://www.gov.pl/web/bip)
- The download buttons are served by a widget from `https://aplikacje.gov.pl/app/bip`; I did not resolve the direct file URL. — [gov.pl/web/bip](https://www.gov.pl/web/bip)
- Entities with their own BIP must register on the BIP home page; closed entities move to the archive. — [gov.pl: Czym jest BIP](https://www.gov.pl/web/bip/czym-jest-bip)
- The old domain `bip.gov.pl` did not answer (HTTP 000 in 0.09 s). — [bip.gov.pl](https://bip.gov.pl/)
- gov.pl robots.txt allows all paths. — [gov.pl robots.txt](https://www.gov.pl/robots.txt)
- GDELT DOC API: three requests over several minutes returned HTTP 429 with "Please limit requests to one every 5 seconds… All high-traffic users should switch to our ngrams dataset". — [GDELT DOC API](https://api.gdeltproject.org/api/v2/doc/doc?query=innowacja%20sourcecountry:poland&mode=artlist&format=json&maxrecords=3)
- Common Crawl: the newest index is CC-MAIN-2026-43 (5–18 October 2026). CDX queries for `bip.erzeszow.pl/*` and `*.edu.pl` returned HTTP 504 after 10 s. — [Common Crawl collinfo](https://index.commoncrawl.org/collinfo.json)

### Inferences
- Plan for detection: (1) seed list = BIP ZIP + RSPO/RAD-on web addresses; (2) fetch only the news or "aktualności" pages and RSS where present; (3) classify texts with keywords and an LLM; (4) respect robots.txt and a low request rate. A crawl of all of Poland is not possible in 20 hours; demo it on Podkarpackie.
- GDELT focuses on national and international news; local school and gmina news is better found directly on the institutions' sites.

### Gaps
- I did not inspect the BIP ZIP contents (fields, count of entities).
- I did not test robots.txt of a sample of school or gmina sites.
- Legal limits of crawling and the TDM exception are covered by another researcher.

## 6. Re-use law basics

### Takeaway
Public-sector information in Poland can be re-used for commercial and non-commercial purposes under the 2021 open data act; each dataset on dane.gov.pl states its licence and conditions in machine-readable fields.

### Cited Findings
- dane.gov.pl datasets carry fields for the licence and re-use conditions (`license_name`, `license_condition_default_cc40`, `license_condition_personal_data`, `license_condition_db_or_copyrighted`). Example licences seen: CC BY 4.0 and CC0 1.0. — [dane.gov.pl dataset 13939](https://api.dane.gov.pl/1.4/datasets/13939)
- NCBR has a page "Ponowne wykorzystywanie informacji sektora publicznego" in its menu (not read). — [NCBR page](https://www.gov.pl/web/ncbr/wyszukiwarka-sukcesow-beneficjentow)
- EU-owned content on Commission web sites is generally under CC BY 4.0 (Decision 2011/833/EU); logos and trade marks are excluded. — [data.europa.eu copyright notice](https://data.europa.eu/en/copyright-notice)

### Inferences
- The act is "ustawa z dnia 11 sierpnia 2021 r. o otwartych danych i ponownym wykorzystywaniu informacji sektora publicznego" (Dz.U. 2021 poz. 1641), which implements Directive (EU) 2019/1024 (from background knowledge, not fetched in this session; see [ISAP](https://isap.sejm.gov.pl/isap.nsf/DocDetails.xsp?id=WDU20210001641)).
- Content that a private body owns (for example ZMP texts) is not public-sector information under this act.

### Gaps
- I did not read the act text in this session. Another researcher covers the law.

## 7. Ranked recommendation for a 20-hour hackathon

### Takeaway
Build on four "zero-key" sources first: FE 2021-2027 list (supply), Kohesio API (supply and similarity), BZP API (demand) and the BIP entity list plus BDL/KRS/ROR/RAD-on (reference). Add NCBR success stories and ZMP good practices as small curated seed sets. Skip sources that need keys, manual approval or that failed in the test.

### Cited Findings
- Supply, ranked by value per hour of effort:
  1. FE 2021-2027 list, XLSX, CC0, national, quarterly, about 36 000 projects with descriptions and gmina locations. — [dane.gov.pl resource 2912686](https://api.dane.gov.pl/1.4/resources/2912686)
  2. Kohesio JSON API and CSV export, no key, 2014-2027, descriptions and coordinates; licence to confirm. — [Kohesio API](https://kohesio.ec.europa.eu/api/queries/statistics)
  3. NCBR success stories, one HTML page, 316 entries, robots allow. — [NCBR](https://www.gov.pl/web/ncbr/wyszukiwarka-sukcesow-beneficjentow)
  4. Interreg 2021-2027 XLSX (207 projects with partners), small and clean. — [dane.gov.pl resource 487769](https://api.dane.gov.pl/1.4/resources/487769)
  5. ZMP Baza Dobrych Praktyk, scrape titles and links only (copyright ZMP). — [Regulamin](https://www.dobrepraktyki.pl/strona/5,regulamin.html)
  6. Erasmus+ PRP XLS export (manual, for schools and universities). — [EC wiki](https://wikis.ec.europa.eu/spaces/NAITDOC/pages/121442219/Searching+in+Erasmus+Project+results+platform)
- Demand, ranked:
  1. BZP notice API, no key, JSON, buyer NIP + CPV + subject. — [BZP API](https://ezamowienia.gov.pl/mo-board/api/v1/notice?NoticeType=ContractNotice&PublicationDateFrom=2026-11-19&PublicationDateTo=2026-11-20&PageSize=2)
  2. TED Search API, anonymous, above-threshold notices. — [TED API docs](https://docs.ted.europa.eu/api/latest/index.html)
  3. Procurement plans in BZP (legal duty since 2021), no open API found. — [UZP](https://www.uzp.gov.pl/__data/assets/pdf_file/0013/50341/Plan_postepowan_okreslony_w_nowej_ustawie_Pzp.pdf)
  4. Baza Konkurencyjności — internal API only; skip. — [BK](https://bazakonkurencyjnosci.funduszeeuropejskie.gov.pl/)
- Reference data, ranked:
  1. BIP entity list ZIP (all public bodies with BIP URL). — [gov.pl/web/bip](https://www.gov.pl/web/bip)
  2. GUS BDL API units and statistics, no key, about 10 000 requests per week anonymous. — [BDL API](https://bdl.stat.gov.pl/api/v1/units?level=2&format=json&page-size=2)
  3. KRS API (by KRS number), no key. — [KRS API](https://api-krs.ms.gov.pl/api/krs/OdpisAktualny/0000019193?rejestr=P&format=json)
  4. RAD-on and ROR for universities and research institutes, no key. — [RAD-on](https://radon.nauka.gov.pl/opendata/polon/institutions?resultNumbers=1); [ROR](https://api.ror.org/v2/organizations?query=Rzesz%C3%B3w&filter=country.country_code:PL)
  5. RSPO — needs Basic Auth credentials now; use only if PCI can provide access. — [RSPO API doc](https://api.rspo.gov.pl/files/RSPO-API-doc.pdf)
- Sources that need registration or a key: keep.eu API (key by e-mail), RSPO API (Basic Auth), REGON BIR1 (key), TERYT ws1 (registration), BDL (optional key), e-Zamówienia write/plan APIs (application + OAuth). — [keep.eu FAQ](https://keep.eu/faq/api-how-to-access-keep_eu-data-in-open-data-format/); [RSPO API doc](https://api.rspo.gov.pl/files/RSPO-API-doc.pdf); [GUS REGON](https://api.stat.gov.pl/Home/RegonApi); [GUS TERYT](https://api.stat.gov.pl/Home/TerytApi); [Regulamin API](https://media.ezamowienia.gov.pl/pod/2023/01/Regulamin-korzystania-z-API.pdf)
- Sources that failed in the test on 21 November 2026: interreg.gov.pl, mapadotacji.gov.pl, bip.gov.pl (old domain), api-rspo.men.gov.pl, produktyefs.pl (no connection); GDELT (429); Common Crawl CDX (504). — see cards above

### Inferences
- A minimal pipeline: download the FE XLSX once at start (72 s), cache it; call Kohesio and BZP live with timeouts; load the BIP ZIP and BDL units once. This covers all of Poland and gives a Podkarpackie demo subset (2 353 FE rows, Interreg PL-SK and PL-UA projects).
- Matching "creator → user": an FE or Kohesio project of institution A plus BZP notices of institution B with similar CPV or text gives a concrete, explainable match ("B bought X in 2026, A built X with EU funds").
- Matching "creator → partner": Interreg partner lists and Kohesio beneficiaries show who already cooperates across borders.

### Gaps
- No test measured the Kohesio or BZP rate limits; use a cache and low request rates.
- I did not test a full download of Kohesio bulk files for Poland.
