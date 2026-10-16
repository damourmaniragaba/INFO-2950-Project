# What makes a successful trader on Polymarket?

INFO 2950 Final Project, Fall 2026
Ha An Pham Le (hp482), Alanna (ay444), Jean D'Amour Maniragaba (jm2876)

## Research question
Which characteristics of a position, and which behaviors of a trader, are associated with higher returns on Polymarket?

- **Position level:** Is a position's return related to its entry price, category, size, holding time, and the event's trading volume?
- **Trader level:** Is a trader's return related to how much risk they take, how focused they are on a few categories, how often they trade, and how long they have been on Polymarket? Is this different for leaderboard (top) traders and ordinary traders?

We measure success with **return on investment (ROI)**: profit or loss ÷ money spent on the position. We look for associations, not causes.

## Data
All data comes from Polymarket's free, public APIs:
- **Data API** (`https://data-api.polymarket.com/v2`): leaderboard, trades, positions, user stats
- **Gamma API** (`https://gamma-api.polymarket.com`): event tags and trading volume

**Sample**
- **Leaderboard group:** the 1,000 wallets with the highest all-time profit.
- **Random group:** 1,000 non-leaderboard wallets, one picked at random from the recent trades of randomly chosen markets that closed between Oct 1, 2025 and Oct 1, 2026.
- **Positions:** for each wallet, up to 500 of its most recent positions for each status (`CLOSED`, `REDEEMABLE`, `REDEEMABLE_LOST`), with activity between Oct 1, 2025 and Oct 1, 2026.

Data collected on [10/11/2026].

## Files
| File | Description |
|---|---|
| `Phase2_Part1.ipynb` | Phase II Part 1: research questions, data collection, cleaning, data description, limitations, EDA |
| `Phase2_Part2_Preregistration.pdf` | Phase II Part 2: preregistered hypotheses and analyses |
| `data/leaderboard_raw.csv` | Top 1,000 wallets from the all-time profit leaderboard |
| `data/wallets_raw.csv` | All sampled wallets and their group (leaderboard or random) |
| `data/positions_raw.csv` | Every downloaded position, as returned by the API |
| `data/user_stats_raw.csv` | Join date for each wallet |
| `data/events_raw.csv` | Tags and trading volume for each event |
| `data/positions_clean.csv` | Analysis-ready table: one row per position |
| `data/traders_clean.csv` | Analysis-ready table: one row per trader |

## Key variables
- `roi`: return on investment (`total_pnl` ÷ `cost_basis`)
- `price_group`: highRisk (under $0.20), mediumRisk ($0.20 to $0.80), lowRisk ($0.80 or more)
- `category`: crypto, sports, weather, econ, tech, culture, politics, other, or unknown
- `trader_roi`: a trader's total profit ÷ total money spent
- `highRisk_share`: share of a trader's money spent on contracts priced under $0.20 (our measure of risk)
- `category_hhi`: how focused a trader is on few categories (1 = all money in one category)
- `top_trader`: 1 for leaderboard wallets, 0 for random wallets

## How to run

### Option 1: Google Colab (no installation)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/damourmaniragaba/INFO-2950-Project/blob/main/Phase2_Part1.ipynb)

1. Click the badge above, or go to [colab.research.google.com](https://colab.research.google.com), choose **File → Open notebook → GitHub**, and paste this repo's link.
2. Add a new cell at the top and run it to install DuckDB (the other packages are already in Colab):
   `!pip install duckdb`
3. Get the data files, using either option:
   - **Re-collect the data:** run all cells from top to bottom. The data collection cells send several thousand API requests, so this can take a while.
   - **Use our saved data:** add and run a cell with
     `!git clone https://github.com/damourmaniragaba/INFO-2950-Project.git` and then `%cd INFO-2950-Project`,
     then skip the data collection cells and run the notebook from the Data Cleaning Appendix onward.
4. Colab deletes all files when the session ends. To keep the data you collect, download the `data/` folder from the **Files** panel on the left, or save it to Google Drive.

### Option 2: Jupyter on your own computer
1. Install the packages: `requests`, `numpy`, `pandas`, `duckdb`, `seaborn`, `matplotlib`.
2. Open `Phase2_Part1.ipynb` and run all cells from top to bottom.
   - The data collection cells send several thousand API requests and save the raw files in `data/`. They only need to run once.
   - The cleaning section reads those saved files and creates `positions_clean.csv` and `traders_clean.csv`.
## Privacy
Raw files contain wallet addresses exactly as Polymarket publishes them. In the clean files, we replaced each wallet address with a `trader_id` number and dropped names, so the analysis tables do not list addresses.

See the Data Limitations section of the notebook for known issues with the sample and measurements.
