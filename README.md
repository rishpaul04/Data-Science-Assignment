# Data-Science-Assignment
# Trader Performance and Sentiment Analysis

This repository contains a Jupyter Notebook (`trader_analysis.ipynb`) that explores the relationship between market sentiment (Fear & Greed Index) and individual trader performance and behavior. It includes data loading, cleaning, exploratory data analysis, trader segmentation, strategy recommendations, and a simple predictive model for next-day trader profitability.

## Setup and How to Run

### 1. Clone the Repository

```bash
git clone <repository-url>
cd Data-Science-Assignment

```

### 2. Download Data
1) Bitcoin Market Sentiment (Fear/Greed)
Columns: Date, Classification (Fear / Greed)
Link: https://drive.google.com/file/d/1PgQC0tO8XN-wqkNyghWc_-mnrYv_nhSf/view?usp=sharing

2) Historical Trader Data (Hyperliquid)
Includes fields like: account, symbol, execution price, size, side, time, start position, event, closedPnL, leverage, etc.
Link: https://drive.google.com/file/d/1IAfLZwu6rJzyWKgBToqwSmmVYU6VbjVs/view?usp=sharing


Ensure you have the following CSV files in the same directory as the notebook (or update paths in the notebook accordingly):

*   `fear_greed_index.csv`: Contains sentiment data (timestamp, value, classification).
*   `historical_data.csv`: Contains historical trading data (Account, Timestamp, PnL, etc.).

### 3. Install Dependencies

This project requires Python 3.x and the following libraries. You can install them using pip:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

### 4. Run the Notebook

Open and run the `trader_analysis.ipynb` notebook using Jupyter Notebook, JupyterLab, or Google Colab.

*   **Jupyter Notebook/Lab:**
    ```bash
jupyter notebook
    ```
    Then navigate to and open `trader_analysis.ipynb`.

*   **Google Colab:** Upload the notebook to Google Colab and run all cells.

Follow the cells sequentially to load data, preprocess it, perform analysis, generate visualizations, and train the predictive model.

## Output Charts and Tables

All generated charts (e.g., performance by sentiment, behavior by sentiment, segment distributions) are displayed inline within the notebook. Key tables and metrics (e.g., `performance_by_sentiment`, `behavior_by_sentiment`, `trader_summary`, classification reports, feature importances) are printed to the console output of the respective cells.

To save charts as image files, uncomment or add `plt.savefig('path/to/save/image.png')` after `plt.show()` in the plotting cells.

## Methodology, Insights, and Strategy Recommendations

### Methodology

This analysis integrates market sentiment data (Fear & Greed Index) with granular historical trading data to understand their interplay. The methodology involves several key steps:

1.  **Data Loading and Preprocessing:** Two datasets, `fear_greed_index.csv` (sentiment) and `historical_data.csv` (trader activity), were loaded. Timestamps were standardized to daily dates, and the datasets were merged based on these dates.
2.  **Feature Engineering (Daily Metrics):** For each trader (`Account`) on each day, key performance and behavior metrics were calculated:
    *   `daily_pnl`: Sum of `Closed PnL`.
    *   `trade_count`: Total trades.
    *   `avg_trade_size`: Mean `Size USD`.
    *   `longs`, `shorts`: Count of 'Buy' and 'Sell' trades.
    *   `win_rate`: Proportion of profitable trades.
    *   `ls_ratio`: Longs / Shorts ratio.
3.  **Sentiment-Based Analysis:** Data was grouped by `classification` (e.g., Fear, Greed, Neutral) to assess average daily PnL, win rate, trade count, average trade size, and L/S ratio across different sentiment states. Visualizations (bar charts) were used to illustrate these findings.
4.  **Trader Segmentation:** Traders were segmented into archetypes based on their overall performance and behavior. This involved aggregating daily metrics to a per-trader level, then categorizing them into 'Frequent' vs. 'Infrequent' (based on median `total_trade_count`) and 'Consistent Winner' vs. 'Consistent Loser' vs. 'Inconsistent' (based on `total_pnl` and `avg_win_rate`).
5.  **Predictive Modeling:** A Random Forest Classifier was built to predict `next_day_profitable` (whether `daily_pnl` > 0 for the next day). Lagged features (previous day's `daily_pnl`, `trade_count`, `avg_trade_size`, `win_rate`, `ls_ratio`) and one-hot encoded sentiment were used as predictors. The model's accuracy, classification report, and feature importances were evaluated.

### Key Insights

*   **Fear Days as Profit Opportunities:** Days classified as 'Fear' or 'Extreme Fear' consistently showed the highest average daily PnL and win rates for traders. This suggests that periods of market apprehension might present more profitable opportunities for the analyzed traders.
*   **Sentiment Drives Behavior:** Traders tend to be most active (highest `mean_trade_count`) and use larger position sizes (`mean_avg_trade_size`) during 'Fear' days. Conversely, 'Neutral' days are associated with lower activity and smaller trade sizes, correlating with reduced profitability.
*   **Consistent 'Sell' Bias:** A notable observation was a `mean_ls_ratio` of 0.0 across all sentiment classifications, indicating a strong predominant 'Sell' bias or very limited 'Buy' activity within the dataset. This pattern remained constant irrespective of market sentiment.
*   **Diverse Trader Archetypes:** The segmentation revealed a diverse trading population, with a balanced split between 'Frequent' and 'Infrequent' traders. A significant portion of traders were classified as 'Consistent Winners', indicating effective trading strategies among a subset of accounts.
*   **Predictive Power of Lagged Metrics:** The predictive model highlighted that previous day's `avg_trade_size` and `win_rate` are the most influential features for forecasting next-day profitability, underscoring the importance of recent individual performance and sizing in predicting future success.

### Strategy Recommendations

Based on these insights, two actionable 'rules of thumb' for traders emerge:

1.  **Capitalize on 'Fear' with increased 'Sell' activity:** During 'Fear' days, 'Frequent' and 'Consistent Winner' traders could consider increasing their trade frequency and average position sizes. Given the observed predominant 'Sell' bias in the dataset, focusing on short-side opportunities during these periods might be a beneficial strategy to leverage the market's downturn.

2.  **Exercise caution during 'Neutral' sentiment periods:** 'Infrequent' traders or those classified as 'Inconsistent' or 'Consistent Losers' should consider reducing their trade frequency and average position sizes during 'Neutral' days. These periods are associated with the lowest average daily PnL and win rates, and minimizing exposure could help preserve capital and avoid unnecessary losses.
