
# One Ip Table API Design

## ***GET*** /V1/CMDB/Topology/OneIPTable{?Ip}&{?beginIndex}&{?count}
This API is used to get the One-IP Table.

If user provides an input value of `ip` attribute, then this API will return all items which have the same ip address in One-IP Table;

If user sets `ip = null` or `ip = ""`, but provides the input values of `beginIndex` and `count`, API will return One-IP Table with items number equal to `count` values start from `beginIndex`. <br>
But note that the default maximum value of `count` is 100,000. So if the input value of `count` is greater than 100,000, API will only return 100,000 itmes. If start from `beginIndex` to the end of table, but there are not enough count items, API will return the rest of items.

>**Note:** The One-IP table records the physical connections for all IP addresses in your workspace. It is retrieved during the Layer 2 topology discovery. One-IP table can be used to troubleshoot any Layer 2 connection issues.<br>

>**Important:** There has been a change in the pagination logic to better manage performance, by introducing two modes:
>1. <b>Offset Mode</b> (Legacy Behaviour)
>> Request includes `beginIndex` >= 0, and doesn't rely on `afterId`. 

>2. <b>Cursor Mode</b> (New, recommended for large data)
>> Request does not include `beginIndex`, or `afterId` is utilized. <br>Results are sorted by `_id` ascending and filtered with `_id` > `afterId` when `afterId` is provided. <br>Use this to iterate through One-IP Table with large data efficiently.
<br>
Please refer to the examples below for more information.

## Detail Information

> **Title** : Get One-Ip Table API<br>

> **Version** : 10/12/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server /ServicesAPI/API/V1/CMDB/Topology/OneIPTable

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Parameters | Authentication token | 

## Request body(****required***)

>No request body.

## Query Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|||`*` - required <br>`^` - optional|
| ip^ | string | The IP address of the current device. If the user provides an input value of `ip` attribute, then this API will return all items with the same IP address in One-IP Table. |
|lan^|string|The LAN Segment of the IP address. If the user provides an input value of `lan` attribute, then this API will returns all items with the same LAN segement in One-IP Table.|
|mac^|string|The MAC address related to the IP address. If the user provides an input value of `mac` attribute, then this API will return all items with the same MAC address in One-IP Table.|
|switch_name^|string|The switch name connected to the end system. If the user provides an input value of `switch_name` attribute, then this API will return all items with the same switch name in One-IP Table.|
|dns^|string|The resolved DNS name of the end system, or the combination of the device name and interface name. If the DNS name is not resolved, it is null.|
|source^|string| The source field of the One-IP record.|
| count^ | int | Count number of returned data; API will return OneIP Table items with the total number of `count`. <br> Default: `100,000` <br>API will only return 100,000 items even if the input value of `count` is greater than 100,000. If the total number of items which start from `beginIndex` to the end of table is less than `count` value, API will return the rest of items. |
| beginIndex^ | int | Beginning index of data. <br> If present, the API will return OneIP Table items starting from `beginIndex` (offset paging). <br>Recommended to only use small offsets, large value could cause performance issues. |
| afterId^ | string | Cursor for paging based on `_id`. <br>If beginIndex is not provided, the API uses cursor paging and returns records with `_id > afterId`, sorted by `_id` ascending. <br>For the initial cursor paging, `afterId` can be omitted, or sent as an empty string. |

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
| token* | string  | Authentication token, get from login API. |

