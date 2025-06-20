
# Device API Design

## ***GET*** /V1/CMDB/Devices
This API call is used to get the corresponding device by an IP address or device name. For duplicate IP addresses, this API returns a device list.

If none of hostname and ip provided, response will return all devices of current domain.<br>


## Detail Information

> **Title** : Get Devices API<br>

> **Version** : 01/25/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Devices

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
| hostname | string  | The host name of device. |
| ip | string  | The management ip of device. |
|||If both `hostname` and `ip` are provided that does not belong to one device, then the response device corresponds to the hostname.|
|ignoreCase|boolean|Recognizes as case-insensitive hostname|


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
|devices| string[] | A list of devices. |
|devices.id| string | The device ID. |
|devices.mgmtIP| string | The management IP address of the returned device. |
|devices.name| string | The hostname of returned device. |
|devices.subTypeName| string | Device type of the returned device, such as Cisco Router. |
|devices.fDiscoveryTime| Time | First discovery time of device. |
|devices.lDiscoveryTime| Time | Last discovery time of device. |
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

# Full Example:

## Example 1: with `hostname` value
```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Set the request inputs
token = "13c7ed6e-781d-4b22-83e7-b1722de4e31d"
nb_url = "http://192.168.x.x"

query = {'hostname': 'ASARouter'}

try:
    response = requests.get(full_url, headers=headers, params = query, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Get Devices failed! - " + str(response.text))
except Exception as e:
    print (str(e)) 
```
```
{
  "devices": [
    {
      "id": "96697e56-192a-4c2a-8a01-3fd7391e176e",
      "mgmtIP": "172.1.1.1",
      "name": "ASARouter",
      "subTypeName": "Cisco IOS Switch",
      "fDiscoveryTime": "2023-08-25T17:34:31Z",
      "lDiscoveryTime": "2023-08-25T17:34:31Z"
    }
  ],
  "statusCode": 790200,
  "statusDescription": "Success."
}
``` 


# cURL Code from Postman

```python
curl -X GET \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Devices?token=e074d192-3f21-4ae8-b5f1-405d240b65ca&ip=10.1.12.2' \
  -H 'Postman-Token: e0ba3bb8-aac1-4084-9e95-de424cbf7feb' \
  -H 'cache-control: no-cache'
```

# Error Examples

## Error Example 1: The Device With Specified Hostname Does Not Exist

```python
Input:
    hostname = "blahblahblah" # Device with hostname "blahblahblah" doesn't exist
    
Response:
    
    "{
        'devices': [], 
        'statusCode': 790200, 
        'statusDescription': 'Success.'
    }"
```

## Error Example 2: Wrong IP Format
```
Input:
    ip = "101122" #the correct ip should be "10.1.12.2"
    
Response:
    
    "{
        'devices': [], 
        'statusCode': 790200, 
        'statusDescription': 'Success.'
    }"
```

"""Note 1 : If user provides both hostname and ip that does not belongs to one device, 
            then the response device corresponds to the hostname"""
```
