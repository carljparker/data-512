---
jupyter:
  jupytext:
    formats: ipynb,md,py:percent
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.7.1
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

# Exploration of Gender Differences when Detecting and Assessing Agression and Toxicity in Wikipeda Talk Comments


## Introduction and objective


I want to explore two questions using the data in these datasets. Both questions consider possible gender differences in the way the crowdworkers interpreted the Talk Page comments. One of the alleged differences between men and women is that woman report higher Agreeableness scores in measures of the Big Five personality traits. Another is that women have greater empathy than men. (See references below.)

The difference in Agreeableness suggests the possibility that women might have--in some sense--greater reactivity to aggression than men and therefore might be more likely to flag it in the Talk Page comments. Similarly, for toxicity. Here we are operating on the assumption that Agreeableness, as a personality trait, is at odds with both aggression and toxicity.

Empathy is associated with understanding the mental or emotional state of another person. If women in general have higher empathy than men, then this could manifest in multiple ways. Women might tend to assess aggression or toxicity in the Wikipedia talk comments with greater _precision_ than men. Or women might simply tend to assess higher level of aggression and toxicity than men.

Note that detecting aggression/toxicity and assessing its level are somewhat distinct. For example, men and women could agree on which comments are toxic, but in general, disagree about the level of toxicity. There is also the scenario where men and women seem to disagree on which comments are toxic, but when they do agree, they rate the toxicity level similarly; admittedly, this second scenario seems less likely.

-  Question 1: Do women have greater sensitivity than men _in detecting_ instances of a) aggression or b) toxicity in the Talk Page comments--related to differences in Agreeableness.
-  Question 2: Do women have greater sensitivity than men _to the level_ of a) aggression or b) toxicity in the Talk Page comments--related to differences in empathy?


## References


