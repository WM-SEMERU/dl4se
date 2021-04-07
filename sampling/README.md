# Sampling Strategies

> *Last update:* April 2021; *Maintaned by* @danaderp

To draw conclusions for the SLR, we had to employ different techniques to obtain a representative sample of DL4SE papers. We want to reduce the risk of "sampling bias" by introducing a combined strategy of Non-probability and Probability Sampling. The goal of the sampling is to provide quantifiable evidence about the statistical significance of the results based on the SLR methodology. 

## Non-Probability Sampling
The non-probability sampling is -indeed- the SLR methodology. If we analyze this methodology (Fig 2 on the paper) initially introduced by Kitchenham, then we notice it is based on non-probability sampling methods (purposive and snowball). The problem with this non-probability sampling method is the higher risk of sampling bias. That's why we decide to diversify the population. Please refer to the SLR Methodology for a detailed explanation. 

## Probability Sampling
We employed "Stratified Sampling" to compute the proportion of samples by conference and reduce the risk of sampling bias. This sampling was performed for a population of the top 24 venues in Software Engineering and six most influential venues in AI (for a total of 30 venues). The stratified sampling was performed with confidence values of 90% and 99%; and a margin error of 5 papers. Then, we calculate the relative difference between the SLR methodology and the stratified sampling to quantify how distant is the SLR methodology from statistically significant samplings. The relative difference Δ = (random sampling - SLR sampling)/f(random sampling, SLR sampling), where f(a,b) = max(a,b). 

![Sampling Table](https://wm-semeru.github.io/dl4se/sampling/samplingTable.PNG)

For a population of 2184 papers from 30 venues, the required conventional sampling at 90% confidence is *243* papers and at 99% is *511* papers. On the other hand, the first non-random sampling has a total of *1121* papers and the second non-random sampling (aka the whole SLR methodology) has a total of *111* papers. The relative difference Δ at 99% is on average 0.39 [median: 0.44] and at 90% is on average 0.29 [median: 0.14]. In summary, the SLR methodology has a relative difference of 0.29 for a confidence level of 90% with a margin error of 5 papers. 
