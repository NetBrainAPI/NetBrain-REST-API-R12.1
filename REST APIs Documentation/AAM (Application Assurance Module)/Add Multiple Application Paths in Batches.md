
# Add Multiple Application Paths in Batches API Design

## ***POST*** V3/AAM/Batch/Add
This API is used to add Multiple Paths of Applications in Batches. <br>
If there are duplicates, only the first one will be retained.

## Detail Information

> **Title** : Add Multiple Application Paths in Batches<br>

> **Version** : 30/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/AAM/Batch/Add

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
|applicationPaths*|array| Batch Application Path. |
|applicationPaths[].name*|string| Application Name. |
|applicationPaths[].paths*|array| Path Information to add to the Application. |
|applicationPaths[].paths[].name*|string| Path Name.|
|applicationPaths[].paths[].source*|string|Path Source Information. |
|applicationPaths[].paths[].sourcePort^|string| Source Port. <br> This field is required if the protocol is TCP/UDP. |
|applicationPaths[].paths[].dest*|string| Path Destination Information.|
|applicationPaths[].paths[].destPort^|string| Path Destination Information.|
|applicationPaths[].paths[].routingScheme^|int| Routing scheme of Path. <br> `0` - Unicast <br>`1` - Multicast|
|applicationPaths[].paths[].group^|string| Multicast Group. Only required when `routingScheme=Multicast`|
|applicationPaths[].paths[].protocolKeyWord^|string| Path protocol.<br> Default: `IP`|
|applicationPaths[].paths[].advancedOption^ |object| Advanced Option of Path.|
|applicationPaths[].paths[].advancedOption.useSettingType^|int| |
|applicationPaths[].paths[].advancedOption.debugMode^|bool| Default: `False`|
|applicationPaths[].paths[].advancedOption.calcWhenDeniedByACL^|bool| Default: `False`|
|applicationPaths[].paths[].advancedOption.calcWhenDeniedByPolicy^|bool| Default: `True` |
|applicationPaths[].paths[].advancedOption.calcL3ActivePath^|bool| Default: `False` |
|applicationPaths[].paths[].advancedOption.useCommandWithArguments^|bool| Default: `False`|
|applicationPaths[].paths[].advancedOption.enablePathFixup^|bool| Default: `True`|
|applicationPaths[].paths[].advancedOption.expertSettings^|string | Defines Parameter in JSON string format.|
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
|data| object | Application Name. |
|aaName| string | Application Name. |
|aamId| string | Application ID. |
|id| string | Path ID.  |
|name| string | Path Name.  |
|sourceIp|string| Source IP address.|
|sourceName|string| Source Name.|
|destIp|string| Destination IP address.|
|destName|string| Destination Name.|
|routingScheme| int | Routing Scheme. |
|group| string | Group. |
|protocolKeyWord|string| Protocol keyword.|
|advancedOption|object| Advanced Option of the Path Details.|
|historyApplicationCount| int |  |
|goldenPathType| int |  |
|autoSetGolden| object |  |
|latestPathApplication| object |  |
|taskType| int |  |
|taskTypeDisplay| object |  |
|statusCode| int | The returned status code of executing the API. |
|statusDescription| string | The explanation of the status code. |


