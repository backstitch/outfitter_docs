# API 2.0 Endpoints

The backstitch API 2.0 includes the ability to build and manage topic pages from a RESTful interface along with a
more consistent endpoint schema for consuming content from topics.

All API 2.0 requests use the base url of `https://api.backstit.ch/v2` and authenticate by passing the topic's token as
a url parameter and the organization's key as a query parameter.

<aside class="success">
  It is recommended to use API 2.0 as it will be the version to receive incremental updates.
</aside>

## Get Organization Details

```ruby
require 'rest_client'

response = RestClient.get 'https://api.backstit.ch/v2/organization/70b5aa707ca6013231ce482a14180728/details'
```

```python
import urllib2

response = urllib2.urlopen('https://api.backstit.ch/v2/organization/70b5aa707ca6013231ce482a14180728/details')
```

```shell
curl https://api.backstit.ch/v2/organization/70b5aa707ca6013231ce482a14180728/details
```

> The above command returns JSON structured like this:

```json
{
  "id": 123,
  "name": "Awesome Rocket",
  "logo": {
    "url": "http://backstitch-user-uploads.s3.amazonaws.com/production/organization_logos/awesome_rocket_logo.png",
    "width": 181,
    "height": 36
  },
  "highlight_color": "#1ba5ca",
  "key": "70b5aa707ca6013231ce482a14180728"
}
```

This endpoint retrieves details about an organization.

### HTTP Request

`GET https://api.backstit.ch/v2/organization/{ORGANIZATION_KEY}/details`

### Returns

| Field | Data Type | Description |
|---------|:-------:|:-----------|
| uid | integer | A unique identifier for the organization |
| name | string | The name given to the organization |
| logo | object | An optional logo given to the organization |
| highlight_color | string | An optional highlight given to the organization |
| key | string | The organizations's api key |

| Field | Data Type | Description |
|---------|:-------:|:-----------|
| uid | integer | A unique identifier for the organization |
| name | string | The name given to the organization |
| logo | object | An optional logo given to the organization |
| highlight_color | string | An optional highlight given to the organization |
| key | string | The organizations's api key |

## Get Organization Topics

```ruby
require 'rest_client'

response = RestClient.get 'https://api.backstit.ch/v2/organization/70b5aa707ca6013231ce482a14180728/topics'
```

```python
import urllib2

response = urllib2.urlopen('https://api.backstit.ch/v2/organization/70b5aa707ca6013231ce482a14180728/topics')
```

```shell
curl https://api.backstit.ch/v2/organization/70b5aa707ca6013231ce482a14180728/topics
```

> The above command returns JSON structured like this:

```json
[
  {
    "uid": 12,
    "name": "Apple",
    "description": "All about Apple",
    "banner": {
      "url": "http://images-backstitch.s3.amazonzws.com/next/logos/backstitch_purple_icon.png",
      "width": 300,
      "height": 300
    },
    "token": "700cc690776001326156482a14180728",
    "filters": [
      {
        "uid": 23,
        "phrase": "mac",
        "type": "include"
      }
    ],
    "sources":[
      {
        "filters":[
          {
            "uid": 234,
            "phrase": "imac",
            "type": "exclude"
          }
        ],
        "uid": 345,
        "name": "Apple Hot News",
        "icon":{
          "url": "http://apple.com/favicon.ico",
          "width": null,
          "height": null
        },
        "banner":{
          "url": "http://images.apple.com/hotnews/promos/images/promo_ipad_air_2.jpg",
          "width": null,
          "height": null
        },
        "params":{
          "feed_url":{
            "type": "url",
            "value": "https://www.apple.com/main/rss/hotnews/hotnews.rss"
          }
        },
        "service_name": "rss"
      }
    ]
  }
]
```

This endpoint retrieves a list of organization owned topics.

### HTTP Request

`GET https://api.backstit.ch/v2/organization/{ORGANIZATION_KEY}/topics`

### Returns

| Field | Data Type | Description |
|---------|:-------:|:-----------|
| uid | integer | A unique identifier for the topic |
| name | string | The name given to the topic |
| description | string | An optional description of the topic |
| banner | object | The topic's displayed banner image if one was uploaded |
| token | string | The topic's api token |
| filters | array | A list of global keyword filters set on the topic |
| sources | array | A list of included sources |

## Create Topic

```ruby
require 'rest_client'

# Create a new topic called 'News Around Detroit'
response = RestClient.post 'https://api.backstit.ch/v2/topic', :params => {:key => '70b5aa707ca6013231ce482a14180728', :name => 'News around Detroit'}
```

