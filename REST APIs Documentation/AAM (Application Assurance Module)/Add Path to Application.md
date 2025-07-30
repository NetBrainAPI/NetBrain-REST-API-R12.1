
# Add Path to Application API Design

## ***POST*** V3/AAM/Application/Path
This API is used to add Path to existing Application. <br>
If the Application doesn't exist, error message indicating that the Application was not found will be returned and the call will be aborted. <br>
If the Application does exist and 
> 1. `overwrite` = False, the existing paths will be ignored and only the new paths will be added to the Application. <br>
> 2. `overwrite` = True, the existing paths will be updated with the new paths in the Application.


## Detail Information

> **Title** : Add Path to Application<br>

> **Version** : 29/07/2025

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
|applicationId*|string| Application ID. |
|paths*|array| Path Information to add to the Application. |
|paths[].name*|string| Name of the Path.|
|paths[].source*|string|Path Source Information. |
|paths[].sourcePort^|string| Source Port. <br> This field is required if the protocol is TCP/UDP. |
|paths[].dest*|string| Path Destination Information.|
|paths[].destPort^|string| Path Destination Information.|
|paths[].routingScheme^|int| Routing scheme of Path. <br> `0` - Unicast <br>`1` - Multicast|
|paths[].group^|string| Multicast Group. Only required when `routingScheme=Multicast`|
|paths[].protocolKeyWord*|string| Path protocol.<br> Default: `IP`|
|paths[].advancedOption^ |object| Advanced Option of Path.|
|paths[].advancedOption.useSettingType^|int| |
|paths[].advancedOption.debugMode^|bool| Default: `False`|
|paths[].advancedOption.calcWhenDeniedByACL^|bool| Default: `False`|
|paths[].advancedOption.calcWhenDeniedByPolicy^|bool| Default: `True` |
|paths[].advancedOption.calcL3ActivePath^|bool| Default: `False` |
|paths[].advancedOption.useCommandWithArguments^|bool| Default: `False`|
|paths[].advancedOption.enablePathFixup^|bool| Default: `True`|
|paths[].advancedOption.expertSettings^|string | Defines Parameter in JSON string format.|
|overwrite^|bool|Controls whether to overwrite an existing path if it already exists. <br> Default: `False`|

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
|path| array | Path of the Application. |
|id| string | ID of the Path. |
|name| string | Name of the Path.  |
|source|string| Source Information.|
|sourceIp|string| Source IP address.|
|dest|string| Destination Information.|
|destIp|string| Destination IP address.|
|protocolKeyWord|string| Protocol keyword.|
|advancedOption|object| Advanced Option of the Path Details.|
|routinScheme| int | Routing Scheme. |
|group| string | Group. |
|applicationId| string | ID of the Application. |
|applicationName| string | Name of the Application. |
|statusCode| integer | The returned status code of executing the API. |
|statusDescription| string | The explanation of the status code. |


