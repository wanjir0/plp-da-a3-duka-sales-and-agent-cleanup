# Data Analytics with Python: Weekly Project (Two Client Engagements)

This repository holds the deliverables for two client engagements from a Nairobi analytics consultancy:

| Engagement | Client | Task | Folder |
|---|---|---|---|
| 1 | Duka Electronics | Explore and explain six months of sales data (Jan-Jun 2026) | `duka-sales_exploration.ipynb/` |
| 2 | Mobile money agent network | Audit, clean, merge and summarise an unreliable transaction extract | `agent_cleanup.ipynb/` |



## Setup

```bash
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt
pip install notebook               # only needed to open the .ipynb files locally
```

The notebooks also run in Google Colab: upload the folder's files to the session and run the notebook from the same folder as the data.

## How to regenerate the data

Both datasets are synthetic and seeded, so everyone gets identical files.

```bash
cd duka-electronics
python generate_duka_data.py        # writes duka_sales.csv (seed 2026, 1,000 orders)

cd ../mobile-money
python generate_agent_data.py       # writes agent_transactions.csv and agents.csv (seed 44)
```

## How to run each notebook

Always run a notebook from inside its own folder, because it reads files by relative path.

**Engagement 1:** `cd data_sales_exploration.ipynb`, generate the data, then open `duka_sales_analysis.ipynb` and choose *Restart and run all*. It writes `premium_online_coast_capital.csv` and reads `excel/duka_branch_summary.xlsx` to confirm the spreadsheet totals match pandas.

**Engagement 2:** `cd `, generate the data, then open `agent_cleanup.ipynb` and choose *Restart and run all*. The cleaning steps are also available without the notebook:

```bash
python cleaning_pipeline.py         # writes clean_agent_transactions.csv and cleaning_log.csv
```

or from Python:

```python
import pandas as pd
from cleaning_pipeline import clean_agent_data

clean, log = clean_agent_data(pd.read_csv("agent_transactions.csv"), pd.read_csv("agents.csv"))
```

## Engagement 1 summary: Duka Electronics

Duka Electronics earned KES 71.8M from 1,000 orders in the half-year. The analysis covers data inspection, derived columns, branch and product rankings, weekend versus weekday behaviour, best and worst months, a Premium online filter for Nairobi and Mombasa (exported to CSV), and a spreadsheet reconciliation (SUMIFS, XLOOKUP, PivotTable) that matches pandas to the shilling.

### Executive Summary

- Duka Electronics earned **KES 71.8M** from 1,000 orders in January-June 2026, and **Nairobi contributed KES 26.1M (36.4%)** from 376 orders, more than three times Eldoret's 117.
- **Branch rankings are identical by revenue and by orders** (Nairobi, Mombasa, Nakuru, Kisumu, Eldoret) because average order value sits in a narrow KES 68k-76k band, so volume alone drives the ranking.
- **Laptops generated KES 41.5M (57.9% of revenue) from just 639 units**, while Tablets sold more units (691) but earned only KES 19.3M, so keeping laptops in stock matters more than chasing unit counts.
- **Weekends are slightly quieter per day, not weaker per order**: 277 weekend orders (5.3 per day) versus 723 weekday orders
