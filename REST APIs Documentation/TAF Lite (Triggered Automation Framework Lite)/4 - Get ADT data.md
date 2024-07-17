
# Get ADT Data API Design

## ***POST*** V3/TAF/Lite/adt/data
Call this API to get ADT data

## Detail Information

> **Title** : Get ADT Data<br>

> **Version** : 17/07/2024

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/TAF/Lite/adt/data

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
|endpoint*|string|The endpoint in the TAFLite definition. |
|passKey*|string|Access permission. |
|filterDevices^|array|Used to quickly filter rows by device. </br>device Name List - this parameter is optional </br>If this parameter has a value, these device names will be used to match ADT rows with the Device Column of ADT. If the device name of any device colun is in the device name list, the row will be deemed to meet the conditions.|
|columns^|array|Column display name list, used to express the returned value. </br>If there is no value, or the array does not have any items, all columns are returned. |
|option^|object|Advanced setting|
|option.rowFilter	|object|JSON object <br /><br />ColumnDisplayName: value <br /><br />Use AND to perform operations between JSON attributes |
|pageSize|int|Returns the number of data items. </br>Default: 100|
|pageNumber|int|Returns the page of data. </br>Default: 1|


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
The data model returned by each DataType:

|**Cell Data Type**|**Description**|
|------|------|
|<img width=100/>|<img width=500/>|
|string|````python</br>{    "value": "hello world"}````|
|int|Intent Name|
|bool|NI execution time point|
|float|NI status code|
|DateTime|NI CSV results|
|device|Command results under device|
|Interface|Device name|
|DeviceList|Name of the command|
|InterfaceList|Information of the command|
|map|Map results executed by NI|
|site|------|
|path|------|
|NI|------|
|Dataset|------|



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