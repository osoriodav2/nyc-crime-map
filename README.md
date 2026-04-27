# NYC Crime Explorer

An interactive map and data analysis tool for NYC crime statistics using NYPD Open Data and the US Census ACS API.

## Live Website

Open `index.html` directly in a browser, or deploy to GitHub Pages (see below).

## Features

- **Interactive map** of NYC criminal complaints, color-coded by severity (Felony / Misdemeanor / Violation)
- **Filter by**: Borough, Severity, Victim Race/Sex, Suspect Race/Sex, Year Range, Offense keyword
- **Census controls panel**: Pulls median income, poverty rate, unemployment, and population per borough from the US Census ACS 5-Year 2022 dataset
- **Incident detail popups**: Click any dot to see full complaint details
- **Results list**: Scrollable sidebar list of the most recent incidents in the current query

## Data Sources

| Source | Dataset | URL |
|--------|---------|-----|
| NYC Open Data / NYPD | NYPD Complaint Data Historic | https://data.cityofnewyork.us/resource/qgea-i56i.json |
| US Census Bureau | ACS 5-Year 2022 | https://api.census.gov/data/2022/acs/acs5 |

## Deploy to GitHub Pages

1. Create a new GitHub repository
2. Upload `index.html` (and optionally `analysis.py`) to the repo root
3. Go to **Settings → Pages → Source → Deploy from branch → main / root**
4. Your site will be live at `https://<username>.github.io/<repo-name>`

No build step required — it's pure HTML/CSS/JS.

## Python Analysis Script

`scripts/analysis.py` performs deeper statistical analysis:

```bash
pip install pandas requests matplotlib seaborn scipy
python scripts/analysis.py
```

This will:
- Pull a larger sample of NYPD complaint data
- Merge with Census ACS borough-level controls (income, poverty, population density)
- Produce charts for: crime by borough, severity breakdown, victim/suspect demographics, top offense types
- Run a simple regression controlling for income and poverty rate

## Key Fields in the NYPD Dataset

| Field | Description |
|-------|-------------|
| `boro_nm` | Borough name |
| `law_cat_cd` | FELONY / MISDEMEANOR / VIOLATION |
| `ofns_desc` | Offense description |
| `vic_race` / `vic_sex` / `vic_age_group` | Victim demographics |
| `susp_race` / `susp_sex` / `susp_age_group` | Suspect demographics |
| `latitude` / `longitude` | Coordinates |
| `cmplnt_fr_dt` | Date of complaint |
| `prem_typ_desc` | Premise type (street, residence, etc.) |

## Census Variables Used

| Variable | Description |
|----------|-------------|
| `B19013_001E` | Median Household Income |
| `B01003_001E` | Total Population |
| `B17001_002E` | Population Below Poverty |
| `B23025_005E` | Unemployed |
| `B25001_001E` | Total Housing Units |

## Notes & Limitations

- Suspect demographics reflect **reported** data, which may be incomplete or biased
- "Unknown" race/sex values are common — filter carefully to avoid misleading rates
- Census data is at county (borough) level, not census tract — for finer spatial controls, you'd need to join on lat/lng to tract FIPS codes
- The dataset covers **complaints filed**, not convictions

## Extensions to Try

- Add census tract-level income choropleth layer behind the dots
- Cluster markers at low zoom using Leaflet.markercluster
- Add a time-series chart showing crime trends by year
- Pull the YTD dataset alongside the historic one for up-to-the-month data
- Add a heatmap layer toggle using Leaflet.heat
