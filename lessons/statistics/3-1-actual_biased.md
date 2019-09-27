[Think Stats Chapter 3 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2004.html#toc31) (actual vs. biased)


>> In this exercise, we look at the difference with actual and biased PMF based on the number of
>> children under 18 in the NSFG respondent data.

>> Running the Python code below, we see that the actual distribution has a mean of **1.0242**,
>> while the biased distribution is **2.4036** - a significant increase due to the bias.

>> And here's a graph of the 2 distributions:  
>> ![pre](img/actual%20vs%20biased.png)  



```
#Python Code

import nsfg
import thinkstats2
import thinkplot

## Load data from previous
resp = nsfg.ReadFemResp()

## Create pmf and plot it
pmf = thinkstats2.Pmf(resp.numkdhh, label='numkdhh')
thinkplot.Pmf(pmf)
thinkplot.Config(xlabel='Number of children', ylabel='PMF')

## Function to find Biased PMF based on Think Stats text
def BiasPmf(pmf, label):
    """Returns the Pmf with oversampling proportional to value.

    If pmf is the distribution of true values, the result is the
    distribution that would be seen if values are oversampled in
    proportion to their values; for example, if you ask students
    how big their classes are, large classes are oversampled in
    proportion to their size.

    Args:
      pmf: Pmf object.
      label: string label for the new Pmf.

     Returns:
       Pmf object
    """
    new_pmf = pmf.Copy(label=label)

    for x, p in pmf.Items():
        new_pmf.Mult(x, x)

    new_pmf.Normalize()
    return new_pmf

## Create a biased PMF
biased = BiasPmf(pmf, label='biased')

## Plot both PMFs in comparison
thinkplot.PrePlot(2, cols=2)
thinkplot.Pmfs([pmf, biased])
thinkplot.Config(xlabel='Number of children', ylabel='PMF')

## Compare their means:
print('Mean of PMF: %r' %pmf.Mean())

print('Mean of Biased PMF: %r' %biasedpmf.Mean())

```

