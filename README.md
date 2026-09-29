# data-wrangling-project
Project for Week 3 of the Ironhack Data Analytics Bootcamp

# Rental Crisis in Portugal: Rents, Wages and Short-Term Rentals

**Team:** TODO (names)
**Bootcamp:** Ironhack Data Analytics, Module 1 Project
**Presentation slides:** TODO (link)
**Kanban board:** TODO (Trello link)

---

## Introduction

Portugal has had one of the sharpest rent increases in Europe over the past decade, especially in Lisbon and Porto.
This project uses official statistics, short-term rental data and web-scraped reference data to test three hypotheses about affordability and short-term rental (STR) pressure.

TODO: 2–3 sentences on why this matters (policy relevance, who is affected).

---

## Research Questions & Hypotheses

| # | Hypothesis | Status |
|---|---|---|
| H1 | Rents on new leases have grown faster than wages since 2017 | TODO: supported / refuted / partially |
| H2 | Municipalities with more STR units per 1,000 residents have higher median rents (€/m²) | TODO |
| H3 | Airbnb supply in Lisbon and Porto is dominated by entire homes run by multi-listing hosts | TODO |

---

## Data Sources

| Source | Collection method | Content | Period / snapshot | Link |
|---|---|---|---|---|
| INE (Statistics Portugal): rent statistics | API (JSON) | Median rent €/m² of new leases, last 12 months, semestral (indicator `0009817` / `0012598`) | H2 2017 – TODO | https://www.ine.pt/xurl/indx/0009817/PT |
| INE: wages | API (JSON) | Average gross monthly pay per job, quarterly (indicator `0011132`) | TODO | https://www.ine.pt/bddXplorer/htdocs/minfo.jsp?var_cd=0011132&lingua=PT |
| Inside Airbnb | Dataset (CSV) | Listings for Lisbon and Porto | Snapshot: TODO (date) | https://insideairbnb.com/get-the-data/ |
| Wikipedia: Municipalities of Portugal | Web scraping (BeautifulSoup) | Population and area per municipality | TODO | TODO |

**Terms of use:** TODO. Confirm and note the terms for each source (INE open data, Inside Airbnb licence, Wikipedia CC BY-SA, robots.txt checked).

---

## Repository Structure

```
data-wrangling-project/
├── data/
│   ├── raw/          # untouched API responses and downloads
│   └── clean/        # cleaned, merged datasets
├── notebooks/
│   └── rental_crisis_portugal.ipynb
├── figures/          # exported charts for slides
└── README.md
```

**How to run:** TODO (Python version, `pip install -r requirements.txt`, run notebook top to bottom).

---

## Methodology

### 1. Data collection
- INE API: one request per indicator, responses cached locally in `data/raw/` to avoid repeat calls.
- TODO: Inside Airbnb download, Wikipedia scraping.

### 2. Data cleaning
TODO: list the techniques actually applied, e.g.:
- Flattening nested JSON into tabular form
- Parsing period labels (semesters/quarters) into dates
- Converting value strings to numeric; handling suppressed values
- Dropping unused columns
- Normalising municipality names for joins
- Removing duplicates and outliers

### 3. Transformation & analysis
- **H1:** wages aggregated to a rolling 4-quarter mean to match the rent series' 12-month window; both series indexed to H2 2017 = 100.
- **H2:** TODO
- **H3:** TODO

### 4. Visualisation
TODO: list the main charts and the libraries used.

---

## Main Findings

TODO: one short paragraph per hypothesis, with the key number.

---

## Limitations

- **Methodology break:** INE introduced a revised rent series ("Metodologia 2026") in June 2026. Old and new values are not directly comparable. TODO: state which series was used and how the break was handled.
- **Different measures:** rent is a *median of new leases per m²*; wages are a *mean across all jobs*. H1 measures pressure on people signing new leases, not on all tenants.
- **Nominal values:** both series are nominal (not inflation-adjusted).
- **Correlation vs causation (H2):** central, touristy municipalities attract both STRs and high rents.
- **Coverage:** INE data covers only declared leases; the informal rental market is not captured.

---

## Further Questions & Next Steps

TODO: e.g. inflation-adjusted comparison, municipality-level affordability, EU comparison via Eurostat, time-series analysis of STR growth.

---

## Links

- Kanban board: TODO
- Slides: TODO
- INE API documentation: https://www.ine.pt/xportal/xmain?xpid=INE&xpgid=ine_api&INST=322751522&xlang=en
- Inside Airbnb: https://insideairbnb.com/get-the-data/
