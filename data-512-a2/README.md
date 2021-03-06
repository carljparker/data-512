
# Project objective #

I want to explore two questions using the data in these datasets. Both questions consider possible gender differences in the way the crowdworkers interpreted the Talk Page comments. One of the alleged differences between men and women is that woman report higher Agreeableness scores in measures of the Big Five personality traits. Another is that women have greater empathy than men. (See references below.)

The difference in Agreeableness suggests the possibility that women might have--in some sense--greater reactivity to aggression than men and therefore might be more likely to flag it in the Talk Page comments. Similarly, for toxicity. Here we are operating on the assumption that Agreeableness, as a personality trait, is at odds with both aggression and toxicity.

Empathy is associated with understanding the mental or emotional state of another person. If women in general have higher empathy than men, then this could manifest in multiple ways. Women might tend to assess aggression or toxicity in the Wikipedia talk comments with greater _precision_ than men. Or women might simply tend to assess higher level of aggression and toxicity than men.

Note that detecting aggression/toxicity and assessing its level are somewhat distinct. For example, men and women could agree on which comments are toxic, but in general, disagree about the level of toxicity. There is also the scenario where men and women seem to disagree on which comments are toxic, but when they do agree, they rate the toxicity level similarly; admittedly, this second scenario seems less likely.

-  Question 1: Do women have greater sensitivity than men _in detecting_ instances of a) aggression or b) toxicity in the Talk Page comments--related to differences in Agreeableness.
-  Question 2: Do women have greater sensitivity than men _to the level_ of a) aggression or b) toxicity in the Talk Page comments--related to differences in empathy?


# Data sources #

The data used in this project was obtained from the
[Wikipedia Talk project](https://figshare.com/projects/Wikipedia_Talk/16731)
on **Figshare**. 

This project uses the following datasets specifically:

- [Wikipedia Talk Labels: Aggression](https://figshare.com/articles/dataset/Wikipedia_Talk_Labels_Aggression/4267550)
- [Wikipedia Talk Labels: Toxicity](https://figshare.com/articles/dataset/Wikipedia_Talk_Labels_Toxicity/4563973)


## Citation for data ##

Wulczyn, Ellery; Thain, Nithum; Dixon, Lucas (2016): Wikipedia Detox. figshare. 
[doi.org/10.6084/m9.figshare.4054689](doi.org/10.6084/m9.figshare.4054689)


## Notes ##

The datasets used in this project were downloaded to local storage and
accessed from there by the project's Jupypter notebook. Because of the
size of the datasets, the notebook does not access them directly from
their location on **Figshare**.

To reproduce the analysis in this directory, you will need to download
the datasets.


# Data file format #

[Complete information about the format of the data files is available on
the 
[Research:Detox/Data Release](https://meta.wikimedia.org/wiki/Research:Detox/Data_Release)
page at Wikimedia.]

Each of these datasets comprises three files:

- Comments that were extracted from Wikipedia Talk pages. This file has a basename suffix of `_annotated_comments`.
- Annotations for these comments specified by workers. This file has a basename suffix of `_annotations`.
- Demographic information for the workers. This file has a basename suffix of `_worker_demographics`.

All these files have an _extension_ of `.tsv`, which indicates that they
are formatted as tab-separated values.

The columns for the `_annotated_comments` and `_worker_demographics`
files are the same across both the Aggression and Personal Attacks
datasets. However, the columns for the `_annotations` file differ
between these datasets. The following section provides more detail.


## `annotated_comments` columns ##

`rev_id` 

MediaWiki revision id of the edit that added the comment to a talk page
(i.e. discussion).

`comment` 

Comment text. Consists of the concatenation of content added during a
revision/edit of a talk page. MediaWiki markup and HTML have been
stripped out. To simplify tsv parsing, \n has been mapped to
NEWLINE_TOKEN, \t has been mapped to TAB_TOKEN and `"` has been mapped
to a single quote.

`year` 

The year the comment was posted in.

`logged_in` 

Indicator for whether the user who made the comment was logged in. Takes
on values in {0, 1}.

`ns` 

Namespace of the discussion page the comment was made in. Takes on values in {user, article}.

`sample` 

Indicates whether the comment came via random sampling of all comments,
or whether it came from random sampling of the 5 comments around a block
event for violating WP:npa or WP:HA. Takes on values in {random,
blocked}.

`split` 

For model building in our paper we split comments into train, dev and
test sets. Takes on values in {train, dev, test}.



## `demographic_information` columns ##

`worker_id` 

Anonymized crowd-worker id.

`gender` 

The gender of the crowd-worker. Takes a value in {'male', 'female', and
'other'}.

`english_first_language` 

Does the crowd-worker describe English as their first language. Takes a
value in {0, 1}.

`age_group` 

The age group of the crowd-worker. Takes on values in {'Under 18',
'18-30', '30-45', '45-60', 'Over 60'}.

`education` 

The highest education level obtained by the crowd-worker. Takes on
values in {'none', 'some', 'hs', 'bachelors', 'masters', 'doctorate',
'professional'}. Here 'none' means no schooling, some means 'some
schooling', 'hs' means high school completion, and the remaining terms
indicate completion of the corresponding degree type.


## `annotations` columns for aggression ##

`rev_id` 

MediaWiki revision id of the edit that added the comment to a talk page
(i.e. discussion).

`worker_id` 

Anonymized crowd-worker id.

`aggression_score` 

Categorical variable ranging from very aggressive (-2), to neutral (0),
to very friendly (2).

`aggression` 

Indicator variable for whether the worker thought the comment has an
aggressive tone . The annotation takes on the value 1 if the worker
considered the comment aggressive (i.e worker gave an `aggression_score`
less than 0) and value 0 if the worker considered the comment neutral or
friendly (i.e worker gave an `aggression_score` greater or equal to 0).
Takes on values in {0, 1}.


## `annotations` columns for toxicity ##

`rev_id` 

MediaWiki revision id of the edit that added the comment to a talk page
(i.e. discussion).  

`worker_id` 

Anonymized crowd-worker id.

`toxicity_score` 

Categorical variable ranging from very toxic (-2), to neutral (0), to
very healthy (2).

`toxicity` 

Indicator variable for whether the worker thought the comment is toxic.
The annotation takes on the value 1 if the worker considered the comment
toxic (i.e worker gave a `toxicity_score` less than 0) and value 0 if the
worker considered the comment neutral or healthy (i.e worker gave a
`toxicity_score` greater or equal to 0). Takes on values in {0, 1}.


# Licensing for data used #

The data for this project is used under the 
**Creative Commons CC0 1.0 Universal (CC0 1.0) Public Domain Dedication**
as indicated on the web page for each of the datasets on **Figshare**.

For more information about this version of the Creative Commons CC0
license, see the [web page](https://creativecommons.org/publicdomain/zero/1.0/)
for that license at the Creative Commons web site.  

See also the **Figshare** 
[terms of use (TOU)](https://figshare.com/terms)
and 
[privacy policy](https://figshare.com/privacy).


# License #

The contents of this subbranch of this repository are licensed under The
MIT License. For the text of the license, see the `LICENSE` file in the
same directory as this `README.md` file. For more information about The
MIT License, see the following page:

>  <https://opensource.org/licenses/MIT>


### --- END --- ###
 
