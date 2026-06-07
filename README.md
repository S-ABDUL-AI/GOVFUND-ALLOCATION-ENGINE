# GovFund Allocation Engine
**Cost-Effectiveness Decision Tool for Public Health Funders**

GovFund Allocation Engine is a production-style Streamlit app that helps NGOs and public health funders allocate fixed budgets across interventions using transparent cost-effectiveness logic, uncertainty adjustment, and scenario testing. The current portfolio is calibrated for malaria intervention decision support — SMC, ITNs, cash transfers, and related options — applying methodology consistent with GiveWell and DIV Fund evaluation frameworks.

---

## Why this app exists

Public health funders face a high-stakes question with every budget cycle: **where should this dollar go?**

This tool provides a transparent decision-support workflow that makes trade-offs visible — converting intervention evidence into comparable scores, allocating budgets proportionally, and producing a board-ready policy memo.

---

## Primary users

- GiveWell-style research teams
- Gates Foundation and other philanthropic funders
- USAID and bilateral programme officers
- Global health policy and allocation teams

---

## Quickstart

```bash
pip install -r requirements.txt
streamlit run app.py
```

Then:
1. Set budget, region, and interventions in the sidebar
2. Review the Executive Summary at the top of the page
3. Click **Download Report** for the decision memo

---

## Core model

The scoring stack follows this sequence:

```
expected_impact        = adjusted_effect_size * population_affected * baseline_risk
adjusted_impact        = expected_impact * (evidence_strength / 5)
cost_effectiveness     = adjusted_impact / (adjusted_cost * population_affected)
uncertainty_adjusted   = cost_effectiveness * (1 - uncertainty_level)
final_score            = uncertainty_adjusted * funding_gap * scalability
```

Budget allocation:

```
allocation = (final_score / sum(final_score)) * total_budget
```

If all scores are zero, the app falls back to equal split.

---

## Features

**Inputs**
- Total budget
- Region filter
- Intervention selector
- Scenario selector (Base, Optimistic, Pessimistic)
- Sensitivity sliders
- Optional moral weights
- Optional malaria cost stress test

**Outputs**
- Executive summary with recommendation and risk notes
- Key metrics and ranking
- Budget what-if table (+50%)
- Sensitivity snapshot table
- Allocation and score charts (Plotly)
- Allocation table with CSV export
- Downloadable PDF/HTML funding report

---

## Data schema

Expected columns in `interventions.csv`:

| Column | Description |
|---|---|
| intervention_id | Unique identifier |
| intervention_name | Display name |
| sector | Health, nutrition, WASH, etc. |
| region | Geographic filter |
| cost_per_beneficiary | USD |
| cost_per_life_saved | USD |
| effect_size | Adjusted effect estimate |
| evidence_strength | Score 1–5 |
| population_affected | Absolute number |
| baseline_risk | Baseline mortality or incidence rate |
| expected_lives_saved | Model output field |
| income_increase_per_person | USD |
| funding_gap | Proportion of need unmet |
| scalability | Score 0–1 |
| time_to_impact | Years |
| uncertainty_level | Score 0–1 |

---

## Project structure

```
GOVFUND-ALLOCATION-ENGINE/
  app.py              # Streamlit UI entry point
  data_loader.py      # Remote/local/fallback data loading and validation
  modeling.py         # Scoring and budget allocation logic
  policy.py           # Executive summary and policy insights
  report_html.py      # Downloadable HTML report builder
  report_generator.py # PDF report engine (build_report_bytes)
  interventions.csv   # Sample intervention dataset
  requirements.txt    # Dependency list
```

---

## Data loading

The app tries sources in this order:
1. Remote CSV URL (if supplied and available)
2. Local bundled `interventions.csv`
3. Synthetic fallback with matching schema

This keeps the app resilient in both cloud and local environments.

---

## Deployment

- Built for Streamlit Cloud or any Streamlit-compatible environment
- `requirements.txt` pins compatible major versions
- App runs using local or fallback data if remote source is unavailable

---

## Disclaimer

This is a transparent modelling aid, not a substitute for grant diligence, partner assessment, governance review, or board approval. Recommendations should be validated with local context, partner capacity, and implementation expertise.

---

## Author

**Sherriff Abdul-Hamid**
Data Scientist · Development Economist · Public Infrastructure Analytics
[poverty360.org](https://poverty360.org) | [LinkedIn](https://www.linkedin.com/in/abdul-hamid-sherriff-08583354/) | [GitHub](https://github.com/S-ABDUL-AI)
