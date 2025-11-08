
# One Ip Table API Design

## ***GET*** /V1/CMDB/Topology/OneIPTable{?Ip}&{?beginIndex}&{?count}
This API is used to get the One-IP Table.

If user provides an input value of `ip` attribute, then this API will return all items which have the same ip address in One-IP Table;

If user sets `ip = null` or `ip = ""`, but provides the input values of `beginIndex` and `count`, API will return One-IP Table with items number equal to `count` values start from `beginIndex`. <br>
But note that the default maximum value of `count` is 100,000. So if the input value of `count` is greater than 100,000, API will only return 100,000 itmes. If start from `beginIndex` to the end of table, but there are not enough count items, API will return the rest of items.

>**Note:** The One-IP table records the physical connections for all IP addresses in your workspace. It is retrieved during the Layer 2 topology discovery. One-IP table can be used to troubleshoot any Layer 2 connection issues.<br><br>

>**Important:** There has been a change in the pagination logic to better manage performance. For API calls with `beginIndex > 20,000`, please utilize the two parameters `afterIpInt` and `afterId` and their values retrieved from the API response. <br>
Please refer to the examples below for more information.

## Detail Information

> **Title** : Get One-Ip Table API<br>

> **Version** : 07/11/2025

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
| ip | string  | The IP address of the current device. If the user provides an input value of `ip` attribute, then this API will return all items with the same IP address in One-IP Table. |
|lan|string|The LAN Segment of the IP address. If the user provides an input value of `lan` attribute, then this API will returns all items with the same LAN segement in One-IP Table.|
|mac|string|The MAC address related to the IP address. If the user provides an input value of `mac` attribute, then this API will return all items with the same MAC address in One-IP Table.|
|switch_name|string|The switch name connected to the end system. If the user provides an input value of `switch_name` attribute, then this API will return all items with the same switch name in One-IP Table.|
|switch_port|string|The [fullname](https://www.netbraintech.com/docs/ie71/help/index.html?interface-name-translation.htm) of switchport to connected to the end system or the device interface configured with this IP address. This is not an independent attribute; to use this attribute, `switch_name` is required.|
|dns|string|The resolved DNS name of the end system, or the combination of the device name and interface name. If the DNS name is not resolved, it is null.|
| count | int | Count number of returned data; API will return OneIP Table items with the total number of `count`. <br> Maximum: 100,000. <br>API will only return 100,000 items even if the input value of `count` is greater than 100,000. If the total number of items which start from `beginIndex` to the end of table is less than `count` value, API will return the rest of items. |
| beginIndex | int | Beginning index of data; API will return OneIP Table items starting from `beginIndex`. <br>Default: `0` <br>If `beginIndex` > 20,000, please use the below two parameters instead: `afterIpInt` and `afterId`. |
|afterIpInt | string | The integer retrieved from the preceeding `Get One-IP Table API` call, to be used in the succeeding call. Please refer to the examples below. <br> This parameter is not necessary if `beginIndex` < 20,000. |
|afterIpId | string | The ID retrieved from the preceeding `Get One-IP Table API` call, to be used in the succeeding call. Please refer to the examples below. <br> This parameter is not necessary if `beginIndex` < 20,000. |

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
|paging.afterIpInt | string | The integer retrieved from the preceeding `Get One-IP Table API` call, to be used in the succeeding call. Please refer to the examples below. <br> This parameter is not necessary if `beginIndex` < 20,000. |
|paging.afterIpId | string | The ID retrieved from the preceeding `Get One-IP Table API` call, to be used in the succeeding call. Please refer to the examples below. <br> This parameter is not necessary if `beginIndex` < 20,000. |
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example***


```python
{
  "OneIPList": [
    {
      "lanSegment": "10.61.41.28/30",
      "ip": "10.61.41.19",
      "mac": "aabb.cc00.0b31",
      "devName": "bjta002440-SW11",
      "interfaceName": "Ethernet1/3",
      "switchName": "bjta002302-SW8",
      "portName": "Ethernet1/3",
      "alias": "btv/SCa-vSCb/10.61.41.28/30",
      "dns": "bjta002440-SW11.Ethernet1/3",
      "sourceDevice": "bjta002440-SW11",
      "serverType": 2001,
      "switchType": 2001,
      "gateway": "bjta002440-SW11.Ethernet1/3",
      "vlanId": "",
      "vlanGroupId": "bjta002302-SW8##Ethernet1/3",
      "updateTime": "2025-11-04T01:27:59Z",
      "userFlag": 9,
      "source": "Device Interface",
      "vendor": "",
      "descr": "btv/SCa-vSCb/10.61.41.28/30",
      "extSwitchPorts": []
    },
    ...
    {
      "lanSegment": "10.61.162.1/29",
      "ip": "10.61.16.4",
      "mac": "aabb.cc00.0621",
      "devName": "bjta002115-SW6",
      "interfaceName": "Ethernet1/2",
      "switchName": "",
      "portName": "",
      "alias": "",
      "dns": "bjta002115-SW6.Ethernet1/2",
      "sourceDevice": "bjta002115-SW6",
      "serverType": 2001,
      "switchType": 1009,
      "gateway": "bjta002115-SW6.Ethernet1/2",
      "vlanId": "",
      "vlanGroupId": "bjta002115-SW6##Ethernet1/2",
      "updateTime": "2025-11-04T01:28:00Z",
      "userFlag": 9,
      "source": "Device Interface",
      "vendor": "",
      "descr": "",
      "extSwitchPorts": []
    }
  ],
  "paging": {
    "limit": 100,
    "hasMore": true,
    "nextAfterIpInt": 171811076,
    "nextAfterId": "5f62938c-a1c5-4597-8a6b-eebadf520f40"
  },
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

# Full Examples:
## Example 1: `beginIndex` < 20,000
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

ip = "123.20.1.11"
beginIndex = 0
count = 5

data = {
    "ip" : ip,
    "beginIndex" : beginIndex,
    "count" : count
}

try:
    response = requests.get(full_url, params = data, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Get One-Ip Table failed! - " + str(response.text))
    
except Exception as e:
    print (str(e))  
```
```python
    {'OneIPList': [{'lanSegment': '192.168.180.6/32', 'ip': '192.168.180.6', 'mac': '0050.56be.10f6', 'devName': 'UCSPE', 'interfaceName': 'Network adapter 1','switchName': '', 'portName': '', 'alias': '', 'dns': 'UCSPE.Network adapter 1', 'sourceDevice': 'UCSPE', 'serverType': 13002, 'switchType': 1009, 'gateway': 'UCSPE.Network adapter 1', 'vlanId': '', 'vlanGroupId': 'UCSPE##Network adapter 1', 'updateTime': '2024-02-27T21:33:59Z', 'userFlag': 9, 'source': 'Device Interface', 'vendor': 'VMware, Inc.', 'descr': '', 'extSwitchPorts': []}], 'statusCode': 790200, 'statusDescription': 'Success.'}
```
## Example 2: `beginIndex` > 20,000
### Step 1 - Call the API to get 100 items
```python
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Topology/OneIPTable"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

count = 100
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
        print ("Get One-Ip Table failed! - " + str(response.text))
    
except Exception as e:
    print (str(e))  

```
```python
{
  "OneIPList": [
    {
      "lanSegment": "10.61.41.28/30",
      "ip": "10.61.41.19",
      "mac": "aabb.cc00.0b31",
      "devName": "bjta002440-SW11",
      "interfaceName": "Ethernet1/3",
      "switchName": "bjta002302-SW8",
      "portName": "Ethernet1/3",
      "alias": "btv/SCa-vSCb/10.61.41.28/30",
      "dns": "bjta002440-SW11.Ethernet1/3",
      "sourceDevice": "bjta002440-SW11",
      "serverType": 2001,
      "switchType": 2001,
      "gateway": "bjta002440-SW11.Ethernet1/3",
      "vlanId": "",
      "vlanGroupId": "bjta002302-SW8##Ethernet1/3",
      "updateTime": "2025-11-04T01:27:59Z",
      "userFlag": 9,
      "source": "Device Interface",
      "vendor": "",
      "descr": "btv/SCa-vSCb/10.61.41.28/30",
      "extSwitchPorts": []
    },
    ... {1 - 99}
    {
      "lanSegment": "10.61.162.1/29",
      "ip": "10.61.16.4",
      "mac": "aabb.cc00.0621",
      "devName": "bjta002115-SW6",
      "interfaceName": "Ethernet1/2",
      "switchName": "",
      "portName": "",
      "alias": "",
      "dns": "bjta002115-SW6.Ethernet1/2",
      "sourceDevice": "bjta002115-SW6",
      "serverType": 2001,
      "switchType": 1009,
      "gateway": "bjta002115-SW6.Ethernet1/2",
      "vlanId": "",
      "vlanGroupId": "bjta002115-SW6##Ethernet1/2",
      "updateTime": "2025-11-04T01:28:00Z",
      "userFlag": 9,
      "source": "Device Interface",
      "vendor": "",
      "descr": "",
      "extSwitchPorts": []
    }
  ],
  "paging": {
    "limit": 100,
    "hasMore": true,
    "nextAfterIpInt": 171811076,
    "nextAfterId": "5f62938c-a1c5-4597-8a6b-eebadf520f40"
  },
  "statusCode": 790200,
  "statusDescription": "Success."
}
```
### Step 2 - To get the next 1000 items, set `afterIpInt` and `afterIpId` and their values from the previous API response.
```python
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Topology/OneIPTable"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

count = 1000
afterIpInt = "171811076"
afterId = "5f62938c-a1c5-4597-8a6b-eebadf520f40"

data = {
    "afterIpInt" : afterIpInt,
    "afterId" : afterId,
    "count" : count
}

try:
    response = requests.get(full_url, params = data, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Get One-Ip Table failed! - " + str(response.text))
    
except Exception as e:
    print (str(e))  
```
```python
{
  "OneIPList": [
    {
      "lanSegment": "10.180.180.0/24",
      "ip": "10.180.180.1",
      "mac": "0022.bdf8.19ff",
      "devName": "NBLEAF-2",
      "interfaceName": "Vlan111",
      "switchName": "",
      "portName": "",
      "alias": "",
      "dns": "NBLEAF-2.Vlan111",
      "sourceDevice": "NBLEAF-2",
      "serverType": 30003,
      "switchType": 1009,
      "gateway": "NBLEAF-2.Vlan111",
      "vlanId": "",
      "vlanGroupId": "NBLEAF-2##Vlan111",
      "updateTime": "2025-11-08T05:13:20Z",
      "userFlag": 9,
      "source": "Device Interface",
      "vendor": "Cisco Systems, Inc",
      "descr": "",
      "extSwitchPorts": []
    },
    ... {1-998}
    {
      "lanSegment": "20.0.112.0/24",
      "ip": "20.0.112.1",
      "mac": "0022.bdf8.19ff",
      "devName": "NBLEAF-4",
      "interfaceName": "Vlan29",
      "switchName": "",
      "portName": "",
      "alias": "",
      "dns": "NBLEAF-4.Vlan29",
      "sourceDevice": "NBLEAF-4",
      "serverType": 30003,
      "switchType": 1009,
      "gateway": "NBLEAF-4.Vlan29",
      "vlanId": "",
      "vlanGroupId": "NBLEAF-4##Vlan29",
      "updateTime": "2025-11-08T05:13:20Z",
      "userFlag": 9,
      "source": "Device Interface",
      "vendor": "Cisco Systems, Inc",
      "descr": "",
      "extSwitchPorts": []
    }
  ],
  "paging": {
    "limit": 1000,
    "hasMore": true,
    "nextAfterIpInt": 335547393,
    "nextAfterId": "eb7ed962-35c7-4b78-b895-ba61511ae909"
  },
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

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
    
    Get One-Ip Table failed! - {"statusCode":791001,"statusDescription":"Invalid parameter: the parameter 'IP' is invalid."}
    
```
## Error Example 2: `count` > 100,000
```python
Input 1:
    
        ip = "" 
        count = 1000000 # count must be between 0 to 100,000
        beginIndex = 50 
          
Response:
    
    Get One-Ip Table failed! - {"statusCode":791002,"statusDescription":"Count can between 0 and 100000"}
```
```python
Input 2:
    
        ip = "" 
        count = 100002 # count must be between 0 to 100,000
        beginIndex = 1 
          
Response:
    
    Get One-Ip Table failed! - {"statusCode":791002,"statusDescription":"Count can between 0 and 100000"}
```
## Error Example 3: `beginIndex` > size of One-Ip Table
```python
Input:
    
        ip = "" 
        count = 1 
        beginIndex = 99999999999 # There are only 117 items in this table.
          
Response:
    
    Get One-Ip Table failed! - {"statusCode":791009,"statusDescription":"The parameter 'BeginIndex' is invalid value."}

```
