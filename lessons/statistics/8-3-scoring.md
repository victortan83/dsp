[Think Stats Chapter 8 Exercise 3](http://greenteapress.com/thinkstats2/html/thinkstats2009.html#toc77)

>> ---

>> In games like hockey and soccer, the time between goals is roughly exponential. So you could estimate a team’s goal-
>> scoring rate by observing the number of goals they score in a game. This estimation process is a little different from sampling the time between goals, so let’s see how it works.

>> Write a function that takes a goal-scoring rate, lam, in goals per game, and simulates a game by generating the time 
>> between goals until the total time exceeds 1 game, then returns the number of goals scored.

>> Write another function that simulates many games, stores the estimates of lam, then computes their mean error and RMSE.

>> Is this way of making an estimate biased?

>> ---
```
# Python Code
def SimulateGame(lam):
    """Simulates a game and returns the estimated goal-scoring rate.

    lam: actual goal scoring rate in goals per game
    """
    goals = 0
    t = 0
    while True:
        time_between_goals = random.expovariate(lam)
        t += time_between_goals
        if t > 1:
            break
        goals += 1

    # estimated goal-scoring rate is the actual number of goals scored
    L = goals
    return L
    
    
# The following function simulates many games, then uses the
# number of goals scored as an estimate of the true long-term
# goal-scoring rate.

def Estimate6(lam=2, m=1000000):

    estimates = []
    for i in range(m):
        L = SimulateGame(lam)
        estimates.append(L)

    print('Experiment 4')
    print('rmse L', RMSE(estimates, lam))
    print('mean error L', MeanError(estimates, lam))
    
    pmf = thinkstats2.Pmf(estimates)
    thinkplot.Hist(pmf)
    thinkplot.Config(xlabel='Goals scored', ylabel='PMF')
    
Estimate6()
```
>> Experiment 4
>> rmse L 1.4127193634972235
>> mean error L -0.001214
>> ![estimate](img/estimate.png) 
```
# My conclusions:

# 1) RMSE for this way of estimating lambda is 1.4

# 2) The mean error is small and decreases with m, so this estimator
#    appears to be unbiased.

# One note: If the time between goals is exponential, the distribution
# of goals scored in a game is Poisson.

# See https://en.wikipedia.org/wiki/Poisson_distribution
```
