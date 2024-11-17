
Relative Price Strength (RPS) - Amibroker AFL Script

An Amibroker Formula Language (AFL) script for calculating and ranking stocks based on Relative Price Strength (RPS) over customizable time frames. This script is designed for traders and analysts to identify top-performing stocks. It integrates seamlessly with Norgate data to provide sector, industry, and shares outstanding information.

Features

	•	Relative Price Strength (RPS):
	•	Calculates RPS for 1, 3, and 6-month time frames.
	•	Allows combined ranking for 1+3+6 months with a weighted score.
	•	Filtering and Ranking:
	•	Filters and ranks stocks by the top X% based on RPS or combined score.
	•	Customizable Parameters:
	•	Dynamic configuration for time frames, top percentage of stocks, and ranking mode (individual or combined).
	•	Output Information:
	•	Percentage gain for 1, 3, and 6 months.
	•	Sector, industry, and shares outstanding for each stock.
	•	Results for the most recent trading day only.

Requirements

	1.	Amibroker:
	•	Amibroker software installed on your system.
	2.	Norgate Data:
	•	Norgate data must be linked with Amibroker to access sector, industry, and shares outstanding fields.
	3.	AFL Script:
	•	The script file RelativePriceStrength.afl included in this repository.

Setup Instructions

	1.	Clone this Repository:



	2.	Copy the Script:
	•	Save the RelativePriceStrength.afl file to your Amibroker Formulas/Custom directory:

C:\Program Files\Amibroker\Formulas\Custom\


	3.	Open Amibroker:
	•	Launch Amibroker and select your database (e.g., Norgate).
	4.	Load the Script:
	•	Go to Analysis > New Analysis.
	•	Select RelativePriceStrength.afl from the dropdown menu.

Usage Instructions

	1.	Configure Parameters:
	•	Open the Parameters window in Amibroker.
	•	Adjust the following settings:
	•	Time Frames: 1-Month, 3-Month, 6-Month.
	•	Top Percentage: Customize the top percentage of stocks to display (default: 5%).
	•	Combined Mode: Toggle between individual or combined 1+3+6-month ranking.
	2.	Run the Exploration:
	•	In the Analysis window, click Explore.
	•	Ensure the date range is set to All Quotes or your desired range.
	3.	Review the Results:
	•	The output includes:
	•	% Gain 1 Month, % Gain 3 Months, % Gain 6 Months.
	•	Relative Price Strength or Combined Score.
	•	Sector, Industry, and Shares Outstanding.

Script Explanation

Below is the RelativePriceStrength.afl script included in this repository:

// User-configurable parameters
period1 = Param("1-Month Time Frame", 22, 1, 252, 1);
period3 = Param("3-Month Time Frame", 66, 1, 252, 1);
period6 = Param("6-Month Time Frame", 132, 1, 252, 1);
topPercent = Param("Top % to Show", 5, 1, 100, 1);
combinePeriods = ParamToggle("Combined 1+3+6 Scan", "No|Yes", 0);

// Calculate relative price strength
RPS1 = C / Ref(C, -period1);
RPS3 = C / Ref(C, -period3);
RPS6 = C / Ref(C, -period6);

// Percentage gain calculations
Gain1 = 100 * (C - Ref(C, -period1)) / Ref(C, -period1);
Gain3 = 100 * (C - Ref(C, -period3)) / Ref(C, -period3);
Gain6 = 100 * (C - Ref(C, -period6)) / Ref(C, -period6);

// Combined score calculation for 1+3+6 months
CombinedScore = 2 * RPS1 + RPS3 + RPS6;

// Filtering and Ranking Logic
Filter = 1;
if (combinePeriods)
{
    RankCombined = 100 * CombinedScore / Highest(CombinedScore);
    Filter = RankCombined > (100 - topPercent);
    AddColumn(CombinedScore, "Combined Score", 1.2);
}
else
{
    timeFrame = ParamList("Select Time Frame", "1 Month|3 Months|6 Months", 0);
    if (timeFrame == "1 Month") { RankRPS = 100 * RPS1 / Highest(RPS1); }
    else if (timeFrame == "3 Months") { RankRPS = 100 * RPS3 / Highest(RPS3); }
    else if (timeFrame == "6 Months") { RankRPS = 100 * RPS6 / Highest(RPS6); }
    Filter = RankRPS > (100 - topPercent);
}

// Find the most recent bar in the dataset
RecentBar = BarIndex() == LastValue(BarIndex());
RecentDate = DateNum() * RecentBar;

// Apply the filter for the most recent date
Filter = Filter AND RecentBar;

// Exploration Output
AddColumn(Gain1, "% Gain 1 Month", 1.2);
AddColumn(Gain3, "% Gain 3 Months", 1.2);
AddColumn(Gain6, "% Gain 6 Months", 1.2);
AddColumn(IIf(combinePeriods, CombinedScore, RPS1), "Relative Price Strength", 1.2);

// Sector, Industry, and Shares Outstanding
AddTextColumn(SectorID(Mode=1), "Sector Name", 1.2, colorDefault, colorDefault, 90);
AddTextColumn(IndustryID(Mode=1), "Industry Name", 1.2, colorDefault, colorDefault, 90);
AddColumn(GetFnData("SharesOut"), "Shares Outstanding", 1.2);

// Title
if (combinePeriods)
    Title = "Top " + topPercent + "% Stocks by Combined 1+3+6 Month Score";
else
    Title = "Top " + topPercent + "% Stocks by " + timeFrame + " Relative Price Strength";

Troubleshooting

Common Issues

	1.	No Results Displayed:
	•	Ensure valid Norgate data is loaded for all symbols.
	•	Check parameter settings (e.g., top percentage, time frame).
	2.	Missing Sector or Industry Data:
	•	Confirm that Norgate data provides SectorID, IndustryID, and SharesOut fields.
	3.	Warnings About LastValue():
	•	The script resolves this by using BarIndex() to fetch the most recent bar without “future-looking.”

Contribution

Contributions are welcome! Feel free to open issues or submit pull requests for improvements.

License

This project is licensed under the MIT License. See the LICENSE file for details.

How to Export Results

	1.	After running the exploration, view the results in the Result List.
	2.	Click the Export button to save the results as a .csv file for external analysis.

