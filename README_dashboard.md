# Landmark Customer Intelligence Dashboard

A Streamlit app that turns the notebook's results into ten pages a manager can read without any technical background.
It only reads the notebook's output files (CSV and JSON). It never retrains a model, so every number matches the notebook exactly.

## Run it (Windows, Mac or Linux)
1. Run `Landmark_Capstone.ipynb` in Google Colab (Runtime → Run all). The last cell downloads **landmark_outputs.zip**.
2. Put `landmark_outputs.zip` in this folder, next to `app.py`. You don't need to unzip it: the app unzips it into `outputs/`
   and re-unzips automatically whenever you drop in a newer zip.
3. Open a terminal in this folder and run:
   ```
   pip install -r requirements.txt
   streamlit run app.py
   ```
4. Your browser opens at http://localhost:8501.

Requires Python 3.10+ and Streamlit 1.66 or newer (tested on 1.66.1). If the app shows a message that your Streamlit is too
old, run `pip install -U streamlit` and start it again.

## What's in this folder
| Item | What it is |
|---|---|
| `app.py` | Entry point: page setup, the global filters and the navigation |
| `lmk/data.py` | Loads the outputs, builds one table with a row per member, applies filters, handles drill-down clicks |
| `lmk/theme.py` | Colours, fonts, chart template and the small visual components (KPI cards, chips, callouts) |
| `lmk/ui.py` | Panels, tables, downloads and the filter breadcrumb |
| `lmk/pages/` | One file per page |
| `.streamlit/config.toml` | The light, executive colour theme |
| `landmark_outputs.zip` | The notebook's outputs (replace it with your own run) |

## The ten pages
| Page | Question it answers | Main visuals |
|---|---|---|
| Executive summary | All five questions in one screen, and what to do first | KPI cards, "the brief, answered", ranked action plan, segment and status mix, where growth came from (spend by banner + active shoppers vs spend per shopper), churn cliff, emirates, Viva gap |
| Segments | Who are our customers? | Segment cards, drill-down sunburst (segment → home banner → status; click a ring to zoom), radar profiles, banner mix, spend box plots, PCA behaviour map, demographics heatmap, discount dependency (statistically tested), migration targets, families' banner mix, Abu Dhabi discount heatmap, deep dive |
| Retention | Who is about to leave? | Lapse chance by days since last purchase, status by segment (with the chi-square test), late joiners, leavers before they left, when leavers stopped, risk histogram, margin at risk, **what-if simulator** (offer cost, save rate, margin, call capacity) with call list (late joiners flagged), drivers, contact-rule profit, win-back list |
| Customer value | Who is most valuable? | Lorenz curve, value distribution, value × risk priority matrix, actual vs expected (under-spenders), value drivers, momentum by segment, top-member lists |
| Banner routing | Which banner does each customer belong to? | Leakage ladder, score overlap, gains by decile, signals, emirate shares, home × fashion tier, member lookup, upgrade targets |
| Bundles | What should we offer? | Live Apriori with your own thresholds, banner network, lift heatmap, rules table, "is it a pairing or just families?", days between visits, bundle value, reach by segment, threshold sensitivity |
| Member 360 | Everything about one member | Next best action, KPIs, purchase timeline with status zones, banner mix vs segment, percentiles, position on the behaviour map, purchase history |
| Models | How were the models chosen? | Rubric coverage, PCA (scree, loadings), K selection, every model's test scores, tuning, bias–variance (dumbbells, learning and complexity curves, bagging vs boosting) |
| Method & data | How was every number produced? | Pipeline diagram with the reason for each stage, data checks, the Viva blind spot, the Viva candidate-key test, findings checked and rejected, assumptions, how to reproduce, glossary |
| Campaign plan | What exactly should we run, and how will we know it worked? | Five campaigns ranked by expected net value, editable assumptions, budget vs value, 12-week timeline, one-campaign-per-member overlap, per-campaign detail with control-group size and detectable effect, list downloads with random control groups |

## Filters and drill-down
- **Global filters** (left sidebar) apply to every page: segment, member status, emirate, home banner, fashion banner,
  household, gender, age and lifetime spend. The strip under the title always shows what is active, with a "Clear filters" button.
- **Page filters** sit in a box at the top of a page and only change that page (for example the risk band on Retention,
  the value measure on Customer value, the Apriori thresholds on Bundles).
- **Click to drill down**: click a segment, status or emirate in a chart to filter every page to it.
  Click a member's dot in a scatter chart, or select a row in a list, to open their Member 360 card.
- Every list has a **Download (CSV)** button.

## Colour coding
- Each segment and each banner keeps one colour on every page (for example Premium Home-Makers is always navy blue).
- Member status always uses the same four colours **and** a symbol, so it still reads in black-and-white print:
  ✓ Active (bought in the last 30 days) · ↓ Cooling (31–45 days) · ⚠ At risk (46–90 days) · ✕ Lapsed (90+ days).
- A grey "Model" or "Group-wide" tag on a chart means it comes from the notebook's model results or the whole Group,
  so it does not change with the filters.

## Notes
- Every figure is Shukran-only. The Viva 4-digit card-number pairing was tested and rejected; the test is shown on
  *Method & data* only, and no other page uses it.
- The app needs no scikit-learn or XGBoost: all model scores come from the notebook.
- The data is a synthetic case study, not real Landmark customers.
- The font (IBM Plex Sans) loads from Google Fonts. Offline, the app falls back to Segoe UI or Arial.
