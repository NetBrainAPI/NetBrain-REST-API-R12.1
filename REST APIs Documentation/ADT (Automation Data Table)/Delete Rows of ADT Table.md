
# Delete Rows of ADT Table API Design

## ***DELETE*** V3/CMDB/ADT/Manual/Tables/{id}/Rows
This API is used to delete rows of the ADT Table. <br> 
Note that the ADT type must be <b>manual</b>. To check on NetBrain UI, <i>`Fill Each Row with:`</i> must be <b>Manual Input</b> (found under <i>Define Base Table</i>) <br>

`id` of the ADT Table is used to call this API, which can be retrieved from [Create New ADT Table](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Create%20New%20ADT%20Table.md), [Get ADT Table](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Get%20ADT%20Table.md), etc.

## Detail Information

> **Title** : Delete Rows of ADT Table<br>

> **Version** : 25/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{id}/Rows

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
|||* - required<br />^ - optional|
|Condition*| string | Name and value of the corresponding row to be deleted. |
|Column1*|string| Value of row to be deleted. |
|...|||

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
|count| integer | Number of deleted rows. <br> `0` - indicates failure.  |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |


# Full Example:
```python
table_id = "4a12cc7b-ba54-4c41-b37d-7c9d255343ee"
full_url = nb_url + f"/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{table_id}/Rows"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    "condition": {
        "column1":"test1",
    }  
}

try:
    response = requests.delete(full_url, data = json.dumps(data), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Delete Rows of ADT Table! - " + str(response.text))
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
curl -X DELETE \
  http://192.168.36.19/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/4a12cc7b-ba54-4c41-b37d-7c9d255343ee/Rows \
  -H "Content-Type: application/json" \
  -H "cache-control: no-cache" \
  -H "token: ee348c7e-a1cf-4296-8104-1d8e1123e733" \
  -d '{
    "condition": {
        "column1":"test1",
    }  
}'
```