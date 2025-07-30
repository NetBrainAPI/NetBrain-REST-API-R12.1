
# Get Path Information API Design

## ***GET*** V3/AAM/Application/Path?Application={ApplicationName}&path={pathName}
## ***GET*** V3/AAM/Application/Path/{ID}

This API is used to get the complete information of Path by specifying the Application and Path name, or Path ID.

## Detail Information

> **Title** : Get Path Information<br>

> **Version** : 29/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/AAM/Application/Path?Application={ApplicationName}&path={pathName} <br>
> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/AAM/Application/Path/{ID}

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)
>No request body required.

## Parameters(****required***)
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
||| Either `application` AND `path`, or `id` can be provided in calling this API. <br> Please refer to the examples below. |
|application| string | Name of the Application. |
|path|string| Name of the Path.|
|id|string| ID of the Path. <br>`id` is a path parameter, to be passed as part of the URL. |

## Headers

> **Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| Content-Type | string | support "application/json" |
| Accept | string | support "application/json" |

> **Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| token | string  | Authentication token, get from login API. |

## Response
The depth of the response may vary per application.
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|path| array | Path of the Application.  |
|id|string| ID of the Path.|
|name| string | Name of the Path.  |
|source|string| Source Information.|
|sourceIp|string| Source IP address.|
|dest|string| Destination Information.|
|destIp|string| Destination IP address.|
|protocol|int| Protocol number.|
|protocolKeyWord|string| Protocol keyword.|
|advancedOption|object| Advanced Option of the Path Details.|
|routinScheme| int | Routing Scheme. |
|group| string | Group. |
|applicationId| string | ID of the Application. |
|applicationName| string | Name of the Application. |
|statusCode| integer | The returned status code of executing the API. |
|statusDescription| string | The explanation of the status code. |


# Full Example:
## Example 1: Get Path Information with `application` and `path`
```python
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Application/Path"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

params = {
    'application': 'App01',
    'path':'test'
}
try:
    response = requests.get(full_url, params=params, headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Get Path Information! - " + str(response.text))
except Exception as e:
    print (str(e))
```
```python
{
  "path": {
    "id": "3ddd10ed-0247-4713-98fb-48ae9fb822b6",
    "name": "test",
    "source": "192.168.1.1",
    "sourceIp": "192.168.1.1",
    "dest": "192.168.1.3",
    "destIp": "192.168.1.3",
    "protocol": 256,
    "protocolKeyWord": "IP",
    "advancedOption": {
      "useSettingType": 0,
      "debugMode": false,
      "calcWhenDeniedByACL": false,
      "calcWhenDeniedByPolicy": false,
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

## Example 2: Get Path Information with `id`
```python
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Application/Path/{ID}"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

params = {
    'id': '3ddd10ed-0247-4713-98fb-48ae9fb822b6'
}
try:
    response = requests.get(full_url, params=params, headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Get Path Information! - " + str(response.text))
except Exception as e:
    print (str(e))
```
```python
{
  "path": {
    "id": "3ddd10ed-0247-4713-98fb-48ae9fb822b6",
    "name": "test",
    "source": "192.168.1.1",
    "sourceIp": "192.168.1.1",
    "dest": "192.168.1.3",
    "destIp": "192.168.1.3",
    "protocol": 256,
    "protocolKeyWord": "IP",
    "advancedOption": {
      "useSettingType": 0,
      "debugMode": false,
      "calcWhenDeniedByACL": false,
      "calcWhenDeniedByPolicy": false,
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
This cURL command is based on Example 1.
```python
curl -X GET \
  "http://192.168.36.19/ServicesAPI/API/V3/AAM/Application/Path?application=Hello11&path=test" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "cache-control: no-cache" \
  -H "token: b8088539-c000-440a-b7c7-b9b2d52f046f"
```
