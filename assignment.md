General
=======
Two months of market data are provided below. File format is CSV, each day is in its own file but the data is 24/7 continuous. All data is in chronological order and all timestamps are in milliseconds since the UNIX epoch, in the UTC timezone. Each row is a 1-second OHLC bar (open, high, low, close), also known as candlestick, japanese candle, k-line, etc. and essentially contains the asset price data observed in the market during that 1 second. All prices are in USD. The input file has the following columns:

Name|Description
:-|:-
open_time|Timestamp at the start of the bar
open|Price at the opening of the bar
high|Highest price during the bar
low|Lowest price during the bar
close|Price at the end of the bar
volume|Sum of asset quantity traded during the bar
close_time|Timestamp at the end of the bar
count|Count of market trades that occured within the bar time


Input data files
===========
* [ETH_2023-08.zip](https://emventures.io/assignment/ETH_2023-08.zip)
* [ETH_2023-09.zip](https://emventures.io/assignment/ETH_2023-09.zip)

Instructions
============
* Explore the provided data and develop a model that can identify and predict price trends / momentums, i.e. when the price consistently moves upwards or downwards for an extended period, and also when the trend is over
* The model's lookback window should not exceed 1 minute (60 lines of input), but it can be less
* Notice there are a couple more features in the data other than prices - you are free to use those as well, and combine them in various ways; some examples:
	* Calculate turnover in USD by multiplying `volume` by the `close` price (or some form of weighted price)
	*	Calculate the average trade size by dividing `volume` by `count`
* Using this model build a program to process each input file **line-by-line** and compute a score after each line that conveys how strong the current trend is and in which direction (upwards or downwards)
* The score should be normalized to the interval `[-1, 1]` with certain thresholds:

	Range|Description|Recommended action
	:-|:-|:-
	`[-1.00, -0.75]`|Strong downward trend|Sell
	`(-0.75, -0.50]`|Weak downward trend|Sell hold
	`(-0.50,  0.50)`|Neutral|Close
	`[ 0.50,  0.75)`|Weak upwards trend|Buy hold
	`[ 0.75,  1.00]`|Strong upwards trend|Buy
* Below are the descriptions of the recommended actions in the table above (being in a position in this context means having exposure to the asset):

	Action|Description
	:-|:-
	Sell|Sell the asset, i.e. enter a short position
	Sell hold|If currently in a sell position then hold, otherwise close
	Close|If currently in any position then close it
	Buy hold|If currently in a buy position then hold, otherwise close
	Buy|Buy the asset, i.e. enter a long position
* For the purpose of this assignment we ignore the existence of trading fees, carry costs, slippage, friction, liquidity, latency, etc. and focus purely on price movements. We also don't have to worry about how to execute trades or manage positions - that's a problem for the execution layer not us. Specifically do not worry about how we can open and close short positions, or any funds we might have in our accounts. The goal of this program is only to recommend a certain action in a specific time, not to act on it
* The program should output a CSV file where each output line corresponds to a line in the input file, with the format `time, score` where `time` corresponds to `close_time` from the input line
* It should be possible to feed the program any CSV file that conforms to the input format and receive a corresponding output CSV file
* All code should be written in Python