# Full Example
```python
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Batch/Add"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
            "applicationPaths": [
                {
                    'name': "testApp1",
                    'paths': [
                        {
                            'name': "testPath1",
                            'source': '10.10.0.17',
                            'dest': "10.10.19.11",
                            'protocolKeyWord': "IPv4",
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
                            }
                        },
                        {
                            'name': "testPath2",
                            'source': '10.10.0.17',
                            'dest': "10.10.19.11",
                            'protocolKeyWord': "IPv4",
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
                            }
                        }
                    ]
                },
                {
                    'name': "testApp2",
                    'paths': [
                        {
                            'name': "testPath1",
                            'source': '10.10.0.17',
                            'dest': "10.10.19.11",
                            'protocolKeyWord': "IPv4",
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
                            }
                        },
                        {
                            'name': "testPath2",
                            'source': '10.10.0.17',
                            'dest': "10.10.19.11",
                            'protocolKeyWord': "IPv4",
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
                            }
                        }
                    ]
                }
            ]
        }
try:
    response = requests.post(full_url, data = json.dumps(data), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Add Multiple Paths in Batches in Application! - " + str(response.text))
except Exception as e:
    print (str(e))
```
```python
{
  "data": [
    {
      "aamName": "testApp1",
      "aamId": "35c076e7-ccca-4a9e-8599-6adb4d3a85c2",
      "id": "fe965436-b160-495b-b321-b433f0901d94",
      "name": "testPath1",
      "sourceIp": "10.10.0.17",
      "sourceName": "10.10.0.17",
      "destIp": "10.10.19.11",
      "destName": "10.10.19.11",
      "routingScheme": 0,
      "group": "",
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
      "historyApplicationCount": 0,
      "goldenPathType": 0,
      "autoSetGolden": {
        "finished": false,
        "calcCount": 0,
        "notChangedCount": 0
      },
      "latestPathApplication": {
        "verifiedResult": 0,
        "compareResult": {
          "goldenPath": {},
          "previousPath": {}
        },
        "taskType": 0,
        "taskTypeDisplay": ""
      }
    },
    {
      "aamName": "testApp1",
      "aamId": "35c076e7-ccca-4a9e-8599-6adb4d3a85c2",
      "id": "d35919ed-2f50-4e0d-a843-31adadbac117",
      "name": "testPath2",
      "sourceIp": "10.10.0.17",
      "sourceName": "10.10.0.17",
      "destIp": "10.10.19.11",
      "destName": "10.10.19.11",
      "routingScheme": 0,
      "group": "",
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
      "historyApplicationCount": 0,
      "goldenPathType": 0,
      "autoSetGolden": {
        "finished": false,
        "calcCount": 0,
        "notChangedCount": 0
      },
      "latestPathApplication": {
        "verifiedResult": 0,
        "compareResult": {
          "goldenPath": {},
          "previousPath": {}
        },
        "taskType": 0,
        "taskTypeDisplay": ""
      }
    },
    {
      "aamName": "testApp2",
      "aamId": "9a549064-e0f3-48e8-af4c-f663dd19cd82",
      "id": "f21d3174-d849-4291-b94a-be4b5fa7849d",
      "name": "testPath1",
      "sourceIp": "10.10.0.17",
      "sourceName": "10.10.0.17",
      "destIp": "10.10.19.11",
      "destName": "10.10.19.11",
      "routingScheme": 0,
      "group": "",
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
      "historyApplicationCount": 0,
      "goldenPathType": 0,
      "autoSetGolden": {
        "finished": false,
        "calcCount": 0,
        "notChangedCount": 0
      },
      "latestPathApplication": {
        "verifiedResult": 0,
        "compareResult": {
          "goldenPath": {},
          "previousPath": {}
        },
        "taskType": 0,
        "taskTypeDisplay": ""
      }
    },
    {
      "aamName": "testApp2",
      "aamId": "9a549064-e0f3-48e8-af4c-f663dd19cd82",
      "id": "c2f3aa15-2f2d-4c70-bb24-2b79f296b28c",
      "name": "testPath2",
      "sourceIp": "10.10.0.17",
      "sourceName": "10.10.0.17",
      "destIp": "10.10.19.11",
      "destName": "10.10.19.11",
      "routingScheme": 0,
      "group": "",
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
      "historyApplicationCount": 0,
      "goldenPathType": 0,
      "autoSetGolden": {
        "finished": false,
        "calcCount": 0,
        "notChangedCount": 0
      },
      "latestPathApplication": {
        "verifiedResult": 0,
        "compareResult": {
          "goldenPath": {},
          "previousPath": {}
        },
        "taskType": 0,
        "taskTypeDisplay": ""
      }
    }
  ],
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

# cURL Code from Postman
```python
curl -X POST \
  "http://192.168.36.19/ServicesAPI/API/V3/AAM/Batch/Add" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "cache-control: no-cache" \
  -H "token: 1333a5eb-5e13-4d85-b813-8f87f80fb970"
  -d '{
        "applicationPaths": [
            {
                'name': "testApp1",
                'paths': [
                    {
                        'name': "testPath1",
                        'source': "10.10.0.17",
                        'dest': "10.10.19.11",
                        'protocolKeyWord': "IPv4",
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
                        }
                    },
                    {
                        'name': "testPath2",
                        'source': "10.10.0.17",
                        'dest': "10.10.19.11",
                        'protocolKeyWord': "IPv4",
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
                        }
                    }
                ]
            },
            {
                'name': "testApp2",
                'paths': [
                    {
                        'name': "testPath1",
                        'source': "10.10.0.17",
                        'dest': "10.10.19.11",
                        'protocolKeyWord': "IPv4",
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
                        }
                    },
                    {
                        'name': "testPath2",
                        'source': "10.10.0.17",
                        'dest': "10.10.19.11",
                        'protocolKeyWord': "IPv4",
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
                        }
                    }
                ]
            }
        ]
    }'
```
