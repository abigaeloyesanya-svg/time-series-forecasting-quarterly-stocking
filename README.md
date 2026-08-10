# Project 4: Time Series Forecasting for Quarterly Stocking

## Scenario

The inventory management team needs to know how many units of a key product to stock for the next quarter. This project builds a forecasting model, evaluates its accuracy honestly against a naive baseline and a formal significance test, and translates the result into a stocking recommendation.

## A note on the dataset

The original brief recommended two datasets: a Superstore sales dataset and a Daily Website Traffic dataset. Neither was used, for a specific reason.

The scenario asks how many units to stock. Superstore only records the dollar value of each sale, with no quantity field at all, so it cannot answer a units question without inventing a proxy. The website traffic dataset measures site visits, not sales of a physical product, so connecting it to a stocking decision would require an invented conversion rate that isn't in the data.

Instead, this project uses Kaggle's [Store Item Demand Forecasting Challenge](https://www.kaggle.com/competitions/demand-forecasting-kernels-only). It is a public Kaggle competition dataset with daily sales records for 10 stores and 50 products, from 2013 to 2017. The sales figure in it is a genuine count of units sold per day, which is a direct match to the actual question in the scenario.

## Getting the data

The raw data file is not included in this repo, in line with the competition's data redistribution terms. To run the notebook:

1. Go to the [competition data page](https://www.kaggle.com/competitions/demand-forecasting-kernels-only/data) and accept the competition rules (free, takes a few seconds).
2. Download `train.csv`.
3. Place it at `data/train.csv` in this repo, relative to the notebook.

## Files in this repo

- `Project4_Time_Series_Forecasting.ipynb`: the full analysis. Structured in two parts.
  - Part A, Core Analysis: decomposition, a single chronological train/test split, SARIMAX and Prophet models, RMSE/MAPE evaluation, forecast visualized against actuals. This section on its own completes the assignment as specified.
  - Part B, Extended Validation: rolling origin cross validation across three volume tiers and three forecast quarters, a Diebold Mariano significance test, and a prediction interval calibration check. This goes beyond what the brief asked for, included to confirm the Part A result holds up under scrutiny rather than resting on one lucky split.
- `dashboard_snippet.png`: a compact visual summary of the forecast, model comparison, and interval calibration.
- `Project4_Recommended_Actions.docx`: a written summary of findings and recommended actions, including a plain language explanation of the key terms for a non technical reader.

## Key findings

- Demand ran roughly 20 percent lower in the fourth quarter than the third quarter, across every product tier tested. This is a seasonal decline, not the holiday season increase a generic forecast might assume.
- Prophet, fit with US holiday effects, was the most accurate model across every tier and every quarter tested. Its advantage over both a naive seasonal baseline and a SARIMAX model is statistically significant, not just numerically lower on one split.
- A SARIMAX model with annual Fourier terms is not reliably better than the naive baseline. It beat the baseline on two of three product tiers but was statistically indistinguishable from it on the third, so it should not be deployed to a new product without per product validation.
- Neither model's stated confidence interval was well calibrated out of the box. Prophet's 90 percent interval only captured actual outcomes 80 to 85 percent of the time. The recommended actions use an empirically adjusted buffer rather than trusting either model's raw interval.

## Limitations

- Rolling origin validation covered three folds across one year of quarters, not a multi year window.
- This is a historical backtest against known 2017 outcomes, used for model selection. It is not a live forecast into an unknown future quarter.
- Three products were tested out of 500 store and item combinations in the full dataset. This establishes a directional, statistically supported model choice. It does not certify per product accuracy across the whole catalog.

## Author

Oluwapelumi Abigael Oyesanya, Data Analyst
[LinkedIn](https://www.linkedin.com/in/oluwapelumi-oyesanya) · [Email](mailto:abigaeloyesanya@gmail.com)
