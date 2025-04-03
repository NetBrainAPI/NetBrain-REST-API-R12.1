# Update Site Properties API Design
 
## ***PUT*** /V1/CMDB/Sites/SiteInfo
Call this API to update site properties in NetBrain Domain Management.

**Note that some site management APIs require calling of other site management APIs prior to, or after.
Refer to [Golden Use Case Templates - Site Management by Calling APIs](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/Golden%20Use%20Case%20Templates/Sites%20Management%20by%20Calling%20APIs.md) for more information.

## Detail Information
 
> **Title** : Update Site Properties API<br>
 
> **Version** : 26/10/2023
 
> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Sites/SiteInfo
 
> **Authentication** :
 
|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token |

## Request body(****required***)
 
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| siteId^ | string | The unique id of specified site. Optional, Need either or of siteId and sitePath. |
| sitePath^ | string | Full path name of a site. For example, 'My Network/Site1/Boston'. Optional |
| siteCategory^ | integer | Category of Site. If there is no site with the same path at the same level, you do not need to pass this parameter. ContainerSite = 1, LeafSite = 2, |
| properties.region | string | Region of customer |
| properties.locAdr | string | Location/Address of customer |
| properties.employeeNum | integer | Number of employees |
| properties.contactName | string | Site admin contact name |
| properties.phone | string | Site admin phone number |
| properties.email | string | Site admin email |
| properties.siteType | string | Site type. (Headquarter, Data Center, Regional Office, Disaster Recovery) |
| properties.description | string | Description of site |
| properties.customizedInfo | object | CustomizedInfo of site |
 
> ***Example***
 
 
```python
{
    "siteId": "c3845ef0-1fac-4797-96fb-b9884a8297ed",
    //"sitePath": "My Network/Unnamed-site4",
    "filter": {
        "expression": "A and (b and C) or d or e and f",
        "conditions": [
            {
                "schema": "subType", // the type of expression (e.g. A and B) should match with the type of the schema
                "operator": 0, // range: 0~13
                "expression": "rootType"
            },
            {
                "schema": "intfs.speed",
                "operator": 4,
                "expression": "1"
            },
            {
                "schema": "ExternalServers.ExternalServerId",
                "operator": 0,
                "expression": "78ed6162-1b65-4f82-bd2f-45fc4a443e7a"
            },
             {
                "schema": "proxyServer",
                "operator": 0,
                "expression": "none;FS1"
            },
            {
                "expression": "False",
                "operator": 0,
                "schema": "hasEIGRPConfig",
                "fieldType": 2
            }
        ]
    }
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
 
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| statusCode| integer | The returned status code of executing the API. |
| statusDescription | string | The explanation of the status code. |
 
> ***Example***
 
 
```python
{
    'statusCode': 790200,
    'statusDescription': 'Success'
}
```
 
# Full Example:
 
 
```python
# import python modules
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
 
# Set the request inputs
token = '2cd5a9ef-0100-4071-8dad-112e55f8c957'
nb_url = "http://192.168.31.191"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/SiteInfo"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token
 
sitePath = "My Network/Unnamed-site1"
properties = {
        "region": "2354",
        "locAdr": "523",
        "employeeNum": 0,
        "contactName": "23523",
        "phone": "5235",
        "email": "23523@1.com",
        "siteType": "None",
        "description": "12412125",
        "customizedInfo": {"tags":"112312"}
}

body = {
    "sitePath": sitePath,
    "properties" : properties
}

 
try:
    response = requests.put(full_url, data = json.dumps(body), headers=headers, verify=False)
    print(response)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Failed to update site properties - " + str(response.text))

except Exception as e:
    print (str(e)) 

```
 
    {'statusCode': 790200, 'statusDescription': 'Success.'}
     
 
# cURL Code from Postman:
 
 
```python
curl -X PUT \
  http://192.168.31.191//ServicesAPI/API/V1/CMDB/Sites/SiteInfo \
  -H "Content-Type: application/json"
  -H 'token: 2cd5a9ef-0100-4071-8dad-112e55f8c957' \
  -d '{
    "sitePath": "My Network/Unnamed-site1",
    "properties" : {
        "region": "2354",
        "locAdr": "523",
        "employeeNum": 0,
        "contactName": "23523",
        "phone": "5235",
        "email": "23523@1.com",
        "siteType": "None",
        "description": "12412125",
        "customizedInfo": {"tags":"112312"}
  }
}'
```