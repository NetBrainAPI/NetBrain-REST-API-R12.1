
# Get Search Results API Design

## ***GET*** V1/CMDB/Search
Call this API to get the list of device names with the matching search keyword from NetBrain IE.

## Detail Information

> **Title** : Get Search Results API<br>

> **Version** : 12/03/2024.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/DataEngine/SearchResult

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

>No parameters required.

## Parameters(****required***)
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|keyword|string|Search keyword inputted by user. Max length: 10 chars, rest ignored. |
|limit|integer|The up limit amount of device records to return per API call. The "limit" value valid range is 10 - 100, if the assigned value exceeds the range, the server will respond error message "Parameter 'limit' must be greater than or equal to 10 and less than or equal to 100." The value cannot be negative. If the value is negative, API throws exception {"statusCode":791001,"statusDescription":"Parameter 'limit' cannot be negative"}. |
|index|integer|The index number is used to indicate the last record scanned in DB from the last API call. The value of this parameter is the last API call returned index number plus 1. For example, the last call returned index is 1000, then provide parameter value as 1001 to avoid duplicate record compare with the last API call. This is a required parameter if amount of records need to be skipped. The value cannot be negative. If the value is negative, API throws exception {"statusCode":791001,"statusDescription":"Parameter 'index' cannot be negative"}. |
|||limit and index parameters are based on the search result from DB. If both limit and index are not provided, return the device list with 50 devices start from the first device result in DB. If only provide limit value, return from the first device result in DB. If only provide index value, return the device list with 50 devices start from the index number. If provided both limit and index, return as required. Error exceptions follow each parameter's description. |
|type|integer|Search type, device search value is 1. Other values can be considered for expansion in the future. |

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
|deviceResults| object array | List of search results. This result contains the device name and its configuration file, which contains the search keyword inputted in Parameter.  |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***


```python
{
   "deviceResults":[
 
      {  
           "deviceName":"r1"
      },
     {
           "deviceName":"r2"
      }
   ]
    'statusCode': 790200, 
    'statusDescription': 'Success.'
}
```

# Full Example:


```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Set the request inputs
token = "9c717c9a-4302-45b5-a068-2a3e9c4ea1a3"
nb_url = "http://192.168.30.166"
full_url = nb_url + "/ServicesAPI/API/V1/TAF/QuickResult"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["token"] = token

try:
    response = requests.get(full_url, headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Get modules failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman

```python
curl -X GET \
  http://192.168.31.191/ServicesAPI/API/V1/CMDB/NetworkSettings/TelnetInfo/RefreshDeviceCount \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \ 
  -H 'token: b0181119-7b1b-4faf-be97-b8566a39a640' \
```