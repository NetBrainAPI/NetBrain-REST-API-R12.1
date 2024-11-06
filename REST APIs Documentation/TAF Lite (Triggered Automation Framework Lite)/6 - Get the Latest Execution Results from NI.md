
# Get the Latest Execution Results from NI API Design

## ***POST*** V3/CMDB/NI/result
Call this API to get the running status of Trigger Task and running ResultID of NI

## Detail Information

> **Title** : Get the Latest Execution Results from NI<br>

> **Version** : 17/07/2024

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/CMDB/NI/result

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
|niIdOrPath*|string|NI ID or NI Path |
|startTime^|DateTime|Start time of NI execution |
|endTime^|DateTime|End time of NI execution |
|output*|array|<table><tr><th>Value</th><th>Enum</th><th>Description</th></tr> <tr><td>0</td><td>all</td><td>All data, except raw data</td></tr> <tr><td>1</td><td>status_code</td><td>Status code and message</td></tr> <tr><td>2</td><td>raw_data</td><td>Raw data</td></tr> <tr><td>3</td><td>csv</td><td>CSV</td></tr> <tr><td>4</td><td>map</td><td>Intent map and external map</td></tr> </table>|

> ***Example***
```python
{      
   niIdOrPath: 'Public\\ni1',
   output: [0, 1, 2, 3, 4] //0:all, 1: rawdata, 2: status code, 3: csv, 4: map
}
```

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
The returned NI execution result content includes Home NI and Follow NI results, hence the result is of type List.

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|[].niId|string|Intent ID|
|[].niName|string|Intent Name|
|[].timePoint|string|NI execution time point|
|[].statusCodes|list|NI status code|
|[].csvs|list of objects|NI CSV results|
|[].rawDatas|string|Command results under device|
|[].rawDatas[].deviceName|string|Device name|
|[].rawDatas[].command|string|Name of the command|
|[].rawDatas[].rawData|string|Information of the command|
|[].maps|list of objects|Map results executed by NI|


> ***Example***

```python
[{
    "niId": "intent1 id",
    "niName": "intent1 name",
    "statusCodes": [
      "intent status code1",
      "intent status code2"
    ],
    "csvs": [ {
        "fileName": "csv file name1", 
        "csv": "a1,b1,c1"
      }],
    "rawDatas": [
      {
        "device": "BJ-R1", 
        "command": "show config",
        "rawData": "raw data1"
      }
    ],
    "maps": [
        {
            "id": "map id 1",
            "name": "map Name 1",
            "url": "map url 1",
            "type": "Intent Map" //Intent Map; External Map
        }
    ]
}]
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
headers1 = {'Content-Type': 'application/json', 'Accept': 'application/json'}

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
        print("Failed to get token with log:", token.error)
        return None


def get_ni_latest_execution_data(API_Body):
 
    # Trigger  API url
    API_URL = r"/ServicesAPI/API/V3/CMDB/NI/result"
    # Trigger API payload
    print(headers)
    api_full_url = host_url + API_URL
    api_result = requests.post(api_full_url, data=json.dumps(
        API_Body), headers=headers, verify=False)
    if api_result.status_code == 200:
        if "downloadTicketId" in api_result.json():
            ticketId = api_result.json()["downloadTicketId"]
            do_download(ticketId)
        return api_result.json()
    else:
        return api_result.json()
 
def do_download(ticketId):
 
    # Trigger  API url
    API_URL = r"/ServicesAPI/API/V3/download?dl_ticket=" + ticketId
    # Trigger API payload
    print(headers)
    api_full_url = host_url + API_URL
    api_result = requests.get(api_full_url, headers=headers, verify=False)
    download_file(api_result, ticketId)
 
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
    API_BODY = {
        "niIdOrPath": "4be2334a-5608-4b2c-809b-a72da260ece7",
        "output": [0]
    }

    try:
        # get token
        token = getTokens(user,pwd)
        if token:
            headers["Token"] = token
            print(get_ni_latest_execution_data(API_BODY))
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

# call get latest execution results from NI
curl -X POST \
  http://10.10.0.29/ServicesAPI/API/V3/CMDB/NI/result \
  -H "Content-Type: application/json"
  -H 'token: 32ef82d4-44a5-4b48-a089-97daf5e1d92e' \
  -d '{
        "niIdOrPath": "4be2334a-5608-4b2c-809b-a72da260ece7",
        "output": [0]
    }'
```
