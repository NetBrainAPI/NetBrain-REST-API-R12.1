
# Site API Design

## ***GET*** /V1/CMDB/Sites/SiteInfo{?sitePath}|{?siteId}
Call this API to get the basic information of a site by site path or ID.

**Note that some site management APIs require calling of other site management APIs prior to, or after.
Refer to [Golden Use Case Templates - Site Management by Calling APIs](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/Golden%20Use%20Case%20Templates/Sites%20Management%20by%20Calling%20APIs.md) for more information.

## Detail Information

> **Title** : Get Site Info API<br>

> **Version** : 02/04/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Sites/SiteInfo

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

>No request body.

## Query Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|sitePath^ | string  | Full path name of the site. e.g. 'My Network/Site1/Boston/Dev' |
|siteId^ | string  |  Unique ID of the specified site. |
>**Note :** ^ required if the other parameter is null.

## Headers

> **Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| Content-Type | string  | support "application/json" |
| Accept | string  | support "application/json" |

> **Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| token | string  | Authentication token, get from login API. |

## Response

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |
|siteInfo | object | An object with the basic information of the site.  |
|siteInfo.sitePath | string | Full path of the site.  |
|siteInfo.siteId| string | ID of the site. This is the only way to get the id of root site. |
|siteInfo.siteType| integer | Type of this site; 0 root site, 1 container site, 2 leaf site.  |

> ***Example***


```python
{
  "siteInfo": [
    {
      "siteId": "1da4fda8-5d04-491b-8bb0-2e9abb989a60",
      "sitePath": "My Network/NA/US",
      "isContainer": true,
      "siteType": 0
    }
  ]
}
```
```python
{
  "siteInfo": {
    "siteId": "732e8ab6-6b69-417d-ad03-2cc447100166",
    "sitePath": "My Network",
    "isContainer": true,
    "children": null,
    "siteType": 0
  },
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

# Full Example：


```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Set the request inputs
token = "9603ce1d-8271-4f77-a2df-0b748ef32084"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/SiteInfo"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

sitePath = "My Network/Site2"
siteId = ""

data = {
           "sitePath" : sitePath
            #"siteId": siteId
        } 

try:
    response = requests.get(full_url, params = data, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Failed to get Site Info! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    Failed to get Site Info! - {"statusCode":791006,"statusDescription":"site with path My Network/Site2 does not exist."}
    

# cURL Code from Postman: 


```python
curl -X GET \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Sites/SiteInfo?sitePath=My%20Network/Site2' \
  -H 'Postman-Token: e76aaf99-8664-4018-81db-9166ec56ef7c' \
  -H 'cache-control: no-cache' \
  -H 'token: 9603ce1d-8271-4f77-a2df-0b748ef32084'
```

# Error Examples：
See API _Delete Site API_ for error examples.
