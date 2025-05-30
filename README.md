# Forecasting-Bitcoin-Prices
Forecasting Bitcoin Prices with RSI and Twitter Sentiment:   A Comparison Between Traditional and Transformer Models

Background and Motivation
Bitcoin’s price is highly volatile and influenced by both technical and behavioral factors. Traditional forecasting models often rely on linear assumptions and fail to capture the non-linear dynamics of cryptocurrency markets.
This study investigates whether Transformer-based models can provide better predictive performance by incorporating technical indicators like RSI and sentiment signals extracted from Twitter.

Market Data and Preprocessing
We used 10 years of daily Bitcoin price data (2015–2025) from Investing.com.
 The dataset includes Open, Close, High, Low, Volume, and two engineered technical indicators: RSI, which captures price momentum, and MACD, which signals trend direction.
 Missing values were handled using forward-fill, and all features were normalized using z-score.
 The data was split chronologically to avoid leakage: 90% for training, 5% for validation, and 5% for testing.

Models and Methodology
          Goal: Identify the most effective model for Bitcoin price forecasting.
We evaluated multiple forecasting models to identify the most effective approach for Bitcoin price prediction.
The models tested included:
 
Statistical --> ARIMA, Prophet --> Simple, fast, limited to linear patterns

Machine Learning --> XGBoost --> Strong with features, lacks sequence awareness

Deep Learning --> LSTM, mLSTM, sLSTM, xLSTM --> Good for sequences, but high training cost

Transformer-based --> PatchTST --> Best overall performance, captures complex dependencies
 
The main contribution of PatchTST lies in its use of patch-based representation and channel-independent attention, making it especially suited for capturing non-linear temporal dependencies in financial data.
Forecasting was performed using input windows of 24, 36, 48, and 60 days.
Performance was evaluated using both regression metrics (MSE, MAE, RMSE, MAPE, R²) and classification metrics (Accuracy, Precision, Recall, F1-score), the latter based on the direction of price movement.

Sentiment Analysis Pipeline
To investigate whether public sentiment could enhance forecasting performance, we extended our dataset by incorporating Twitter data.
 Bitcoin-related tweets were collected from 2015 to 2019, providing a textual representation of market sentiment during that period.
 We applied a multi-step preprocessing pipeline: tweets were cleaned by removing emojis, hashtags, mentions, URLs, and stopwords to reduce noise.
 To ensure consistency, only English-language tweets were retained using a pretrained XLM-RoBERTa model for language detection.
 We then used XLM-RoBERTa-base-sentiment, a model fine-tuned for social media text, to classify each tweet as Positive, Neutral, or Negative.
 Sentiment scores were aggregated on a daily basis, producing one average sentiment vector per day.
 Finally, these daily sentiment scores were merged with Bitcoin price data using date-based alignment, allowing us to examine their influence on model performance.

Model Evaluation with Price History, RSI, and MACD (No Sentiment)
We benchmarked eight forecasting models using only historical Bitcoin market data from 2015 to 2025, including technical indicators RSI and MACD.
 All models were tested across four forecasting windows (24, 36, 48, 60 days). The 24-day input window consistently provided the most accurate results, while longer windows led to decreased performance due to noise in extended time horizons.
Among all models, PatchTST delivered the best performance, achieving an R² of 0.9881 and the lowest MSE of 0.021. It also demonstrated superior accuracy in predicting RSI values, with the lowest RSI MAE of 3.47.
 In contrast, LSTM performed the worst across all metrics, especially in predicting RSI, where its error exceeded 20, indicating poor generalization in volatile environments.
These results confirm that Transformer-based architectures like PatchTST, when applied to pure market data enhanced with RSI and MACD, outperform both traditional and deep learning models in regression accuracy and robustness.

Model Results with Twitter-Enhanced Sentiment Integration
To assess the impact of Twitter sentiment on Bitcoin price forecasting, we designed a three-way evaluation:
Full historical data (2015–2025)

Shorter historical window (2015–2019)

Historical data (2015–2019) + Twitter sentiment

This setup was tested across various models, including LSTM variants and TSTPlus, a Transformer-based model used in place of PatchTST due to integration limitations.
The results showed mixed effects.
 Models such as xLSTM and TSTPlus demonstrated improved performance when sentiment was included, achieving lower MSE and higher R² compared to the historical-only setting.
 In contrast, LSTM performance declined sharply, with a notable increase in error and drop in R² — suggesting limited robustness in handling noisy textual data.
 mLSTM and sLSTM remained stable, showing minimal sensitivity to the added input.
Additionally, comparisons across timeframes revealed that 10-year data (2015–2025) produced higher errors across nearly all models, indicating that recent patterns (5-year data) are more relevant and predictive in volatile markets like cryptocurrency.
Among all models, TSTPlus stood out as the most robust, consistently delivering strong performance across different setups and demonstrating the best balance between numeric and sentiment-driven features.

Key Findings and Conclusion
This study evaluated a wide range of forecasting models — from traditional statistical approaches to advanced Transformer-based architectures — for predicting Bitcoin prices using 10 years of historical data, technical indicators (RSI and MACD), and Twitter-derived sentiment signals.
Key findings include:
Transformer-based models, particularly PatchTST and TSTPlus, consistently outperformed traditional and deep learning models in terms of regression accuracy. PatchTST achieved the lowest error (MSE = 0.021) and the highest R² (0.9881) in the historical-only setup.


Technical indicators such as RSI and MACD significantly improved the interpretability and predictive power of the models, especially in highly volatile conditions like cryptocurrency markets.


Incorporating Twitter sentiment yielded mixed results. Models like xLSTM and TSTPlus showed improvement when sentiment was added, while LSTM experienced notable performance degradation. This underscores the importance of model choice and robustness when integrating noisy textual signals.


Shorter historical windows (e.g., 24-day, 5-year datasets) consistently produced better results than longer ones, confirming that recent trends are more informative for forecasting in dynamic markets.


TSTPlus emerged as the most stable and adaptable model, maintaining strong performance both with and without sentiment input, and offering a promising direction for future research in sentiment-integrated financial forecasting.

Contact
📧 Email: shakiba.mirbagheri@studio.unibo.it
🔗 LinkedIn: linkedin.com/in/shakiba-mirbagheri
