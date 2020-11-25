---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.7.1
  kernelspec:
    display_name: Python [conda env:root] *
    language: python
    name: conda-root-py
---

This code is derived from code at:

https://public.paws.wmcloud.org/User:Jtmorgan/data512_a1_example.ipynb


```python
import json
import requests
import numpy as np
import pandas as pd
```

The following strings represent the HTTP endpoints for the two REST APIs that we'll use.

The parameters for the APIs are encoded in these strings and are represented below by the URL segments that are in braces, such as `{granularity}`. These are placeholders that are replaced by values that we specify in blocks of JSON and pass into the function that makes the API call. (That function is defined later in this file.)

```python
endpoint_pageviews = 'https://wikimedia.org/api/rest_v1/metrics/pageviews/aggregate/{project}/{access}/{agent}/{granularity}/{start}/{end}'

endpoint_legacy = 'https://wikimedia.org/api/rest_v1/metrics/legacy/pagecounts/aggregate/{project}/{access-site}/{granularity}/{start}/{end}'
```

In this section, we specify blocks of JSON that contain the parameters for each of the **five** API calls that we make.

The first **three** parameter blocks are for the Pageviews API. The following **two** parameter blocks are for the Pagecount (legacy) API.

```python
#
# Parameters for getting aggregated current standard pageview data
#
# See: https://wikimedia.org/api/rest_v1/#!/Pageviews_data/get_metrics_pageviews_aggregate_project_access_agent_granularity_start_end
#
params_pageviews_desktop = {
                    "project" : "en.wikipedia.org",
                    "access" : "desktop",
                    "agent" : "user",
                    "granularity" : "monthly",
                    "start" : "2015070100",
                    # for end use 1st day of month following final month of data
                    "end" : '2020100100'
                        }

params_pageviews_mobile_web = {
                    "project" : "en.wikipedia.org",
                    "access" : "mobile-web",
                    "agent" : "user",
                    "granularity" : "monthly",
                    "start" : "2015070100",
                    # for end use 1st day of month following final month of data
                    "end" : '2020100100'
                        }

params_pageviews_mobile_app = {
                    "project" : "en.wikipedia.org",
                    "access" : "mobile-app",
                    "agent" : "user",
                    "granularity" : "monthly",
                    "start" : "2015070100",
                    # for end use 1st day of month following final month of data
                    "end" : '2020100100'
                        }

#
# Parameters for getting aggregated legacy view data 
#
# See: https://wikimedia.org/api/rest_v1/#!/Legacy_data/get_metrics_legacy_pagecounts_aggregate_project_access_site_granularity_start_end
#
params_pagecounts_legacy_desktop = {
                 "project" : "en.wikipedia.org",
                 "access-site" : "desktop-site",
                 "granularity" : "monthly",
                 "start" : "2008010100",
                 # for end use 1st day of month following final month of data
                 "end" : "2016080100"
                    }

params_pagecounts_legacy_mobile = {
                 "project" : "en.wikipedia.org",
                 "access-site" : "mobile-site",
                 "granularity" : "monthly",
                 "start" : "2008010100",
                 # for end use 1st day of month following final month of data
                 "end" : "2016080100"
                    }

```

```python
#
# Identify myself in the headers of the HTTP request
#
headers = {
    'User-Agent': 'https://github.com/carljparker',
    'From': 'cajopa@uw.edu'
}
```

```python
def api_call(endpoint,parameters):
    call = requests.get(endpoint.format(**parameters), headers=headers)
    response = call.json()
    
    return response
```

## Extract pageview data for desktop site ##

```python
pageviews_desktop = api_call(endpoint_pageviews, params_pageviews)
```

The REST API returns a Python dictionary with a single key, `items`.

```python
type( pageviews_desktop )
```

```python
type( pageviews_desktop['items'] )
```

Write the JSON data to an external file.

```python
with open("pageviews_desktop_201507-202009.json", "w") as write_file:
    json.dump(pageviews_desktop, write_file)
```

Read the JSON data into a Pandas dataframe.

```python
df_pageviews_desktop = pandas.read_json( json.dumps( pageviews_desktop[ 'items' ] ), orient='records', convert_dates = False )
```

```python
df_pageviews_desktop
```

Remove the following columns: `project` (0), `access` (1), `agent` (2), `granularity` (3). The project and granularity are the same for all the data that we retrieved. The agent value isn't necessary for the analysis, but was only important when specifying to the REST API which data to retrieve. We'll rename the `views` column to specify the access type.

```python
df_pageviews_desktop.drop( df_pageviews_desktop.columns[ [ 0, 1, 2, 3 ] ], axis = 1, inplace = True )
```

```python
df_pageviews_desktop
```

Rename views to the access type: desktop.

```python
df_pageviews_desktop.columns = [ 'timestamp', 'pageview_desktop_views' ]
```