```python
import urllib
import urllib2

url = 'https://api.backstit.ch/v2/topic/'
response = urllib2.urlopen(endpoint)
params = {'skip': 20}
data = urllib.urlencode(params)
req = urllib2.Request(endpoint, data)
response = urllib2.urlopen(req)
```

```shell
curl https://api.backstit.ch/v2/topic?key=70b5aa707ca6013231ce482a14180728&name=Detroit
```

> The above command returns JSON structured like this:

```json
{
  "uid": 13,
  "name": "Detroit",
  "description": "",
  "banner": {
    "url": "http://images-backstitch.s3.amazonzws.com/next/logos/backstitch_purple_icon.png",
    "width": 300,
    "height": 300
  },
  "token": "9b5d30a07d4001325ede482a14180728",
  "filters": [],
  "sources":[]
}
```

This endpoint allows the creation of a new organization owned topic that has API support.

### HTTP Request

`POST https://api.backstit.ch/v2/topic`

### Query Parameters

| Parameter | Required | Default | Description |
|---------|:-------:|:-------:|:-----------|
| key | yes | | Your organization's api key obtained from the organization dashboard. |
| name | yes | | The name of your topic. |
| team | no | all user team | The name of the team this topic belongs to. |
| sources | no | | Add source to your topic at the time of creation.  Use the same structure as adding sources to a topic. |
| filters | no | | Add filters to your topic at the time of creation.  Use the same structure as adding filters to a topic. |

### Returns

| Field | Data Type | Description |
|---------|:-------:|:-----------|
| uid | integer | A unique identifier for the topic |
| name | string | The name given to the topic |
| description | string | An optional description of the topic |
| banner | object | The topic's displayed banner image if one was uploaded |
| token | string | The topic's api token |
| filters | array | A list of global keyword filters set on the topic |
| sources | array | A list of included sources |

## Add Topic Sources

```ruby
require 'rest_client'

response = RestClient.post 'https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/sources', :params => {:key => '70b5aa707ca6013231ce482a14180728', :data => [{:service => 'facebook_user', :value => 'backstitchapp'}]}

response = RestClient.post 'https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/sources', :params => {:key => '70b5aa707ca6013231ce482a14180728', :data => [{:service => 'facebook_user', :value => 'backstitchapp', :filters => [{:type => 'include_term', :value => 'detroit'}]}]}
```

```python
import urllib
import urllib2

url = 'https://api.backstit.ch/v2/topic/'
response = urllib2.urlopen(endpoint)
params = {'skip': 20}
data = urllib.urlencode(params)
req = urllib2.Request(endpoint, data)
response = urllib2.urlopen(req)
```

```shell
curl https://api.backstit.ch/v2/organization/{TOPIC_TOKEN}/sources
```

> The above command returns JSON structured like this:

```json
{
  "errors":[
    {
      "message": "Invalid service",
      "service": "facebook",
      "value": "backstitchapp"
    }
  ],
  "sources":[
    {
      "filters":[],
      "uid": 76,
      "name": "Facebook Posts from backstitchapp",
      "icon":{
        "url": "http://images-backstitch.s3.amazonaws.com/services/icons/facebook.png",
        "width": 64,
        "height": 64
      },
      "banner":{
        "url": "http://images-backstitch.s3.amazonaws.com/next/service_catalog/facebook_banner.jpg",
        "width": 650,
        "height": 240
      },
      "params":{
        "user":{
          "type": "search_term",
          "value": "backstitchapp"
        }
      },
      "service_name": "facebook_user"
    }
  ]
}
```

This endpoint adds new sources to the topic.

### HTTP Request

`POST https://api.backstit.ch/v2/topic/{TOPIC_TOKEN}/sources`

### Query Parameters

| Parameter | Required | Default | Description |
|---------|:-------:|:-------:|:-----------|
| key | yes | | Your organization's api key obtained from the organization dashboard. |
| data | yes| | An Array of source to add to your topic. |

### Data Child Parameters

| Parameter | Required | Default | Description |
|---------|:-------:|:-------:|:-----------|
| service | yes | | The name of the source service to add. |
| value | yes | | The value of the service. |
| filters | no | | Optional filters. |

### Filters Child Parameters

| Parameter | Required | Default | Description |
|---------|:-------:|:-------:|:-----------|
| type | yes | | The filter type. |
| value | yes | | The value of the filter. |

### Available Services

| Service | Value | Description
|---------|:-------:|:-------:|
| rss | url | An rss feed's url |

### Available Filters

| Type | Value | Description
|---------|:-------:|:-------:|
| include | phrase | Keywords that have to icluded in the feed |
| exclude | phrase | Keywords that have to excluded from the feed |


