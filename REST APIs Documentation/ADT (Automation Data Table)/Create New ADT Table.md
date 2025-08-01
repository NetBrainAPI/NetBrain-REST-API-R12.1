
# Create New ADT API Design

## ***POST*** V3/CMDB/ADT/Manual/Tables
This API is used to create a new <b>manual type</b> ADT. <br>
This API only supports static data types such as string columns, int columns, etc.; it does not support dynamic types such as intent columns.

## Detail Information

> **Title** : Create New ADT<br>

> **Version** : 21/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)
Only static data types are supported, such as string, int columns, etc.; dynamic types such as intent columns are not supported.

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|||* - required<br />^ - optional|
|name*|string| Name of ADT. |
|location*|string|Location of the ADT. |
|description^|array|Description of the ADT.|
|columns*|object|Corresponoding columns of the ADT |
|columns.columnName*|string|Column Name, unique key. |
|columns.displayName*|string|Display Name of the column. |
|columns.dataType*|string| Data Type of the column.|
|columns.description^|string|Description of the column |
|columns.candidateValue^|list|Alternatives. This can be found in `Edit Column` of the ADT.|

> ***Example***
```python
{
    'name': 'New ADT Tes1t',
    'location': '/Shared Tables',
    'description': 'test',
    'columns':[
        {
            'columnName':"column1",
            'displayName':"Column1",
            'dataType':"String",
            'description':"test",
            'candidateValue':
            [
                'value1',
                'value2'
            ]
        }
    ]
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
The response body of the successful API call matches the request body of this API call.
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|id| string | ID of the created ADT.  |
|name| string | Name of the created ADT.  |
|location| string | Path of where the created ADT is located.  |
|description|string|Description of the created ADT.|
|columns|object|Corresponoding columns of the ADT |
|columns.columnName|string|Column Name, unique key. |
|columns.displayName|string|Display Name of the column. |
|columns.dataType|string| Data Type of the column.|
|columns.description|string|Description of the column |
|columns.candidateValue|list|Alternatives|
|rows|list|Rows of ADT Table. Rows can be added/modified separately through another API. |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |


# Full Example:

```python
full_url = nb_url + "/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    'name': 'New ADT Test',
    'location': '/Shared Tables',
    'description': 'test',
    'columns':[
        {
            'columnName':"column1",
            'displayName':"Column1",
            'dataType':"String",
            'description':"test",
            'candidateValue':
            [
                'value1',
                'value2'
            ]
        }
    ]
     
}

try:
    response = requests.post(full_url, data = json.dumps(data), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Create New ADT! - " + str(response.text))
except Exception as e:
    print (str(e)) 
```
```python
{
  "tableInfo": {
    "id": "8c90c9c7-70be-4071-b1a0-6a5a14014294",
    "name": "New ADT Test",
    "location": "Shared Tables/New ADT Test",
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

```python
curl -X POST \
  http://192.168.36.19/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \ 
  -H 'token: bf2e7f60-fd23-4eb3-a9a3-7fe10ca0a363' \
-d '{
    "name": "New ADT Test",
    "location": "/Shared Tables",
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
    ]
  }'
```
