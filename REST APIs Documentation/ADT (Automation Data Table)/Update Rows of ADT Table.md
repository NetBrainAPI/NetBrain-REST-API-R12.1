
# Update Rows of ADT Table API Design

## ***PUT*** V3/CMDB/ADT/Manual/Tables/{id}/Rows
This API is used to update rows of the existing <b>manually built</b> ADT Table. <br>
`id` of the ADT Table is used to call this API, which can be retrieved from [Create New ADT Table](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Create%20New%20ADT%20Table.md), [Get ADT Table](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/blob/main/REST%20APIs%20Documentation/ADT%20(Automation%20Data%20Table)/Get%20ADT%20Table.md), etc.

## Detail Information

> **Title** : Update Rows of the ADT Table<br>

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
|condition*|object| Corresponding row name and value of the ADT to be updated. |
|condition.column1*|string| Existing values of the unique column name. |
|...|||
|updateData*|object|Corresponding row name and value|
|condition.column1*|string| To-be-updated values of the unique column name. |
|...|||
|insertifnotMatch^|boolean| Controls whether to insert a new row if there are no match. <br>Default: `False`|

## Parameters(****required***)
>No parameters required.


## Headers

> **Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| Content-Type | string  | support "application/json" |
| Accept | string | support "application/json" |

> **Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| token | string  | Authentication token, get from login API. |

## Response
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|result| string | Status of API Call; `Update`, `NotMatched`, `Insert`  |
|count| integer | Number of updated rows. <br> `0` - indicates failure.  |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |


# Full Example:
## Example 1: Successful Update of Rows
```python
table_id = "82595bb5-e964-4e23-b8f7-f1c0b13d96a0"
full_url = nb_url + f"/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{table_id}/Rows"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    "condition": {
        "column1":"test",
        "column2":"test"
    },   
    "updateData": {
        "column1":"test1",
        "column2":"test1"
    },
    "insertifnotMatch": False
}

try:
    response = requests.put(full_url, data = json.dumps(data), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Update Rows of ADT Table! - " + str(response.text))
except Exception as e:
    print (str(e)) 
```
```python
{
  "result": "Update",
  "count": 2,
  "statusCode": 790200,
  "statusDescription": "Success."
}
```
## Example 2: Unsuccessful Update of Rows - Value of Column doesn't match
After successfully calling API based on Example 1, the values of each columns are updated to `test1`. <br>
Now, if there is a second API call with the same body from Example 1, the values of the columns do not match (ie. values of `condition.column1` and `condition.column2` have been updated to `test1`, but the second API call uses `test` as the values of `condition.column1` and `condition.column2`).

```python
table_id = "4a12cc7b-ba54-4c41-b37d-7c9d255343ee"
full_url = nb_url + f"/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{table_id}/Rows"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    "condition": {
        "column1":"test",
        "column2":"test"
    },   
    "updateData": {
        "column1":"test1",
        "column2":"test1"
    },
    "insertifnotMatch": False
}

try:
    response = requests.put(full_url, data = json.dumps(data), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Update Rows of ADT Table! - " + str(response.text))
except Exception as e:
    print (str(e)) 
```
```python
{
  "result": "NotMatched",
  "count": 0,
  "statusCode": 790200,
  "statusDescription": "Success."
}
```


## Example 3: `InsertifnotMatch` = True
```python
table_id = "4a12cc7b-ba54-4c41-b37d-7c9d255343ee"
full_url = nb_url + f"/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/{table_id}/Rows"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    "condition": {
        "column1":"test",
        "column2":"test"
    },   
    "updateData": {
        "column1":"test1",
        "column2":"test1"
    },
    "insertifnotMatch": True
}

try:
    response = requests.put(full_url, data = json.dumps(data), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Update Rows of ADT Table! - " + str(response.text))
except Exception as e:
    print (str(e)) 
```
```python
{
  "result": "Insert",
  "count": 1,
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

# cURL Code from Postman
The cURL command is based on Example 1.
```python
curl -X PUT \
  http://192.168.36.19/ServicesAPI/API/V3/CMDB/ADT/Manual/Tables/4a12cc7b-ba54-4c41-b37d-7c9d255343ee/Rows \
  -H "Content-Type: application/json" \
  -H "cache-control: no-cache" \
  -H "token: ee348c7e-a1cf-4296-8104-1d8e1123e733" \
  -d '{
    "condition": {
        "column1":"test",
        "column2":"test"
    },   
    "updateData": {
        "column1":"test1",
        "column2":"test1"
    },
    "insertifnotMatch": false
}'
```
