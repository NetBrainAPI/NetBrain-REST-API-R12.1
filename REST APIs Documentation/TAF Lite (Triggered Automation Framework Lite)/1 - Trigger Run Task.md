
# Trigger Run Task API Design

## ***POST*** V3/TAF/Lite/run
Call this API to trigger run task

## Detail Information

> **Title** : Trigger Run Task<br>

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
|||* - required<br />^ - optional|
|endpoint*|string|The endpoint in the TAFLite definition. |
|passKey*|string|The access permission needs to match the Endpoint selected above for Trigger Run to be successful. |
|filterDevices^|array|Used to quickly filter rows by device. <br />device Name List - optional parameter. <br />If this parameter has a value, these device names will be used to match ADT rows with the Device Column of ADT. <br />If the device name of any device column is in the device name list, the row will be deemed to meet the condition |
|intentColumns|array|Corresponds to the endpoint in the TAFLite definition. |
|option^|object|Advanced setting |
|option.rawData|bool|True - indicates that there is always raw data \False - indicates that it is not mandatory to save raw data. Whether to save raw data depends on the definition of NI <br /><br />Default: False <br /><br />e.g. if status code is defined, there must be raw data |
|option.dataSource|int|0: Live <br />1: Baseline <br /><br />Default: 0 (Live Execution Intent) |
|option.rowFilter|object|JSON object <br /><br />ColumnDisplayName: value <br /><br />Use AND to perform operations between JSON attributes |
|option.maxExecuteNIColumn|int|The maximum number of columns of NI Column allowed to run <br />Default: 1 |

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
import json
import time
import requests.packages.urllib3 as urllib3
 
urllib3.disable_warnings()
 
user = "abc"
pwd = "1234"
host_url = "http://10.10.0.29"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}

# Get token for netbrain
def getTokens(user,password):
    login_api_url = r"/ServicesAPI/API/V1/Session"
    Login_url = host_url + login_api_url
    data = {
        "username": user,
        "password": password
    }
    token = requests.post(Login_url, data=json.dumps(
        data), headers=headers, verify=False)
    if token.status_code == 200:
        print(token.json())
        return token.json()["token"]
    else:
        return "error"
# get token
token = getTokens(user,pwd)
headers["Token"] = token
 
def lite_run(API_Body):
 
    # Trigger  API url
    API_URL = r"/ServicesAPI/API/V3/TAF/Lite/run"
    # Trigger API payload
    api_full_url = host_url + API_URL
    api_result = requests.post(api_full_url, data=json.dumps(
        API_Body), headers=headers, verify=False)
    if api_result.status_code == 200:
        return api_result.json()
    else:
        return api_result.json()
 
if __name__ =="__main__": 
    API_BODY = {
        "endpoint": "T100007",
        "passKey": "TAFLite!1234567890",
        "filterDevices": [],
        "intentColumns": ["NI2"],
        "option": {
        }
    }
    
    try:
        print(lite_run(API_BODY))
    
    except Exception as e:
        print (str(e)) 

```
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman

```python

# call login API
curl -X POST \
http://10.10.0.29/ServicesAPI/API/V1/Session \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -d '{
    "username" : "chendezhi",
    "password" : "Qwer@1234"  
}'

# call One IP Table API
curl -X POST \
  'http://10.10.0.29//ServicesAPI/API/V3/TAF/Lite/run' \
  -H 'cache-control: no-cache' \
  -H "Content-Type: application/json"
  -H 'token: b6bd93cd-5e5a-43fd-83ac-2c92b304e5c8'
  -d '{
        "endpoint": "T100007",
        "passKey": "TAFLite!1234567890",
        "filterDevices": [],
        "intentColumns": ["NI2"],
        "option": {
        }
    }'
```