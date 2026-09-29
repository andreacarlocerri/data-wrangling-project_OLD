# Rental crisis in Portugal

Data wrangling group project by **Andrea Carlo Cerri** and **Juliana Therezo**.

Long-term rental prices in Portugal have risen sharply in recent years. In this project we collect, clean and combine official open data to test three hypotheses about the rental crisis.

## Hypotheses

| # | Hypothesis | Owner | Notebook |
|---|---|---|---|
| H1 | Long-term rents have grown faster than wages | Andrea | *(to be added)* |
| H2 | Municipalities with more short-term rentals (Alojamento Local) have higher long-term rental prices | Juliana | `notebooks/h2_short_term_rentals_vs_rent.ipynb` |
| H3 | Long-term rents grew faster around Lisbon and Porto, and in the rest of Portugal, than in the two cities themselves: the crisis is spreading | Juliana | `notebooks/h3_long_term_rent_spreading.ipynb` |

## Data sources

| Data | Source | How we get it | Used in |
|---|---|---|---|
| Long-term rental price: median €/m² per month of new lease contracts, last 12 months (indicator `0014696`) | INE – Statistics Portugal | **API** (JSON) | H2, H3 |
| Resident population by municipality (indicator `0012918`) | INE – Statistics Portugal | **API** (JSON) | H2, H3 |
| Short-term rental registrations (RNAL – Registo Nacional de Alojamento Local) | Turismo de Portugal | **Dataset** (CSV download) | H2 |
| *(H1 wage data – to be added by Andrea)* | | | H1 |

* INE API documentation: https://www.ine.pt/xportal/xmain?xpid=INE&xpgid=ine_api_db
* RNAL dataset: https://dadosabertos.turismodeportugal.pt/datasets/estabelecimentos-de-alojamento-local

**About the €/m² values:** INE uses the lease contracts that landlords declare to the Tax Authority. These are **real signed long-term rental contracts**, not advertised prices, which is why they are lower than prices seen on Idealista or Imovirtual. Example: 10 €/m² × 80 m² flat ≈ 800 € per month.

## Project structure

```
data-wrangling-project/
├── data/
│   ├── raw/          # data as downloaded (INE API responses are saved here, RNAL CSV goes here)
│   └── clean/        # cleaned tables: h2_clean.csv, h3_clean.csv
├── figures/          # charts for the presentation (h2_*.png, h3_*.png)
├── notebooks/        # one Jupyter notebook per hypothesis
└── README.md
```

## How to run

1. Install the libraries:
   ```
   pip install pandas numpy matplotlib requests jupyter
   ```
2. Download the RNAL CSV from the Turismo de Portugal open-data portal and save it as `data/raw/rnal.csv`.
3. Open the notebooks in `notebooks/` and run all cells (**Kernel → Restart & Run All**). Run H2 first: it downloads the INE data that H3 also uses.

The INE API is only called the first time. The responses are saved in `data/raw/`, so the next runs read the saved files. To download fresh data, delete the `ine_*.csv` files in `data/raw/`.

## Data wrangling

Each notebook follows the same steps: **collection → understanding → cleaning → combining → analysis → conclusions**.

**Functions** (reused in H2 and H3)

| Function | What it does |
|---|---|
| `get_json()` | calls an API, waits 1 second between requests and retries if the server answers 429 (too many requests) |
| `ine_json_to_table()` | turns the nested INE JSON into a table |
| `load_ine()` | reads the saved file if it exists, otherwise calls the INE API and saves the result |
| `clean_ine()` | **main cleaning function** for INE tables: one row per municipality |
| `clean_rnal()` | **main cleaning function** for the RNAL dataset: one row per short-term rental |

**Cleaning techniques**

| Problem | Technique |
|---|---|
| Many columns we do not need | drop columns |
| INE mixes country, regions, municipalities and parishes | filter rows (municipalities only) |
| Numbers and dates stored as text | convert data types (`pd.to_numeric`, `pd.to_datetime`) |
| Municipality code hidden inside longer codes | string slicing (`.str[:4]`, `.str[-4:]`) |
| Municipalities without a long-term rent value (too few contracts) | drop missing values, and report how many |
| Repeated registrations | remove duplicates |
| Place names in Portuguese | rename values (e.g. Lisboa → Lisbon) |

The datasets are merged on the **municipality code**, not on names, so spelling differences cannot break the merge.

## Main findings

*Long-term rent: 1st quarter 2020 to 2nd quarter 2026. Population: 2025. RNAL: up to 29 September 2026.*

**H1 – Long-term rent vs wages:** *(to be added by Andrea)*

**H2 – Short-term rentals vs long-term rent: partly supported**

* Large municipalities (> 50,000 residents) with more short-term rentals have **41% higher** long-term rent than large municipalities with fewer (9.75 vs 6.92 €/m²).
* In small municipalities (≤ 50,000 residents) there is no difference.
* Since 2020, long-term rent rose about **71%** almost everywhere, whether short-term rentals grew or not.
* Short-term rentals are one part of the story, not the main cause of the rental crisis.

**H3 – Is the crisis spreading? Supported**

* Lisbon and Porto are still the most expensive (≈ 16 €/m², about 1,275 € per month for 80 m²).
* But since 2020 long-term rent grew **faster around the two cities (≈ +73%)** and in the rest of Portugal (≈ +71%) than in the cities themselves (≈ +52%).
* The gap is closing: in 2020 the cities were 80% more expensive than the municipalities around them; now they are 57% more expensive.
* The biggest rises are in the outer ring (Moita, Trofa, Barreiro: +87% to +104%), and 9 of the 10 fastest-growing municipalities with at least 20,000 residents are outside the Lisbon and Porto metropolitan areas.

## Limitations

* **Correlation is not causation:** our data show where rents are high or grew, not why.
* Azores and Madeira are not included.
* Municipalities with too few contracts have no INE long-term rent value and are left out.
* INE values are signed contracts (lower than advertised prices) and a 12-month median.
* RNAL lists registrations that exist today. Some may no longer be rented, and closed ones are not in the file.
* Percentage growth is larger when the starting price is low, so we also show the €/m² values.

## Tools

Python · pandas · NumPy · Matplotlib · requests · Jupyter · Git/GitHub · Trello