```python
df_pageviews_desktop
```

## Extract pageview data for mobile web ##

```python
pageviews_mobile_web = api_call(endpoint_pageviews, params_pageviews_mobile_web)
```

Write the JSON data to an external file.

```python
with open("pageviews_mobile-web_201507-202009.json", "w") as write_file:
    json.dump(pageviews_mobile_web, write_file)
```

Read the JSON data into a Pandas dataframe.

```python
df_pageviews_mobile_web = pandas.read_json( json.dumps( pageviews_mobile_web[ 'items' ] ), orient='records', convert_dates = False )
```

```python
df_pageviews_mobile_web
```

Remove the following columns: `project` (0), `access` (1), `agent` (2), `granularity` (3). The project and granularity are the same for all the data that we retrieved. The agent value isn't necessary for the analysis, but was only important when specifying to the REST API which data to retrieve. We'll rename the `views` column to specify the access type.

```python
df_pageviews_mobile_web.drop( df_pageviews_mobile_web.columns[ [ 0, 1, 2, 3 ] ], axis = 1, inplace = True )
```

```python
df_pageviews_mobile_web
```

Now rename the columns. It is easier when there are fewer of them.

```python
df_pageviews_mobile_web.columns = [ 'timestamp', 'mobile_web' ]
```

```python
df_pageviews_mobile_web
```

## Extract pageview data for mobile app ##

```python
pageviews_mobile_app = api_call(endpoint_pageviews, params_pageviews_mobile_app)
```

Write the JSON data to an external file.

```python
with open("pageviews_mobile-app_201507-202009.json", "w") as write_file:
    json.dump(pageviews_mobile_app, write_file)
```

Read the JSON data into a Pandas dataframe.

```python
df_pageviews_mobile_app = pandas.read_json( json.dumps( pageviews_mobile_app[ 'items' ] ), orient='records', convert_dates = False )
```

```python
df_pageviews_mobile_app
```

Remove the following columns: `project` (0), `access` (1), `agent` (2), `granularity` (3). The project and granularity are the same for all the data that we retrieved. The agent value isn't necessary for the analysis, but was only important when specifying to the REST API which data to retrieve. We'll rename the `views` column to specify the access type.

```python
df_pageviews_mobile_app.drop( df_pageviews_mobile_app.columns[ [ 0, 1, 2, 3 ] ], axis = 1, inplace = True )
```

```python
df_pageviews_mobile_app
```

Now rename the columns. It is easier when there are fewer of them.

```python
df_pageviews_mobile_app.columns = [ 'timestamp', 'mobile_app' ]
```

```python
df_pageviews_mobile_app
```

## Merge moble pageviews (web and app) data ##

```python
df_pageviews_mobile_merged = pd.merge( df_pageviews_mobile_web, df_pageviews_mobile_app, on = 'timestamp' )
```

```python
df_pageviews_mobile_merged
```

Create a new column which is the sum of the two types of mobile page views.

```python
df_pageviews_mobile_merged[ 'pageview_mobile_views' ] = df_pageviews_mobile_merged[ 'mobile_web'] + df_pageviews_mobile_merged[ 'mobile_app' ]
```

```python
df_pageviews_mobile_merged
```

Remove the two columns that refer to the individual types of mobile data: `mobile_web` (1) and `mobile_app` (2)

```python
df_pageviews_mobile_merged.drop( df_pageviews_mobile_merged.columns[ [ 1, 2 ] ], axis = 1, inplace = True )
```

```python
df_pageviews_mobile_merged
```

## Merge pageview data for desktop and mobile

```python
df_pageviews_all_access = pd.merge( df_pageviews_mobile_merged, df_pageviews_desktop, on = 'timestamp' )
```

```python
df_pageviews_all_access
```

## Extract pagecount (legacy) data for desktop ##

```python
pagecount_legacy_desktop = api_call(endpoint_legacy, params_pagecount_legacy_desktop)
```

Write the JSON data to an external file.

```python
with open("pagecounts_desktop_200801-201607.json", "w") as write_file:
    json.dump(pagecount_legacy_desktop, write_file)
```

Read the JSON data into a Pandas dataframe.

```python
df_pagecount_legacy_desktop = pandas.read_json( json.dumps( pagecount_legacy_desktop[ 'items' ] ), orient='records', convert_dates = False )
```

```python
df_pagecount_legacy_desktop
```

Remove the following columns: `project` (0), `access-site` (1), `granularity` (2). The project and granularity are the same for all the data that we retrieved. We'll rename the `count` column to indicate the value of access-site.

```python
df_pagecount_legacy_desktop.drop( df_pagecount_legacy_desktop.columns[ [ 0, 1, 2 ] ], axis = 1, inplace = True )
```

```python
df_pagecount_legacy_desktop.columns = [ 'timestamp', 'pagecount_desktop_views' ]
```

