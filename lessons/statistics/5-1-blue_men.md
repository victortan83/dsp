[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)

>> In the BRFSS (see Section 5.4), the distribution of heights is roughly normal with parameters µ = 178 cm and σ = >>7.7 cm for men, and µ = 163 cm and σ = 7.3 cm for women.

>>In order to join Blue Man Group, you have to be male between 5’10” and 6’1” (see http://bluemancasting.com). What >>percentage of the U.S. male population is in this range? Hint: use scipy.stats.norm.cdf.
>>scipy.stats contains objects that represent analytic distributions

```
import scipy.stats
#For example scipy.stats.norm represents a normal distribution.
## Set up the normal distribution with mean and standard deviation of male height
mu = 178
sigma = 7.7
dist = scipy.stats.norm(loc=mu, scale=sigma)
type(dist)
>>scipy.stats._distn_infrastructure.rv_frozen

#A "frozen random variable" can compute its mean and standard deviation.

dist.mean(), dist.std()
>>(178.0, 7.7)

#It can also evaluate its CDF. How many people are more than one standard deviation below the mean? About 16%
dist.cdf(mu-sigma)
>>0.1586552539314574

#How many people are between 5'10" and 6'1"?
low = dist.cdf((5*12+10)*2.54)     # 5'10"
high = dist.cdf((6*12+1)*2.54)      # 6'1"
low, high, high-low
>>(0.48963902786483265, 0.8317337108107857, 0.3420946829459531)
```
>>So about 34.21% of US male population are within the height range
