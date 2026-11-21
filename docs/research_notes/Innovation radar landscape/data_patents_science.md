# Patent and science data sources for a Polish innovation radar (state on 2026-11-21)

Scope: data sources that a Python prototype for the task "Radar innowacji" (PCI Rzeszów, HackNation 2026) can use. The prototype finds innovations in Polish public institutions, scores them, finds similar solutions and matches creators with partners.

Test method: all "Tested" lines come from real requests on 2026-11-21 with curl and a generic User-Agent, without an API key and without an account. No account was made. No form was sent. No personal data was sent.

Identifiers found in the tests (use them in code):

| Institution | ROR | OpenAlex | RAD-on institutionUuid |
|---|---|---|---|
| Politechnika Rzeszowska im. Ignacego Łukasiewicza | https://ror.org/056xse072 | https://openalex.org/I111664937 (15,078 works) | 63374761-1ae0-43c0-9fa0-dd7d530ea4ab |
| Uniwersytet Rzeszowski | https://ror.org/03pfsnq21 | not tested | not tested |
| WSIiZ Rzeszów (University of Information Technology and Management) | https://ror.org/01t81sv44 | not tested | not tested |
| Rzeszów School of Engineering and Economics | https://ror.org/0310y6579 | not tested | not tested |
| WSPiA University of Rzeszów | https://ror.org/03hbved79 | not tested | not tested |

