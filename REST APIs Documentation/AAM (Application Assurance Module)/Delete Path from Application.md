
# Delete Path from Application API Design

## ***DELETE*** V3/AAM/Application/Path/{id}
This API is used to delete Path from the Application by its path `id`. <br>

## Detail Information

> **Title** : Delete Path from Application<br>

> **Version** : 30/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/AAM/Application/Path/{id} <br>

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
|id| string | ID of the Path. |

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
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Application/Path/{id}"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = {
    'id': '1d232e8d-98f8-434f-9ffa-bb5dc3169dbf'
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
  "http://192.168.36.19/ServicesAPI/API/V3/AAM/Application/Path/1d1ac66a-d484-456f-b66c-0ba1a08000fd" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "cache-control: no-cache" \
  -H "token: 1333a5eb-5e13-4d85-b813-8f87f80fb970"
```