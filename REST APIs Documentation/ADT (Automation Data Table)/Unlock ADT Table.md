
# Unlock ADT Table API Design

## ***PUT*** V3/CMDB/ADT/Manual/Tables/{id}/unLock
This API is used to unlock the ADT Table. <br>
`id` of the ADT Table is used to call this API, which can be retrieved from [Create New ADT Table](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Create%20New%20ADT%20Table.md), [Get ADT Table](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Get%20ADT%20Table.md), etc. <br>

<b>Note</b>: The following situations do not require a password: 
> 1) The user owns the table
> 2) The user is a domain admin
> 3) Table is Locked Without Password (`lockMode=0` in Lock ADT Table API)

## Detail Information

> **Title** : Unlock ADT Table<br>

> **Version** : 28/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{id}/unLock

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
|password*|string| This parameter is only required when `lockMode=1` from Lock ADT Table API, i.e. ADT Table is locked.<br> <b>Note</b>: The following situations do not require a password: <br> 1) The user owns the table, <br> 2) The user is a domain admin, or <br> 3) Table is Locked Without Password (`lockMode=0` in Lock ADT Table API)  |

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
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |


# Full Example:
```python
table_id = "1caebf98-ff67-43f2-84c6-5aae9b8c8405"
full_url = nb_url + f"/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{table_id}/unLock"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    "password":"test"
}

try:
    response = requests.put(full_url, data = json.dumps(data), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Unlock ADT Table! - " + str(response.text))
except Exception as e:
    print (str(e)) 
```
```python
{
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

# cURL Code from Postman
```python
curl -X PUT \
  http://192.168.36.19/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/1caebf98-ff67-43f2-84c6-5aae9b8c8405/unLock \
  -H "Content-Type: application/json" \
  -H "cache-control: no-cache" \
  -H "token: f725d607-60d6-4ecd-99c9-197d8ccc103e" \
  -d '{
    "password":"test"
}'
```
