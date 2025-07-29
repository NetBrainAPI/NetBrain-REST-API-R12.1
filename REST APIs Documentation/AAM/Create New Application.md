
# Create New Application API Design

## ***POST*** V3/AAM/Application
This API is used to create a new application. <br>
If the Application already exists, the subsequent actions are as follows: <br>
> 1. If `overwrite` = False, the queried Application and its associated Device data are returned.
> 2. If `overwrite` = True, the Application data are updated using the queried information; Application ID remains unchanged.

## Detail Information

> **Title** : Create New Application<br>

> **Version** : 29/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/AAM/Application

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
|name*|string| Name of Application; must be unique. |
|description^|string|Description of the Application.|
|relatedDevices^|object|Device information and weight associated with the application. |
|relatedDevices.deviceName^|string| Associated device name. <br>The device name must exist in the device table of the system domain; if it doesn't exist, it will be ignored. |
|relatedDevices.weight^|int| The corresponding weight of the device. <br> Default: `10` |
|overwrite^|bool| Controls the subsequential action if the newly created application name already exists; <br> `True` - overwrite <br> `False` - Do not overwrite <br> Default: `false`|

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
The response body of the successful API call matches the request body of this API call.
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|id| string | ID of the created Application.  |
|name| string | Name of the created Application.  |
|description|string|Description of the created Application.|
|relatedDevices|object|Associated devices of the Application. |
|relatedDevices.deviceId|string| Associated device ID. |
|relatedDevices.deviceName|string| Associated device name. |
|relatedDevices.weight|int| Associated device weight.|
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |


# Full Example:

```python
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Application"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    'name': 'Hello',
    'description': "worker server application",
    'relatedDevices': [
        {
            "deviceName": "BJ*POP",
            "weight": 10
        },
        {
            "deviceName": "BJ-R1",
            "weight": 20
        }
    ],
    'overwrite': False
}

try:
    response = requests.post(full_url, data = json.dumps(data), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Create New Application! - " + str(response.text))
except Exception as e:
    print (str(e))  
```
```python
{
  "id": "ddc769ef-3c8b-4f96-a68c-0a4b6d9f81d3",
  "name": "App01",
  "description": "worker server application",
  "relatedDevices": [
    {
      "deviceId": "549b0899-111c-4e5c-9a76-d5b413ae4843",
      "deviceName": "BJ*POP",
      "weight": 10
    },
    {
      "deviceId": "df3ac9f8-4292-45ed-8fb1-d52e38ad2504",
      "deviceName": "BJ-R1",
      "weight": 20
    }
  ],
  "statusCode": 790200,
  "statusDescription": "Success."
}
```
# cURL Code from Postman

```python
curl -X POST \
  "http://192.168.36.19/ServicesAPI/API/V3/AAM/Application" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "cache-control: no-cache" \
  -H "token: b8088539-c000-440a-b7c7-b9b2d52f046f"
  -d '{
    'name': '111',
    'description': "worker server application",
    'relatedDevices': [
        {
            "deviceName": "BJ*POP",
            "weight": 10
        },
        {
            "deviceName": "BJ-R1",
            "weight": 20
        }
    ],
    'overwrite': true
}'
```
