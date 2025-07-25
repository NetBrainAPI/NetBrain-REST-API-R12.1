
# Add ADT Columns to ADT Table API Design

## ***POST*** V3/CMDB/ADT/Manual/Tables/{id}/Columns
This API is used to add columns to the existing API. <br>
`id` of the ADT is used to call this API.

## Detail Information

> **Title** : Add ADT Columns to ADT Table<br>

> **Version** : 23/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{id}/Columns

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
|columns*|object|Corresponoding columns of the ADT |
|columns.columnName*|string|Column Name, unique key. |
|columns.displayName*|string|Display Name of the column. |
|columns.dataType*|string| Data Type of the column.|
|columns.description^|string|Description of the column |
|columns.candidateValue^|list|Alternatives. This can be found in `Edit Column` of the ADT.|

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
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|count| integer | Number of added columns. <br> 0 - indicates failure.  |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |


# Full Example:

```python
table_id = "82595bb5-e964-4e23-b8f7-f1c0b13d96a0" # ID of the ADT Table
full_url = nb_url + f"/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{table_id}/Columns"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    'Columns':[
        {
            'ColumnName':"ABC1",
            'DisplayName':"ABC1",
            'DataType':"String",
            'Description':"test",
            'CandidateValue':
            [
                'value1',
                'value2'
            ]
        },
        {
            'ColumnName':"column2",
            'DisplayName':"Column2",
            'DataType':"String",
            'Description':"test",
            'CandidateValue':
            [
                'value1',
                'value2'
            ]
        }
    ],
}

try:
    response = requests.post(full_url, data = json.dumps(data), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Add Colunms to ADT Table! - " + str(response.text))
except Exception as e:
    print (str(e)) 
```
```python
{
  "count": 2,
  "statusCode": 790200,
  "statusDescription": "Success."
}
```
# cURL Code from Postman
```python
curl -X POST \
  http://192.168.36.19/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/82595bb5-e964-4e23-b8f7-f1c0b13d96a0/Columns \
  -H "Content-Type: application/json" \
  -H "cache-control: no-cache" \
  -H "token: f9560dff-449f-4b97-a372-23978c2951d4" \
  -d '{
    "Columns": [
        {
            "ColumnName": "ABC1",
            "DisplayName": "ABC1",
            "DataType": "String",
            "Description": "test",
            "CandidateValue": [
                "value1",
                "value2"
            ]
        },
        {
            "ColumnName": "column2",
            "DisplayName": "Column2",
            "DataType": "String",
            "Description": "test",
            "CandidateValue": [
                "value1",
                "value2"
            ]
        }
    ]
}'
```