# Full Example:
## Example 1: Add Path to Application with `overwrite=False`
```python
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Application/Path"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    "applicationId": "ddc769ef-3c8b-4f96-a68c-0a4b6d9f81d3",
    "paths": [
        {
            "name": "testAPIPath103",
            "source": "172.24.10.10",
            "sourcePort": None,
            "dest": "BJ-R3",
            "destPort": None,
            "protocolKeyWord": "IPv4",
            "isLiveUseBaseLineConfig": True,
            "advancedOption": {
                "useSettingType": 0,
                "debugMode": False,
                "calcWhenDeniedByACL": False,
                "calcWhenDeniedByPolicy": True,
                "calcL3ActivePath": False,
                "useCommandsWithArguments": False,
                "enablePathFixup": True,
                "expertSettings": ""
            },
            "routingScheme": 0,
            "group": ""
        }
    ],
    "overwrite": False
}
try:
    response = requests.post(full_url, data = json.dumps(data), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Add Path to Application! - " + str(response.text))
except Exception as e:
    print (str(e))
```
```python
{
  "paths": [
    {
      "id": "1d232e8d-98f8-434f-9ffa-bb5dc3169dbf",
      "name": "testAPIPath103",
      "source": "BJ-R2",
      "sourceIp": "172.24.10.10",
      "dest": "BJ-R3",
      "destIp": "BJ-R3",
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
      "applicationName": "Hello11"
    }
  ],
  "statusCode": 790200,
  "statusDescription": "Success."
}
```
## Example 2: Add Path to Application with `overwrite=True`
From Example 1, update `dest` to `BJ-R1`
```python
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Application/Path"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    "applicationId": "ddc769ef-3c8b-4f96-a68c-0a4b6d9f81d3",
    "paths": [
        {
            "name": "testAPIPath103",
            "source": "172.24.10.10",
            "sourcePort": None,
            "dest": "BJ-R1",
            "destPort": None,
            "protocolKeyWord": "IPv4",
            "isLiveUseBaseLineConfig": True,
            "advancedOption": {
                "useSettingType": 0,
                "debugMode": False,
                "calcWhenDeniedByACL": False,
                "calcWhenDeniedByPolicy": True,
                "calcL3ActivePath": False,
                "useCommandsWithArguments": False,
                "enablePathFixup": True,
                "expertSettings": ""
            },
            "routingScheme": 0,
            "group": ""
        }
    ],
    "overwrite": True
}
try:
    response = requests.post(full_url, data = json.dumps(data), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Add Path to Application! - " + str(response.text))
except Exception as e:
    print (str(e))
```
```python
{
  "paths": [
    {
      "id": "1d232e8d-98f8-434f-9ffa-bb5dc3169dbf",
      "name": "testAPIPath103",
      "source": "BJ-R2",
      "sourceIp": "172.24.10.10",
      "dest": "BJ-R1",
      "destIp": "BJ-R1",
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
      "applicationName": "Hello11"
    }
  ],
  "statusCode": 790200,
  "statusDescription": "Success."
}
```


## Example 3: Failed to Add Path to Application - Application Does Not Exist
The ID of the Application does not exist <br>

```python
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Application/Path"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    "applicationId": "ddc769ef-3c8b-4f96-a68c-0a4b6d9f81d32",
    "paths": [
        {
            "name": "testAPIPath103",
            "source": "172.24.10.10",
            "sourcePort": None,
            "dest": "BJ-R3",
            "destPort": None,
            "protocolKeyWord": "IPv4",
            "isLiveUseBaseLineConfig": True,
            "advancedOption": {
                "useSettingType": 0,
                "debugMode": False,
                "calcWhenDeniedByACL": False,
                "calcWhenDeniedByPolicy": True,
                "calcL3ActivePath": False,
                "useCommandsWithArguments": False,
                "enablePathFixup": True,
                "expertSettings": ""
            },
            "routingScheme": 0,
            "group": ""
        }
    ],
    "overwrite": False
}
try:
    response = requests.post(full_url, data = json.dumps(data), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Add Path to Application! - " + str(response.text))
except Exception as e:
    print (str(e))
```
```python
Failed to Add Path to Application! - {"statusCode":791006,"statusDescription":"The application (id: ddc769ef-3c8b-4f96-a68c-0a4b6d9f81d32) does not exist."}

```

# cURL Code from Postman
The cURL command is based on Example 1.
```python
curl -X POST \
  "http://192.168.36.19/ServicesAPI/API/V3/AAM/Application/Path" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "cache-control: no-cache" \
  -H "token: 1333a5eb-5e13-4d85-b813-8f87f80fb970"
  -d '{
    "applicationId": "ddc769ef-3c8b-4f96-a68c-0a4b6d9f81d3",
    "paths": [
        {
            "name": "testAPIPath103",
            "source": "172.24.10.10",
            "sourcePort": null,
            "dest": "BJ-R1",
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
        }
    ],
    "overwrite": false
}'
```
