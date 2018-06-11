
### Unpaired(Independent) Parametric T-test ###

***

**Where We Can Apply Unpaired Prametric T-test ?**

- two groups needs to be independent
- parametric assumption needs to be satisfy



### Outline for performing Unpaired(independent) Parametric T-test ###

1. Formulate Problem statement(research question) and hypothesis
2. Import data
3. Check independent parametric t-test assumption
4. compute unpaired t-test(based on t-test selection)
5. interpret result



### T-test type and selection ###

Two types:
1. student t-test(if assuming two group variance is equal)
2. welch t-test(if assuming two group variance is uneuqal)

we are going to implement both.. :)

**Important : we used Levene's test for variance equality check **

### Parametric Assumption ###

Assumption 1 : Are the two samples(two group) independent
Assumption 2 : Are the data from each of the 2 groups follow a normal distribution?

**Important : we used Shapiro-Wilk normality test for checking two group normal distribution  **

Shapiro-Wilk return two result first is **w-value** and second is **p-value**


### Implementation section ###

### 1. Problem statement(Research Question)

we have measured the weight of 100 individuals: 50 women (group A) and 50 men (group B).    
**We want to know if the mean weight of women (mA) is significantly different from that of men (mB).**

Bold one is problem statement



### Formulate hypothesis ###

null hypothesis (H0): H0:mA=mB   
alternative hypothesis (Ha): Ha:mA≠mB  (different) 

**Important:two tailed hypothesis**

### 2. Import data ###


```python
import pandas as pd
```


```python
data=pd.read_csv("dataset/mw.csv")
data.shape
```




    (18, 2)




```python
data.shape

```




    (18, 2)



### 3. Parametric Assumptions ###

**Parametric Asumption 1: Are the two samples(two group) independent?   
Conclusion : Yes, since the samples from men and women are not related.**

**Assumtion 2: Are the data from each of the 2 groups follow a normal distribution?**   
  
            
**step 1 : formulate hypothesis **
- null hypothesis (H0): data are normaly distributed   
- alternative hypothesis (Ha): note normaly distributed

**step 2 : if Shapiro-Wilk test result p-values are greater then the significance p-value(0.05) then accept
null hypothesis that means data is normal distributed**

Conclusion:   
man p-value(0.106) > 0.05 hence normal distributed   
woman p-value(0.601) > 0.05 hence normal distributed


```python
import scipy.stats as stats

```


```python
man=data[data['group']!='Woman']
woman=data[data['group']=='Woman']

```


```python
stats.shapiro(man.weight.dropna())

```




    (0.8642458319664001, 0.10660982877016068)




```python
stats.shapiro(woman.weight.dropna())

```




    (0.9426567554473877, 0.6101282835006714)



### Assume Equal variance t-test hence student t-test ###

*** Levene test***

**Problem formulation: both group have equal variance?**   


**step 1 : formulate hypothesis**

null hypothesis (H0): both group have equal variance
alternative hypothesis (Ha): both group have unequal variance

**step 2 : if Levene test result p-values are greater then the significance p-value(0.05) then accept null hypothesis that means both group have equal variance**

Conclusion : p-value(0.2255) > 0.05 hence both group have equal variance



```python
stats.levene(man.weight.dropna(), woman.weight.dropna())

```




    LeveneResult(statistic=1.5888334612432846, pvalue=0.22556594075964187)



### Parametric Normality and Equal Variance Criteria True so we apply student t-test ###


```python
t, p = stats.ttest_ind(man.weight.dropna(), woman.weight.dropna())
t, p
```




    (2.7842353699254567, 0.013265602643801042)



### Interpretation of Result ###

The p-value(0.013)>0.05 which is false so we accept alternate hypothesis which is We can conclude that men’s average weight is significantly different from women’s average weight

### Parametric Normality and unEqual Variance Criteria True so we apply student welch-t-test ###


```python
t, p = stats.ttest_ind(man.weight.dropna(), woman.weight.dropna(),equal_var = False)
t, p
```




    (2.7842353699254567, 0.015384235142669895)


