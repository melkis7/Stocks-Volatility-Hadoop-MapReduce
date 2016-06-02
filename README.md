# Stocks-Volatility-Hadoop-MapReduce

This project is used to compute the monthly volatility of about 3000 stocks with each having data of about three years.
There were a total of 40000 files having historical price data of each stock for three years.

Standard deviation of stock volatility is widely used by traders to find out stocks with higher earning potential.
This project helps to find the Top 10 stocks with maximum and minimum volatility.

There are three phases of MapReduce jobs:

In the first phase, the filename and date are extracted from input file (present in input folder). Key is stockname/moth/year and value is the line of data. The reducer receives the input from mapper with values of every stock name and every month combined. The reducer extracts the first and last date of every month and calculate Xi value. Output of first stage is (stockname/moth/year, Xivalue).

In the second phase, the mapper receives the input from the first reducer. The key is stockname/moth/year, the mapper removes the month and year from key and puts the stock name as the key which will combine the Xi values of a stock for all months. The reducer receives the input from the mapper as stock name and it's Xi values (stockname, Xivalues) and it computes the stock volatility using formula. The output of second reduer is (stockname, volatility). We set a counter in job2's configuration and increment it every time when we calculate the volatility of stock and write it in Intermediate file.

In the third phase, mapper receives input as (stockname, volatility). To find the Top 10 stocks with minimum and maximum volatility, the data is sorted using MapReduce internal sorting . In MapReduce internal sorting, the key is always sorted in output. So the key and value received in input are swapped as (volatility, stockname) in this phase.

The next steps are to do performance analysis with different data size, number of nodes and cores.

