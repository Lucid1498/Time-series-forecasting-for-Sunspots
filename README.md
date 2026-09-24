# Sunspot Time-Series Forecasting

This project forecasts daily, monthly, and yearly total sunspot activity with [Prophet](https://facebook.github.io/prophet/). It uses historical observations from the World Data Center SILSO and evaluates several model configurations before producing future forecasts at each time scale.

## Project goals

- Build reproducible preprocessing pipelines for three SILSO datasets.
- Represent the approximately 11-year solar cycle with custom Prophet seasonality.
- Compare linear, flat, and logistic growth assumptions.
- Test different seasonality periods, Fourier orders, and changepoint settings.
- Evaluate forecasts on chronological holdout periods.
- Produce short-, medium-, and longer-horizon forecasts with uncertainty intervals.

## Data

The source files come from the [World Data Center SILSO](https://www.sidc.be/SILSO/datafiles):

| Time scale | Source file | Source coverage | Modeling coverage |
|---|---|---:|---:|
| Daily | `SN_d_tot_V2.0.csv` | 1818-2022 | 1900-2022 |
| Monthly | `SN_m_tot_V2.0.csv` | 1749-2022 | 1749-2022 |
| Yearly | `SN_y_tot_V2.0.csv` | 1700-2021 | 1750-2021 |

The daily model starts in 1900 because all 3,247 unavailable daily observations occur before that year, while the 1900-2022 period contains no `-1` sunspot markers. The yearly model starts in 1750 to keep the date span within the range supported by Prophet's pandas-based datetime calculations.

## Methodology

Each notebook follows the same workflow:

1. Load the raw semicolon-delimited SILSO file.
2. Assign the documented column names and construct timestamps.
3. Infer whether the timestamps are daily, monthly, or yearly from their median spacing.
4. Remove unavailable observations and validate dates.
5. Create a chronological training and holdout split.
6. Compare five Prophet configurations.
7. Select the model with the lowest holdout MAE.
8. Refit the selected configuration on the complete modeling history.
9. Generate the selected future horizons and an 80% uncertainty interval.

The model comparison covers:

- Linear, flat, and logistic growth
- Solar-cycle periods of 10.5, 11, and 11.5 years
- Fourier orders of 5, 10, and 15
- Between 25 and 75 changepoints
- Changepoint prior scales from 0.05 to 0.20

## Holdout results

| Time scale | Holdout period | Selected configuration | MAE | Nonzero MAPE | R² |
|---|---|---|---:|---:|---:|
| Daily | 365 days | Linear, 11-year cycle, Fourier order 10 | 22.772 | 56.902% | 0.192 |
| Monthly | 24 months | Linear, 11-year cycle, Fourier order 10 | 12.079 | 505.414% | 0.572 |
| Yearly | 22 years | Logistic, 11-year cycle, Fourier order 10 | 40.039 | 248.632% | 0.355 |

MAE is the primary selection metric. Ordinary MAPE is undefined when the actual value is zero, so the notebooks calculate MAPE only over nonzero actual observations. It remains highly sensitive when actual counts are close to zero.

## Forecast horizons

| Time scale | Horizons | Point forecasts |
|---|---|---|
| Daily | 100, 200, and 365 days | 69.511, 71.491, and 76.735 |
| Monthly | 1, 6, and 9 months | 53.320, 64.740, and 68.090 |
| Yearly | 1, 10, and 20 years | 77.688, 21.988, and 33.034 |

These estimates demonstrate Prophet-based time-series modeling. They are not operational space-weather predictions, and uncertainty increases with the forecast horizon.

## Repository structure

```text
sunspot-time-series-forecasting/
├── data/
│   ├── raw/
│   │   ├── SN_d_tot_V2.0.csv
│   │   ├── SN_m_tot_V2.0.csv
│   │   └── SN_y_tot_V2.0.csv
│   ├── processed/
│   │   ├── daily_sunspots.csv
│   │   ├── monthly_sunspots.csv
│   │   └── yearly_sunspots.csv
│   └── README.md
├── notebooks/
│   ├── 01_daily_sunspot_forecasting.ipynb
│   ├── 02_monthly_sunspot_forecasting.ipynb
│   └── 03_yearly_sunspot_forecasting.ipynb
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

## Running the project

### Local environment

```bash
git clone https://github.com/Lucid1498/Time-series-forecasting-for-Sunspots.git
cd Time-series-forecasting-for-Sunspots
python -m venv .venv
```

Activate the environment on Windows:

```powershell
.venv\Scripts\Activate.ps1
```

Install the dependencies:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Open the notebooks in this order:

1. `notebooks/01_daily_sunspot_forecasting.ipynb`
2. `notebooks/02_monthly_sunspot_forecasting.ipynb`
3. `notebooks/03_yearly_sunspot_forecasting.ipynb`

The notebooks detect whether they are launched from the repository root or the `notebooks` directory. No machine-specific file paths are required.

### Google Colab

Clone the repository in a Colab session, install `requirements.txt`, and open the notebooks from the cloned directory. The analysis does not require Google Drive mounting.

## Technologies

- Python
- pandas and NumPy
- Prophet and CmdStanPy
- scikit-learn
- Matplotlib
- Jupyter Notebook

## License

This project is available under the [MIT License](LICENSE).
