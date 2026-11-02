# Investment-Risk-and-Return-Analysis-EDA
## Business problem
A portfolio team needs to answer four questions from one investments file:

1. Can the source data be trusted for reporting?
2. Is higher risk actually rewarded with higher return?
3. Where does the profit really come from?
4. Where should new capital go?

## Key findings
| Metric | Value |
|---|---|
| Valid investments / unique investors | 1,576 / 439 |
| Total capital invested | ₹435.5M |
| Total profit | ₹55.2M |
| Portfolio ROI | 12.7% |
| Average actual vs expected return | 12.8% vs 11.2% (+1.5 pp) |
| Average volatility | 16.9% |
| Investments beating expected return | 53.8% |
| Share of capital in High and Very High risk | 23% |
| Share of profit from High and Very High risk | 42% |

- Actual return beats expected return at **every** risk level. The gap widens with risk: +0.1 pp (Low) to +6.7 pp (Very High).
- **Crypto** earns about 29% of total profit from about 12% of capital.
- Return per unit of risk falls steadily from **2.36 (Low)** to **0.54 (Very High)**.

## Dashboard pages
| Page | Purpose | Main visuals |
|---|---|---|
| **1. Overview** | Portfolio health at a glance | KPI cards (Total Invested, Total Profit, ROI, Avg Actual and Expected Return), capital by risk level, profit by asset type, capital vs return trend, capital by region |
| **2. Risk & Return** | Is risk rewarded? | Risk vs return bubble map, expected vs actual return by risk level, investment counts |
| **3. Portfolio Drilldown** | Who and where performs | Profit by manager, return by holding period, capital by region and risk level, sector treemap, profit by status |

**Interactivity**
- Six slicers (Year, Risk Level, Asset Type, Region, Portfolio Manager, Status), **synced across all three pages**.
- Navigation buttons between pages.
- Frosted-glass design with a risk colour scale: green (Low), amber (Medium), orange (High), red (Very High).

## Dataset
`data/investments_EDA.csv` has **1,601 rows and 15 columns**.

| Column | Description |
|---|---|
| `Investment_ID` | Investment identifier |
| `Investor_ID` | Investor identifier |
| `Investment_Date` | Date of investment (`dd-MMM-yyyy`) |
| `Asset_Type` | Bond, Crypto, ETF, Equity, Fixed Deposit, Gold, Mutual Fund, Real Estate |
| `Sector` | Industry or theme (for example Banking, Energy, Diversified) |
| `Risk_Level` | Low, Medium, High, Very High |
| `Investment_Amount` | Capital invested (INR) |
| `Expected_Return_Pct` | Expected return (%) |
| `Actual_Return_Pct` | Realised return (%) |
| `Profit_Loss` | Profit in INR |
| `Volatility_Pct` | Volatility (%) |
| `Holding_Period_Months` | Holding period in months |
| `Region` | City / region |
| `Portfolio_Manager` | Managing person |
| `Status` | Active, Matured and similar states |

## Data cleaning
The file is intentionally messy (an "EDA" dataset). Placeholder and inconsistent values were detected and handled with explicit rules:

| Issue | Detection rule | Treatment |
|---|---|---|
| Invalid amount | `<= 0`, `= 99,999,999`, or the repeated placeholder `76,000` | Rebuilt as `Profit / (Return% / 100)` |
| Invalid return | `450.5`, `999`, or `>= 100` | Rebuilt as `Profit / Amount x 100` |
| Invalid profit | `237,017.88` placeholder | Rebuilt as `Amount x Return% / 100` |


## Dax
Total Invested      = SUM ( Investments[Amount Clean] )
Total Profit        = SUM ( Investments[Profit Clean] )
Portfolio ROI %     = DIVIDE ( [Total Profit], [Total Invested] )
Avg Actual Return   = AVERAGE ( Investments[Actual Return Clean] )
Avg Expected Return = AVERAGE ( Investments[Expected Return Clean] )
Return Gap pp       = [Avg Actual Return] - [Avg Expected Return]
Avg Volatility      = AVERAGE ( Investments[Volatility Clean] )

## Insights and recommendations
**Insights**
- Medium risk is the core of the portfolio: 52% of capital and 45% of profit.
- High and Very High risk hold 23% of capital but earn 42% of profit, so profit is concentrated.
- Low risk is the most efficient (2.36 return per unit of risk) but earns only 13% of profit on 25% of capital.
- 18-month (14.2%) and 48-month (13.9%) holds returned the most.
