#Data Extraction (Data_extractor_wazirx.ipynb)


##Step 1: Get the list of coins available at WazirX (Tickers API ) 
- Output Format: JSON. 
- 	Sample row: "btcinr":{"base_unit":"btc","quote_unit":"inr","low":"3983889.0","high":"4111958.0","last":"3968287.0","type":"SPOT","open":4079595.0,"volume":"246.09257","sell":"3974072.0","buy":"3968683.0","at":1638960896,"name":"BTC/INR"}
-	Key: Coin Name; Snapshot of the coin
-		File Name: tickers

##Step 2: Use “tickers” to extract historical data for every coin (API Invoked)
- Parameters needed
●	Market: Name of the coin extracted from tickers file
●	Limit: Number of rows in the output. 
●	Wazirx will not provide more than 2000 lines per request.
●	Period: Frequency of data in seconds. We choose 1 Hour
●	Timestamp: Initial timestamp of the historical data.
- Output Format: Nested Array/List ["Date", "Open", "High", "Low", "Close", "Volume"]
- 	Sample Row: [1636675200,65104.0,66048.0,63000.0,63000.0,5.0392]


##Step 3: Repeated Extraction. As Wazirx restricts the output to 2000 lines, make repeat requests to get entire historical data. Following calculation is used to update timestamp parameter in the request URL – “startTime += 2000*int(period)*60”

#Data preparation

##Part 1: Cumulative Data Preparation (Cumulative_data_preparation.ipynb)
-	Hourly to Daily - Date: Day starts at 02-11-2021  00:00:00 and ends at 23:00:00. Date must be in 02-11-2021 format.
-	Daily to Monthly - Date: Date starts at 01-11-2021 and ends at 30-11-2021. Date must be changed to Month (1, 2...12).


##Part 2: Choosing the top 10 Coins (Top_CryptoCoins_Extraction.ipynb).
-	Based on the market cap, data related to the top 10 coins in the market was created, for Correlation analysis.
-	All the Crypto list is extracted from Coinmarketcap [4], using GET request (Coinmarketcap API) via POSTMAN Application. This request returns a JSON object which contains all the Crypto currencies list.
-	Top 10 coins were extracted from this JSON object/file.
-	Ranking was provided in JSON object property "cmc_rank".


##Part 3: Extracting the actual name of the coin (Fetch_coin_name.ipynb).
-	The WazirX platform does not provide the actual name of the coin. It uses short names like btc and eth. 
-	Fetched the coin name from CoinMarketCap [4] Free API (Coinmarketcap API). Parsed the JSON output to fetch Coin Name. 
-	Created a key-value pair with coin short name (from WazirX) as Key.


##Part 4: Extracting the actual name of the coin (GoldDataAnalysis.ipynb).
-	The Historical Gold Commodity data was fetched from MCX India [5]. 
-	The Data was cleaned and transformed to be used together with the cryptocurrency data.
