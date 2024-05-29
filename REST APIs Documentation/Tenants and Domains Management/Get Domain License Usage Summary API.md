
# Get Domain License Usage Summary API Design

## ***GET*** /V1/CMDB/Domains/LicenseUsageSummary
Call this API to get domain license usage information.

## Detail Information

> **Title** : Get Domain License Usage Summary<br>

> **Version** : 17/05/2024.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Domains/LicenseUsageSummary

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

>No parameters required.

## Parameters(****required***)
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|domainId^|string|Domain ID. Optional. |
|tenantId^|string|Domain ID. Optional. |


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
|domainId| string | Domain Id.  |
|domainName| string | Domain Name.  |
|tenantId| string | Tenant Id.  |
|tenantName| string | Tenant Name.  |
|license summary| object | Object of license usage.  |
|nodeLicenseUsage| list of objects | List of licenses and their usages.  |
|nodeLicenseUsage.type| string | Type of node license.  |
|nodeLicenseUsage.amount| integer | Used count of nodes.  |
|nodeLicenseUsage.details| list of objects | Each device type used.  |
|seatLicenseUsage| object | Object of Seat License Usage.  |
|seatLicenseUsage.current| object | Currently used seats of domain and system.  |
|seatLicenseUsage.historyOfMaxConcurrent| object | Number of seats used by specific time frames.  |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***


```python
{
  "statusCode": 0,
  "statusDescription": "",
  "data": [
    {
      "domainId": "",
      "domainName": "",
      "tenantId": "",
      "tenantName": "",
      "licenseSummary": {
        "nodeLicenseUsage": [
          {
            "type": "Foundataion",
            "amount": 1,
            "details": [
              {
                "deviceTypeName": "Router",
                "amount": 1
              }
            ]
          },
          {
            "type": "WAP",
            "amount": 1
          },
          {
            "type": "Cisco ACI",
            "amount": 1
          },
          {
            "type": "vCenter",
            "amount": 1
          },
          {
            "type": "NSX-v",
            "amount": 1
          },
          {
            "type": "Amazon AWS",
            "amount": 1
          }
        ],
        "seatLicenseUsage": {
          "current": {
            "domainUsedSeats": 17,
            "systemUsedSeats": 52
          },
          "historyOfMaxConcurrent": {           
            "30Days": "68/101",
            "l7Days": "24/52",
            "today": "23/61"
          }
        }
      }
    }
  ]
    'statusCode': 790200, 
    'statusDescription': 'Success.'
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
token = "9c717c9a-4302-45b5-a068-2a3e9c4ea1a3"
nb_url = "http://192.168.30.166"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Domains/LicenseUsageSummary"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["token"] = token

domainId = ""
tenantId = ""

data = {
    "domainId": domainId,
    "tenantId": tenantId
    }

try:
    response = requests.get(full_url, params = data, headers = headers, verify = False)
    print(response)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Get Domain License failed - " + str(response.text))
    
except Exception as e:
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman

```python
curl -X GET \
'http://192.168.20.45/ServicesAPI/API/V1/CMDB/Domains/LicenseUsageSummary' \
-H 'Content-Type: application/json' \
-H 'token: 54e2addc-dc77-40da-aa0f-f32e33d328f5'
-d '{
    "domainId" : "",
    "tenantId" : ""
}'
```