## Response

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|OneIPList| list of object | list of OneIP item  |
|OneIPList.lanSegment| string | IP subnet |
|OneIPList.ip| string | IP address |
|OneIPList.mac| string | MAC address of which the IP is on  |
|OneIPList.devName| string | Device name of which the IP is on  |
|OneIPList.interfaceName| string | Device interface name of the IP  |
|OneIPList.switchName| string | Switch name in which the IP is connected to.  |
|OneIPList.portName| string | Switch port name in which the IP is connected to  |
|OneIPList.alias| string | Device interface is HSRP/GLBP/VRRP |
|OneIPList.dns| string | DNS of the IP  |
|OneIPList.sourceDevice| string | The source device which this One IP come from |
|OneIPList.serverType| int | Device Type |
|OneIPList.switchType| int | Swtich type |
|OneIPList.gateway| string | Gateway |
|OneIPList.vlanId| string | The vlan of the entry learned on the switch  |
|OneIPList.vlanGroupId| string | VLAN Group of the device assigned to the same LAN  |
|OneIPList.updateTime| DataTime | The update time of the One IP  |
|OneIPList.userFlag| int | one ip type<br><br>USERFLAG_AUTO = 0,<br>USERFLAG_MANUAL = 1,<br>OUTSIDE_ANOYMOUS_SOURCE = 2,<br>AUTO_CDPLLDP_TABLE = 5,<br>AUTO_MAC_TABLE = 6,<br>AUTO_ARP_TABLE = 7,<br>AUTO_CDPLLDP_MAC_TABLE = 8,<br>AUTO_DEVICE_INTERFACE = 9,<br>USERFLAG_DRIVER = 10,<br>USERFLAG_VALID_FLAGS_END,<br>USERFLAG_ABNORMAL_FLAGS_START = 0X7FFFFF00,<br>UNSIGNED_USERFLAG = USERFLAG_ABNORMAL_FLAGS_START+1,<br>ERR_USERFLAG ,<br>USERFLAG_ABNORMAL_FLAGS_END,|
|OneIPList.source| string | userFlag to string<br><br> "Auto", //USERFLAG_AUTO<br>"Manual",//USERFLAG_MANUAL<br>"Provide Outside", //OUTSIDE_ANOYMOUS_SOURCE<br>"NDP Table", //AUTO_CDPLLDP_TABLE<br>"MAC Table",//AUTO_MAC_TABLE<br>"ARP Table", //AUTO_ARP_TABLE<br>"NDP & MAC table",//AUTO_CDPLLDP_MAC_TABLE<br>"Device Interface",//AUTO_DEVICE_INTERFACE<br>"Driver"//USERFLAG_DRIVER |
|OneIPList.vendor| string | Device vendor |
|OneIPList.descr| string | Description of the switch port  |
|extSwitchPorts| list | Ext Switch Ports. |
|paging| object | Paging information used to call respetive API calls.  |
|paging.limit| int | Limit of API call. |
|paging.hasMore| boolean | If `False`, there is no more data. |
|paging.nextAfterId | string | `_id` of the last record from current paging, to be used in the succeeding paging call. <br> In offset mode (where `beginIndex` is used), the paging block may be omitted. <br> Please refer to the examples below. |
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example***
```python
{
  "OneIPList": [
    {
      "lanSegment": "20.10.14.0/24",
      "ip": "20.10.14.1",
      "mac": "0022.bdf8.19ff",
      "devName": "NBLEAF-2",
      "interfaceName": "Vlan53",
      "switchName": "",
      "portName": "",
      "alias": "",
      "dns": "NBLEAF-2.Vlan53",
      "sourceDevice": "NBLEAF-2",
      "serverType": 30003,
      "switchType": 1009,
      "gateway": "NBLEAF-2.Vlan53",
      "vlanId": "",
      "vlanGroupId": "NBLEAF-2##Vlan53",
      "updateTime": "2025-05-12T20:52:13Z",
      "userFlag": 9,
      "source": "Device Interface",
      "vendor": "Cisco Systems, Inc",
      "descr": "",
      "extSwitchPorts": []
    },
    ...
    {
      "lanSegment": "20.30.6.2/32",
      "ip": "20.30.6.2",
      "mac": "0050.56be.73bf",
      "devName": "ASAv37-WebServer",
      "interfaceName": "Network adapter 1",
      "switchName": "",
      "portName": "",
      "alias": "",
      "dns": "ASAv37-WebServer.Network adapter 1",
      "sourceDevice": "ASAv37-WebServer",
      "serverType": 13002,
      "switchType": 1009,
      "gateway": "ASAv37-WebServer.Network adapter 1",
      "vlanId": "",
      "vlanGroupId": "ASAv37-WebServer##Network adapter 1",
      "updateTime": "2025-05-12T20:52:14Z",
      "userFlag": 9,
      "source": "Device Interface",
      "vendor": "VMware, Inc.",
      "descr": "",
      "extSwitchPorts": []
    }
  ],
  "paging": {
    "limit": 1000,
    "hasMore": false
  },
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

# Full Examples:
## Example 1: Offset Mode (Legacy)
Returns up to 10,000 matching records starting from offset 0. <br>
```
{
 "count": 10000,
 "beginIndex": 0
}
```
Uses legacy 'natural order' (no guaranteed `_id` sort) <br>

To get the next page:
```
{
 "count": 10000,
 "beginIndex": 10000
}
```
```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Set the request inputs
token = "220d6462-ba64-4058-83cb-affb2d55de78"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Topology/OneIPTable"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

