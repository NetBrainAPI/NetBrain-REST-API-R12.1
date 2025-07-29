
# Update Rows of ADT Table API Design

## ***PUT*** V3/AAM/Application
This API is used to update rows of the existing Application. <br>
If `name` field matches the `name` of a different application, the update will not occur and the error message will be returned.

## Detail Information

> **Title** : Update Application<br>

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
|id*|object| Application ID. |
|name*|string| Name of the Application; must be unique. |
|description^|string|Description of the Application.|
|relatedDevices|array| Device information and weights associated with the Application. |
|relatedDevices.deviceName|string| Associated device name. <br>The device name must exist in the device table of the system domain; if it doesn't exist, it will be ignored. |
|relatedDevices.weight^|int| The corresponding weight of the device. <br> Default: `10`|

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
## Example 1: Successful Application Update
```python
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Application"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    "id": "ddc769ef-3c8b-4f96-a68c-0a4b6d9f81d3",
    "name": "Hello",
    "description": "worker server application",
    "relatedDevices": [
        {
            "deviceName": "BJ*POP",
            "weight": 10
        },
        {
            "deviceName": "BJ-R1",
            "weight": 40
        }
    ]
}

try:
    response = requests.put(full_url, data = json.dumps(data), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Update Application! - " + str(response.text))
except Exception as e:
    print (str(e)) 
```
```python
{
  "id": "ddc769ef-3c8b-4f96-a68c-0a4b6d9f81d3",
  "name": "ABC",
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
      "weight": 40
    }
  ],
  "statusCode": 790200,
  "statusDescription": "Success."
}
```
## Example 2: Unsuccessful Application Update - Another Application with `name` already exists
Existing Application #1 - `name=ABC` <br>
Existing Application #2 - `name=xxx`

```python
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Application"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    "id": "ddc769ef-3c8b-4f96-a68c-0a4b6d9f81d3",
    "name": "xxx",
    "description": "worker server application",
    "relatedDevices": [
        {
            "deviceName": "BJ*POP",
            "weight": 10
        },
        {
            "deviceName": "BJ-R1",
            "weight": 40
        }
    ]
}

try:
    response = requests.put(full_url, data = json.dumps(data), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Update Application! - " + str(response.text))
except Exception as e:
    print (str(e))  
```
```python
Failed to Update Application! - {"statusCode":791007,"statusDescription":"The application with name xxx already exists."}

```

# cURL Code from Postman
The cURL command is based on Example 1.
```python
curl -X PUT \
  "http://192.168.36.19/ServicesAPI/API/V3/AAM/Application" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "cache-control: no-cache" \
  -H "token: b8088539-c000-440a-b7c7-b9b2d52f046f"
  -d '{
    "id": "ddc769ef-3c8b-4f96-a68c-0a4b6d9f81d3",
    "name": "ABC",
    "description": "worker server application",
    "relatedDevices": [
        {
            "deviceName": "BJ*POP",
            "weight": 10
        },
        {
            "deviceName": "BJ-R1",
            "weight": 40
        }
    ]
}'
```
