
# Delete Application API Design

## ***DELETE*** V3/AAM/Application/{id}
This API is used to delete the Application by its `id`. <br>

## Detail Information

> **Title** : Delete Application<br>

> **Version** : 29/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/AAM/Application/{id} <br>

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
|id| string | ID of the Application. |

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
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Application/{id}"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    'id': 'b4a126e2-49e2-426c-9e65-ff148cca3300'
}

try:
    response = requests.delete(full_url, params = data, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Delete Application! - " + str(response.text))
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
curl -X DELETE \
  "http://192.168.36.19/ServicesAPI/API/V3/AAM/Application/1d6b8d92-fd5e-446e-8028-f5d064720855" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "cache-control: no-cache" \
  -H "token: b8088539-c000-440a-b7c7-b9b2d52f046f"
```