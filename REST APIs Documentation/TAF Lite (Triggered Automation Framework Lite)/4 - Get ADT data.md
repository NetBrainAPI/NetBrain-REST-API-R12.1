
# Get ADT Data API Design

## ***POST*** V3/TAF/Lite/adt/data
Call this API to get ADT data

## Detail Information

> **Title** : Get ADT Data<br>

> **Version** : 17/07/2024

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/TAF/Lite/adt/data

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
|endpoint*|string|The endpoint in the TAFLite definition. |
|passKey*|string|Access permission. |
|filterDevices^|array|Used to quickly filter rows by device. </br>device Name List - this parameter is optional </br>If this parameter has a value, these device names will be used to match ADT rows with the Device Column of ADT. If the device name of any device column is in the device name list, the row will be deemed to meet the conditions.|
|columns^|array|Column display name list, used to express the returned value. </br>If there is no value, or the array does not have any items, all columns are returned. |
|option^|object|Advanced setting|
|option.rowFilter	|object|JSON object <br /><br />ColumnDisplayName: value <br /><br />Use AND to perform operations between JSON attributes |
|option.pageSize|int|Returns the number of data items. </br>Default: 100|
|option.pageNumber|int|Returns the page of data. </br>Default: 1|


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
The data model returned by each DataType:

<table>
  <tr>
    <th>Cell Data Type</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>String</td>
    <td><pre><code>
{
    "value": "hello world"
}
</code></pre></td>
  </tr>
  <tr>
    <td>Int</td>
    <td><pre><code>
{
    "value": "1"
}
</code></pre></td>
  </tr>
  <tr>
    <td>Bool</td>
    <td><pre><code>
{
    "value": "true"
}
</code></pre></td>
  </tr>
  <tr>
    <td>Float</td>
    <td><pre><code>
{
    "value": "123.456"
}
</code></pre></td>
  </tr>
  <tr>
    <td>DateTime</td>
    <td><pre><code>
{
    "value": "2022-11-15T07:23:39.196Z"
}
</code></pre></td>
  </tr>
  <tr>
    <td>Device</td>
    <td><pre><code>
{
    "value": "BJ-R1"
}
</code></pre></td>
  </tr>
  <tr>
    <td>Interface</td>
    <td><pre><code>
{
    "value": "BJ*POP - FastEthernet0/0"
}
</code></pre></td>
  </tr>
  <tr>
    <td>DeviceList</td>
    <td><pre><code>
{
    "value": ["BJ-3750-1", "BJ-3750-2"]
}
</code></pre></td>
  </tr>
  <tr>
    <td>InterfaceList</td>
    <td><pre><code>
{
    "value": ["BJ*POP - FastEthernet0/0", "BJ*POP - FastEthernet0/1"]
}
</code></pre></td>
  </tr>
  <tr>
    <td>Map</td>
    <td><pre><code>
{
    "id": "mapId 1",
    "name": "Map Name 1",
    "url": "map.html?t=1650bf6e-67ed-3c07-3357-b070528c4b19&d=2b662e5d-fe4e-436a-8fa9-c847c1752511&id=1812bd18-633b-4f7d-8e06-8eb5f259660a&maptype=3"
}
</code></pre></td>
  </tr>
  <tr>
    <td>Site</td>
    <td><pre><code>
{
    "id": "site id 1",
    "name": "Site Name 1",
    "path": "My Network\\testSummary",
    "mapUrl": "map.html?t=1650bf6e-67ed-3c07-3357-b070528c4b19&d=2b662e5d-fe4e-436a-8fa9-c847c1752511&id=1812bd18-633b-4f7d-8e06-8eb5f259660a&maptype=3"
}
</code></pre></td>
  </tr>
  <tr>
    <td>Path</td>
    <td><pre><code>
{
    "id": "path template id 1",
    "name": "path Name 1",
    "application": "AAM Name"
}
</code></pre></td>
  </tr>
  <tr>
    <td>NI</td>
    <td><pre><code>
{
    "id": "ni id 1",
    "name": "ni Name 1"
}
</code></pre></td>
  </tr>
  <tr>
    <td>Dataset</td>
    <td><pre><code>
{
  "value": "{\"devices\":[{\"devId\":\"f9accd47-a4cf-495e-b031-54e5e33d35f3\",\"devName\":\"GW2Lab\",\"ls\":[{\"t\":\"2024-03-15T06:16:10Z\",\"c\":\"DEData_2024_3\",\"f\":\"e8ff3a67-ae2e-4764-ae87-6b68b5d87d74\",\"o\":[{\"n\":\"show ip cef exact-route virtual 10.10.7.253 172.24.30.6\",\"t\":\"cmd\",\"o\":0,\"l\":146}]},{\"t\":\"2024-03-15T06:16:10Z\",\"c\":\"DEData_2024_3\",\"f\":\"7c2e7998-9a84-44a9-995a-53504f72af14\",\"o\":[{\"n\":\"show ip cef exact-route virtual 10.10.7.253 172.24.31.2\",\"t\":\"cmd\",\"o\":0,\"l\":146}]},{\"t\":\"2024-03-21T07:56:06Z\",\"c\":\"DEData_2024_3\",\"f\":\"2ee9202f-5603-4d7a-90ad-bdd38e1eff2d\",\"o\":[{\"n\":\"show ip cef 10.10.7.253\",\"t\":\"cmd\",\"o\":259,\"l\":85},{\"n\":\"show ip route 10.10.7.253\",\"t\":\"cmd\",\"o\":0,\"l\":259}]},{\"t\":\"2024-03-21T07:56:06Z\",\"c\":\"DEData_2024_3\",\"f\":\"88b14e19-1b5d-4d65-a06d-cecccef183e9\",\"o\":[{\"n\":\"show ip cef exact-route virtual 10.10.3.253 10.10.7.253\",\"t\":\"cmd\",\"o\":0,\"l\":103}]}]}]}"
}
</code></pre></td>
  </tr>
</table>


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


def get_adt_data(API_Body):
 
    # Trigger  API url
    API_URL = r"/ServicesAPI/API/V3/TAF/Lite/adt/data"
    # Trigger API payload
    print(headers)
    api_full_url = host_url + API_URL
    api_result = requests.post(api_full_url, data=json.dumps(
        API_Body), headers=headers, verify=False)
    if api_result.status_code == 200:
        return api_result.json()
    else:
        return api_result.json()
 
if __name__ =="__main__":
    API_BODY = {
        "endpoint": "T100007",
        "passKey": "TAFLite!1234567890",
        "filterDevices": ["BJ_core_3550"],
        "columns": [],
        "option": {
        }
    }
    try:
        # get token
        token = getTokens(user,pwd)
        if token:
            headers["Token"] = token 
            print(get_adt_data(API_BODY))
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

# call get ADT data
curl -X POST \
  http://10.10.0.29/ServicesAPI/API/V3/TAF/Lite/adt/data \
  -H "Content-Type: application/json"
  -H 'token: b50c2c49-70a1-4bb6-b211-641c9bebbf6b' \
  -d '{
        "endpoint": "T100007",
        "passKey": "TAFLite!1234567890",
        "filterDevices": ["BJ_core_3550"],
        "columns": [],
        "option": {
        }
    }'
```
