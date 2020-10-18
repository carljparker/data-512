
# Project objective #


# Data sources #

The data used in this project was obtained from the
[Wikipedia Talk project](https://figshare.com/projects/Wikipedia_Talk/16731)
on **Figshare**. 

This project uses the following datasets specifically:

- [Wikipedia Talk Labels: Personal Attacks](https://figshare.com/articles/dataset/Wikipedia_Talk_Labels_Personal_Attacks/4054689)
- [Wikipedia Talk Labels: Aggression](https://figshare.com/articles/dataset/Wikipedia_Talk_Labels_Aggression/4267550)


## Notes ##

The datasets used in this project were downloaded to local storage and
accessed from there by the project's Jupypter notebook. Because of the
size of the datasets, the notebook does not access them directly from
their location on **Figshare**.


# Data file format #

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


## Annotated comments columns ##

`rev_id`

`comment`

`year`

`logged_in`

`ns`

`sample`

`split`


## Demographic information columns ##

`worker_id`

`gender`

`english_first_language`

`age_group`

`education`


## Annotations columns for aggression ##


## Annotations columns for personal attacks ##

`rev_id`

`worker_id`

`recipient` attack

`third_party_attack`

`other_attack`

`attack`


## Annotations columns for personal attacks ##

`rev_id`

`worker_id`

`aggression`

`aggression_score`


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
 
