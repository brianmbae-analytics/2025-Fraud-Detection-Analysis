# Fraud Transactions Analysis (2025)

Exploratory analysis of 60,000 Kenyan-shilling (KES) finance transactions to find where fraud concentrates by channel, time of day, country and merchant category, and to turn the patterns into practical controls.

**Tools:** Python, pandas, matplotlib, Jupyter Notebook

---

## Goal

Identify which segments of a transaction portfolio carry the most fraud risk, so that controls can target them without adding friction for the safe majority of customers.

## Dataset

60,000 transactions from 1 Jan 2025 to 31 Dec 2025, with no missing values and no duplicate rows or transaction IDs.

| Column | Type | Description |
|---|---|---|
| `transaction_id` | text | Unique transaction ID |
| `customer_id` | integer | Customer identifier |
| `timestamp` | datetime | Transaction time (converted from text and sorted) |
| `amount_kes` | decimal | Transaction value in KES |
| `merchant_category` | text | Utilities, Airtime, Fuel, Restaurants, Groceries, Electronics, Health, Betting, Travel, Transfers |
| `channel` | text | M-Pesa, Card POS, Card Online, Bank Transfer, ATM |
| `country` | text | KE, US, AE, UG, TZ, GB |
| `new_device` | 0/1 | Whether a new device was used |
| `is_fraud` | 0/1 | Fraud label |

An `hour` feature (0 to 23) is derived from `timestamp`.

## Method

1. Load the data and check types, missing values and duplicates
2. Convert `timestamp` to datetime and derive `hour`
3. Calculate the overall fraud rate (`is_fraud` mean)
4. Use `groupby` to compute the fraud rate by channel, hour, country and merchant category
5. Plot each breakdown as a bar chart and interpret the results

## Key findings

- **Overall:** 720 fraudulent transactions out of 60,000 (**1.2%**), totalling **KES 2,665,780**.
- **Time of day:** between 00:00 and 04:59 the fraud rate is **6.7% to 9.2%** per hour (peak 9.22% at 03:00), up to 8x the average. Most other hours are below 1.2%.
- **Channel:** **Card Online** is highest at **3.68%**, about 3x the average. M-Pesa and Card POS are near 0.6%.
- **Country:** **US (17.2%), UAE (16.4%) and UK (15.0%)** are more than ten times the average. Uganda and Tanzania are around 3.3%. Kenya is the only country below average at 0.93%.
- **Merchant category:** **Electronics (6.1%)**, Betting (3.4%), Travel (2.7%) and Transfers (1.4%) are above average. Groceries and Utilities are the safest (about 0.3%).

## Recommendations

1. **Night-time step-up checks:** require extra authentication or a short hold for transactions between 00:00 and 05:00.
2. **Card Online protection:** apply 3-D Secure or tighter velocity limits to online card payments.
3. **Cross-border rules:** flag or challenge transactions from the US, UAE and UK, especially for customers who normally transact in Kenya.
4. **High-risk categories:** add scoring or limits on Electronics, Betting and Travel, and monitor Transfers.

Each control should be tested against historical data before rollout.

## Limitations and next steps

- Findings are single-variable views; overlaps between factors are untested.
- Transaction counts and fraud counts are not yet reported alongside rates for hours, countries and categories.
- Next: analyse `new_device` and `amount_kes`, cross-tabulate channel, country and hour, and build a classification model (for example logistic regression or gradient boosting) evaluated with precision and recall, since fraud is only 1.2% of the data.

## Repository structure

```
fraud_detection_project/
├── data/
│   └── 01_finance_transactions_fraud.csv
├── fraud_detection_analysis.ipynb
└── README.md
```

## How to run

```bash
git clone <your-repo-url>
cd fraud_detection_project
pip install pandas matplotlib jupyter
jupyter notebook fraud_detection_analysis.ipynb
```

The notebook currently loads the CSV from an absolute local path. Update the `pd.read_csv(...)` call to a relative path such as `data/01_finance_transactions_fraud.csv` before running.

## Author

**Brian Mbae Mwarania**, data analyst based in Kenya
