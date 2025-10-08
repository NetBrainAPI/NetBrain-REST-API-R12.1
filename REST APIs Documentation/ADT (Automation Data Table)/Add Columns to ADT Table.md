
# Add Columns to ADT Table API Design

## ***POST*** V3/CMDB/ADT/Manual/Tables/{id}/Columns
This API is used to add columns to the existing ADT Table. <br>
Note that the ADT type must be <b>manual</b>. To check on NetBrain UI, <i>`Fill Each Row with:`</i> must be <b>Manual Input</b> (found under <i>Define Base Table</i>) <br>

`id` of the ADT Table is used to call this API, which can be retrieved from [Create New ADT Table](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Create%20New%20ADT%20Table.md), [Get ADT Table](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Get%20ADT%20Table.md), etc.

## Detail Information

> **Title** : Add Columns to ADT Table<br>

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
|columns*|object|Corresponding columns of the ADT |
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
|count| integer | Number of added columns. <br> `0` - indicates failure.  |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |


# Full Example:

```python
table_id = "82595bb5-e964-4e23-b8f7-f1c0b13d96a0" # ID of the ADT Table
full_url = nb_url + f"/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{table_id}/Columns"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    'columns':[
        {
            'columnName':"ABC1",
            'displayName':"ABC1",
            'dataType':"String",
            'description':"test",
            'candidateValue':
            [
                'value1',
                'value2'
            ]
        },
        {
            'columnName':"column2",
            'displayName':"Column2",
            'dataType':"String",
            'description':"test",
            'candidateValue':
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
    "columns": [
        {
            "columnName": "ABC1",
            "displayName": "ABC1",
            "dataType": "String",
            "description": "test",
            "candidateValue": [
                "value1",
                "value2"
            ]
        },
        {
            "columnName": "column2",
            "displayName": "Column2",
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
