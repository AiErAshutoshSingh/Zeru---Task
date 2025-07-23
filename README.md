# 🏦 Aave V2 DeFi Wallet Credit Scorer

This project builds a machine learning-inspired rule-based model that assigns a **credit score (0–1000)** to wallets interacting with the Aave V2 protocol. Higher scores represent healthy, responsible behavior; lower scores flag exploitative, bot-like, or risky usage.

---

## ✅ Features

- 📊 **Behavioral feature engineering** from raw transaction data
- 🧠 **Scoring model** using heuristics and statistical signals
- 🧼 One-step CLI pipeline: `wallet_credit_scorer.py`
- 📁 Designed to scale and extend to DeFi protocols

---

## ⚙️ Architecture

### Input:
- JSON file of raw Aave V2 transaction logs (user-level)

### Processing Pipeline:
Raw JSON → Normalize by wallet → Feature Engineering → Score → Output CSV


### Features Engineered:
| Feature             | Description                                  |
|---------------------|----------------------------------------------|
| Total deposits      | Sum of all deposited amounts                 |
| Total borrows       | Sum of all borrow actions                    |
| Repay ratio         | Total repaid / total borrowed                |
| Redeem ratio        | Total withdrawn / total deposits             |
| Liquidations        | Count of liquidation events                  |
| Time gaps           | Avg time between wallet transactions         |
| Action entropy      | Entropy of actions (bot detection)           |
| Active days         | # of distinct days a user was active         |

### Credit Scoring Logic:

```python
score = 500
+100 × repay ratio
+50 × (1 - utilization ratio)
+50 × redeem ratio
+30 × action entropy
-100 × # of liquidations
-20 × 1/(1+avg_time_gap)
