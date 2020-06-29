[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

First babies in the data have a mean birth weight of approximately **7.201 lbs** with a standard deviation of **1.421 lbs**, making them slightly lighter on average than non-first babies with an average of **7.326 lbs** and standard deviation **1.394 lbs.** The difference is **0.124 lbs**, roughly **0.017%** of mean live birth weight. 
Cohen's d tells us there is a **0.0887** standard deviation difference between these means, implying birth order is largely unimpactful on birth weight, with only a very slight tendency towards lighter first babies. 
Cohen states that "Mean pregnancy length for first babies is 38.601; for other babies it is 38.523. The difference is 0.078 weeks, which works out to 13 hours. As a fraction of the typical pregnancy length, this difference is about 0.2%." The relative difference as a proportion of the overall means is comparable across both birth weight and pregnancy length, both being approximately 0.2%. 
Cohen's d for pregnancy lengths comes out to a roughly **0.0289** standard deviations difference for the two groups. This is smaller by nearly a factor of 3 compared to that of birth weight (0.0887), but in absolute terms is similarly minute.

'''
import nsfg
import math
import numpy as np

# generate Cohen's d
def CohenEffectSize(group1, group2):
    diff = group1.mean() - group2.mean()

    var1 = group1.var()
    var2 = group2.var()
    n1, n2 = len(group1), len(group2)

    pooled_var = (n1 * var1 + n2 * var2) / (n1 + n2)
    d = diff / np.sqrt(pooled_var)
    return d

# create df of live births
preg = nsfg.ReadFemPreg()
live = preg[preg.outcome == 1]

# split into first births vs others
firsts = live[live.birthord == 1]
others = live[live.birthord != 1]

# print descriptive stats and Cohen's d
weights = (firsts['totalwgt_lb'], others['totalwgt_lb'])
for i in weights:
    print('mean: ' + str(i.mean()))
    print('var: ' + str(i.var()))
    print('std: ' + str(i.std()) + '\n')

print("Mean Difference: " + str(firsts['totalwgt_lb'].mean() - others['totalwgt_lb'].mean()))
print("% of Overall Mean Weight: " + str((firsts['totalwgt_lb'].mean() - others['totalwgt_lb'].mean()) / live['totalwgt_lb'].mean()))
    
print("\nCohen's d (weight): \n" + str(CohenEffectSize(firsts['totalwgt_lb'], others['totalwgt_lb'])))
print("\nCohen's d (length): \n" + str(CohenEffectSize(firsts['prglngth'], others['prglngth'])))

'''

![output](2-4_output)
