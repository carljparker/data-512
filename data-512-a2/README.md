
# Project objective #


# Data sources #

The data used in this project was obtained from 
[Wikipedia Talk project](https://figshare.com/projects/Wikipedia_Talk/16731).
on **Figshare**. 

This project uses the following datasets specifically:

- [Wikipedia Talk Labels: Personal Attacks](https://figshare.com/articles/dataset/Wikipedia_Talk_Labels_Personal_Attacks/4054689)
- [Wikipedia Talk Labels: Aggression](https://figshare.com/articles/dataset/Wikipedia_Talk_Labels_Aggression/4267550)

Each of these datasets comprises three files:

- Comments that were annotated by workers. This file has a basename suffix of `_annotated_comments`.
- Annotations specified by the workers. This file has a basename suffix of `_annotations`.
- Demographic information on the workers. This file has a basename suffix of `_worker_demographics`.

All these files have an _extension_ of `.tsv`.


## Notes ##

The datasets used in this project were downloaded to local storage and
accessed from there by the project's Jupypter notebook. Because of the
size of the datasets, the notebook does not access them directly from
their location on **Figshare**.


# Data file columns #

The CSV file, `en-wikipedia_traffic_200712-202008.csv` has the following
columns. Not the resolution--or _granularity_--of the data collected is
_monthly_.

`year`

Year corresponding to the traffic data for all cells in this row.

`month`

Month corresponding to the traffic data for all cells in this row.

`pagecount_all_views`

The total views--both desktop site and mobile site--collected by the
pagecount (legacy) API for this month and year.

`pagecount_desktop_views`

Just the desktop views collected by the pagecount (legacy) API for this
month and year.

`pagecount_mobile_views`

Just the views of the mobile site collected by the pagecount (legacy)
API for this month and year. The presumption is that the views are from
browsers on mobile devices.

`pageview_all_views`

The total views--both desktop and mobile--collected by the pageview API
for this month and year.

`pageview_desktop_views`

Just the desktop views collected by the pageview API for this month and
year.

`pageview_mobile_views`

Just the mobile views collected by the pageview API for this month and
year. This value is the sum of views from mobile app and views from
browsers that navigated to the mobile site.


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
 
