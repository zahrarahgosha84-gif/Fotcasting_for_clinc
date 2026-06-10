Revenue Forecasting for Dental Clinic

This project performs time series forecasting of monthly revenue for a dental clinic using Prophet (Facebook's forecasting library). It processes two Excel reports, cleans and merges data, handles Persian (Jalali) dates, and generates future revenue predictions with confidence intervals.

📌 Overview

The notebook demonstrates a complete data science workflow:

1. Data Loading – Reads two Excel files (report (4).xlsx, report (6).xlsx).
2. Date Alignment – Converts Persian (Jalali) dates to Gregorian and extracts date components.
3. Data Merging – Joins both datasets on شماره پرونده (case number) and زمان درمان (treatment date).
4. Outlier Removal – Removes negative/zero revenues and extreme values (using median-based thresholds).
5. Forecasting – Uses Prophet with additive seasonality and tuned parameters to predict the next 12 months.
6. Evaluation – Cross‑validation and performance metrics (MAE, RMSE, MAPE).
7. Visualization – Plots historical data, forecast, trend, and yearly seasonality.

---

🗂️ Input Files

· report (4).xlsx – Contains detailed treatment records with timestamps (including hours/minutes).
· report (6).xlsx – Contains aggregated financial data (discounts, total amount, Jalali dates).

Both files are expected to share columns:
شماره پرونده (case number), زمان درمان (treatment date), مبلغ کل (total amount), تخفیف پزشک (doctor discount), تخفیف مجموعه (clinic discount).

---

⚙️ Installation

Clone the repository and install the required packages:

```bash
git clone https://github.com/yourusername/revenue-forecast.git
cd revenue-forecast
pip install -r requirements.txt
```

requirements.txt example:

```
pandas
jdatetime
prophet
matplotlib
openpyxl
```

Note: Prophet requires cmdstanpy or pystan. On some systems, you may need to install pystan first.

---

🚀 Usage

1. Place the two Excel files in the same directory as the notebook.
2. Run the notebook cells sequentially.
3. The final output includes:
   · A plot of historical data + forecast.
   · Trend and seasonality components.
   · A table of future predictions (in billion IRR).
   · Saved Excel file: Final_Revenue_Forecast_Billion.xlsx.

Key Code Snippets

Date Conversion (Jalali → Gregorian)

```python
def jalali_to_gregorian(date_str):
    y, m, d = map(int, str(date_str).split('/'))
    return jdatetime.date(y, m, d).togregorian()
```

Prophet Model Setup

```python
model = Prophet(
    seasonality_mode='additive',
    changepoint_prior_scale=0.01,
    yearly_seasonality=True
)
model.fit(df_cleaned)
future = model.make_future_dataframe(periods=12, freq='ME')
forecast = model.predict(future)
```

Outlier Filtering

```python
median_income = monthly_income['y'].median()
dynamic_upper_limit = median_income * 3
df_cleaned = monthly_income[
    (monthly_income['y'] <= dynamic_upper_limit) &
    (monthly_income['y'] >= 50_000_000)
]
```

---

📊 Results

· The forecast shows revenue for the next 12 months in billions of IRR.
· The model captures a positive linear trend and a yearly seasonal pattern (peak in mid‑summer, dip in spring/autumn).
· Cross‑validation metrics (MAE, RMSE, MAPE) indicate reasonable accuracy.

Example output (last 5 months):

Month (Gregorian) Forecast (billion IRR)
August 2026 2.64
September 2026 2.77
October 2026 2.90
November 2026 3.02
December 2026 3.15

---

📁 Output Files

· Final_Revenue_Forecast_Billion.xlsx – Forecast values (12 months) in billions.
· Plots are displayed inline or saved via plt.savefig() if desired.

---

🤝 Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

---


🙏 Acknowledgements

· Prophet by Meta
· jdatetime
· pandas
