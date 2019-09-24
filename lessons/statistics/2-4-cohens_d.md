[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)



        First borns are lighter than later born babies by about 2 ounces. But we'll look at a few statistics for details       
        on the difference.

        Mean comparison:
        All babies: 7.265628457623368
        First born: 7.201094430437772
        Others: 7.325855614973262

        First borns are lighter than others by 1.9961789525678455 ounces.

        Variance comparison
        First born: 2.0180273009157768
        Others: 1.9437810258964572
        Cohen's d: -0.0886729270726
    >>     

        ```
        Python Code:

        import nsfg
        import thinkstats2
        import math

        ## First we read the data from the nsfg package and separate them
        ## based on actual birth and their order
        preg = nsfg.ReadFemPreg()
        born = preg[preg.outcome == 1]
        firstborn = born[born.birthord == 1]
        later = born[born.birthord != 1]

        ## Then we compare the difference between first and later babies
        ## with some statistics

        ### Mean:
        mean_born = born.totalwgt_lb.mean()
        mean_firstborn = firstborn.totalwgt_lb.mean()
        mean_later = later.totalwgt_lb.mean()

        print("Comparing averages:")
        print("All babies: %r" %mean_born)
        print("First born: %r" %mean_firstborn)
        print("Others: %r" %mean_later)
        print("First borns are lighter than others by %r ounces." %(abs(mean_firstborn-mean_later)*16))

        var_firstborn = firstborn.totalwgt_lb.var()
        var_later = later.totalwgt_lb.var()

        print("\nComparing variances")
        print("First born: %r" %var_firstborn)
        print("Others: %r" %var_later)

        ## We also modify the function in the book to calculate the Cohen's d to analyze the effect size
        s = (len(firstborn) * var_firstborn + len(later) * var_later) / (len(firstborn)+len(later))
        d = (mean_firstborn - mean_later) / math.sqrt(s)
        print("\nCohen's d:", d)
        ```
