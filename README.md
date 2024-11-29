

Relative Price Strength Exploration Script for Amibroker

This repository contains a fully customizable Relative Price Strength Exploration Script for Amibroker, designed to identify and rank the top-performing stocks based on relative price strength over customizable time frames.

Features

	•	Relative Strength Calculation: Ranks stocks by their relative strength over user-defined time frames (1 month, 3 months, 6 months, or a combined score).
	•	Top Percentile Filtering: Dynamically filters and displays the top X% of stocks (default: 5%).
	•	Comprehensive Data Display:
	•	Relative Strength scores.
	•	Percentage gains for 1-month, 3-month, and 6-month periods.
	•	Stock Sector, Industry, and Shares Outstanding (requires Norgate data).
	•	Single Row Per Stock: Ensures only the most recent data for each stock is displayed.
	•	Customizable Parameters: Modify time frames and top percentile filtering directly from Amibroker’s parameter interface.

Installation

Prerequisites

	1.	Amibroker: Ensure you have Amibroker installed.
	2.	Norgate Data Subscription: Required for fields like Sector, Industry, and Shares Outstanding.

Steps

	1.	Download or copy the RelativePriceStrength.afl script from this repository.
	2.	Open Amibroker and go to Formula Editor (Ctrl + E).
	3.	Paste the script into the editor.
	4.	Save the script in your Amibroker formulas directory (e.g., Formulas\Custom) as RelativePriceStrength.afl.

Usage

Running the Exploration

	1.	Open Amibroker and go to Analysis > New Analysis.
	2.	Select the RelativePriceStrength.afl script from the formula list.
	3.	Configure the following parameters in the Analysis window:
	•	Time Frame: Choose from 1 Month, 3 Months, 6 Months, or Combined.
	•	Top Percentile: Specify the percentage of top stocks to display (default: 5%).
	4.	Click Explore to run the script.

Results

The exploration will display:
	•	Top X% of Stocks: Based on relative strength or combined score.
	•	Columns Displayed:
	•	Relative Strength.
	•	% Gain for 1M, 3M, and 6M periods.
	•	Sector, Industry, and Shares Outstanding.

Parameters

Parameter	Description	Default Value
Time Frame	Select the time frame for relative strength calculation (1 Month, 3 Months, 6 Months, Combined).	Combined
Top Percentile	Define the percentage of top-performing stocks to display.	5%

Example Output

Ticker	Date/Time	Relative Strength	% Gain 1M	% Gain 3M	% Gain 6M	Sector	Industry	Shares Outstanding
MSFT	16/11/2018	4.12	0.97%	1.01%	1.11%	Technology	Software	7,986,999,808
AAPL	16/11/2018	3.72	-10.41%	0.90%	1.03%	Technology	Computers	5,575,000,064

Troubleshooting

No Results Returned

	•	Ensure your watchlist contains symbols with sufficient historical data (e.g., at least 6 months).
	•	Verify that your Norgate data is configured correctly and includes fields like Sector, Industry, and Shares Outstanding.

Amibroker Crash

	•	Reduce the size of your watchlist or ensure sufficient system memory is available.
	•	If necessary, run the script with smaller symbol sets and gradually increase.

Acknowledgments

This script uses Amibroker’s AFL scripting language and integrates with Norgate data to provide robust exploration capabilities. Special thanks to Norgate Data for their excellent database integration.

License

This project is licensed under the MIT License. See the LICENSE file for details.

