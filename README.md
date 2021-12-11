# CryptoAnalysis

## Following are the high level data engineering approach taken to identify various insights from the crypto currency data:
  - Data Extraction of relevant crypto currency data from WazirX public API.
  - Data Preparation - Data cleaning and processing to create meaningful dataset.
  - Data Transformation and analysis - Performance Analysis of Cryptocurrencies.
  - Further Analysis to identify various insights like, finding Correlation of different Crypto Currencies and external factors like Pandemic.
  - Setting up a Graph-based Data Visualization Tool and computing various Trend Trading Indicators for visual representation.
 
 ## Module Description
 ### Data Extraction
 
 Step 1: Get the list of coins available at WazirX (https://api.wazirx.com/api/v2/tickers )
 - Code: Data_extractor_wazirx.ipynb
 - Output Format: JSON
 - Sample row: "btcinr":{"base_unit":"btc","quote_unit":"inr","low":"3983889.0","high":"4111958.0","last":"3968287.0","type":"SPOT","open":4079595.0,"volume":"246.09257","sell":"3974072.0","buy":"3968683.0","at":1638960896,"name":"BTC/INR"}
 - Key: Coin Name; Snapshot of the coin
 - File Name: tickers
 
 Step 2: Use “tickers” to extract historical data for every coin -https://x.wazirx.com/api/v2/k?market=btcusdt&limit=2000&period=1440&timestamp=1636688470 
 - ⮚	Parameters needed
 - Market: Name of the coin extracted from tickers file
 - Limit: Number of rows in the output. Wazirx will not provide more than 2000 lines per request.
 - Period: Frequency of data in seconds. We choose 1 Hour
 - Timestamp: Initial timestamp of the historical data.

Step 3: Repeated Extraction. As Wazirx restricts the output to 2000 lines, make repeat requests to get entire historical data. Following calculation is used to update timestamp parameter in the request URL – “startTime += 2000*int(period)*60”


 ### Data Preparation
 
 Part 1: Cumulative Data Preparation (Code: Cumulative_data_preparation.ipynb)
 - Hourly to Daily - Date: Day starts at 02-11-2021  00:00:00 and ends at 23:00:00. Date must be in 02-11-2021 format.
 - Daily to Monthly - Date: Date starts at 01-11-2021 and ends at 30-11-2021. Date must be changed to Month (1, 2...12)
 
 Part 2: Choosing the top 10 Coins (Code: Top_CryptoCoins_Extraction.ipynb).
 - Based on the market cap, data related to top 10 coins in the market was created, for Correlation analysis.
 - All the Crypto list is extracted from Coinmarketcap [3], using GET request (https://pro-api.coinmarketcap.com/v1/cryptocurrency/listings/latest ) via POSTMAN Application. This request returns a JSON object which contains all the Crypto currencies list.
 - Top 10 coins were extracted from this JSON object/file.
 - Ranking was provided in JSON object property "cmc_rank".
 
Part 3: Extracting the actual name of the coin (Code: fetchCoinName.ipynb).
⮚	The Wazirx platform does not provide the actual name of the coin. It uses short names like btc and eth. 
⮚	Fetched the coin name from CoinMarketCap [3] Free API (https://pro-api.coinmarketcap.com/v1/cryptocurrency/listings/lates ). Parsed the JSON output to fetch Coin Name. 
⮚	Created a key-value pair with coin short name (from Wazirx) as Key.


### Performance Metrics

Price/Volume Trend Analysis (Code: Price_Trend_Module Volume_Trend_Module)
- Input: Coin Name, Frequency of Calculation, Option - Price or Volume
 - HOURLY ANALYSIS 
●	Inference: Which hour of the day is best for the coin in terms of price and volume.
●	Find the hour in which max price or volume of the day is reported.
●	Take the frequency of these hours to identify which hour of the day likely to have the max price / volume of transaction
- DAILY ANALYSIS 
●	Inference: Which day of the week is best for the coin in terms of price and volume.
●	Find the day in which max price or volume of the week is reported.
●	Take the frequency of these days to identify which day of the week likely to have the max price / volume of transaction.
- MONTHLY ANALYSIS
●	Inference: Which date of the month (as in 1 2 3) is best for the coin in terms of price and volume.
●	Find the date in which max price or volume of the month is reported.
●	Take the frequency of these date to identify which date of the month likely to have the max price / volume of transaction

### Correlation Analysis

Correlation Analysis (Correlation_Analysis_Module.ipynb)
	- Correlation Matrix
  - Tool: Spark ML
  - Method: Pearson correlation matrix
	
Covariance (Correlation_Analysis_Module.ipynb)
  - measure of the relationship between two coins
  - Tool: RDD Transformation


Pandemic Impact (Correlation_Analysis_Module.ipynb)
  - Price and Volume Trend during Pandemic
  - Tool: DF Transformation


Linear Regression (Linear_Regression_Analysis_Module.ipynb)
  - Linear Regression module of Top-10 coins.
  - Tool: Spark ML

Gold Trade Data Correlation (Gold_Data_Analysis_Module.ipynb)
  - MCX India Trade Data
  - Correlation of 4 years Gold Trade and Top-10 Coins

### NOTE: The code is using Shared Drive path for accessing dataset "/content/drive/Shareddrives/ProjectSharedDrive/wazirxCSVCumulative".  If you want to access the data uploaded to this repository instead of shared drive, the data path must be updated accordingly.
 

 