### Returns

| Field | Data Type | Description |
|---------|:-------:|:-----------|
| errors | array | A list of errors for sources that were not added  |
| sources | array | A list of sources added |

## Add Topic Filters

```ruby
require 'rest_client'

response = RestClient.post 'https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/filters', :params => {:key => '70b5aa707ca6013231ce482a14180728' :data => [{:type => 'include', :value => 'downtown'}]}
```

```python
import urllib
import urllib2

url = 'https://api.backstit.ch/v2/topic/'
response = urllib2.urlopen(endpoint)
params = {'skip': 20}
data = urllib.urlencode(params)
req = urllib2.Request(endpoint, data)
response = urllib2.urlopen(req)
```

```shell
curl https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/filters
```

> The above command returns JSON structured like this:

```json
{
  "errors":[
    {
      "message": "Invalid type",
      "type": "included",
      "value": "downtown"
    }
  ],
  "filters":[
    {
      "uid": 345,
      "phrase": "downtown",
      "type": "include"
    }
  ]
}
```

This endpoint adds new sources to the topic.

### HTTP Request

`POST https://api.backstit.ch/v2/topic/{TOPIC_TOKEN}/filters`

### Query Parameters

| Parameter | Required | Default | Description |
|---------|:-------:|:-------:|:-----------|
| key | yes | | Your organization's api key obtained from the organization dashboard. |
| data | yes | | The array of filters to add |

### Data Child Parameters

| Parameter | Required | Default | Description |
|---------|:-------:|:-------:|:-----------|
| type | yes | | The filter type. |
| value | yes | | The value of the filter. |

### Available Filters

| Type | Value | Description
|---------|:-------:|:-------:|
| include | phrase | Keywords that have to icluded in the feed |
| exclude | phrase | Keywords that have to excluded from the feed |


### Returns

| Field | Data Type | Description |
|---------|:-------:|:-----------|
| errors | array | A list of error for sources that were not added |
| filters | array | A list of global keyword filters added |

## Delete Topic Sources

```ruby
require 'rest_client'

response = RestClient.delete 'https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/sources', :params => {:key => '70b5aa707ca6013231ce482a14180728' :data => [{:service => 'facebook_user', :value => 'backstitchapp'}]}
```

```python
import urllib
import urllib2

url = 'https://api.backstit.ch/v2/topic/'
response = urllib2.urlopen(endpoint)
params = {'skip': 20}
data = urllib.urlencode(params)
req = urllib2.Request(endpoint, data)
response = urllib2.urlopen(req)
```

```shell
curl https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/sources
```

> The above command returns JSON structured like this:

```json
{
  "errors":[
    {
      "message": "Invalid service",
      "service": "facebook",
      "value": "backstitchapp"
    }
  ],
  "sources":[
    {
      "filters":[],
      "uid": 76,
      "name": "Facebook Posts from backstitchapp",
      "icon":{
        "url": "http://images-backstitch.s3.amazonaws.com/services/icons/facebook.png",
        "width": 64,
        "height": 64
      },
      "banner":{
        "url": "http://images-backstitch.s3.amazonaws.com/next/service_catalog/facebook_banner.jpg",
        "width": 650,
        "height": 240
      },
      "params":{
        "user":{
          "type": "search_term",
          "value": "backstitchapp"
        }
      },
      "service_name": "facebook_user"
    }
  ]
}
```

This endpoint adds new sources to the topic.

### HTTP Request

`DELETE https://api.backstit.ch/v2/topic/{TOPIC_TOKEN}/sources`

### Query Parameters

| Parameter | Required | Default | Description |
|---------|:-------:|:-------:|:-----------|
| key | yes | | Your organization's api key obtained from the organization dashboard. |
| data | yes| | An Array of source to add to your topic. |

### Data Child Parameters

| Parameter | Required | Default | Description |
|---------|:-------:|:-------:|:-----------|
| service | yes | | The name of the source service to add. |
| value | yes | | The value of the service. |

### Available Services

| Service | Value | Description
|---------|:-------:|:-------:|
| rss | url | An rss feed's url |


### Returns

| Field | Data Type | Description |
|---------|:-------:|:-----------|
| errors | array | A list of errors for sources that were not removed |
| sources | array | A list of sources that were removed |

## Delete Topic Filters

```ruby
require 'rest_client'

response = RestClient.delete 'https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/filters', :params => {:key => '70b5aa707ca6013231ce482a14180728' :data => [{:type => 'include', :value => 'downtown'}]}
```

