[Think Stats Chapter 6 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2007.html#toc60) (household income)

>> With upper bound set at $1,000,000:

>> Mean Income: 74278.7075311872
>> Median Income: 51226.93306562372
>> Standard Deviation: 93946.9299634784
>> Skewness: 4.949920244429579
>> Pearson's skewness: 0.7361105192428788
>> Percent of respondants below mean income: 66.0005879566872

>> ![mean](img/mean%20household%20income.png)  


>> With upper bound set at $10,000,000:
>> Mean Income: 124267.39722164703
>> Median Income: 51226.93306562372
>> Standard Deviation: 559608.5013743478
>> Skewness: 11.603690267537795
>> Pearson's skewness: 0.39156194362653113
>> Percent of respondants below mean income: 85.65630665207664


>> ![mean2](img/mean%20household%20income 2.png)  
```
import thinkstats2
import thinkplot
import hinc
import numpy as np
import math




def InterpolateSample(df, log_upper=6.0):
    """Makes a sample of log10 household income.

    Assumes that log10 income is uniform in each range.

    df: DataFrame with columns income and freq
    log_upper: log10 of the assumed upper bound for the highest range

    returns: NumPy array of log10 household income
    """
    # compute the log10 of the upper bound for each range
    df['log_upper'] = np.log10(df.income)

    # get the lower bounds by shifting the upper bound and filling in
    # the first element
    df['log_lower'] = df.log_upper.shift(1)
    df.loc[0, 'log_lower'] = 3.0

    # plug in a value for the unknown upper bound of the highest range
    df.loc[41, 'log_upper'] = log_upper
    
    # use the freq column to generate the right number of values in
    # each range
    arrays = []
    for _, row in df.iterrows():
        vals = np.linspace(row.log_lower, row.log_upper, row.freq)
        arrays.append(vals)

    # collect the arrays into a single sample
    log_sample = np.concatenate(arrays)
    return log_sample


## Skewness and related functions borrowed from Think Stats book
def RawMoment(xs, k):
    return sum(x**k for x in xs) / len(xs)
def CentralMoment(xs, k):
    mean = RawMoment(xs, 1)
    return sum((x - mean)**k for x in xs) / len(xs)
def StandardizedMoment(xs, k):
    var = CentralMoment(xs, 2)
    std = math.sqrt(var)
    return CentralMoment(xs, k) / std**k
def Skewness(xs):
    return StandardizedMoment(xs, 3)

def main():
    ## Read income data from hinc.py provided in Think Stats
    income_df = hinc.ReadData()

    ## Get an interpolated sample to model the income data in log scale. Default upper bound is 10^6

    log_sample = InterpolateSample(income_df, log_upper=6.0)

    ## Plot the CDF
    log_cdf = thinkstats2.Cdf(log_sample)
    thinkplot.Cdf(log_cdf)
    #thinkplot.Show(xlabel='Income', ylabel='CDF')

    ## Calculate statistics
    sample = np.power(10, log_sample)

    mean = np.mean(sample)
    median = np.median(sample)
    std = np.std(sample)
    skewness = Skewness(sample)
    pearson = 3*(mean-median)/std

    print ('Mean Income:', mean)
    print ('Median Income:', median)
    print ('Standard Deviation:', std)
    print ('Skewness:', skewness)
    print ("Pearson's skewness:", pearson)

    cdf = thinkstats2.Cdf(sample)
    print ('Percent of respondants below mean income: %r' %(cdf[mean]*100))

if __name__ == '__main__':
    main()
```