Sources of the table: [ROR query "Rzeszow"](https://api.ror.org/v2/organizations?query=Rzeszow), [OpenAlex singleton](https://api.openalex.org/institutions/ror:056xse072?select=id,display_name,works_count), [RAD-on products for PRz](https://radon.nauka.gov.pl/opendata/polon/products?resultNumbers=2&institutionName=Politechnika%20Rzeszowska%20im.%20Ignacego%20%C5%81ukasiewicza).

## UPRP (Polish Patent Office): API, export, open data sets, terms

### Takeaway
UPRP has a real JSON REST API for e-Wyszukiwarka, but it needs an e-Profil account, an API key and a JWT token, so it is not usable without registration. The quick path is the UPRP bulk CSV "Lista Wynalazków" on dane.gov.pl (CC BY 4.0, 139,381 Polish invention applications, filing dates to 2018-11-15). The e-Wyszukiwarka web page has Imperva Incapsula bot protection, so do not scrape it.

### Cited Findings
- Integration card, e-Wyszukiwarka API:
  - Content: data of industrial property rights (PWP): inventions, utility models, European patents validated in PL, SPC, trademarks, designs, topographies, geographical indications — [dane.gov.pl dataset 3477 "Zbiory danych PWP - API"](https://api.dane.gov.pl/1.4/datasets/3477)
  - Access: JSON REST API. Functions: PWP changed since a date (`/api/external/pwps-changed-since/<yyyy-mm-dd>`), one PWP by application number (`/api/external/pwp/<number>`), search by criteria (`POST /api/external/determenistic-search`, then `GET /api/external/search-results/{criteriaId}`), bibliographic data by BUP/WUP number or date (`/api/external/search/bibliographics-results`), lists of attributes, collections and PWP types — [UPRP API guide (PDF, 28 pages)](https://api.uprp.gov.pl/assets/instrukcje/PROD/e-Wyszukiwarka.pdf)
  - Endpoint documentation (Swagger): https://api.uprp.gov.pl/ewyszukiwarka/api/doc/external and https://api.uprp.gov.pl/doc/ewyszukiwarka — [UPRP API guide](https://api.uprp.gov.pl/assets/instrukcje/PROD/e-Wyszukiwarka.pdf)
  - Authentication: "Dostęp do API uzyskać może każdy użytkownik posiadający konto e-Profil." Steps: generate an API key in e-Profil (https://eprofil.pue.uprp.gov.pl/dashboard/account-settings/access-tokens), get a JWT from `/api/external/auth/token` of the CMUiA module (https://api.uprp.gov.pl/doc/cmuia), send `Authorization: Bearer <JWT>` — [UPRP API guide](https://api.uprp.gov.pl/assets/instrukcje/PROD/e-Wyszukiwarka.pdf)
  - Tested: https://api.uprp.gov.pl/ returns an Angular page "PUEUP API Zewnętrzne" (HTTP 200); Swagger pages answer (HTTP 200). Data calls were not tested because they need a token — [api.uprp.gov.pl](https://api.uprp.gov.pl/)
  - A second service (api2.uprp.gov.pl) exists since 2023 for selected IP addresses without a token; UPRP publishes only a link to the web service on dane.gov.pl, not files — [WIPO CWS Annual Technical Report 2023, Poland](https://confluence.wipo.int/confluence/x/oYETXQ)
- Tested: the web UI https://ewyszukiwarka.pue.uprp.gov.pl/search/simple-search returns a 212-byte page with `/_Incapsula_Resource` script and `noindex,nofollow` (Imperva bot protection) — [e-Wyszukiwarka](https://ewyszukiwarka.pue.uprp.gov.pl/search/simple-search)
- Integration card, UPRP open data on dane.gov.pl:
  - UPRP (dane.gov.pl institution 19) has 22 datasets. Examples: "Lista Wynalazków" (744), "Lista Wzorów Użytkowych" (464), "Lista Wzorów Przemysłowych" (697), statistics "Wynalazki i wzory użytkowe zgłoszone przez podmioty krajowe według województw" (2117), "Patenty i prawa ochronne ... według województw" (2124), "Raport roczny UPRP" (2077). Most have license CC BY 4.0; dataset 3477 (API link) and 2798 have CC0 1.0. Last modification of most: 2024-03-19 — [dane.gov.pl API, UPRP datasets](https://api.dane.gov.pl/1.4/institutions/19/datasets?per_page=50)
  - The newest "Lista Wynalazków" resource is a ZIP of 2019-04-09 (14.7 MB; CSV 49 MB, UTF-8, `;` separator) — [dataset 744 resources](https://api.dane.gov.pl/1.4/datasets/744/resources?per_page=5); [ZIP file](https://www.dane.gov.pl/media/resources/20190409/2019_04_09_listaWynalazkow.zip)
  - Tested: CSV columns are `Numer_zgloszenia; Numer_prawa_wylacznego; Data_pierwszenstwa; Data_zgloszenia; Numer_BUP; Data_Publikacji_BUP; Numer_WUP; Data_Publikacji_WUP; Tytul; Uprawnieni; Miasto_uprawnionego; Kraj_uprawnionego; Tworcy; Miasto_tworcy; Kraj_tworcy; Pelnomocnicy; Klasyfikacja_MKP; Status_sprawy` — [ZIP file](https://www.dane.gov.pl/media/resources/20190409/2019_04_09_listaWynalazkow.zip)
  - Tested: 139,381 rows; first filing date 1989-01-02; last filing date 2018-11-15; 1,162 rows with right holder city "Rzeszów"; 400 rows with "Politechnika Rzeszowska" and 38 rows with "Uniwersytet Rzeszowski" as right holder — [ZIP file](https://www.dane.gov.pl/media/resources/20190409/2019_04_09_listaWynalazkow.zip)
  - dane.gov.pl has a public JSON:API (`https://api.dane.gov.pl/1.4/datasets?q=...`), no key needed. Tested: query "patent" gives 338 datasets — [dane.gov.pl API](https://api.dane.gov.pl/1.4/datasets?q=patent&per_page=10)

### Inferences
- For the hackathon, load the UPRP CSV once into a local table. It gives title, applicants, inventors, city and IPC for 30 years of Polish inventions. Use the city and the applicant name to find Podkarpackie institutions. Write in the report that the data stops at 2018-11-15.
- Use RAD-on (see the PBN/RAD-on section) for university patents after 2018, because the UPRP API needs an e-Profil account.
- An e-Profil account is probably tied to a natural person (login with a Polish e-ID). Thus a public tool must not ship a personal token. This is an inference, not confirmed.

### Gaps
- No terms of use, rate limits or data license for the e-Wyszukiwarka API were found. The guide does not state them.
- It is not known if a newer bulk file than 2019-04-09 exists for "Lista Wynalazków".
- The access rules for api2.uprp.gov.pl (who gets IP whitelisting) were not found.

## EPO: OPS, Register, Espacenet, Linked Open EP data; other global patent sources

### Takeaway
EPO Linked Open EP data is the best patent source for a hackathon: free, no registration, CC BY 4.0, SPARQL and JSON, with applicant and inventor names and country code. An exact-name SPARQL query for "Politechnika Rzeszowska im. Ignacego Lukasiewicza" answered in 0.17 s. OPS needs registration (anonymous calls get HTTP 403). Espacenet and the EP Register forbid robots. PATENTSCOPE web service, Lens and Google BigQuery all need an account or a payment.

### Cited Findings
- Integration card, OPS (Open Patent Services):
  - Free up to 4 GB per week; above that a yearly flat rate applies (EUR 2,800 in the price list according to a search summary, not checked on the page) — [EPO fair use charter](https://epo.org/en/service-support/ordering/fair-use); [EPO price list](https://epo.org/pricelist)
  - OPS and EPS: "the maximum traffic volume allowed is approximately 1 Mbit per second" — [EPO fair use charter](https://epo.org/en/service-support/ordering/fair-use)
  - Registration is necessary, also for tests — [EPO FAQ: why registration](https://www.epo.org/en/service-support/faq/searching-patents/open-patent-services/ops-v32/why-registration-needed-ops-32)
  - Tested: anonymous `GET https://ops.epo.org/3.2/rest-services/published-data/search?q=pa%3DRzeszow` returns HTTP 403 "This request has been rejected due to the violation of Fair Use policy" — [OPS endpoint](https://ops.epo.org/3.2/rest-services/published-data/search?q=pa%3DRzeszow)
  - Terms: users "may use and include these data in their own machine-readable databases, products and services" (3.1), but "are not permitted to make the data as such available to the public or to copy and/or distribute the data as such" (3.2). No royalties on products (4.7). Credentials must stay confidential (5.5). German law (10.1) — [OPS terms and conditions](https://www.epo.org/en/service-support/ordering/ops-terms-and-conditions)
  - Python client: `python-epo-ops-client` 4.2.3, license Apache-2.0 — [PyPI](https://pypi.org/project/python-epo-ops-client/)
- Espacenet and European Patent Register (web): "The threshold for individual users (IP addresses) is 10 search-related actions per minute." "Automated searches (robots) are not supported and may result in access being denied." Use OPS or EPS REST interfaces for automated access — [EPO fair use charter](https://epo.org/en/service-support/ordering/fair-use)
- Integration card, EPO Linked Open EP data:
  - Free service with a fair use policy; license Creative Commons Attribution 4.0; the data browser, linked data API and SPARQL endpoint are for "occasional use"; for production use, download the data — [EPO linked open data page](https://www.epo.org/en/searching-for-patents/data/linked-open-data); [user guide PDF](https://link.epo.org/web/linked_open_ep_data_user_guide_en.pdf); [coverage PDF](https://link.epo.org/web/coverage_of_linked_open_ep_data_en.pdf)
  - SPARQL endpoint: `https://data.epo.org/linked-data/query` (POST `query=`, `Accept: application/sparql-results+json`). JSON per publication: `https://data.epo.org/linked-data/data/publication/EP/<number>/<kind>/-.json` — [data.epo.org](https://data.epo.org/linked-data/)
  - Tested: `.../publication/EP/2981385/B1/-.json` returns applicantVC `fn` "Politechnika Rzeszowska im. Ignacego Lukasiewicza" with `countryCode` "PL", inventorVC names (TYCZYNSKI, Piotr; SLIWA, Romana), application EP 14719116 filed 2014-04-01, IPC B23B 51/00 and B23B 51/02, priority PL 403436, publication date 2017-08-02, links to HTML and PDF of the specification — [EP 2981385 B1 JSON](https://data.epo.org/linked-data/data/publication/EP/2981385/B1/-.json)
  - Tested: SPARQL `?vc vcard:fn "Politechnika Rzeszowska im. Ignacego Lukasiewicza" . ?pub pat:applicantVC ?vc` returns PRz publications (for example EP 2886569 A1/B1, EP 2910526 A2/A3/B1, EP 3091166 A1) in 0.17 s. Prefixes: `vcard: <http://www.w3.org/2006/vcard/ns#>`, `pat: <http://data.epo.org/linked-data/def/patent/>` — [SPARQL endpoint](https://data.epo.org/linked-data/query)
  - Tested: aggregate queries (GROUP BY over all publications, `CONTAINS` over all names) time out after 60 s — [SPARQL endpoint](https://data.epo.org/linked-data/query)
  - Address data has only `countryCode` in the tested record, no city — [EP 2981385 B1 JSON](https://data.epo.org/linked-data/data/publication/EP/2981385/B1/-.json)
- WIPO PATENTSCOPE: programmatic access is a SOAP/Java web service sold as a data product (subscription; the price of the web service tier was not clear in the search result; 50% discount for not-for-profit organisations was valid in 2012) — [PATENTSCOPE data products](https://www.wipo.int/en/web/patentscope/data); [subscription page](https://www.wipo.int/en/web/patentscope/data/subscribe-pct-data-products-services); [WIPO news 2012](https://www.wipo.int/patentscope/en/news/pctdb/2012/news_0009.html)
- Google Patents Public Datasets (BigQuery): data "by IFI CLAIMS Patent Services and Google, CC BY 4.0"; first 1 TB processed per month is free; needs a Google Cloud project — [Google Cloud blog 2017](https://cloud.google.com/blog/products/gcp/google-patents-public-datasets-connecting-public-paid-and-private-patent-data); [BigQuery public datasets](https://docs.cloud.google.com/bigquery/public-data); [google/patents-public-data](https://github.com/google/patents-public-data)
- Lens.org: API access must be requested from a Lens user profile; scholarly API applicants give full name, organisation address, official e-mail and the intended use; users must show attribution and the Lens logo — [Lens API documentation](https://docs.api.lens.org/)
- Lens ownership: Cambia sold The Lens to The Lens Limited (a SPIE subsidiary) on 1 June 2026 (only Wikipedia; not confirmed by a primary source) — [Wikipedia: The Lens](https://en.wikipedia.org/wiki/The_Lens)
- Lens free tier: one institutional plan example is 5,000 requests per month and 10 requests per minute (KAUST library page; not an official Lens page) — [KAUST library](https://library.kaust.edu.sa/apis/lens)

### Inferences
- Use EPO Linked Open EP data with exact applicant names. Take the names from RAD-on or UPRP data, then query SPARQL. Do not use free-text `CONTAINS` filters on the endpoint.
- Do not show raw OPS data in a public EUPL tool (OPS terms 3.2). Linked Open EP data under CC BY 4.0 has no such limit; give the attribution "Provided by the European Patent Office".
- Filtering by applicant region (Rzeszów) is not possible in EPO data, because the tested address has only the country code. Filter by applicant name or by a list of institutions.

### Gaps
- PatentsView (USPTO) was not checked in this session. It covers only US patents, thus it has low value for Polish public institutions.
- The exact current text of the EPO Linked Open EP data terms and the bulk download location were not read (the documentation pages under data.epo.org/linked-data/documentation/ return HTTP 404).
- The current Lens free or non-commercial plan and the new owner terms (after June 2026) were not confirmed.
- The price of the PATENTSCOPE web service tier was not confirmed.

## PBN, RAD-on / POL-on, Nauka Polska, MNiSW/OPI data on dane.gov.pl

### Takeaway
RAD-on open data is the single most useful Polish source: a public JSON API without key, CC0 (per dane.gov.pl), with patents of science institutions ("products": 19,032 records, with inventors, ORCID, abstracts), research projects with abstracts and keywords, employees (people of science), publications and institutions with voivodeship. The filter `institutionName` works. PBN API needs an application token from an institution.

### Cited Findings
- Integration card, RAD-on open data API (OPI):
  - Base URL `https://radon.nauka.gov.pl/opendata/polon/<collection>`. Collections that answered HTTP 200 without key: `products` (patents and protection rights), `projects`, `employees`, `publications`, `institutions`. Pagination: `resultNumbers=<n>` and a `token` from `pagination.token`; the response has `pagination.maxCount` — Tested: [products](https://radon.nauka.gov.pl/opendata/polon/products?resultNumbers=1), [projects](https://radon.nauka.gov.pl/opendata/polon/projects?resultNumbers=1), [employees](https://radon.nauka.gov.pl/opendata/polon/employees?resultNumbers=1), [publications](https://radon.nauka.gov.pl/opendata/polon/publications?resultNumbers=1), [institutions](https://radon.nauka.gov.pl/opendata/polon/institutions?resultNumbers=1)
  - Tested: `products` has `maxCount` 19,032 in total. With `institutionName=Politechnika Rzeszowska im. Ignacego Łukasiewicza`, `maxCount` is 600; with `institutionName=Uniwersytet Rzeszowski`, `maxCount` is 125. Other tested parameter names (`relatedInstitutionName`, `applicantName`, `protectionTitle`, `applicationDateFrom`) are ignored (no filter) — [products, PRz](https://radon.nauka.gov.pl/opendata/polon/products?resultNumbers=100&institutionName=Politechnika%20Rzeszowska%20im.%20Ignacego%20%C5%81ukasiewicza)
  - Tested: one product record has these fields: `protectionTitle`, `productDescription` (abstract), `protectionType` (for example "Patent na wynalazek", "Prawo ochronne na wzór użytkowy"), `protectionRegion`, `grantingInstitutionName`, `applicationNumber`, `applicationDate`, `publicationNumber`, `publicationDate`, `applicants` (with institution UUID), `inventors` (names, `relatedOrcid`, disciplines), `dataSource` "POLON", `lastRefresh`. Of 100 PRz records, 93 are Polish patents, 5 are Polish utility models, 1 is an EP patent (EP 2981385), 1 is a GB patent — [products, PRz](https://radon.nauka.gov.pl/opendata/polon/products?resultNumbers=100&institutionName=Politechnika%20Rzeszowska%20im.%20Ignacego%20%C5%81ukasiewicza)
  - Tested: a `projects` record has `projectAbstractPl`, `projectAbstractEn`, `keywords`, `projectStartDate`, `implementingInstitutions` (leader flag), `nationalFunds`, funding call edition — [projects](https://radon.nauka.gov.pl/opendata/polon/projects?resultNumbers=1)
  - Tested: an `employees` record has name, employing institution, academic degrees and declared disciplines (personal data of researchers) — [employees](https://radon.nauka.gov.pl/opendata/polon/employees?resultNumbers=1)
  - Tested: an `institutions` query with `name=Politechnika Rzeszowska` returns the address with `voivodeship` "podkarpackie", city, REGON and `institutionUuid` — [institutions](https://radon.nauka.gov.pl/opendata/polon/institutions?resultNumbers=1&name=Politechnika%20Rzeszowska)
  - License: dane.gov.pl lists "Patenty i prawa ochronne" (dataset 294, source RAD-on, update "Kwartalnie") and "Projekty naukowe oraz patenty" (dataset 1758) with license CC0 1.0 — [dataset 294](https://api.dane.gov.pl/1.4/datasets/294); [dataset 1758](https://api.dane.gov.pl/1.4/datasets/1758)
  - OPI says RAD-on lets users download and use the data free of charge in applications, databases and services — [OPI RAD-on brochure (PDF)](https://opi.org.pl/wp-content/uploads/2021/05/Radon-broszura-web.pdf)
  - The POL-on 2 APIs (`polon2.opi.org.pl/*-api/swagger-ui.html`: institutions, employees, patents, projects, theses) are listed separately; they are the data-entry APIs for institutions, not open data (inference from the list, not tested) — [RAD-on endpoint list](https://radon.nauka.gov.pl/opendata/polon2/endpoints)
- Integration card, PBN (Polska Bibliografia Naukowa) API:
  - The OpenAPI spec "API PBN 2.0", version V2.13, with 79 paths is public — [PBN OpenAPI JSON](https://pbn.nauka.gov.pl/api/v3/api-docs); [Swagger UI](https://pbn.nauka.gov.pl/api/swagger-ui/index.html)
  - Read endpoints include `/v1/search/publications`, `/v1/search/persons`, `/v1/search/institutions` (POST), `/v1/person/orcid/{orcidId}`, `/v1/person/polon/{uid}`, `/v1/institutions/polon/page`, `/v1/publications/doi` — [PBN OpenAPI JSON](https://pbn.nauka.gov.pl/api/v3/api-docs)
  - Tested: `GET /api/v1/institutions/page` and `/api/v1/journals/page` return HTTP 403 "W celu poprawnej autentykacji należy podać poprawny token aplikacji" — [PBN endpoint](https://pbn.nauka.gov.pl/api/v1/institutions/page?size=1)
- Nauka Polska (OPI): Tested: https://nauka-polska.pl/ returns a small page titled "Nauka Polska Redirection" — [nauka-polska.pl](https://nauka-polska.pl/)

### Inferences
- RAD-on `products` is the direct answer to "hidden innovations in universities": each patent has an abstract, inventors with ORCID and the institution. Join it with OpenAlex by ORCID for expert profiles.
- RAD-on `projects` with abstracts and keywords supports similarity search (avoid duplicate work) and partner matching.
- RAD-on `publications` seems to expose PBN data without a token (the record has a PBN-style `objectId`). Thus the prototype does not need the PBN API. This is an inference.
- RAD-on `employees` holds personal data. Show only the work data that the profile needs, and do not store more than necessary (GDPR minimisation).

### Gaps
- No official RAD-on open data documentation page with all filter parameters and rate limits was read (the site is an Angular single-page application). Rate limits are unknown.
- The process to get a PBN application token was not confirmed (probably only for institutions).
- The status of the "Nauka Polska" database (closed, or moved to RAD-on) was not confirmed.
- It is not known if RAD-on covers schools and local governments. It covers science and higher education institutions only (from the content of the records).

## PPM (ppm.edu.pl), Omega-PSIR / Expertus repositories, university OAI-PMH

### Takeaway
PPM is "Polska Platforma Medyczna" (Polish Platform of Medical Research), led by Wrocław Medical University, built on Omega-PSIR (Warsaw University of Technology). It covers medical universities, not Podkarpackie technical schools. The site timed out in all tests and no OAI-PMH URL is registered. Uniwersytet Rzeszowski has a working DSpace OAI-PMH endpoint.

### Cited Findings
- PPM is a CRIS and knowledge base on Omega-PSIR software; each partner medical university runs a local Omega-PSIR and a central database exists; Omega-PSIR is used by about 40 Polish institutions — [PNITT WUM: Polish Medical Platform](https://pnitt.wum.edu.pl/en/node/151); [EUNIS 2022 paper 40 (PDF)](https://www.eunis.org/download/2022/EUNIS_2022_paper_40.pdf); [DRIS record](https://dspacecris.eurocris.org/cris/dris/dris01840)
- OpenDOAR record: name "Polish Platform of Medical Research", organisation Wroclaw Medical University, software "Other (Omega-PSIR)", content: articles, theses, reports, datasets; no OAI-PMH base URL and no policy links — [OpenDOAR 10049](https://opendoar.ac.uk/repository/10049)
- Tested: http://ppm.edu.pl/ redirects (HTTP 302) to https; `https://ppm.edu.pl/index.seam` and `/oai?verb=Identify` time out (no answer in 45–60 s) — [ppm.edu.pl](https://ppm.edu.pl/index.seam)
- Tested: Uniwersytet Rzeszowski DSpace OAI-PMH `https://repozytorium.ur.edu.pl/server/oai/request?verb=Identify` returns HTTP 200 (the old path `/oai/request` returns HTTP 404) — [UR OAI-PMH](https://repozytorium.ur.edu.pl/server/oai/request?verb=Identify)
- Tested: guesses `https://repozytorium.prz.edu.pl/` and `https://bib.prz.edu.pl/` for Politechnika Rzeszowska do not answer — [probe](https://repozytorium.prz.edu.pl/)

### Inferences
- Skip PPM for a Podkarpackie demo. Medical universities are not the main target, and the site does not answer.
- Use the OAI-PMH Python client `sickle` (BSD) for the UR repository and dLibra — [sickle on PyPI](https://pypi.org/project/sickle/)

### Gaps
- The repository or CRIS system of Politechnika Rzeszowska and its OAI-PMH URL were not found.
- It is not confirmed if Omega-PSIR installations expose OAI-PMH by default.

## dLibra (czasopisma.pan.pl), Federacja Bibliotek Cyfrowych, BazTOL

### Takeaway
The PAN journals dLibra has a working OAI-PMH 2.0 endpoint without registration. FBC shows a proof-of-work browser check, so automated access is hard. BazTOL is a small static guide to web resources, not updated since 1 January 2022; it has low value.

### Cited Findings
- Tested: `https://www.czasopisma.pan.pl/dlibra/oai-pmh-repository.xml?verb=Identify` returns repositoryName "PAS Journals", baseURL `https://journals.pan.pl/oai-pmh-repository.xml`, protocol 2.0, earliest datestamp 2016-09-05, admin dlibra-techhelp@man.poznan.pl — [PAN OAI Identify](https://www.czasopisma.pan.pl/dlibra/oai-pmh-repository.xml?verb=Identify)
- Tested: `ListRecords&metadataPrefix=oai_dc` returns records (31 KB first page); `ListSets` returns 101 sets — [PAN OAI ListRecords](https://www.czasopisma.pan.pl/dlibra/oai-pmh-repository.xml?verb=ListRecords&metadataPrefix=oai_dc)
- Tested: https://fbc.pionier.net.pl/ (also `/oai?verb=Identify`) returns "Site is under high load – Verifying Browser" with a JavaScript MD5 proof-of-work puzzle — [FBC](https://fbc.pionier.net.pl/)
- Tested: BazTOL is "Polskie zasoby sieciowe z zakresu nauk technicznych", a subject guide to quality-controlled Polish web resources in technical sciences (about 1,400 entries in 19 subject groups, by the counts on the page), part of the BazTech consortium. The page says: "Od 1 stycznia 2022 r. zasoby Portalu BazTOL nie są aktualizowane." No export or API is visible — [BazTOL](http://baztol.library.put.poznan.pl/baztol_czytelnik/baztol)

### Inferences
- PAN journals give Polish research articles (Dublin Core with authors and affiliations in text). They help topic profiles, but OpenAlex already covers most of these articles with structured affiliations.
- Skip FBC and BazTOL in a 20-hour hackathon.

### Gaps
- The metadata license of PAN journals OAI records was not found.
- An FBC API (if any) and its terms were not found.

## Global open scholarly sources: OpenAlex, OpenAIRE, Crossref, ORCID, CORE, Semantic Scholar, CORDIS, EU Funding & Tenders

### Takeaway
OpenAlex (CC0) is the best global source for experts, affiliations (ROR) and topics, but list queries now need a free API key in practice: the keyless budget per IP was already empty in the test, while singleton lookups still work at zero cost. OpenAIRE Graph (CC BY), Crossref, ORCID, CORE, CORDIS and the EU Funding & Tenders search API all answered without a key. Semantic Scholar refused anonymous calls (HTTP 429) and its license restricts redistribution.

### Cited Findings
- Integration card, OpenAlex:
  - Tested: list query `https://api.openalex.org/institutions?search=...` without key returns HTTP 429 "Insufficient budget. This request has no API key, so it counts against the free daily budget shared by everyone on your network's IP address", `creditsRequired` 10, reset at midnight UTC. The same message offers key creation by e-mail (not used) — [OpenAlex list query](https://api.openalex.org/institutions?search=Rzeszow%20University%20of%20Technology)
  - Tested: singleton `https://api.openalex.org/institutions/ror:056xse072` and `https://api.openalex.org/works/doi:10.4271/2020-01-2206` return HTTP 200 with header `x-ratelimit-cost-usd: 0` — [OpenAlex singleton](https://api.openalex.org/institutions/ror:056xse072?select=id,display_name,works_count)
  - Help page: free key; free key budget USD 1 per day, "10× the keyless budget"; example costs: singleton USD 0, list USD 0.0001, search USD 0.001; rate limit 10 requests/s without key, 30/s with a free key — [OpenAlex authentication help](https://help.openalex.org/api/authentication/)
  - Announcement: "API keys required starting Feb 13" (2026) — [openalex-users group](https://groups.google.com/g/openalex-users/c/rI1GIAySpVQ); the help page says casual use without key is possible. The test shows that both are partly true (keyless budget exists but is small and shared per IP).
  - Data license CC0; full snapshot in the public S3 bucket `openalex` (`aws s3 ... --no-sign-request`), about 300 GB, free public snapshot refreshed quarterly (sources conflict: monthly in the AWS registry) — [OpenAlex snapshot help](https://help.openalex.org/access/snapshot/); [AWS Registry of Open Data](https://registry.opendata.aws/openalex/)
  - Python client `pyalex` 0.21, MIT — [PyPI](https://pypi.org/project/pyalex/)
- Integration card, OpenAIRE Graph API:
  - Tested: `https://api.openaire.eu/graph/v1/organizations?search=Rzeszow` returns 9 organisations; `https://api.openaire.eu/graph/v1/researchProducts?search=Rzeszow` returns 11,088 research products with authors, open access colour, FOS and SDG subjects — [OpenAIRE organizations](https://api.openaire.eu/graph/v1/organizations?search=Rzeszow&pageSize=3); [OpenAIRE research products](https://api.openaire.eu/graph/v1/researchProducts?search=Rzeszow&pageSize=1)
  - Terms: records "can be freely re-used by commercial and non-commercial partners under CC-BY license" with OpenAIRE as acknowledged source; up to 60 requests/hour without authentication, up to 7,200 requests/hour with a personal token (registration needed) — [OpenAIRE Graph API terms](http://graph.openaire.eu/docs/apis/terms/)
  - The "next" docs give 7,200/hour also for unauthenticated calls for a transition period, then 60/hour (conflict with the terms page) — [OpenAIRE authenticated requests (next)](https://graph.openaire.eu/docs/next/apis/authenticated-requests)
- Integration card, Crossref REST API:
  - Tested: `https://api.crossref.org/works?query.affiliation=Rzeszow+University+of+Technology` returns HTTP 200 with author affiliation names; headers in the public pool: `x-rate-limit-limit: 1`, `x-rate-limit-interval: 1s`, `x-concurrency-limit: 1` — [Crossref query](https://api.crossref.org/works?query.affiliation=Rzeszow+University+of+Technology&rows=1&select=DOI,title,author)
  - From 21 July 2026: single-record requests 5/s (public) and 10/s (polite), list requests 1/s (public) and 3/s (polite); polite pool needs `mailto` with an e-mail — [Crossref forum, July 2026](https://community.crossref.org/t/refining-rest-api-limits-for-improved-stability-and-reliability/16137); [Crossref access docs](https://crossref.org/documentation/retrieve-metadata/rest-api/access-and-authentication/)
  - Python client `habanero` 2.9.2, MIT — [PyPI](https://pypi.org/project/habanero/)
- Integration card, ORCID public API:
  - Tested: `https://pub.orcid.org/v3.0/expanded-search/?q=affiliation-org-name:"Rzeszow University of Technology"` returns 115 people with ORCID iD and institution names — [ORCID expanded search](https://pub.orcid.org/v3.0/expanded-search/?q=affiliation-org-name:%22Rzeszow%20University%20of%20Technology%22&rows=2)
  - Anonymous API: 12 requests/s, burst 40/s, 25,000 reads per day per IP; "free for non-commercial use" per the Public APIs Terms of Service — [ORCID API limits FAQ](https://info.orcid.org/faq/what-are-the-api-limits/)
  - Tested: some records are empty in public view (for example `/0000-0002-0193-9867/employments` has no affiliation groups) — [ORCID record](https://pub.orcid.org/v3.0/0000-0002-0193-9867/employments)
- Integration card, CORE API v3:
  - Tested: `https://api.core.ac.uk/v3/search/works/?q=Politechnika%20Rzeszowska` (note the trailing slash) returns HTTP 200 without key; header `x-ratelimit-limit: 10`; field `fullText` is "Not available for public API users" — [CORE search](https://api.core.ac.uk/v3/search/works/?q=Politechnika%20Rzeszowska&limit=1)
- Integration card, Semantic Scholar API:
  - Tested: anonymous paper search returns HTTP 429 "Too Many Requests ... apply for a key" (two tries) — [S2 search](https://api.semanticscholar.org/graph/v1/paper/search?query=Rzeszow+University+of+Technology&limit=1&fields=title,year)
  - License: no right to "repackage, sell, rent, lease, lend, distribute, or sublicense the API"; attribution to "Semantic Scholar" required; data licenses vary (for example CC BY-NC or ODC-BY); API key must not be disclosed; Washington State law — [S2 API license](https://www.semanticscholar.org/product/api/license)
- Integration card, CORDIS:
  - Content license CC BY 4.0 (Commission Decision 2011/833/EU), attribution "European Union, CORDIS — https://cordis.europa.eu — CC BY 4.0"; bulk datasets on data.europa.eu (CSV, JSON, XML, RDF), produced monthly; deliverables of beneficiaries are not under CC BY — [CORDIS llms.txt](https://cordis.europa.eu/llms.txt)
  - Tested: `https://cordis.europa.eu/data/cordis-HORIZONprojects-csv.zip` answers (HTTP 206 for a range request, application/zip) — [CORDIS Horizon Europe CSV](https://cordis.europa.eu/data/cordis-HORIZONprojects-csv.zip)
  - Tested: SPARQL endpoint `https://cordis.europa.eu/datalab/sparql` (Virtuoso) answers a query — [CORDIS SPARQL](https://cordis.europa.eu/datalab/sparql)
- Integration card, EU Funding & Tenders search API:
  - Tested: `POST https://api.tech.ec.europa.eu/search-api/prod/rest/search?apiKey=SEDIA&text=hydrogen&pageSize=1&pageNumber=1` returns HTTP 200 with 30,743 results (calls, FAQ, topics), API version 2.159 — [F&T search API](https://api.tech.ec.europa.eu/search-api/prod/rest/search?apiKey=SEDIA&text=hydrogen&pageSize=1&pageNumber=1)

### Inferences
- The team must get one free OpenAlex key (one team member, with consent) and keep it in `.env`. Without a key, list queries fail when the shared IP budget is empty (for example on hackathon Wi-Fi).
- OpenAlex `works` with `filter=institutions.ror:056xse072` gives authors, ORCID and topics for a PRz expert profile. Join with RAD-on by ORCID.
- CORDIS organisations data gives EU projects with Polish partners. It supports "find a partner or a funder" functions. F&T search API can show open calls that match an innovation topic.
- Do not use Semantic Scholar in the public tool: it needs a key in practice, and its license forbids redistribution of the API.

### Gaps
- The exact free CORE quota (the window of `x-ratelimit-limit: 10`) and CORE terms were not read.
- The official documentation for the F&T search API and the status of the public key "SEDIA" were not read.
- The ORCID Public APIs Terms of Service text was not read; "non-commercial" may matter if KOWR or PCI use the tool in a commercial way.

## Which sources identify Polish public-institution authors, affiliations (ROR) and topics for expert and partner profiles?

### Takeaway
Use ROR IDs as the join key for global sources (OpenAlex, OpenAIRE, Crossref) and RAD-on institution UUIDs plus ORCID for Polish sources. RAD-on gives patents and projects per Polish institution; OpenAlex gives topics and co-authors; EPO Linked Data gives EP patents by exact applicant name.

### Cited Findings
- ROR API v2 works without key; query "Rzeszow" returns 5 organisations (PRz 056xse072, URz 03pfsnq21, WSIiZ 01t81sv44, Rzeszów School of Engineering and Economics 0310y6579, WSPiA 03hbved79) with types and city — [ROR query](https://api.ror.org/v2/organizations?query=Rzeszow)
- OpenAlex maps ROR to its institution ID (PRz = I111664937, 15,078 works) and gives authorships with ORCID — [OpenAlex institution](https://api.openalex.org/institutions/ror:056xse072?select=id,display_name,works_count); [OpenAlex work](https://api.openalex.org/works/doi:10.4271/2020-01-2206?select=id,title,authorships)
- RAD-on patents give inventor ORCID (`relatedOrcid`) and disciplines — [RAD-on products, PRz](https://radon.nauka.gov.pl/opendata/polon/products?resultNumbers=100&institutionName=Politechnika%20Rzeszowska%20im.%20Ignacego%20%C5%81ukasiewicza)
- RAD-on institutions give the voivodeship ("podkarpackie"), so the prototype can list all science institutions in the region — [RAD-on institutions](https://radon.nauka.gov.pl/opendata/polon/institutions?resultNumbers=1&name=Politechnika%20Rzeszowska)
- OpenAIRE organisation records for Rzeszów are partly "pending" with country "UNKNOWN" and no PIDs (for example "University of Rzeszow - Institute of History") — [OpenAIRE organizations](https://api.openaire.eu/graph/v1/organizations?search=Rzeszow&pageSize=3)
- Crossref affiliation data is free text (for example "Rzeszow University of Technology") — [Crossref query](https://api.crossref.org/works?query.affiliation=Rzeszow+University+of+Technology&rows=1&select=DOI,title,author)

### Inferences
- Profile pipeline: RAD-on institutions (podkarpackie) → RAD-on products and projects per institution → inventor ORCID → OpenAlex author (topics, co-authors, works) → EPO Linked Data (EP patents by exact applicant name).
- Schools and local governments are not in these science sources. Another researcher must cover them (for example public procurement or local data).

### Gaps
- ROR IDs for smaller Podkarpackie institutions (Akademia Nauk Stosowanych in Krosno, Przemyśl, Jarosław, Tarnobrzeg, Sanok; research institutes) were not checked.
- The quality of ORCID coverage in RAD-on records (share of inventors with ORCID) was not measured.

## Legal limits on reuse in an EUPL-1.2 public tool

### Takeaway
CC0 and CC BY sources (RAD-on, OpenAlex, OpenAIRE, EPO Linked Data, CORDIS, UPRP dane.gov.pl files) fit an EUPL-1.2 public tool with attribution. OPS, Semantic Scholar, Lens and ORCID have terms that limit redistribution or commercial use; keep them out of the core or use them only behind private keys without republishing raw data.

### Cited Findings
- OPS: no public redistribution of "the data as such" (3.2); embedding in own products is allowed (3.1) — [OPS terms](https://www.epo.org/en/service-support/ordering/ops-terms-and-conditions)
- Espacenet and Register: robots are not supported — [EPO fair use charter](https://epo.org/en/service-support/ordering/fair-use)
- EPO Linked Open EP data: CC BY 4.0; for occasional use; production use should download data — [EPO linked open data](https://www.epo.org/en/searching-for-patents/data/linked-open-data)
- Semantic Scholar: no redistribution or sublicensing of the API; attribution required — [S2 API license](https://www.semanticscholar.org/product/api/license)
- Lens: attribution and logo required; access by request — [Lens API docs](https://docs.api.lens.org/)
- ORCID public API: free for non-commercial use — [ORCID API limits FAQ](https://info.orcid.org/faq/what-are-the-api-limits/)
- OpenAlex: CC0 — [OpenAlex snapshot help](https://help.openalex.org/access/snapshot/)
- OpenAIRE: CC BY with acknowledgement — [OpenAIRE terms](http://graph.openaire.eu/docs/apis/terms/)
- CORDIS: CC BY 4.0 with a fixed attribution string — [CORDIS llms.txt](https://cordis.europa.eu/llms.txt)
- RAD-on datasets on dane.gov.pl: CC0 1.0; UPRP lists: CC BY 4.0 — [dataset 294](https://api.dane.gov.pl/1.4/datasets/294); [UPRP datasets](https://api.dane.gov.pl/1.4/institutions/19/datasets?per_page=50)
- Client libraries: python-epo-ops-client Apache-2.0, pyalex MIT, habanero MIT, sickle BSD — [PyPI python-epo-ops-client](https://pypi.org/project/python-epo-ops-client/); [PyPI pyalex](https://pypi.org/project/pyalex/); [PyPI habanero](https://pypi.org/project/habanero/); [PyPI sickle](https://pypi.org/project/sickle/)

### Inferences
- EUPL-1.2 covers the code. Data licenses apply separately. Record each source with its license and attribution in README.md and in the report footer.
- Keep API keys (OpenAlex, optional OPS) in `.env`; the OPS terms (5.5) and the S2 license forbid sharing credentials.
- Personal data of researchers (names, ORCID, degrees) comes from public registers, but GDPR still applies. Show only data needed for matching and give the source.

### Gaps
- No legal review of GDPR rules for researcher profiles in a public tool was done.
- The Lens terms after the June 2026 ownership change were not read.

## Ranked recommendation: which 3–5 sources give the most value for the least effort in 20 hours?

### Takeaway
Use, in this order: (1) RAD-on open data API, (2) OpenAlex (with one free key, or singletons only), (3) EPO Linked Open EP data, (4) UPRP "Lista Wynalazków" CSV from dane.gov.pl, (5) CORDIS bulk CSV or OpenAIRE Graph for partners and projects. All five are free, legal for a public EUPL tool and tested today.

### Cited Findings
- RAD-on: no key, CC0, JSON, `institutionName` filter, 600 PRz and 125 URz patent records with abstracts, inventors and ORCID — [RAD-on products, PRz](https://radon.nauka.gov.pl/opendata/polon/products?resultNumbers=100&institutionName=Politechnika%20Rzeszowska%20im.%20Ignacego%20%C5%81ukasiewicza); [dataset 294](https://api.dane.gov.pl/1.4/datasets/294)
- OpenAlex: CC0, ROR filter, ORCID, topics; keyless list budget empty in test, singletons free — [OpenAlex help](https://help.openalex.org/api/authentication/); [OpenAlex singleton](https://api.openalex.org/institutions/ror:056xse072?select=id,display_name,works_count)
- EPO Linked Open EP data: CC BY 4.0, no registration, exact-name SPARQL in 0.17 s — [SPARQL endpoint](https://data.epo.org/linked-data/query); [EPO linked open data](https://www.epo.org/en/searching-for-patents/data/linked-open-data)
- UPRP CSV: CC BY 4.0, 139,381 rows (1989-01-02 to 2018-11-15), 1,162 rows with a Rzeszów right holder — [ZIP file](https://www.dane.gov.pl/media/resources/20190409/2019_04_09_listaWynalazkow.zip)
- CORDIS: CC BY 4.0 bulk CSV; OpenAIRE: CC BY API, 60 requests/hour anonymous — [CORDIS llms.txt](https://cordis.europa.eu/llms.txt); [OpenAIRE terms](http://graph.openaire.eu/docs/apis/terms/)

Summary table (effort = hours for one person to get usable data):

| Rank | Source | Access | Key needed | License | Effort | Main use in the radar |
|---|---|---|---|---|---|---|
| 1 | RAD-on open data (`/opendata/polon/products`, `projects`, `institutions`, `employees`) | REST JSON, token pagination | No | CC0 1.0 | 1–2 h | Hidden university patents and projects in Podkarpackie; inventors with ORCID |
| 2 | OpenAlex API (or S3 snapshot) | REST JSON | Free key for list queries | CC0 | 1–2 h | Expert profiles, topics, co-authors, similarity |
| 3 | EPO Linked Open EP data | SPARQL + JSON | No | CC BY 4.0 | 2 h | EP patents of PL applicants, IPC classes, duplicates at EP level |
| 4 | UPRP "Lista Wynalazków" CSV (dane.gov.pl) | One ZIP download | No | CC BY 4.0 | 1 h | 30 years of PL inventions with city, applicants, inventors, IPC (to 2018-11) |
| 5 | CORDIS bulk CSV or OpenAIRE Graph API | ZIP / REST JSON | No | CC BY 4.0 / CC BY | 2 h | EU projects, partner organisations, funding match |

Do not use in the hackathon:

| Source | Reason |
|---|---|
| UPRP e-Wyszukiwarka API | Needs e-Profil account, API key and JWT |
| e-Wyszukiwarka, Espacenet, EP Register web pages | Bot protection (Incapsula) or fair use rule against robots |
| EPO OPS | Registration needed; anonymous calls get HTTP 403; no public redistribution of raw data |
| PBN API | Application token of an institution needed |
| Semantic Scholar | HTTP 429 without key; license forbids redistribution |
| Lens.org | Access by request with personal and organisation data; attribution and logo rules |
| WIPO PATENTSCOPE web service | Paid subscription |
| Google Patents (BigQuery) | Google Cloud account; SQL cost management |
| PPM, FBC, BazTOL | No answer / proof-of-work check / static since 2022 |

### Inferences
- With RAD-on plus OpenAlex, the demo can show for each Podkarpackie institution: patents with abstracts, the inventors, their research topics and similar patents or projects elsewhere in Poland.
- Similarity search can use text embeddings or TF-IDF over RAD-on `productDescription`, project abstracts, UPRP titles and OpenAlex abstracts, plus IPC codes from UPRP and EPO.
- Cache all downloads locally (`data/cache/`) on the first day. This keeps the demo fast and independent of rate limits.

### Gaps
- No source in this list covers schools or local governments. Their innovations need other sources (not in this scope).
- The freshness of RAD-on `products` (field `lastRefresh`, quarterly update per dane.gov.pl) was not verified against UPRP.