```python
import urllib
import urllib2

url = 'https://api.backstit.ch/v2/topic/'
response = urllib2.urlopen(endpoint)
params = {'skip': 20}
data = urllib.urlencode(params)
req = urllib2.Request(endpoint, data)
response = urllib2.urlopen(req)
```

```shell
curl https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/filters
```

> The above command returns JSON structured like this:

```json
{
  "errors":[
    {
      "message": "Invalid type",
      "type": "included",
      "value": "downtown"
    }
  ],
  "filters":[
    {
      "uid": 345,
      "phrase": "downtown",
      "type": "include"
    }
  ]
}
```

This endpoint adds new sources to the topic.

### HTTP Request

`DELETE https://api.backstit.ch/v2/topic/{TOPIC_TOKEN}/filters`

### Query Parameters

| Parameter | Required | Default | Description |
|---------|:-------:|:-------:|:-----------|
| key | yes | | Your organization's api key obtained from the organization dashboard. |
| data | yes | | The array of filters to add |

### Data Child Parameters

| Parameter | Required | Default | Description |
|---------|:-------:|:-------:|:-----------|
| type | yes | | The filter type. |
| value | yes | | The value of the filter. |

### Available Filters

| Type | Value | Description
|---------|:-------:|:-------:|
| include | phrase | Keywords that have to icluded in the feed |
| exclude | phrase | Keywords that have to excluded from the feed |


### Returns

| Field | Data Type | Description |
|---------|:-------:|:-----------|
| errors | array | A lits of error for filters that were not removed |
| filters | array | A list of global keyword filters that were removed |

## Delete Topic

```ruby
require 'rest_client'

response = RestClient.delete 'https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728', :params => {:key => '70b5aa707ca6013231ce482a14180728'}
```

```python
import urllib
import urllib2

url = 'https://api.backstit.ch/v2/topic/'
response = urllib2.urlopen(endpoint)
params = {'skip': 20}
data = urllib.urlencode(params)
req = urllib2.Request(endpoint, data)
response = urllib2.urlopen(req)
```

```shell
curl https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728
```

> The above command returns JSON structured like this:

```json
{
  "uid": 13,
  "name": "Detroit",
  "description": "",
  "banner": {
    "url": "http://images-backstitch.s3.amazonzws.com/next/logos/backstitch_purple_icon.png",
    "width": 300,
    "height": 300
  },
  "token": "9b5d30a07d4001325ede482a14180728",
  "filters": [],
  "sources":[]
}
```

This endpoint adds new sources to the topic.

### HTTP Request

`DELETE https://api.backstit.ch/v2/topic/{TOPIC_TOKEN}`

### Query Parameters

| Parameter | Required | Default | Description |
|---------|:-------:|:-------:|:-----------|
| key | yes | | Your organization's api key obtained from the organization dashboard. |


### Returns

| Field | Data Type | Description |
|---------|:-------:|:-----------|
| uid | integer | A unique identifier for the topic |
| name | string | The name given to the topic |
| description | string | An optional description of the topic |
| banner | object | The topic's displayed banner image if one was uploaded |
| token | string | The topic's api token |
| filters | array | A list of global keyword filters set on the topic |
| sources | array | A list of included sources |

## Get Topic Details

```ruby
require 'rest_client'

response = RestClient.get 'https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/details'
```

```python
import urllib2

response = urllib2.urlopen('https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/details')
```

```shell
curl https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/details
```

> The above command returns JSON structured like this:

```json
{
  "uid": 13,
  "name": "Detroit",
  "description": "",
  "banner": {
    "url": "http://images-backstitch.s3.amazonzws.com/next/logos/backstitch_purple_icon.png",
    "width": 300,
    "height": 300
  },
  "token": "9b5d30a07d4001325ede482a14180728",
  "filters": [
    {
      "uid": 345,
      "phrase": "downtown",
      "type": "include"
    }
  ],
  "sources": [
    {
      "filters":[],
      "uid": 76,
      "name": "Facebook Posts from backstitchapp",
      "icon":{
        "url": "http://images-backstitch.s3.amazonaws.com/services/icons/facebook.png",
        "width": 64,
        "height": 64
      },
      "banner":{
        "url": "http://images-backstitch.s3.amazonaws.com/next/service_catalog/facebook_banner.jpg",
        "width": 650,
        "height": 240
      },
      "params":{
        "user":{
          "type": "search_term",
          "value": "backstitchapp"
        }
      },
      "service_name": "facebook_user"
    }
  ]
}
```

This endpoint retrieves details about a topic.

### HTTP Request

`GET https://api.backstit.ch/v2/topic/{TOPIC_TOKEN}/details`

### Returns

