
# Get ADT Table API Design

## ***GET*** V3/CMDB/ADT/Manual/Tables/{id}
## ***GET*** V3/CMDB/ADT/Manual/Tables?Path={Path}

This API is used to get <b>manually built</b> ADT Table. <br>
The ADT Table can be retrieved with `id` retrieved from [Create New ADT Table](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Create%20New%20ADT%20Table.md), [Get ADT Table](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Get%20ADT%20Table.md), etc., or through `path` of the ADT Table.

## Detail Information

> **Title** : Get ADT Table<br>

> **Version** : 22/07/2025

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
|path|string| Path of the ADT. <br>`path` is a query parameter. |

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
|id| string | ID of the ADT.  |
|name| string | Name of the ADT.  |
|location| string | Path of where the ADT is located.  |
|description|string|Description of the ADT.|
|columns|object|Corresponoding columns of the ADT |
|columns.columnName|string|Column Name, unique key. |
|columns.displayName|string|Display Name of the column. |
|columns.dataType|string| Data Type of the column.|
|columns.description|string|Description of the column |
|columns.candidateValue|list|Alternatives|
|rows|list|Rows of ADT Table|
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |


# Full Example:

## Example 1: Get ADT Table with `id`
```python
full_url = nb_url + "/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{id}"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    'id': '8c90c9c7-70be-4071-b1a0-6a5a14014294'
}

try:
    response = requests.get(full_url, params = data, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Get ADT Table! - " + str(response.text))
except Exception as e:
    print (str(e)) 
```
```python
{
  "tableInfo": {
    "id": "8c90c9c7-70be-4071-b1a0-6a5a14014294",
    "name": "ADT Test1",
    "location": "Shared Tables/ADT Test1",
    "description": "test",
    "columns": [
      {
        "columnName": "column1",
        "displayName": "Column1",
        "dataType": "String",
        "description": "test",
        "candidateValue": [
          "value1",
          "value2"
        ]
      }
    ],
    "rows": []
  },
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

## Example 2: Get ADT Table with `path`
```python
full_url = nb_url + "/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

params = {
    'path': 'Shared Tables/xxx/New ADT Test12'
}

try:
    response = requests.get(full_url, params=params, headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print(result)
    else:
        print("Failed to Get ADT Table! - " + str(response.text))
except Exception as e:
    print(str(e))
```
```python
{
  "tableInfo": {
    "id": "9532ecdd-9426-4d14-990e-5a7e979cadf9",
    "name": "New ADT Test12",
    "location": "Shared Tables/xxx/New ADT Test12",
    "description": "test",
    "columns": [
      {
        "columnName": "column1",
        "displayName": "Column1",
        "dataType": "String",
        "description": "test",
        "candidateValue": [
          "value1",
          "value2"
        ]
      }
    ],
    "rows": []
  },
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

# cURL Code from Postman
## Example 1: Get ADT Table with `id`
```python
curl -X GET \
  http://192.168.36.19/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{id='8c90c9c7-70be-4071-b1a0-6a5a14014294'} \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \ 
  -H 'token: fac2efc7-107c-4e3f-b880-78e7e36d5930' \
```

## Example 2: Get ADT Table with `path`
```python
curl -X GET \
  "http://192.168.36.19/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables?path=Shared%20Tables/xxx/New%20ADT%20Test12&domainId=833df35f-f550-4c9f-8fb0-c208834cf617" \
  -H "Content-Type: application/json" \
  -H "cache-control: no-cache" \
  -H "token: e04267c7-f1b6-44f9-90fd-e06014d9a3f7"
```