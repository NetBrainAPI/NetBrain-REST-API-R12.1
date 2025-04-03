# Create Dynamic Search API Design
 
## ***PUT*** /V1/CMDB/Sites/Leaf/DynamicSearch
Call this API to create dynamic search in NetBrain Domain Management - Site Manager.

**Note that some site management APIs require calling of other site management APIs prior to, or after.
Refer to [Golden Use Case Templates - Site Management by Calling APIs](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/Golden%20Use%20Case%20Templates/Sites%20Management%20by%20Calling%20APIs.md) for more information.

## Detail Information
 
> **Title** : Create Dynamic Search API<br>
 
> **Version** : 03/08/2024
 
> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Sites/Leaf/DynamicSearch
 
> **Authentication** :
 
|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token |

## Request body(****required***)
 
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| sitesId^ | string | TThe unique id of specified site. Need either or of siteId and sitePath. |
| sitePath^ | string | Full path name of a site. e.g. 'My Network/Site1/Boston'. |
| filter | string | This string will include all parameters selected such as Device Property, Interface Property, Module Property, Config File, and Front Server, and their respective parameters. |
| filter.expression | string | e.g. A and B and C |
| filter.conditions | array | |
| filter.conditions.schema | string | Schema name  |
| filter.conditions.operator | integer |<table>  <thead>  <tr>  <th>Operator</th>  <th>Operator int value</th>  <th>Schema Type</th>  <th>Schema</th>  </tr>  </thead>  <tbody>  <tr>  <td>Matches any</td>  <td>0</td>  <td rowspan="2">Select</td>  <td rowspan="2">mainType<br>subType</td>  </tr>  <tr>  <td>Does not match any</td>  <td>1</td>  </tr>  <tr>  <td>Contains</td>  <td>4</td>  <td rowspan="4">String</td><td rowspan="4">  </tr>  <tr>  <td>Does not match</td><td>5</td></tr><tr><td>Matches</td><td>0</td>  </tr>  <tr>  <td>Does not contain</td>  <td>1</td></tr>    <tr>  <td>Matches</td>  <td>0</td>   <td rowspan="2">IP or Int</td><td rowspan="2"><br>iploc<br>mgmtip<br>ips<br>ipv6<br>proxyserver<br>externalservers.externalserverid</td></tr><tr><td>Does not match any</td>  <td>1</td></tr><tr><td>is</td><td>0</td><td>Bool</td><td></td></tr></tbody>  </table>|
| filter.conditions.expression | string | |
| filter.conditions.fieldType^ | integer | String = 0, Double = 1, Boolean = 2, |



> ***Example***
 
 
```python
{
  sitePath : "My Network/Unnamed-site1"
  filter : {
          "expression": "A and (b and C)",
          "conditions": [
              {
                  "schema": "subType",
                  "operator": 0,
                  "expression": "rootType"
              },
              {
                  "schema": "intfs.speed",
                  "operator": 4,
                  "expression": "1"
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
| statusCode | integer | The returned status code of executing the API. |
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
token = '684c4e42-833a-428d-a5c9-3f047f3d3a67'
nb_url = "http://192.168.31.191"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/Leaf/DynamicSearch"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token
 
{
  sitePath : "My Network/Unnamed-site1"
  filter : {
          "expression": "A and (b and C)",
          "conditions": [
              {
                  "schema": "subType",
                  "operator": 0,
                  "expression": "rootType"
              },
              {
                  "schema": "intfs.speed",
                  "operator": 4,
                  "expression": "1"
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

body = {
    "sitePath": sitePath,
    "filter" : filter
}
 
try:
    response = requests.put(full_url, json.dumps(body), headers=headers, verify=False)
    print(response)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Failed to create dynamic search - " + str(response.text))

except Exception as e:
    print (str(e)) 

```
 
    {'statusCode': 790200, 'statusDescription': 'Success.'}
     
 
# cURL Code from Postman:
 
 
```python
curl -X PUT \
  http://192.168.31.191/ServicesAPI/API/V1/CMDB/Sites/Leaf/DynamicSearch \
  -H "Content-Type: application/json"
  -H 'token: db4363c3-2f5f-47da-bfc8-2d14b6992558' \
  -d '{
    "sitePath": "My Network/Unnamed-site1",
    "filter" : {
        "expression": "A and (b and C)",
        "conditions": [
            {
                "schema": "subType",
                "operator": 0,
                "expression": "rootType"
            },
            {
                "schema": "intfs.speed",
                "operator": 4,
                "expression": "1"
            },
            {
                "expression": "False",
                "operator": 0,
                "schema": "hasEIGRPConfig",
                "fieldType": 2
            }
        ]
    }
}'
```