count = 10000
beginIndex = 0

data = {
    "beginIndex" : beginIndex,
    "count" : count
}

try:
    response = requests.get(full_url, params = data, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Failed to Get One-IP Table! - " + str(response.text))
    
except Exception as e:
    print (str(e))
```
```python
{
  "OneIPList": [
    {
      "lanSegment": "20.10.14.0/24",
      "ip": "20.10.14.1",
      "mac": "0022.bdf8.19ff",
      "devName": "NBLEAF-2",
      "interfaceName": "Vlan53",
      "switchName": "",
      "portName": "",
      "alias": "",
      "dns": "NBLEAF-2.Vlan53",
      "sourceDevice": "NBLEAF-2",
      "serverType": 30003,
      "switchType": 1009,
      "gateway": "NBLEAF-2.Vlan53",
      "vlanId": "",
      "vlanGroupId": "NBLEAF-2##Vlan53",
      "updateTime": "2025-05-12T20:52:13Z",
      "userFlag": 9,
      "source": "Device Interface",
      "vendor": "Cisco Systems, Inc",
      "descr": "",
      "extSwitchPorts": []
    },
    ...
    {
      "lanSegment": "20.30.6.2/32",
      "ip": "20.30.6.2",
      "mac": "0050.56be.73bf",
      "devName": "ASAv37-WebServer",
      "interfaceName": "Network adapter 1",
      "switchName": "",
      "portName": "",
      "alias": "",
      "dns": "ASAv37-WebServer.Network adapter 1",
      "sourceDevice": "ASAv37-WebServer",
      "serverType": 13002,
      "switchType": 1009,
      "gateway": "ASAv37-WebServer.Network adapter 1",
      "vlanId": "",
      "vlanGroupId": "ASAv37-WebServer##Network adapter 1",
      "updateTime": "2025-05-12T20:52:14Z",
      "userFlag": 9,
      "source": "Device Interface",
      "vendor": "VMware, Inc.",
      "descr": "",
      "extSwitchPorts": []
    }
  ],
  "paging": {
    "limit": 1000,
    "hasMore": False
  },
  "statusCode": 790200,
  "statusDescription": "Success."
}
```
## Example 2: Cursor Mode - No `beginIndex` or `afterid` (recommended for large tables)
### 1 - First Paging
```python
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Topology/OneIPTable"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

count = 1000
data = {
    "count" : count
}

try:
    response = requests.get(full_url, params = data, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Failed to Get One-IP Table! - " + str(response.text))
    
except Exception as e:
    print (str(e))  

```
```python
{
  "OneIPList": [
    {
      "lanSegment": "20.0.14.0/24",
      "ip": "20.0.14.1",
      "mac": "0022.bdf8.19ff",
      "devName": "NBLEAF-2",
      "interfaceName": "Vlan53",
      "switchName": "",
      "portName": "",
      "alias": "",
      "dns": "NBLEAF-2.Vlan53",
      "sourceDevice": "NBLEAF-2",
      "serverType": 30003,
      "switchType": 1009,
      "gateway": "NBLEAF-2.Vlan53",
      "vlanId": "",
      "vlanGroupId": "NBLEAF-2##Vlan53",
      "updateTime": "2025-05-12T20:52:13Z",
      "userFlag": 9,
      "source": "Device Interface",
      "vendor": "Cisco Systems, Inc",
      "descr": "",
      "extSwitchPorts": []
    },
    ...
  ],
  "paging": {
    "limit": 100,
    "hasMore": True,
    "nextAfterId": "1e1182c1-92ff-4f9c-a7f3-0717866fd97c"
  },
  "statusCode": 790200,
  "statusDescription": "Success."
}
```
### 2 - Next Paging
Use value of `nextAfterId` returned from the previous call to retrieve the next result.
```python
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Topology/OneIPTable"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

count = 1000,
afterId = '1e1182c1-92ff-4f9c-a7f3-0717866fd97c'

data = {
    "count" : count,
    "afterId" : afterId
}

try:
    response = requests.get(full_url, params = data, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Failed to Get One-IP Table! - " + str(response.text))
    
except Exception as e:
    print (str(e))  

```
```python
{
  "OneIPList": [
    {
      "lanSegment": "20.0.41.10/32",
      "ip": "20.0.41.10",
      "mac": "0050.56be.78f4",
      "devName": "ASA-PHY-SERVER-20.0.36.10",
      "interfaceName": "Network adapter 2",
      "switchName": "",
      "portName": "",
      "alias": "",
      "dns": "ASA-PHY-SERVER-20.0.36.10.Network adapter 2",
      "sourceDevice": "ASA-PHY-SERVER-20.0.36.10",
      "serverType": 13002,
      "switchType": 1009,
      "gateway": "ASA-PHY-SERVER-20.0.36.10.Network adapter 2",
      "vlanId": "",
      "vlanGroupId": "ASA-PHY-SERVER-20.0.36.10##Network adapter 2",
      "updateTime": "2025-05-12T20:52:14Z",
      "userFlag": 9,
      "source": "Device Interface",
      "vendor": "VMware, Inc.",
      "descr": "",
      "extSwitchPorts": []
    },
    ...
  ],
  "paging": {
    "limit": 100,
    "hasMore": true,
    "nextAfterId": "42fe53fb-679c-45e1-85b1-f2e73904cb3a"
  },
  "statusCode": 790200,
  "statusDescription": "Success."
}
```
### 3 - Repeat using new `nextAfterId` until `hasMore` returns False

# cURL Code from Postman:


```python
curl -X GET \
  'http://192.168.31.191/ServicesAPI/API/V1/CMDB/Topology/OneIPTable' \
  -H 'cache-control: no-cache' \
  -H 'token: fb1e1360-f3c9-4197-929b-886a146d6bdf'
```

# Error Examples：
<!-- ## Error Example 1: Empty Inputs
```python
Input:
    
        ip = ""
        count = None # cannot be null.
        beginIndex = None # cannot be null.
          
Response:
    
    "Get One-Ip Table failed! - 
    {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'BeginIndex(int)' cannot be null."}

    "Get One-Ip Table failed! - 
    {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'Count(int)' cannot be null."}"

 -->

## Error Example 1: Wrong Input Value Types
```python
Input 1:
    
        ip = ""
        count = "100" # Should be integer
        beginIndex = "50" # Should be integer
          
Response 1:

    {'OneIPList': [], 'statusCode': 790200, 'statusDescription': 'Success.'}

```
```python
Input 2:
    
        ip = "hahahahaah" # There is no such IP address
        count = 100 
        beginIndex = 50 
          
Response 2:
    
    Failed to Get One-IP Table! - {"statusCode":791001,"statusDescription":"Invalid parameter: the parameter 'IP' is invalid."}
    
```
## Error Example 2: `count` > 100,000
```python
Input 1:
    
        ip = "" 
        count = 1000000 # count must be between 0 to 100,000
        beginIndex = 50 
          
Response:
    
    Failed to Get One-IP Table! - {"statusCode":791002,"statusDescription":"Count can between 0 and 100000"}
```
```python
Input 2:
    
        ip = "" 
        count = 100002 # count must be between 0 to 100,000
        beginIndex = 1 
          
Response:
    
    Failed to Get One-IP Table! - {"statusCode":791002,"statusDescription":"Count can between 0 and 100000"}
```
## Error Example 3: `beginIndex` > size of One-Ip Table
```python
Input:
    
        ip = "" 
        count = 1 
        beginIndex = 99999999999 # There are only 117 items in this table.
          
Response:
    
    Failed to Get One-IP Table! - {"statusCode":791009,"statusDescription":"The parameter 'BeginIndex' is invalid value."}

```
