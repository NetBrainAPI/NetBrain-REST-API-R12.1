
# Update Path in Application API Design

## ***PUT*** V3/AAM/Application/Path
This API is used to update Path in an Application. <br>
If the Path doesn't exist, error message indicating that the Path was not found will be returned and the call will be aborted. <br>


## Detail Information

> **Title** : Update Path in Application<br>

> **Version** : 30/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/AAM/Application/Path

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
|id*|string| Path ID. |
|name*|string| Path Name.|
|source*|string|Path Source Information. |
|sourcePort^|string| Source Port. <br> This field is required if the protocol is TCP/UDP. |
|dest*|string| Path Destination Information.|
|destPort^|string| Path Destination Information.|
|routingScheme^|int| Routing scheme of Path. <br> `0` - Unicast <br>`1` - Multicast|
|group^|string| Multicast Group. Only required when `routingScheme=Multicast`|
|protocolKeyWord^|string| Path protocol.<br> Default: `IP`|
|advancedOption^|object| Advanced Option of Path.|
|advancedOption.useSettingType^|int| |
|advancedOption.debugMode^|bool| Default: `False`|
|advancedOption.calcWhenDeniedByACL^|bool| Default: `False`|
|advancedOption.calcWhenDeniedByPolicy^|bool| Default: `True` |
|advancedOption.calcL3ActivePath^|bool| Default: `False` |
|advancedOption.useCommandWithArguments^|bool| Default: `False`|
|advancedOption.enablePathFixup^|bool| Default: `True`|
|advancedOption.expertSettings^|string | Defines Parameter in JSON string format.|

## Parameters(****required***)
>No parameters required.


## Headers

> **Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| Content-Type | string  | support "application/json" |
| Accept | string | support "application/json" |

> **Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| token | string  | Authentication token, get from login API. |

## Response
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|path| object | Path of the Application. |
|id| string | ID of the Path. |
|name| string | Name of the Path.  |
|source|string| Source Information.|
|sourceIp|string| Source IP address.|
|dest|string| Destination Information.|
|destIp|string| Destination IP address.|
|protocolKeyWord|string| Protocol keyword.|
|advancedOption|object| Advanced Option of the Path Details.|
|routingScheme| int | Routing Scheme. |
|group| string | Group. |
|applicationId| string | ID of the Application. |
|applicationName| string | Name of the Application. |
|statusCode| integer | The returned status code of executing the API. |
|statusDescription| string | The explanation of the status code. |


# Full Example
```python
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Application/Path"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
        "id": "b304bbc2-1e6d-430a-93e8-3bb96552b81e",
        "name": "testAPIPath203",
        "source": "BJ-R3",
        "sourcePort": None,
        "dest": "GW2Lab",
        "destPort": None,
        "protocolKeyWord": "IPv4",
        "advancedOption": {
            "useSettingType": 0,
            "debugMode": False,
            "calcWhenDeniedByACL": False,
            "calcWhenDeniedByPolicy": True,
            "calcL3ActivePath": False,
            "useCommandsWithArguments": False,
            "enablePathFixup": True,
            "expertSettings": ""
        }
    }
try:
    response = requests.put(full_url, data = json.dumps(data), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Update Path in Application! - " + str(response.text))
except Exception as e:
    print (str(e))
```
```python
{
  "path": {
    "id": "b304bbc2-1e6d-430a-93e8-3bb96552b81e",
    "name": "testAPIPath203",
    "source": "BJ-R3",
    "sourceIp": "BJ-R3",
    "dest": "GW2Lab",
    "destIp": "GW2Lab",
    "protocolKeyWord": "IPv4",
    "advancedOption": {
      "useSettingType": 0,
      "debugMode": false,
      "calcWhenDeniedByACL": false,
      "calcWhenDeniedByPolicy": true,
      "calcL3ActivePath": false,
      "useCommandsWithArguments": false,
      "enablePathFixup": true,
      "expertSettings": ""
    },
    "routingScheme": 0,
    "group": "",
    "applicationId": "ddc769ef-3c8b-4f96-a68c-0a4b6d9f81d3",
    "applicationName": "App01"
  },
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

# cURL Code from Postman
```python
curl -X PUT \
  "http://192.168.36.19/ServicesAPI/API/V3/AAM/Application/Path" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "cache-control: no-cache" \
  -H "token: 1333a5eb-5e13-4d85-b813-8f87f80fb970"
  -d '{
        "id": "b304bbc2-1e6d-430a-93e8-3bb96552b81e",
        "name": "testAPIPath203",
        "source": "BJ-R3",
        "sourcePort": null,
        "dest": "GW2Lab",
        "destPort": null,
        "protocolKeyWord": "IPv4",
        "isLiveUseBaseLineConfig": true,
        "advancedOption": {
            "useSettingType": 0,
            "debugMode": false,
            "calcWhenDeniedByACL": false,
            "calcWhenDeniedByPolicy": true,
            "calcL3ActivePath": false,
            "useCommandsWithArguments": false,
            "enablePathFixup": true,
            "expertSettings": ""
        },
        "routingScheme": 0,
        "group": ""
    }'
```
