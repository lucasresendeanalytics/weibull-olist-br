# Weibull Survival Analysis — Customer LTV & Opportunity Cost

> **Central question:** Is customer segmentation a data problem or a marketing problem?

This project applies **Weibull survival analysis** to the Olist Brazilian E-Commerce dataset to build a complete pipeline from raw transaction data to actionable CRM decisions — with full mathematical derivation and economic framing.

---

## The Core Idea

Most CRM strategies operate on arbitrary rules: *"email customers who haven't bought in 60 days."*  
This project replaces that rule with a **probabilistic model** that answers a more precise question:

> Given that a customer has already been silent for $t_0$ days, what is the probability they repurchase within the next 30 days?

$$
P\bigl(T \leq t_0 + \Delta t \mid T > t_0\bigr) = \frac{F(t_0 + \Delta t) - F(t_0)}{S(t_0)}
$$

That conditional probability, multiplied by the customer's average ticket and margin, gives the **opportunity cost of inaction** — in real currency, per customer, per day.

---

## Project Structure

```
weibull-olist-br/
│
├── Weibull.ipynb          # Main notebook — full analysis
├── README.md
│
└── plots/                 # Generated on notebook run
    ├── plot1_weibull_fit.png
    ├── plot2_survival_hazard.png
    ├── plot3_conditional_prob.png
    ├── plot4_crm_matrix.png
    ├── plot5_opp_cost_roi.png
    ├── plot6_lorenz.png
    └── weibull_olist_final.png   ← final figure (all 6 combined)
```

---

## Notebook Sections

| # | Section | What it covers |
|---|---|---|
| 0 | Setup & Data Loading | `kagglehub` download, table merge, schema notes |
| 1 | Feature Engineering | `media_dias` — average inter-purchase interval per customer |
| 2 | Weibull: Mathematical Derivation | PDF, CDF, Survival, Hazard, MLE — with full formulas |
| 3 | The Bridge: Past → Future | Conditional repurchase probability formula and decay table |
| 4 | Opportunity Cost Framework | Expected revenue, ENL, OC, ROI of activation |
| 5 | Segmentation | 2×2 matrix — Champions / At Risk / Nurture / Dormant |
| 6 | Final Figure | Publication-quality 6-panel plot |
| 7 | Decision Table | Segment summary with recommended actions |

---

## Key Outputs

**Weibull parameters (MLE):**

$$f(t) = \frac{\beta}{\lambda}\left(\frac{t}{\lambda}\right)^{\beta-1}\exp\!\left[-\left(\frac{t}{\lambda}\right)^\beta\right]$$

- $\hat{\beta} < 1$ on Olist data → **decreasing hazard** — repurchase probability drops fastest in the first weeks after a purchase
- This means the CRM intervention window is measured in **days, not months**

**Segmentation matrix:**

| Segment | P(repurchase) | Avg Ticket | Recommended Action |
|---|:---:|:---:|---|
| 🎯 Champions | High | High | Loyalty program, upsell |
| ⚠️ At Risk | Low | High | Urgent win-back — highest opportunity cost |
| 📢 Nurture | High | Low | Cross-sell to grow basket |
| 💤 Dormant | Low | Low | Low-cost drip or reallocate budget |

**Lorenz curve (Gini of opportunity cost):**  
A high Gini coefficient confirms that the opportunity cost is concentrated in a small fraction of customers — allocating CRM budget uniformly is economically irrational.

---

## Getting Started

### 1. Clone the repo

```bash
git clone https://github.com/<your-username>/weibull-olist-br.git
cd weibull-olist-br
```

### 2. Install dependencies

```bash
pip install kagglehub pandas numpy scipy matplotlib
```

### 3. Configure Kaggle credentials

```bash
# Place your kaggle.json in ~/.kaggle/
# Or set environment variables:
export KAGGLE_USERNAME=your_username
export KAGGLE_KEY=your_api_key
```

### 4. Run the notebook

```bash
jupyter notebook Weibull.ipynb
```

The dataset is downloaded automatically via `kagglehub` in **Section 0**.  
No manual CSV download required.

---

## Parameters to Adjust

| Variable | Default | Description |
|---|---|---|
| `HORIZON` | `30` | Decision window in days |
| `MARGIN` | `0.35` | Net profit margin |
| `ACTIVATION_COST` | `8.0` | Cost (R$) to reach one customer |

These are defined in **Section 4** and propagate through all downstream calculations.

---

## Dependencies

```
python >= 3.9
pandas
numpy
scipy
matplotlib
kagglehub
```

---

## Dataset

**Olist Brazilian E-Commerce Public Dataset**  
[kaggle.com/datasets/olistbr/brazilian-ecommerce](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

~100k orders from a Brazilian e-commerce marketplace (2016–2018).  
License: CC BY-NC-SA 4.0

---

## Methods

- **Weibull MLE** via `scipy.stats.weibull_min.fit(x, floc=0)`
- **Survival analysis** — $S(t)$, $h(t)$, conditional CDF
- **Opportunity cost framework** — expected value × margin - activation cost
- **Lorenz curve / Gini coefficient** applied to CRM budget allocation

---

## Answer to the Central Question

Data without a communication strategy is **diagnosis without treatment**.  
Marketing without data is **treatment without diagnosis**.

The Weibull model doesn't tell you *what* to say — it tells you *to whom, when, and how urgently*.  
**Data solves the information problem. Marketing solves the execution problem. Both, in that order.**