| Field | Data Type | Description |
|---------|:-------:|:-----------|
| uid | integer | A unique identifier for the topic |
| name | string | The name given to the topic |
| description | string | An optional description of the topic |
| banner | object | The topic's displayed banner image if one was uploaded |
| token | string | The topic's api token |
| filters | array | A list of global keyword filters set on the topic |
| feeds | array | A list of included feeds |

## Retrieve Topic Results

```ruby
require 'rest_client'

# Retrieve latest results
response = RestClient.get 'https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/results'

# Retrieve second page of latest results
response = RestClient.get 'https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/results', :params => {:skip => 20} 

# Search Results
response = RestClient.get 'https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/results', :params => {:query => 'restaurants'} 
```

```python
import urllib
import urllib2

endpoint = 'https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/results'

# Retrieve latest results
response = urllib2.urlopen(endpoint)

# Retrieve second page of latest results
params = {'skip': 20}
data = urllib.urlencode(params)
req = urllib2.Request(endpoint, data)
response = urllib2.urlopen(req)

# Search Results
params = {'query': 'restaurants'}
data = urllib.urlencode(params)
req = urllib2.Request(endpoint, data)
response = urllib2.urlopen(req)
```

```shell
# Retrieve latest results
curl https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/results

# Retrieve second page of latest results
curl https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/results \
  -d "skip=20"
  
# Search Results
curl https://api.backstit.ch/v2/topic/9b5d30a07d4001325ede482a14180728/results \
  -d "query=restaurants"
```

> The above command returns JSON structured like this:

```json
[{
  "id": "2ee995b07cb201323db11aa32735a1a3",
  "type": "article",
  "title": "Renderings Revealed: Glass-Enclosed Restaurant & Patio Planned Downtown",
  "description": "Townhouse already announced plans for a new downtown Detroit location, but Eater Detroit has revealed that the Birmingham-based restaurant aspires to go beyond simply filling an empty storefront. Complete with a retractable roof, Townhouse Detroit plans to build a...",
  "full_text": "<div><p>[McIntosh Poris/Townhouse]  </p><p><strong>Townhouse</strong> already <a href=\"http://detroit.eater.com/2014/11/3/7148533/birminghams-townhouse-restaurant-is-headed-for-detroit\">announced</a> plans for a new downtown Detroit location, but <a href=\"http://detroit.eater.com/2015/1/8/7510471/townhouse-plans-greenhouse-style-dining-room-for-detroit-and-open-air\">Eater Detroit</a> has revealed that the Birmingham-based restaurant aspires to go beyond simply filling an empty storefront. Complete with a retractable roof, Townhouse Detroit plans to build a <strong>greenhouse-like dining room</strong> right next to One Detroit Center with seating for 120.</div>",
  "plain_text": "Townhouse already announced plans for a new downtown Detroit location, but Eater Detroit has revealed that the Birmingham-based restaurant aspires to go beyond simply filling an empty storefront. Complete with a retractable roof, Townhouse Detroit plans to build a greenhouse-like dining room right next to One Detroit Center with seating for 120.",
  "author": {
    "name": "Paul Beshouri"
  },
  "published_at": "2015-01-12T17:42:11Z",
  "images": {
    "thumbnail": {
      "url": null,
      "width": null,
      "height": null
    },
    "full_size": {
      "url": "http://detroit.curbed.com/uploads/townhouse1-thumb.jpg",
      "width": null,
      "height": null
    },
    "other": []
  },
  "source": {
    "id": null,
    "name": null,
    "description": null,
    "url": null,
    "icon": {
      "url": null,
      "width": null,
      "height": null
    }
  },
  "origin": {
    "id": null,
    "name": "Curbed Detroit",
    "url": "http://detroit.curbed.com/archives/2015/01/glassenclosed-restaurant-patio-planned-downtown.php",
    "icon": {
      "url": "http://curbed.com/favicon.ico",
      "width": 16,
      "height": 16
    }
  }
}]
```

This endpoint retrieves results from the topic.  By default all results are returned in descending order by the date that the content was originally published.  If a query is passed then the order is based on the most relevent match to
the query.

### HTTP Request

`GET https://api.backstit.ch/v2/topic/{TOPIC_TOKEN}/results`

### Query Parameters

| Parameter | Required | Default | Description |
|---------|:-------:|:-------:|:-----------|
| count | no | 20 | The number of results to return per call, up to a maximum of 50. |
| skip | no | 0 | The number of results to skip per call, to be used for paging. |
| query | no | | This will return results that are most relevant to the passed term. | 

### Returns

An array of results.  Consult the [Result Type Dictionary](/#result-type-dictionary) for field descriptions.