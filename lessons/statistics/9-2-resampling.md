[Think Stats Chapter 9 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2010.html#toc90) (resampling)

>> In Section 9.3, we simulated the null hypothesis by permutation; that is, we treated the observed values as if they 
>> represented the entire population, and randomly assigned the members of the population to the two groups.

>> An alternative is to use the sample to estimate the distribution for the population, then draw a random sample from 
>> that distribution. This process is called resampling. There are several ways to implement resampling, but one of the 
>> simplest is to draw a sample with replacement from the observed values, as in Section 9.10.

>> Write a class named DiffMeansResample that inherits from DiffMeansPermute and overrides RunModel to implement 
>> resampling, rather than permutation.

>> Use this model to test the differences in pregnancy length and birth weight. How much does the model affect the
>> results?
```
# Python Code
class DiffMeansResample(DiffMeansPermute):
    """Tests a difference in means using resampling."""
    
    def RunModel(self):
        """Run the model of the null hypothesis.

        returns: simulated data
        """
        group1 = np.random.choice(self.pool, self.n, replace=True)
        group2 = np.random.choice(self.pool, self.m, replace=True)
        return group1, group2
        
def RunResampleTest(firsts, others):
    """Tests differences in means by resampling.

    firsts: DataFrame
    others: DataFrame
    """
    data = firsts.prglngth.values, others.prglngth.values
    ht = DiffMeansResample(data)
    p_value = ht.PValue(iters=10000)
    print('\ndiff means resample preglength')
    print('p-value =', p_value)
    print('actual =', ht.actual)
    print('ts max =', ht.MaxTestStat())

    data = (firsts.totalwgt_lb.dropna().values,
            others.totalwgt_lb.dropna().values)
    ht = DiffMeansPermute(data)
    p_value = ht.PValue(iters=10000)
    print('\ndiff means resample birthweight')
    print('p-value =', p_value)
    print('actual =', ht.actual)
    print('ts max =', ht.MaxTestStat())
    
 RunResampleTest(firsts, others)
 ```
 >> diff means resample preglength
 
 >> p-value = 0.1665
 
 >> actual = 0.07803726677754952
 
 >> ts max = 0.23979257789515174
 
 

 >> diff means resample birthweight
 
 >> p-value = 0.0
 
 >> actual = 0.12476118453549034
 
 >> ts max = 0.119250558598619
 
 ```
 # Conclusions: Using resampling instead of permutation has very
# little effect on the results.

# The two models are based on slightly difference assumptions, and in
# this example there is no compelling reason to choose one or the other.
# But in general p-values depend on the choice of the null hypothesis;
# different models can yield very different results.