```python
df_pagecount_legacy_desktop
```

## Extract pagecount (legacy) data for mobile ##

```python
pagecount_legacy_mobile = api_call(endpoint_legacy, params_pagecount_legacy_mobile)
```

```python
with open("pagecounts_mobile_200801-201607.json", "w") as write_file:
    json.dump(pagecount_legacy_mobile, write_file)
```

```python
df_pagecount_legacy_mobile = pandas.read_json( json.dumps( pagecount_legacy_mobile[ 'items' ] ), orient='records', convert_dates = False )
```

```python
df_pagecount_legacy_mobile
```

Remove the following columns: `project` (0), `access-site` (1), `granularity` (2). The project and granularity are the same for all the data that we retrieved. We'll rename the `count` column to indicate the value of access-site.

```python
df_pagecount_legacy_mobile.drop( df_pagecount_legacy_mobile.columns[ [ 0, 1, 2 ] ], axis = 1, inplace = True )
```

```python
df_pagecount_legacy_mobile.columns = [ 'timestamp', 'pagecount_mobile_views' ]
```

```python
df_pagecount_legacy_mobile
```

Merge together the pagecount (legacy) data for desktop and mobile. _Perform the equivalent of an outer join, so we do not lose rows_.

```python
df_pagecount_all_access = pd.merge( df_pagecount_legacy_mobile, df_pagecount_legacy_desktop, how = 'outer', on = 'timestamp' )
```

```python
df_pagecount_all_access
```

## Merge pageview and pagecount (legacy) data

```python
df_en_wikipedia_traffic = pd.merge( df_pageviews_all_access, df_pagecount_all_access, how = 'outer', on = 'timestamp' )
```

```python
df_en_wikipedia_traffic
```

Replace the `NaN` values with zeros.

```python
df_en_wikipedia_traffic = df_en_wikipedia_traffic.fillna(0)
```

Create total columns

```python
df_en_wikipedia_traffic[ 'pageview_all_views' ] = df_en_wikipedia_traffic[ 'pageview_mobile_views' ] + df_en_wikipedia_traffic[ 'pageview_desktop_views' ]
```

```python
df_en_wikipedia_traffic[ 'pagecount_all_views' ] = df_en_wikipedia_traffic[ 'pagecount_mobile_views' ] + df_en_wikipedia_traffic[ 'pagecount_desktop_views' ]
```

```python
df_en_wikipedia_traffic
```

Sort by the timestamp.

```python
df_en_wikipedia_traffic[ 'year' ] = df_en_wikipedia_traffic[ 'timestamp' ].apply( str ).str[ 0:4 ]
```

```python
df_en_wikipedia_traffic[ 'month' ] = df_en_wikipedia_traffic[ 'timestamp' ].apply( str ).str[ 4:6 ]
```

```python
df_en_wikipedia_traffic
```

Sort by the timestamp.

```python
df_en_wikipedia_traffic = df_en_wikipedia_traffic.sort_values(by=['timestamp'])
```

Renumber the rows.

```python
df_en_wikipedia_traffic.index = range( len( df_en_wikipedia_traffic ) )
```

Make a copy of the dataframe that we'll use for plotting.

```python
df_plot_data = df_en_wikipedia_traffic
```

Now, remove the `timestamp` (0) column.

```python
df_en_wikipedia_traffic.drop( df_en_wikipedia_traffic.columns[ [ 0 ] ], axis = 1, inplace = True )
```

```python
df_en_wikipedia_traffic
```

Write the dataframe to CSV

```python
df_en_wikipedia_traffic.to_csv( 'en-wikipedia_traffic_200712-202008.csv', index_label = 'Id' )
```

Create new columns for the data that we'll plot.

```python
df_plot_data[ 'main_site' ] = ( df_plot_data[ 'pageview_desktop_views' ] + df_plot_data[ 'pagecount_desktop_views' ] ) / 1000000
```

```python
df_plot_data[ 'mobile_site' ] = ( df_plot_data[ 'pageview_mobile_views' ] + df_plot_data[ 'pagecount_mobile_views' ] ) / 1000000
```

```python
df_plot_data[ 'total' ] = df_plot_data[ 'main_site' ] + df_plot_data[ 'mobile_site' ]
```

Remove previous the non-derived data columns

```python
df_plot_data.drop( df_plot_data.columns[ [ 0, 1, 2, 3, 4, 5 ] ], axis = 1, inplace = True )
```

```python
df_plot_data
```

Plot the dataframe . . . Someday, I'll have better matplot-fu . . . :-P

```python
plot = df_plot_data.plot( x = 'year', title = 'Pageviews on english Wikipedia (x 1,000,000)')
```

Save the plot with the following incantation.

```python
figure_1 = plot.get_figure()
```

```python
figure_1.savefig('en_wikipedia_traffic.png')
```
