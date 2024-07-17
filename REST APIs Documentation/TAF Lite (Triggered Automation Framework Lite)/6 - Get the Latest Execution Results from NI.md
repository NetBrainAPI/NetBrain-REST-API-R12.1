
# Get the Latest Execution Results from NI API Design

## ***POST*** V3/CMDB/NI/result
Call this API to get the running status of Trigger Task and running ResultID of NI

## Detail Information

> **Title** : Get the Latest Execution Results from NI<br>

> **Version** : 17/07/2024

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/CMDB/NI/result

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|||* - required<br />^ - optional|
|niIdOrPath*|string|NI ID or NI Path |
|startTime^|DateTime|Start time of NI execution |
|endTime^|DateTime|End time of NI execution |
|output*|array|<table><tr><th>Value</th><th>Enum</th><th>Description</th></tr> <tr><td>0</td><td>all</td><td>All data, except raw data</td></tr> <tr><td>1</td><td>status_code</td><td>Status code and message</td></tr> <tr><td>2</td><td>raw_data</td><td>Raw data</td></tr> <tr><td>3</td><td>csv</td><td>CSV</td></tr> <tr><td>4</td><td>map</td><td>Intent map and external map</td></tr> </table>|

> ***Example***
```python
{      
   niIdOrPath: 'Public\\ni1',
   output: [0, 1, 2, 3, 4] //0:all, 1: rawdata, 2: status code, 3: csv, 4: map
}
```

## Parameters(****required***)
>No parameters required.


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
The returned NI execution result content includes Home NI and Follow NI results, hence the result is of type List.

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|[].niId|string|Intent ID|
|[].niName|string|Intent Name|
|[].timePoint|string|NI execution time point|
|[].statusCodes|list|NI status code|
|[].csvs|list of objects|NI CSV results|
|[].rawDatas|string|Command results under device|
|[].rawDatas[].deviceName|string|Device name|
|[].rawDatas[].command|string|Name of the command|
|[].rawDatas[].rawData|string|Information of the command|
|[].maps|list of objects|Map results executed by NI|


> ***Example***

```python
[{
    "niId": "intent1 id",
    "niName": "intent1 name",
    "statusCodes": [
      "intent status code1",
      "intent status code2"
    ],
    "csvs": [ {
        "fileName": "csv file name1", 
        "csv": "a1,b1,c1"
      }],
    "rawDatas": [
      {
        "device": "BJ-R1", 
        "command": "show config",
        "rawData": "raw data1"
      }
    ],
    "maps": [
        {
            "id": "map id 1",
            "name": "map Name 1",
            "url": "map url 1",
            "type": "Intent Map" //Intent Map; External Map
        }
    ]
}]
```

# Full Example:
DRAFT

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