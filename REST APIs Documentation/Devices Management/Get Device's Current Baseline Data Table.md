
# Device API Design

## ***GET*** /V1/CMDB/Devices/DeviceTableData
Call this API to export the device's current baseline data table.

## Detail Information

> **Title** : Export (GET) Device's Current Baseline Data Table<br>

> **Version** : 25/04/2025

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Devices/DeviceTableData

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

> No request body.


## Query Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| hostname | string  | Hostname of the device. |
| ip | string  | Management IP address of the device. |
|  |  | *At least one of `hostname` or `ip` should have value. <br> They cannot both be empty. |
| tableName* | string  | Name of the Device Data Table. <br>Names for System Tables are constant; <br>1. routeTable <br>2. arpTable <br>3. macTable  <br>4. cdpTable <br>5. stpTable <br>6. bgpNbrTable <br> <br>For NCT Table, please use the real table name. e.g. QoS Mapping Table|
| vrf^ | string  | Name of VRF, if applicable; otherwise, empty. |
| subTableName^ | string  | Name of NCT Table's sub-table name, if applicable; otherwise, empty. |
| pageIndex* | integer  | Page Index for gets & sets. Starts from 0. |
| pageSize* | integer  | Page size for gets & sets. <br>If PageSize equals 0, PageIndex must be 0; This will return all rows. |
|  |  | ^ indicates optional parameter. <br>* indicates mandatory parameter.  |


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
|columns| list | List of columns according to the Data Table. |
|rows| nested list | List(s) of values of respective columns, according to the Data Table. |
|isAllLoaded | boolean | Indicates if partial of full rows of Data Table are loaded. |
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example***
```python
{
    "columns": [
        "Alg.",
        "Dest.Addr",
        "Mask",
        "Distance",
        "Metric",
        "Interface",
        "Next Hop IP",
        "Next Hop Device",
        "Age"
    ],
    "rows": [
        [
            "O E2",
            "192.168.29.0",
            24,
            110,
            300,
            "FastEthernet0/0",
            "172.24.32.11",
            "BJ_core_3550",
            "4d13h"
        ],
        [
            "O E2",
            "1.1.1.1",
            32,
            110,
            200,
            "FastEthernet0/0",
            "172.24.32.11",
            "BJ_core_3550",
            "4d13h"
        ]
    ],
    "isAllLoaded": false,
    "statusCode": 790200,
    "statusDescription": "Success."
}
```

# Full Example
```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Set the request inputs
token = "8cf08bd4-92ef-4f4f-b56f-d6dc95124f51"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Devices/DeviceTableData"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

hostname = "11"
ip = ''
tableName = 'routeTable'
vrf = ''
subTableName = ''
pageIndex = 0
pageSize = 0

body = {
        "hostname":hostname,
        "ip":ip,
        "tableName":tableName,
        "vrf":vrf,
        "subTableName":subTableName,
        "pageIndex":pageIndex,
        "pageSize":pageSize,
    
    }
try:
    response = requests.get(full_url, headers=headers, params = body, verify=False)
    print(response)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Failed to get Device's Current Baseline Data Table! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

```
{
    "columns": [
        "Alg.",
        "Dest.Addr",
        "Mask",
        "Distance",
        "Metric",
        "Interface",
        "Next Hop IP",
        "Next Hop Device",
        "Age"
    ],
    "rows": [
        [
            "STATIC",
            "0.0.0.0",
            0,
            60,
            0,
            "Vlan-interface10",
            "172.24.101.1",
            "BJ-L2-Core-A",
            ""
        ],
        [
            "C   ",
            "127.0.0.0",
            8,
            0,
            0,
            "InLoopBack0",
            "127.0.0.1",
            "",
            ""
        ],
        [
            "C   ",
            "127.0.0.1",
            32,
            0,
            0,
            "InLoopBack0",
            "127.0.0.1",
            "",
            ""
        ],
        [
            "C   ",
            "172.24.101.0",
            24,
            0,
            0,
            "Vlan-interface10",
            "172.24.101.31",
            "11",
            ""
        ],
        [
            "C   ",
            "172.24.101.31",
            32,
            0,
            0,
            "InLoopBack0",
            "127.0.0.1",
            "",
            ""
        ]
    ],
    "isAllLoaded": true,
    "statusCode": 790200,
    "statusDescription": "Success."
}
```

# cURL Code from Postman
```python
curl -X GET \
  'https://nextgen-training.netbrain.com/ServicesAPI/API/V1/CMDB/Devices/DeviceTableData?hostname=11&ip=&tableName=routeTable&vrf=&subTableName=&pageIndex=0&pageSize=0' \
  -H 'cache-control: no-cache' \
  -H 'token: 9b4395e2-2890-4355-9153-aa38c936946f'
```

