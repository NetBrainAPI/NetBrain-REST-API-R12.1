
# Topology API Design

## ***GET*** /V1/CMDB/Topology/Devices/Neighbors
This API is used to get the neighbor device hostname and connection interface names of the interface pair from any devices in current working domain if without the input hostname and topotype.<br>**Note: The API follows the privilege control of NB system. If there is restriction set by Access Control Policy for the target querying resources, the response will not return queried data.**

## Detail Information

> **Title** : Device Neighbors with Topology Type API<br>

> **Version** : 16/06/2015.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Topology/Devices/Neighbors

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

>No request body.

## Query Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|hostname | list of string  | The device name <br>e.g. ["US-BOS-R1"], or ["US-BOS-R2", "US-BOS-R3", "US-BOS-R4"]|
|topoType | list of int  | Returns the neighbors in specified topology types:<br> `1` (default): `L3_Topo_Type`, <br>`2`: `L2_Topo_Type`, <br>`3`: `Ipv6_L3_Topo_Type`, <br>`4`: `VPN_Topo_Type` <br>e.g. `[1]` or `[2,3,4]`. <br><br> If value other than 1-4 is specified, returns respone: `Please select the exist topology type`. <br>If wrong value type is specified (e.g. ["1", "2"]), returns respone: `Topology type must be insert as Integer`|
|||If both `hostname` and `topoType` are input as filters, but there is no corresponding topology interface on some devices, then returns `Device xxx don't have yyy interface.` <br> If only one input is given, then only that given filter will be considered. <br> e.g. only `["US-BOS-R1"]` - returns all topology type of `US-BOS-R1`.|
|version | string | This is a minor version number. Value of this parameter is `1`.|
|skip|integer|The amount of records to be skipped. <br>The value cannot be negative. If the value is negative, API throws exception `{"statusCode":791001,"statusDescription":"Parameter 'skip' cannot be negative"}`. No upper bound for this parameter.|
|limit|integer|The up limit amount of device records to return per API call. <br>The value cannot be negative. If the value is negative, API throws exception `{"statusCode":791001,"statusDescription":"Parameter 'limit' cannot be negative"}`. <br> The range of this parameter: `10`-`100`. <br><br> default value: `50`|
|||If only `skip` is provided, returns the rest of the full device list. <br>If only `limit` is provided, returns from the first device in DB. If both `skip` and `limit` are provided, returns as required. Error exceptions follow each parameter's descriptions.|
|||**Note:** The `skip` and `limit` parameters are based on the device search result, not topology result record.|
|filterBus | boolean | When set to `True`, returns the list of neighbors in the opposite link. <br>e.g. if the uplink neighbor ID is queried, returns a list of neighbors in downlink, and vice versa|

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
|hostdevice | list of object | List of object content detailed information of neighbor base on this device. |
|hostdevice.interface | string | Interface name of the interface that belongs to `hostdevice`. |
|hostdevice.connected_device | object | Detailed information of neighbor device that connects to `hostdevice`. |
|hostdevice.connected_device.nbr_device| string | The hostname of the neighbor device that connect to `hostdevice`.|
|hostdevice.connected_device.inteface_name| string | The interface name of the neighbor device that connects to the `hostdevice` interface.|
|hostdevice.topology | string | The topology name in which this neighbor device belongs to, such as `L2_Topo_Type`, `L3_Topo_Type`, `Ipv6_L3_Topo_Type` or `VPN_Topo_Type`. |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***



```python
{
    "topology":
    [
        {   
            "hostname": "device_1",
            "neighbors":
            [
                {
                    "interface": "intf_1",
                    "connected_device":
                    {
                        "nbr_device": "device_2",
                        "nbr_intf": "intf_3"
                    },
                    "topology": "L2"
                }
				{
                    "interface": "intf_2",
                    "connected_device":
                    {
                        "nbr_device": "device_3",
                        "nbr_intf": "intf_4"
                    },
                    "topology": "L2"
                }
            ]
        }
		{   
            "hostname": "device_4",
            "neighbors":
            [
                {
                    "interface": "intf_5",
                    "connected_device":
                    {
                        "nbr_device": "device_5",
                        "nbr_intf": "intf_6"
                    },
                    "topology": "L3"
                }
        }
    ]
}
```
# Full Examples:

## Example 1:
```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Set the request inputs
token = "e9c7af7c-eedd-40fb-8b4b-356974a12b91"
nb_url = "http://192.168.28.143"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Topology/Devices/Neighbors"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

hostnames = ["BJ_Acc_Sw4"]
topoTypes = [1]

data = {
        "hostname" : hostnames,
        "topoType" : topoTypes,
        "version": "1"
    }

try:
    response = requests.get(full_url, params = data, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Get neighbors by topology failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'neighbors': [{'hostname': 'R4', 'interface': 'Ethernet0/1 123.10.1.1/30'}, {'hostname': 'R5', 'interface': 'Ethernet0/1 123.10.1.6/30'}], 'statusCode': 790200, 'statusDescription': 'Success.'}
    
## Example 2: Get all devices' topo info in current domain


```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Set the request inputs
token = "e9c7af7c-eedd-40fb-8b4b-356974a12b91"
nb_url = "http://192.168.28.143"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Topology/Devices/Neighbors"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

