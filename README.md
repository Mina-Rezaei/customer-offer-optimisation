# Customer Offer Optimisation under Uncertainty

End-to-end decision science pipeline for digital marketing —
determining the optimal promotion to assign to each customer 
segment to maximise expected redemption value under uncertainty.

## Pipeline

| Step | What it does |
|---|---|
| 1. Data Generation | Simulates 5,000 customers with realistic behavioural signals |
| 2. Feature Engineering | Recency score, engagement score, value-frequency composite |
| 3. Predictive Model | Gradient Boosting — ROC-AUC 0.763, PR-AUC 0.554 |
| 4. Monte Carlo Simulation | 1,000 scenarios per offer — profit distributions with 90% CI |
| 5. LP Optimisation | Offer allocation under budget constraint (scipy linprog) |
| 6. Propensity Scoring | customer_id · score · tier (High/Med/Low) · model_version |
| 7. UCG Holdout | 10% holdout group — simulated lift: +12.4% absolute |
| 8. MLflow Tracking | 2 experiments logged — budget $5,000 vs $500 |

## Key Results

| Metric | Value |
|---|---|
| ROC-AUC | 0.763 |
| PR-AUC | 0.554 |
| Best offer (E[profit]) | BOGO — $2.14 per customer |
| Absolute lift vs holdout | +12.4% |
| MLflow experiments | 2 runs tracked and comparable |

## Offer Performance (Monte Carlo)

| Offer | E[Profit] | 90% CI | P(Profitable) |
|---|---|---|---|
| BOGO | $2.143 | [$1.978, $2.314] | 100% |
| Bundle | $1.878 | [$1.723, $2.024] | 100% |
| Discount_20 | $1.517 | [$1.401, $1.626] | 100% |
| Loyalty_Points | $0.834 | [$0.776, $0.893] | 100% |
| Free_Item | $0.799 | [$0.718, $0.886] | 100% |

## Budget Optimisation Comparison

With **$5,000 budget** — all offers fully funded, no constraints binding.

With **$500 budget** — optimiser correctly deprioritised Free_Item 
(highest cost, lowest net value) cutting it from 100% to 11% 
of customers. BOGO, Bundle and Loyalty_Points maintained at 100%.

## Production Architecture

In a production setting this pipeline would:

Databricks Delta table (dcoe.propensity_scores_offer)
↓ BYOL zero-copy federation
Salesforce Data Cloud
↓ segment attribute
Journey Builder Decision Split
↓ route by tier
High → richer offer arm (BOGO / Bundle)
Med → standard nurture
Low → suppress


## Technologies

Python · scikit-learn · scipy · NumPy · pandas · MLflow · Databricks

**Methods:** Gradient Boosting · Monte Carlo Simulation · 
Linear Programming · Propensity Scoring · UCG Holdout Design

## Author

**Mina Rezaei** — Data Analyst & ML Engineer 
[GitHub](https://github.com/Mina-Rezaei) · 
[LinkedIn](https://linkedin.com/in/mina-rezaei)
