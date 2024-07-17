
# Get Search Results API Design

## ***POST*** V3/TAF/Lite/run
Call this API to trigget run task

## Detail Information

> **Title** : Get Search Results API<br>

> **Version** : 17/07/2024

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/TAF/Lite/run

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
||* - required\n^ - optional|
|endpoint*|string|Corresponds to the endpoint in the TAFLite definition. |
|passKey*|string|The access permission needs to match the Endpoint selected above for Trigger Run to be successful. |
|filterDevices^|array|Used to quickly filter rows by device. \n device Name List - optional parameter. \nIf this parameter has a value, these device names will be used to match ADT rows with the Device Column of ADT. \nIf the device name of any device column is in the device name list, the row will be deemed to meet the condition |
|intentColumns|array|Corresponds to the endpoint in the TAFLite definition. |
|option^|object|Advanced setting |
|option.rawData|bool|True - indicates that there is always raw data \nFalse - indicates that it is not mandatory to save raw data. Whether to save raw data depends on the definition of NI \n\nDefault: False \n\ne.g. if status code is defined, there must be raw data |
|option.dataSource|int|0: Live \n1: Baseline \n\ndefault: 0 (Live Execution Intent) |
|option.rowFilter|object|JSON object \n\nColumnDisplayName: value \n\nUse AND to perform operations between JSON attributes |
|option.maxExecuteNIColumn|int|The maximum number of columns of NI Column allowed to run \nDefault: 1 |

> ***Example***
```python
{
    endpoint: 'T000001',
    passKey: '123456',
    filterDevices: ['BJ-R1', 'BJ-R2'],
    intentColumns: ["NI Column1"],
    option: {
        rawData: false,
        dataSource: 0,
        rowFilter: {
            Column1: 'v1',
            Column2: 'v2',
        },
        maxExecuteNIColumn: 1
    }
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
TaskID executed by this Trigger

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***


```python
{
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