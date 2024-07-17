
# Get NI running results of Trigger Task API Design

## ***POST*** V3/TAF/Lite/result/datas
Call this API to get the NI running results of Trigger Task

## Detail Information

> **Title** : Get NI running results of Trigger Task<br>

> **Version** : 17/07/2024

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/TAF/Lite/result/datas

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
|niResultId*|string|NI's result ID; corresponds to NIExecutionResult's _id |
|output*|array|<table><tr><th>Value</th><th>Enum</th><th>Description</th></tr> <tr><td>0</td><td>all</td><td>All data, except raw data</td></tr>
<tr><td>1</td><td>status_code</td><td>Status code and message</td></tr> </table>|

> ***Example***
```python
{ 
   endpoint: 'endpoint1',
   niResultId: 'ni result id1',
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
TaskID executed by this Trigger

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|taskId|string|taskID executed by this Trigger|
|status|string|The execution status of this task <br />0: Pending <br />1: RUnning <br />2: Finished <br />Only when status is Finished, the following intents will have results|
|intents|list of objects|Trigger Intent List|
|intents[].id|string|Intent ID|
|intents[].name|string|Intent Name|
|intents[].resultId|string|Intent Execution Result ID|
|intents[].hasAlert|boolean|Alert Status Code in Intent Execution Result|

> ***Example***


```python
{
    taskId: 'xxxxx1',
    status: 2, //0: pending, 1: running, 2: finished
    intents: [{
        id: 'xxxx1-111',
        name: 'intent1 name',
        resultId: 'yyyy1-111',
        hasAlert: true
    }, {
        id: 'xxxx1-222',
        name: 'intent2 name',
        resultId: 'yyyy2-2222',
        hasAlert: false
    }]
}
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