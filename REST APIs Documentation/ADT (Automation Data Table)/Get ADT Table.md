
# Create New ADT API Design

## ***POST*** V3/ADT/Manual/Tables
This API is used to get the ADT Table with the ID.

## Detail Information

> **Title** : Get ADT Table<br>

> **Version** : 22/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{id}

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
|id| string | ID of the created ADT. <br>This can be retrieved from the Create New ADT API. |

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
|id| string | ID of the ADT.  |
|name| string | Name of the ADT.  |
|location| string | Path of where the ADT is located.  |
|description|string|Description of the ADT.|
|columns|object|Corresponoding columns of the ADT |
|columns.columnName|string|Column Name, unique key. |
|columns.displayName*|string|Display Name of the column. |
|columns.dataType|string| Data Type of the column.|
|columns.description|string|Description of the column |
|columns.candidateValue|list|Alternatives|
|rows|list|Rows of ADT Table|
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |


# Full Example:

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
# cURL Code from Postman

```python
curl -X GET \
  http://192.168.36.19/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{id='8c90c9c7-70be-4071-b1a0-6a5a14014294'} \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \ 
  -H 'token: fac2efc7-107c-4e3f-b880-78e7e36d5930' \
```
