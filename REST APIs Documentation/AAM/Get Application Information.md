
# Get Application Information API Design

## ***GET*** V3/CMDB/AAM/Applications?Application={ApplicationName}
## ***GET*** V3/CMDB/AAM/Application/{Id}

This API is used to get the Application Information based on the provided Application Name.

## Detail Information

> **Title** : Get Application Information<br>

> **Version** : 29/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/CMDB/AAM/Applications?Application={ApplicationName}
> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/CMDB/AAM/Application/{Id}

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
|||Either `application` or `id` can be provided in calling this API. <br> Please refer to the examples below|
|application| string | Name of the application. <br>`application` is a query parameter.|
|id|string|ID of the Application. <br>`id` is a path variable, to be passed as part of the URL.|

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
The depth of the response may vary per application.
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|id| string | ID of the Application.  |
|name| string | Name of the Application.  |
|description|string| Description of the Application.|
|relatedDevices|object| Associated Device Informations.|
|relatedDevices.deviceId|string| Description of the Application.|
|relatedDevices.deviceName|string| Description of the Application.|
|relatedDevices.weight|string| Description of the Application.|
|statusCode| integer | The returned status code of executing the API. |
|statusDescription| string | The explanation of the status code. |


# Full Example:
## Example 1: Application Information with Full Response & Use of `application`
```python
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Application"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

params = {"Application": "aaa"}

try:
    response = requests.get(full_url, params=params, headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Get Application Information! - " + str(response.text))
except Exception as e:
    print (str(e))
```
```python
{
  "id": "629ffd12-f72f-4d6f-955a-e5f15d856151",
  "name": "aaa",
  "description": "",
  "relatedDevices": [
    {
      "deviceId": "030ae5f8-9703-4d2c-a24b-a93dc608db24",
      "deviceName": "11",
      "weight": 10
    },
    {
      "deviceId": "0ef8e825-93d9-4b28-8f2b-c51143b4f423",
      "deviceName": "!@#$%^&*()_-=+~`:;.'|\\/[]{}",
      "weight": 10
    }
  ],
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

## Example 2: Application Information with Partial Response
This example has less information in its response as this application does not have any associated device.
```python
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Application"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

params = {"Application": "ADT"}

try:
    response = requests.get(full_url, params=params, headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Get Application Information! - " + str(response.text))
except Exception as e:
    print (str(e))
```
```python
{
  "id": "7e301334-038a-494f-8813-49f2a597008f",
  "name": "ADT",
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

## Example 3: Get Application Information with `id`
```python
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Application/{Id}"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

params = {"id": "7e301334-038a-494f-8813-49f2a597008f"}

try:
    response = requests.get(full_url, params=params, headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Get Application Information! - " + str(response.text))
except Exception as e:
    print (str(e))
```
```python
{
  "id": "7e301334-038a-494f-8813-49f2a597008f",
  "name": "ADT",
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

# cURL Code from Postman
This cURL command is based on Example 1.
```python
curl -X GET \
  "http://192.168.36.19/ServicesAPI/API/V3/AAM/Application?Application=Michelle_Test" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "cache-control: no-cache" \
  -H "token: b8088539-c000-440a-b7c7-b9b2d52f046f"
```
