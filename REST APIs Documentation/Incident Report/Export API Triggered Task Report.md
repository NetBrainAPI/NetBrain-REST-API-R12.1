
# Export API Triggered Task Report API Design

## ***POST*** V1/CMDB/Incident/list
Call this API to export the API triggered task report.

## Detail Information

> **Title** : Export (POST) API Triggered Task Report<br>

> **Version** : 22/07/2024.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Incident/list

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

If body={}, it will return all incident files. </br>
Parameters of 'timeRange' are optional, 'from' or 'to' can be set according to their own requirements.

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|||* - required<br />^ - optional|
|timeRange^|object|default null, return incidents in the timeRange. |
|timeRange.from^|dateTime|the start of timeRange. |
|timeRange.to^|dateTime|the end of timeRange. |

> ***Example***

```python
start_time = "2024-03-24T01:57:00.981Z"
  end_time = "2024-03-26T01:57:00.982Z"
  body =  {
      'timeRange': {
          'from': start_time,
          'to': end_time
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
All fields of Response are not provided as it is long, intensive, and could consist of sensitive information. </br>
The important items are 'id' and 'name'. With these two fields, all information can be found.

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|id| string | ID of incident. |
|name| string | Name of incident.  |
|...|||
|statusCode| int | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***


```python
{
  "StatusCode": 200,
  "MainUrl": "API/v1/CMDB/Incident/list",
  "result": {
    "incidents": [
      {
        "alias": "100001",
        "source": {
          "occurredTime": "2024-03-26T01:39:29.791Z",
          "templateId": "b2cbc04b-5be0-e10d-ae70-cf1e52f716c5",
          "templateName": "Embedded_Incident_auto1",
          "templatePath": "All Network Intents/Embedded_Incident_auto1",
          "taskId": "46b4dda1-d282-41be-8b3a-fbbc806119af",
          "type": 65286,
          "name": "Embedded_Incident_auto1"
        },
        "status": 0,
        "type": 2,
        "mapCount": 1,
        "maps": [],
        "memberCount": 0,
        "deviceCount": 1,
        "messageCount": 2,
        "findingCount": 1,
        "invitees": [],
        "attachmentSize": 0,
        "enablePortal": true,
        "emailNotifyIEUser": false,
        "mode": 0,
        "members": [
          {
            "messageCount": 1,
            "findingCount": 1,
            "deleted": false,
            "automatioinContentType": 0,
            "isAutomation": false,
            "id": "12146145-1c10-4571-8085-ef52f1fb0553",
            "type": 0,
            "name": "NBPatchTester"
          }
        ],
        "tasks": [],
        "viewedUserList": [],
        "membership": 0,
        "id": "100001",
        "name": "Embedded_Incident_auto1",
        "description": "",
        "viewedTimes": 0,
        "viewedUsers": [],
        "accessCodeUserCounted": false,
        "viewedUserCount": 0,
        "createInfo": {
          "userId": "12146145-1c10-4571-8085-ef52f1fb0553",
          "userName": "NBPatchTester",
          "dateTime": "2024-03-26T01:39:29.86Z"
        },
        "operateInfo": {
          "opUserId": "12146145-1c10-4571-8085-ef52f1fb0553",
          "opUser": "NBPatchTester",
          "opTime": "2024-03-26T01:39:31.277Z"
        }
      }
    ],
    "statusCode": 0
  }
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
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Incident/list"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["token"] = token

# Export API Triggered Task Report
start_time = "2024-03-24T01:57:00.981Z"
end_time = "2024-03-26T01:57:00.982Z"
body =  {
        'timeRange': {
            'from': start_time,
            'to': end_time
        }
    }

try:
    response = requests.post(full_url, data = json.dumps(body), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Failed to export API Triggered Task Report - " + str(response.text))
    
except Exception as e:
    print (str(e)) 

```

# cURL Code from Postman

```python
curl -X POST \
  http://192.168.30.166/ServicesAPI/API/V1/CMDB/Incident/list \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \ 
  -H 'token: 4e1fd26d-fadf-4edb-bea3-e0c85f01a39f' \
  -d '{}'
```