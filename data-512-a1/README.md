
# Project objective #

This project extracts Wikipedia traffic data using two separate versions
of the Wikipedia REST API. It processes this data and exports it as a
CSV file: `en-wikipedia_traffic_200712-202008.csv`. The project also
creates a plot of the data: `en_wikipedia_traffic.png`.

The project objective, however, is not only to perform the above tasks,
but to do so in a way that would be easily reproducible by others--using
the artifacts that are contained in this repository directory.


# Data sources #

The data used by this software was extracted using the Wikimedia
Analytics Query Service (AQS) REST API. Specifically, the **Pagecounts**
(legacy) and **Pageviews** endpoints. These endpoints are documented at
the following pages.

**Pagecounts**
>  <https://wikitech.wikimedia.org/wiki/Analytics/AQS/Legacy_Pagecounts>

**Pageviews**
>  <https://wikitech.wikimedia.org/wiki/Analytics/AQS/Pageviews>

The endpoints themselves--and sandboxes to test them--are located at the
following URLs:

**Pagecounts**

>  <https://wikimedia.org/api/rest_v1/#/Legacy%20data/get_metrics_legacy_pagecounts_aggregate__project___access_site___granularity___start___end_>

**Pageviews**
>  <https://wikimedia.org/api/rest_v1/#/Pageviews%20data/get_metrics_pageviews_aggregate__project___access___agent___granularity___start___end_>


# Notes #

The pagecount (legacy) API does not provide the ability to specify the
type of user agent that accessed the Wikipedia site. So, for example, it
include traffic from web crawlers and bots. The more recent pageview API
does provide the ability to specify the type of user agent. For these
API calls the Jupyter notebook specifies `user`, that is, excluding
traffic from web crawlers and bots.


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

The data extracted via the above APIs is presumed to be used under the
**Creative Commons Attribution-ShareAlike License** or under the, more
permissive, **Creative Commons CC0 License**. This presumption derives
from the licensing information in the footers of the Wikipedia main page
and the WikiData main page, which you can view at the following URLs.

>  <https://www.wikipedia.org/>

>  <https://www.wikidata.org/>

See also the Wikimedia terms of use (TOU):

>  <https://www.mediawiki.org/wiki/REST_API#Terms_and_conditions>


# License #

The contents of this subbranch of this repository are licensed under The
MIT License. For the text of the license, see the `LICENSE` file in the
same directory as this `README.md` file. For more information about The
MIT License, see the following page:

>  <https://opensource.org/licenses/MIT>


### --- END --- ###
 
