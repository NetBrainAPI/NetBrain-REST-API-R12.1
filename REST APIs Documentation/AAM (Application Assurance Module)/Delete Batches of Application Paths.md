
# Delete Batches of Application Paths API Design

## ***POST*** V3/AAM/Batch/Delete
This API is used to delete Application Paths in Batches. <br>
If the Application ID does not exist, the process is terminated.<br>
<b> Note </b>: This is a <i>POST</i>, not <i>DELETE</i>.

## Detail Information

> **Title** : Delete Batches of Application Paths<br>

> **Version** : 30/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/AAM/Batch/Delete <br>

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
|application*|string|Application Name. |
|paths^|array|Path Name List. |

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
|data|array|Deleted Application Paths. |
|application|string|Name of Application Paths.|
|paths| array | Path Name|
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |


# Full Example:
```python
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Batch/Delete"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = [
        {
            'application': "testApp1"
        },
        {
            'application': 'testApp2',
            'paths': ['testPath1', 'testPath2']
        }
]

try:
    response = requests.post(full_url, data = json.dumps(data), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Delete Multiple Application Paths in Batches! - " + str(response.text))
except Exception as e:
    print (str(e))
```
```python
{
  "data": [
    {
      "application": "testApp1",
      "paths": []
    },
    {
      "application": "testApp2",
      "paths": [
        "testPath1",
        "testPath2"
      ]
    }
  ],
  "statusCode": 790200,
  "statusDescription": "Success."
}
```


# cURL Code from Postman
```python
curl -X POST \
  "http://192.168.36.19/ServicesAPI/API/V3/AAM/Batch/Delete" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "cache-control: no-cache" \
  -H "token: 1333a5eb-5e13-4d85-b813-8f87f80fb970"
  -d '[
        {
            'application': "testApp1"
        },
        {
            'application': "testApp2",
            'paths': ["testPath1", "testPath2"]
        }
]'
```