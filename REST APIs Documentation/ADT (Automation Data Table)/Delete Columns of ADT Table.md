
# Delete Columns of ADT Table API Design

## ***DELETE*** V3/CMDB/ADT/Manual/Tables/{id}/Columns/{ColumnName}
This API is used to delete columns of the <b>manually built</b> ADT Table. <br>
`id` of the ADT Table is used to call this API, which can be retrieved from [Create New ADT Table](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Create%20New%20ADT%20Table.md), [Get ADT Table](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Get%20ADT%20Table.md), etc.

## Detail Information

> **Title** : Delete Columns of ADT Table<br>

> **Version** : 25/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{id}/Columns/{ColumnName}

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
|id| string | ID of the ADT. <br>This can be retrieved from the Create New ADT Table API or Get ADT Table API. |
|columnName|string| Column of the ADT. |

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
```python
full_url = nb_url + "/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{id}/Columns/{ColumnName}"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

params = {
    'id': '82595bb5-e964-4e23-b8f7-f1c0b13d96a0', # ID of ADT Table
    'columnName':'ABC12'
}

try:
    response = requests.delete(full_url, params=params, headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print(result)
    else:
        print("Failed to Delete Columns of ADT Table! - " + str(response.text))
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
```python
curl -X DELETE \
  http://192.168.36.19/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/82595bb5-e964-4e23-b8f7-f1c0b13d96a0/Columns/ABC12 \
  -H "Content-Type: application/json" \
  -H "cache-control: no-cache" \
  -H "token: f9560dff-449f-4b97-a372-23978c2951d4" \
```