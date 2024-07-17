
# Download API Design

## ***GET*** V3/download?dl_ticket=xx
Call this API to download

## Detail Information

> **Title** : Download<br>

> **Version** : 17/07/2024

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/download?dl_ticket=xx

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)
downloadTicketId in request is downloadTicketId returned by API


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
TaskID executed by this Trigger

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***


```python
{
    'statusCode': 790200, 
    'statusDescription': 'Success.'
}
```

# Full Example:

```python
# import python modules 
import requests
import json
import time
import requests.packages.urllib3 as urllib3
 
urllib3.disable_warnings()
 
user = "abc"
pwd = "1234"
host_url = "http://10.10.0.29"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}

# Get token for netbrain
def getTokens(user,password):
    login_api_url = r"/ServicesAPI/API/V1/Session"
    Login_url = host_url + login_api_url
    data = {
        "username": user,
        "password": password
    }
    token = requests.post(Login_url, data=json.dumps(
        data), headers=headers, verify=False)
    if token.status_code == 200:
        print(token.json())
        return token.json()["token"]
    else:
        return "error"

# get token
token = getTokens(user,pwd)
headers["Token"] = token
 
def do_download(ticketId):
 
    # Trigger  API url
    API_URL = r"/ServicesAPI/API/V3/download?dl_ticket=" + ticketId
    # Trigger API payload
    print(headers)
    api_full_url = host_url + API_URL
    api_result = requests.get(api_full_url, headers=headers, verify=False)
    file_name = download_file(api_result, ticketId)
    print(file_name)
 
 
def download_file(r, ticketId):
    local_filename = "d:\\" + ticketId + ".zip"
 
    with open(local_filename, 'wb') as f:
        for chunk in r.iter_content(chunk_size=8192):
            # If you have chunk encoded response uncomment if
            # and set chunk_size parameter to None.
            # if chunk:
            f.write(chunk)
    return local_filename
 
if __name__ =="__main__": 
    ticketId = "ZTA2MjAxYWItMTU5MC00MGU5LWExOTItNDZmNjk5Y2E5ZTI0fDYzODU1NzkzMzE2ODgyNzA5Mg=="

    try:
        print(print(do_download(ticketId)))
    except Exception as e:
        print (str(e)) 
```

# cURL Code from Postman

```python
# call login API
curl -X POST \
http://10.10.0.29/ServicesAPI/API/V1/Session \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -d '{
    "username" : "abc",
    "password" : "1234"  
}'

# call Download
curl -X GET \
  http://10.10.0.29/ServicesAPI/API/V3/download?dl_ticket=ZTA2MjAxYWItMTU5MC00MGU5LWExOTItNDZmNjk5Y2E5ZTI0fDYzODU1NzkzMzE2ODgyNzA5Mg== \
  -H "Content-Type: application/json"
  -H 'token: 32ef82d4-44a5-4b48-a089-97daf5e1d92e'
```