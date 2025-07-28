
# Lock ADT Table API Design

## ***PUT*** V3/CMDB/ADT/Manual/Tables/{id}/LockSettings
This API is used to lock the ADT Table, and make changes to the lock setting. <br>
`id` of the ADT Table is used to call this API, which can be retrieved from [Create New ADT Table](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Create%20New%20ADT%20Table.md), [Get ADT Table](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Get%20ADT%20Table.md), etc.

## Detail Information

> **Title** : Lock ADT Table<br>

> **Version** : 28/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{id}/LockSettings

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
|enable*|bool|`True` - enables Lock Setting. <br>`False` (default) - does not enable Lock Setting. |
|lock*|object|Applicable if `enable` is True. |
|lockMode*|int|`0` - Without Password. <br> `1` - With Password |
|password*|string| This parameter is only required when `lockMode = 1`.|
|lockAnnotation^|string| Lock annotation. |

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
## Example 1: Successful API Call to Lock ADT Table
```python
table_id = "1caebf98-ff67-43f2-84c6-5aae9b8c8405"
full_url = nb_url + f"/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{table_id}/LockSettings"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    "Enable":True,
    "Lock":{
        "LockMode":1,
        "Password":"test",
        "LockAnnotation":"test"
    }    
     
}

try:
    response = requests.put(full_url, data = json.dumps(data), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Lock ADT Table! - " + str(response.text))
except Exception as e:
    print (str(e)) 
```
```python
{
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

## Example 2: Unsuccessful API Call to Modify Lock Setting When ADT Table is Already Locked
```python
table_id = "1caebf98-ff67-43f2-84c6-5aae9b8c8405"
full_url = nb_url + f"/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{table_id}/LockSettings"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    "Enable":True,
    "Lock":{
        "LockMode":0,
        "Password":"test",
        "LockAnnotation":"test"
    }
}

try:
    response = requests.put(full_url, data = json.dumps(data), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Lock ADT Table! - " + str(response.text))
except Exception as e:
    print (str(e))
```
```python
Failed to Lock ADT Table! - {"statusCode":793001,"statusDescription":"Inner exception. This table is locked.  Please enable editing first."}
```

# cURL Code from Postman
This cURL command is based on Example 1
```python
curl -X PUT \
  http://192.168.36.19/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/1caebf98-ff67-43f2-84c6-5aae9b8c8405/LockSettings \
  -H "Content-Type: application/json" \
  -H "cache-control: no-cache" \
  -H "token: f725d607-60d6-4ecd-99c9-197d8ccc103e" \
  -d '{
    "Enable":true,
    "Lock":{
        "LockMode":0,
        "Password":"test",
        "LockAnnotation":"test"
    }
}'
```
