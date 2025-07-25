
# Add Rows to ADT Table API Design

## ***POST*** V3/CMDB/ADT/Manual/Tables/{id}/Rows
This API is used to add rows to the existing ADT Table. <br>
`id` of the ADT Table is used to call this API, which can be retrieved from [Create New ADT Table](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Create%20New%20ADT%20Table.md), [Get ADT Table](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Get%20ADT%20Table.md), etc.

## Detail Information

> **Title** : Add Rows to ADT Table<br>

> **Version** : 25/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{id}/Rows

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
|rowDatas*|object|Corresponding rows and value of the ADT. |
|rowDatas.column1*|string| To-be-assigned values based on unique column name. |
|rowDatas.column2*|string| To-be-assigned values based on unique column name. |
|...|||

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
|count| integer | Number of added rows. <br> 0 - indicates failure.  |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |


# Full Example:

```python
table_id = "82595bb5-e964-4e23-b8f7-f1c0b13d96a0"
full_url = nb_url + f"/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{table_id}/Rows"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    'RowDatas': 
    [
        {
            'column1':"test",
            'column2':"test"
        },
        {
            'column1':"test1",
            'column2':"test1"
        }
    ]
}

try:
    response = requests.post(full_url, data = json.dumps(data), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Add Rows to ADT Table! - " + str(response.text))
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
  http://192.168.36.19/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/82595bb5-e964-4e23-b8f7-f1c0b13d96a0/Rows \
  -H "Content-Type: application/json" \
  -H "cache-control: no-cache" \
  -H "token: f9560dff-449f-4b97-a372-23978c2951d4" \
  -d '{
    "RowDatas": 
    [
        {
            "column1":"test",
            "column2":"test"
        },
        {
            "column1":"test1",
            "column2":"test1"
        }
    ]
}'
```