- Weisberg, Yanna J et al. “Gender Differences in Personality across the Ten Aspects of the Big Five.” Frontiers in psychology vol. 2 178. 1 Aug. 2011, [doi:10.3389/fpsyg.2011.00178](https://dx.doi.org/10.3389%2Ffpsyg.2011.00178)
- Christov-Moore, Leonardo et al. “Empathy: gender effects in brain and behavior.” Neuroscience and biobehavioral reviews vol. 46 Pt 4, Pt 4 (2014): 604-27, [doi:10.1016/j.neubiorev.2014.09.001](https://dx.doi.org/10.1016%2Fj.neubiorev.2014.09.001).
- Mestre, María Vicenta, et al. “Are Women More Empathetic than Men? A Longitudinal Study in Adolescence.” The Spanish Journal of Psychology, vol. 12, no. 1, 2009, pp. 76–83., [doi:10.1017/S1138741600001499](https://doi.org/10.1017/S1138741600001499).




# Import required libraries and modules

```python
import pandas as pd
from matplotlib import pyplot as plt
```

# Data preparation


In this section, we load the datasets, merge them, and filter them, to enable our analysis.


## Load and inspect the aggression tables using pandas

```python
#
# Aggression data set 
#
aggression_annotations = pd.read_csv("aggression_annotations.tsv", delimiter="\t")
aggression_annotated_comments = pd.read_csv("aggression_annotated_comments.tsv", delimiter="\t")
aggression_worker_demographics = pd.read_csv("aggression_worker_demographics.tsv", delimiter="\t")
```

Briefly inspect the annotation and demographic tables.

```python
aggression_annotations.shape
```

```python
aggression_annotations.head()
```

```python
aggression_worker_demographics.shape
```

```python
aggression_worker_demographics.head()
```

## Join annotation table to demographics table [Aggression]

```python
joined_annotations = pd.merge( aggression_annotations, aggression_worker_demographics, left_on="worker_id", right_on="worker_id")
```

```python
joined_annotations.shape
```

```python
joined_annotations.head()
```

**Note:** In the merge, we seem to have lost a large number (37%) of the records. 

```python
num_records_dropped = aggression_annotations.shape[0] - joined_annotations.shape[0]
percent_records_dropped = ( num_records_dropped / aggression_annotations.shape[0] )

'Records lost in merge: {0:,} ({1:.2%})'.format( num_records_dropped, percent_records_dropped )
```

## Separate the records by gender [Aggression]


Filter on only the male workers.

```python
male_joined_aggression = joined_annotations[ ( joined_annotations.gender == "male" ) ]
```

```python
male_joined_aggression.shape
```

```python
male_joined_aggression.head()
```

```python
male_joined_aggression.tail()
```

Now, filter on the female workers.

```python
female_joined_aggression = joined_annotations[ ( joined_annotations.gender == "female" ) ]
```

```python
female_joined_aggression.shape
```

**Note:** There are significantly fewer female workers than male workers.

```python
num_male = male_joined_aggression.shape[0]
num_female = female_joined_aggression.shape[0]
total_workers = num_male + num_female
'Male workers: {0:,} ({1:.2%}) | Female workers: {2:,} ({3:.2%})'.format( num_male, ( num_male / total_workers ), num_female, ( num_female / total_workers ) )
```

```python
female_joined_aggression.head()
```

```python
female_joined_aggression.tail()
```

## Load and inspect the toxicity tables using pandas

```python
#
# Toxicity data set
#
toxicity_annotations = pd.read_csv("toxicity_annotations.tsv", delimiter="\t")
toxicity_annotated_comments = pd.read_csv("toxicity_annotated_comments.tsv", delimiter="\t")
toxicity_worker_demographics = pd.read_csv("toxicity_worker_demographics.tsv", delimiter="\t")
```

Briefly inspect the annotation and demographic tables.

```python
toxicity_annotations.shape
```

```python
toxicity_annotations.head()
```

```python
toxicity_worker_demographics.shape
```

```python
toxicity_worker_demographics.head()
```

## Join annotation table to demographics table [Toxicity]

```python
joined_annotations = pd.merge( toxicity_annotations, toxicity_worker_demographics, left_on="worker_id", right_on="worker_id")
```

```python
joined_annotations.shape
```

```python
joined_annotations.head()
```


**Note:** In this merge, we seem to have lost fewer records (`1.34%`) than when we merged the aggression dataset. 

```python
num_records_dropped = aggression_annotations.shape[0] - joined_annotations.shape[0]
percent_records_dropped = ( num_records_dropped / aggression_annotations.shape[0] )

'Records lost in merge: {0:,} ({1:.2%})'.format( num_records_dropped, percent_records_dropped )
```

## Separate the records by gender [Toxicity]


Filter on only the male workers.

```python
male_joined_toxicity = joined_annotations[ ( joined_annotations.gender == "male" ) ]
```

```python
male_joined_toxicity.shape
```

```python
male_joined_toxicity.head()
```

```python
male_joined_toxicity.tail()
```

Now, filter on the female workers.

```python
female_joined_toxicity = joined_annotations[ ( joined_annotations.gender == "female" ) ]
```

```python
female_joined_toxicity.shape
```

**Note:** There are significantly fewer female workers than male workers.

```python
num_male = male_joined_toxicity.shape[0]
num_female = female_joined_toxicity.shape[0]
total_workers = num_male + num_female
'Male workers: {0:,} ({1:.2%}) | Female workers: {2:,} ({3:.2%})'.format( num_male, ( num_male / total_workers ), num_female, ( num_female / total_workers ) )
```

```python
female_joined_toxicity.head()
```

```python
female_joined_toxicity.tail()
```

# Question 1: Do women have greater sensitivity than men _in detecting_ instances of a) aggression or b) toxicity in the Talk Page comments--related to differences in Agreeableness.


What is the percentage of comments that were flagged as _**aggression**_ by men versus the percentage that were flagged by women?

```python
male_percent_flagged_aggression = male_joined_aggression[ 'aggression' ].sum() / male_joined_aggression[ 'aggression' ].count()
female_percent_flagged_aggression = female_joined_aggression[ 'aggression' ].sum() / female_joined_aggression[ 'aggression' ].count()
'Aggression :: Male: {0:.2%} vs Female: {1:.2%} | Delta: {2:.2%}'.format( male_percent_flagged_aggression, female_percent_flagged_aggression, abs( male_percent_flagged_aggression - female_percent_flagged_aggression) )
```

What is the percentage of comments that were flagged as _**toxicity**_ by men versus the percentage that were flagged by women?

```python
male_percent_flagged_toxicity = male_joined_toxicity[ 'toxicity' ].sum() / male_joined_toxicity[ 'toxicity' ].count()
female_percent_flagged_toxicity = female_joined_toxicity[ 'toxicity' ].sum() / female_joined_toxicity[ 'toxicity' ].count()
'Toxicity :: Male: {0:.2%} vs Female: {1:.2%} | Delta: {2:.2%}'.format( male_percent_flagged_toxicity, female_percent_flagged_toxicity, abs( male_percent_flagged_toxicity - female_percent_flagged_toxicity) )
```

```python
#
# Create a 1 row x 2 col grid for our plots
#
fig, axarr = plt.subplots(1, 2, figsize=(12, 5))

#
# First plot
#
df_aggression_plot_data = pd.DataFrame({'Gender':['Male', 'Female'], 'Percentage':[male_percent_flagged_aggression * 100, female_percent_flagged_aggression * 100]})
df_aggression_plot_data.plot.bar(x='Gender', y='Percentage', rot=0, title='Percent of annotations flagged as aggressive', ax = axarr[0])

#
# Second plot
#
df_toxicity_plot_data = pd.DataFrame({'Gender':['Male', 'Female'], 'Percentage':[male_percent_flagged_toxicity * 100, female_percent_flagged_toxicity * 100]})
df_toxicity_plot_data.plot.bar(x='Gender', y='Percentage', rot=0, title='Percent of annotations flagged as toxic', ax = axarr[1])

#
# Save the dataframe.
#
df_aggression_plot_data.to_csv( 'csv/aggression-plot-data.csv', index_label = 'Id' )
df_toxicity_plot_data.to_csv( 'csv/toxicity-plot-data.csv', index_label = 'Id' )

#
# Save the plot with the following incantation.
#
figure_1 = axarr[0].get_figure()
figure_1.savefig('png/gender-compare-aggress-tox-bar.png')
```

We _do_ see a difference between how likely males and females are to report that a comment was aggressive or toxic. The difference occurs in both datasets and the difference has similar size in both. (See also: The **Conclusions** section (before the appendix) near the end of this notebook.


# Question 2: Do women have greater sensitivity than men _to the level_ of a) aggression or b) toxicity in the Talk Page comments--related to differences in empathy? 


Compare male and female scores (aggression and toxcity) to see if males and females differ in the way that they assess levels of these attributes.


## Create histograms to compare male and female aggression scores

```python
figure_size = [ 18, 5 ]
plt.subplot(1, 2, 1)

male_joined_aggression[ 'aggression_score' ].hist( figsize = figure_size )
plt.ylabel("Number of ratings",fontsize=12)
plt.xlabel("Aggression score",fontsize=12)
plt.title('Males: aggression score', fontsize=15)
plt.grid(None)
#
# Summary statistics: mean and std
#
mean_male = male_joined_aggression[ 'aggression_score' ].mean()
std_male = male_joined_aggression[ 'aggression_score' ].std()
s = "Mean: {0:.3}\nStd: {1:.3}".format( mean_male, std_male)
plt.text(1.5, 300000, s )
#
# Indicate mean and std with vertical lines
#
plt.axvline(x=mean_male, color='red')
plt.axvline(x=mean_male + std_male, color='green')
plt.axvline(x=mean_male - std_male, color='green')


plt.subplot(1, 2, 2)

female_joined_aggression[ 'aggression_score' ].hist( figsize = figure_size )
plt.ylabel("Number of ratings",fontsize=12)
plt.xlabel("Aggression score",fontsize=12)
plt.title('Females: aggression score', fontsize=15)
plt.grid(None)
#
# Summary statistics: mean and std
#
mean_female = female_joined_aggression[ 'aggression_score' ].mean()
std_female = female_joined_aggression[ 'aggression_score' ].std()
s = "Mean: {0:.3}\nStd: {1:.3}".format( mean_female, std_female)
plt.text(1.5, 200000, s )
#
# Indicate mean and std with vertical lines
#
plt.axvline(x=mean_female, color='red')
plt.axvline(x=mean_female + std_female, color='green')
plt.axvline(x=mean_female - std_female, color='green')

#
# Save the dataframe.
#
male_joined_aggression.to_csv( 'csv/male-aggression-assess-data.csv', index_label = 'Id' )
female_joined_aggression.to_csv( 'csv/female-aggression-assess-data.csv', index_label = 'Id' )

#
# Save the plot as PNG
#
plt.savefig('png/gender-aggression-assessment.png')

```

We _aren't_ seeing much difference here. The means differ by `0.069` units and the standard deviations differ by `0.002`. Women appear to rate aggressiveness at `-3` more than `-2` and men the reverse, but without further analysis, I'm not sure how much we can make of that. Otherwise, even the shapes of the histograms are similar.


## Create histograms to compare male and female toxicity scores

```python
figure_size = [ 18, 5 ]
plt.subplot(1, 2, 1)

male_joined_toxicity[ 'toxicity_score' ].hist( figsize = figure_size )
plt.ylabel("Number of ratings",fontsize=12)
plt.xlabel("Toxicity score",fontsize=12)
plt.title('Males: toxicity score', fontsize=15)
plt.grid(None)

#
# Summary statistics: mean and std
#
mean_male = male_joined_toxicity[ 'toxicity_score' ].mean()
std_male = male_joined_toxicity[ 'toxicity_score' ].std()
s = "Mean: {0:.3}\nStd: {1:.3}".format( mean_male, std_male)
plt.text(1.5, 375000, s )

#
# Indicate mean and std with vertical lines
#
plt.axvline(x=mean_male, color='red')
plt.axvline(x=mean_male + std_male, color='green')
plt.axvline(x=mean_male - std_male, color='green')


plt.subplot(1, 2, 2)

female_joined_toxicity[ 'toxicity_score' ].hist( figsize = figure_size )
plt.ylabel("Number of ratings",fontsize=12)
plt.xlabel("Toxicity score",fontsize=12)
plt.title('Females: toxicity score', fontsize=15)
plt.grid(None)

#
# Summary statistics: mean and std
#
mean_female = female_joined_toxicity[ 'toxicity_score' ].mean()
std_female = female_joined_toxicity[ 'toxicity_score' ].std()
s = "Mean: {0:.3}\nStd: {1:.3}".format( mean_female, std_female)
plt.text(1.5, 200000, s )

#
# Indicate mean and std with vertical lines
#
plt.axvline(x=mean_female, color='red')
plt.axvline(x=mean_female + std_female, color='green')
plt.axvline(x=mean_female - std_female, color='green')

#
# Save the dataframe.
#
male_joined_toxicity.to_csv( 'csv/male-toxicity-assess-data.csv', index_label = 'Id' )
female_joined_toxicity.to_csv( 'csv/female-toxicity-assess-data.csv', index_label = 'Id' )

#
# Save the plot as PNG
#
plt.savefig('png/gender-toxicity-assessment.png')
```

In this case (toxicity), the means differ by `0.048` and the standard deviations differ by `0.02`. Also, these two histograms have similar shapes. In fact, they look so similar that if the y-axes didn't have different values, I would suspect that they were the same. 

Based on these two sets of histograms, we can't conclude that there is a difference in the way that men and women evaluate aggression or toxicity in the Wikipedia Talk Page comments.


# Conclusions


**Question 1:** The preliminary exploratory analysis performed here _did_ indicate that women are more likely than men to flag Wikipedia Talk comments as both _aggressive_ (`+2.11%`) and _toxic_ (`+1.72%`). The differences were not large but did occur for both datasets which (presumably) used different sets of workers. 


**Question 2:** In this case--in which we explored whether there were differences between men in women in how they assessed the degree of aggressiveness or toxicity--the analysis did _not_ identify any differences. In fact, the similarity between the performance of men and women in this case was remarkably similar.


# Perspectives API: Discussion of Applicability


### Which, if any, of these demo applications would you expect the Perspective API—or any model trained on the Wikipedia Talk corpus—to perform well in? Why?


I interpret this question as having two parts. One is about the Perspective API, which uses a model trained on Disqus comments. The other is about models trained on the Wikipedia Talk corpus. My assessment is that, in NLP, context is decisive because the way that humans interpret language is highly context dependent. So to answer the question, Perspective would do best when analyzing comments on a service that uses Disqus. A model trained on the Wikipedia Talk corpus would do best when used to analyze Wikipedia Talk comments; such a model would also probably perform decently in other contexts within Wikipedia such as detecting vandalism on Wikipedia content pages. 


### Which, if any, of these demo applications would you expect the Perspective API to perform poorly in? Why?


Building on my answer to the previous question, I would expect the Perspective API to perform poorly in the WikiDetox application. This is assuming that the Perspective API is _**not**_ retrained on the Wikimedia Talk corpus. My understanding from the Wired and Engadget articles is that the Perspective API model is based on Disqus comments.


### What are some other contexts or applications where you would expect the Perspective API to perform particularly well, or particularly poorly? Why?


Here I would identify the Comment Filter application as an application that would be a poor fit for Wikipedia Talk pages. The issue with this application is that there are contexts in which it is inappropriate to filter out the comments--for example, the Talk Pages on Wikipedia. The purpose of the Talk pages is so that Wikipedia editors can negotiate disagreements about the content of Wikipedia content pages. With this as the context, it isn't reasonable to filter out statement made by other parties in these negotiations because the filtered comments might be relevant to ensuring a positive outcome.


# Appendix: Male and female comparison of demographic characteristics


This section performs some exploratory analysis on age and education demographics for the male workers. Although this analysis turned out not to be relevant for the larger analysis (above), I am preserving it here because it demonstrates some important techniques for plotting categorical data.


### Note on ordering the levels on the x-axis of histograms for categorical variables


For the education and age histograms, I needed to engage in some _gymnastics_ in order for the values on the x-axis to come out in a reasonable order. The process involves creating a mapping between the levels of the columns and an ordered set of non-negative integers. Then we use that mapping to create an additional column in the dataframe that we can use to sort the levels.

```python
edu_groups = ['none', 'some', 'hs', 'bachelors', 'masters', 'professional', 'doctorate']
mapping = {edu_map: i for i, edu_map in enumerate(edu_groups)}
#
# Here is what the mapping looks like.
#
mapping
```

```python
edu_map = male_joined_aggression['education'].map(mapping)
#
# Here is the result of running the mapping on the 'education' column
#
edu_map
```

```python
#
# The series has the name of the column that we mapped from. In this case, 'education'.
# Need to change that so we don't end up with two columns named 'education' in the dataframe.
#
edu_map = edu_map.rename( 'edu_sort')
```

```python
pd.concat( [male_joined_aggression, edu_map ], axis = 1).sort_values( by = 'edu_sort')[ 'education' ].hist()
plt.ylabel("Number of ratings",fontsize=12)
plt.xlabel("Education",fontsize=12)
plt.title('Males: education', fontsize=15)
```

We use the following code to enable ordering the columns of the histogram in a reasonable way. For more detail, see the proceeding discussion regarding the 'education' column.

```python
age_groups = ['Under 18', '18-30', '30-45', '45-60', 'Over 60']
mapping = {age_map: i for i, age_map in enumerate( age_groups )}
age_map = male_joined_aggression[ 'age_group' ].map( mapping )
age_map = age_map.rename( 'age_sort' )
```

```python
pd.concat( [male_joined_aggression, age_map ], axis = 1).sort_values( by = 'age_sort')[ 'age_group' ].hist()
plt.ylabel("Number of ratings",fontsize=12)
plt.xlabel("Age",fontsize=12)
plt.title('Males: age', fontsize=15)
```
