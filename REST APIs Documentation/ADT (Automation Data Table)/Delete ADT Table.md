
# Delete ADT Table API Design

## ***DELETE*** V3/CMDB/ADT/Manual/Tables/{id}
This API is used to delete the ADT Table.
The ADT Table can be deleted using `id` retrieved from [Create New ADT](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Create%20New%20ADT.md), or through `path` of the ADT Table.

## Detail Information

> **Title** : Delete ADT Table<br>

> **Version** : 23/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{id} <br>
> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables?Path={Path}

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
|||Either `id` or `path` can be provided in calling this API. <br> Please refer to the examples below.|
|id| string | ID of the ADT. <br>This can be retrieved from the Create New ADT API.<br>`id` is a path variable, to be passed as part of the URL. |
|path|string| Path of the ADT. <br>`path` is a query parameter |

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
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |


# Full Example:
## Example 1: Delete ADT Table with `id`
```python
full_url = nb_url + "/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{id}"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    'id': '8c90c9c7-70be-4071-b1a0-6a5a14014294'
}

try:
    response = requests.delete(full_url, params = data, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Delete ADT Table by ID! - " + str(response.text))
except Exception as e:
    print (str(e)) 
```
```python
{
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

## Example 2: Delete ADT Table with `path`
```python
full_url = nb_url + "/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

params = {
    'path': 'Shared Tables/xxx/New ADT Test12'
}

try:
    response = requests.delete(full_url, params=params, headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print(result)
    else:
        print("Failed to Delete ADT Table! - " + str(response.text))
except Exception as e:
    print(str(e))
```
```python
{
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

# cURL Code from Postman
## Example 1: Delete ADT Table with `id`
```python
curl -X DELETE \
  "http://192.168.36.19/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/4469f077-f71f-4a5f-acb3-364cd38bade3" \
  -H "Content-Type: application/json" \
  -H "cache-control: no-cache" \
  -H "token: e04267c7-f1b6-44f9-90fd-e06014d9a3f7"
```
## Example 2: Delete ADT Table with `path`
```python
curl -X DELETE \
  "http://192.168.36.19/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables?path=Shared%20Tables/Michelle/New%20ADT%20Test12&domainId=833df35f-f550-4c9f-8fb0-c208834cf617" \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \ 
  -H 'token: e04267c7-f1b6-44f9-90fd-e06014d9a3f7' \
```