hostnames = ["BJ_Acc_Sw4"...] # get the list of all device hostnames in current domain by calling get device API first.
topoTypes = [1]

skip = 0
count = 50
try:
    while count == 50:
        data = {
            "hostname" : hostnames,
            "topoType" : topoTypes,
            "version": "1",
            "skip" : skip
        }
        response = requests.get(full_url, params = data, headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()
            count = len(result["topology"])
            skip = skip + count
            print (result)
	    #Un-command next line if want to test calling result length.
            #print (len(result['topology'])) 
        else:
            print("Get neighbors by topology failed! - " + str(response.text))
except Exception as e:
    print (str(e)) 
```

## Example 3: Using `filterBus`
```python
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Topology/Devices/Neighbors"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

hostname = "BJ_L2_Core_3"

data = {
        "hostname" : hostname,
        "topoType" : [2],
        "filterBus": True
    }

try:
    response = requests.get(full_url, params = data, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Get neighbors by topology failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```
```
    {'topology': [{'hostname': 'BJ_L2_Core_3', 'neighbors': [{'interface': 'FastEthernet1/0/8', 'topology': 'L2_Topo_Type', 'connected_device': {'nbr_device': 'BJ_Acc_SW6', 'nbr_intf': 'FastEthernet0/24'}}, {'interface': 'FastEthernet1/0/19', 'topology': 'L2_Topo_Type', 'connected_device': {'nbr_device': 'BJ_Dis_SW2', 'nbr_intf': 'FastEthernet0/24'}}, {'interface': 'FastEthernet1/0/18', 'topology': 'L2_Topo_Type', 'connected_device': {'nbr_device': 'BJ_Acc_SW6', 'nbr_intf': 'FastEthernet0/22'}}, {'interface': 'FastEthernet1/0/17', 'topology': 'L2_Topo_Type', 'connected_device': {'nbr_device': 'BJ_Acc_Sw4', 'nbr_intf': 'FastEthernet0/24'}}, {'interface': 'FastEthernet1/0/4', 'topology': 'L2_Topo_Type', 'connected_device': {'nbr_device': 'NY_Router', 'nbr_intf': 'FastEthernet0/0'}}, {'interface': 'FastEthernet1/0/16', 'topology': 'L2_Topo_Type', 'connected_device': {'nbr_device': 'BJ_Acc_Sw4', 'nbr_intf': 'FastEthernet0/17'}}, {'interface': 'FastEthernet1/0/2', 'topology': 'L2_Topo_Type', 'connected_device': {'nbr_device': 'GW2Lab', 'nbr_intf': 'GigabitEthernet0/1'}}, {'interface': 'FastEthernet1/0/23', 'topology': 'L2_Topo_Type', 'connected_device': {'nbr_device': 'BJ-L2-Core-A', 'nbr_intf': 'FastEthernet0/5'}}]}], 'statusCode': 790200, 'statusDescription': 'Success.'}
```


# cURL Code from Postman:


```python
curl -X GET \
  'http://192.168.28.143/ServicesAPI/API/V1/CMDB/Topology/Devices/Neighbors%0A?hostname=%5B%22BJ_Acc_SW1%22%5D&topoType=%5B1%5D&version=%221%22' \
  -H 'Postman-Token: d43de85c-8de9-4bcf-be28-9bc16ce7b329' \
  -H 'cache-control: no-cache' \
  -H 'token: 3d0f475d-dbae-4c44-9080-7b08ded7d35b'
```

# Error Examples:
```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
        
        hostname = [""] # Cannot be null.
        topoType = [] # Cannot be null.

Response:
    
    "Get neighbors by topology failed! - 
        {'topology': [], 'statusCode': 790200, 'statusDescription': 'Success.'}"
        
###################################################################################################################    

"""Error 2: wrong inputs"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
        
        hostname = "dummy" # No device with a hostname called "dummy"
        topoType = []

Response:
    
    "Get neighbors by topology failed! - 
        {'topology': [], 'statusCode': 790200, 'statusDescription': 'Success.'}"

#--------------------------------------------------------------------------------------------------------------------        
    
Input:
        
        hostname = "R1" 
        topoType = [7] # No topology code for 7.

Response:
    
    "Get neighbors by topology failed! - 
        {'statusCode': 791001, 'statusDescription': "Parameter 'TopoType' value must be greater than 0 and less than 4"}"
```
