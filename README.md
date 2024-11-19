
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

// User Parameters
TimeFrame1 = ParamList("Select Time Frame", "1m|3m|6m|1+3+6m", 0);
TopPercent = Param("Top Percentage (%)", 5, 1, 100, 1);

// Function to get closing price N months ago
Function GetCloseMonthsAgo(N)
{
    BarsAgo = N * 21; // Assuming ~21 trading days per month
    return IIf(BarsAgo <= BarCount, Ref(Close, BarsAgo), Null);
}

// Initialize variables
RelativeStrength = 1;
Gain1 = 0;
Gain3 = 0;
Gain6 = 0;
Score = 0;

// Calculate relative strength based on selected time frame
if (TimeFrame1 == "1m")
{
    PastClose1 = Nz(GetCloseMonthsAgo(1), 1);
    RelativeStrength = Close / PastClose1;
    Gain1 = (RelativeStrength - 1) * 100;
}
else if (TimeFrame1 == "3m")
{
    PastClose3 = Nz(GetCloseMonthsAgo(3), 1);
    RelativeStrength = Close / PastClose3;
    Gain3 = (RelativeStrength - 1) * 100;
}
else if (TimeFrame1 == "6m")
{
    PastClose6 = Nz(GetCloseMonthsAgo(6), 1);
    RelativeStrength = Close / PastClose6;
    Gain6 = (RelativeStrength - 1) * 100;
}
else if (TimeFrame1 == "1+3+6m")
{
    RS1 = Close / Nz(GetCloseMonthsAgo(1), 1);
    RS3 = Close / Nz(GetCloseMonthsAgo(3), 1);
    RS6 = Close / Nz(GetCloseMonthsAgo(6), 1);
    Score = 2 * RS1 + RS3 + RS6;
    Gain1 = (RS1 - 1) * 100;
    Gain3 = (RS3 - 1) * 100;
    Gain6 = (RS6 - 1) * 100;
    RelativeStrength = Score; // Use Score for ranking in combined mode
}

// Add Columns to Display
AddColumn(Close, "Current Close", 1.2);
AddColumn(RelativeStrength, "Relative Strength", 1.2);

if (TimeFrame1 == "1m")
    AddColumn(Gain1, "% Gain 1m", 1.2);

if (TimeFrame1 == "3m")
    AddColumn(Gain3, "% Gain 3m", 1.2);

if (TimeFrame1 == "6m")
    AddColumn(Gain6, "% Gain 6m", 1.2);

if (TimeFrame1 == "1+3+6m")
{
    AddColumn(Gain1, "% Gain 1m", 1.2);
    AddColumn(Gain3, "% Gain 3m", 1.2);
    AddColumn(Gain6, "% Gain 6m", 1.2);
    AddColumn(Score, "Combined Score", 1.2);
}

// Set Title
Title = "Relative Strength Exploration - " + TimeFrame1 + " - Top " + NumToStr(TopPercent, 0) + "%";

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

