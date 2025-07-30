
# Get AAM Report API Design

## ***POST*** V3/AAM/Report
This API is used to get the complete AAM Report. <br><br>
If the Request Body is null or does not contain any data, it means to obtain the Report Data of <i>All</i> Paths under the current Domain.<br>
If the Request Body contains data, but only some path informations under the application is matched, only the matched record will be exported.<br>
If the Request Body contains data, but no path under the application is matched, the record will not be exported.<br><br>
This is consistent with the Export Report logic on the UI page.<br>

![Export Report UI](https://github.com/NetBrainAPI/NetBrain-REST-API-R12.1/raw/main/REST%20APIs%20Documentation/AAM%20(Application%20Assurance%20Module)/AAM%20Images/Export_Report.png)<br>

<b> Note </b>: This is a <i>POST</i>, not <i>GET</i>.


## Detail Information

> **Title** : Get AAM Report<br>

> **Version** : 30/07/2025

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V3/AAM/Report

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
|application*|string|Application Name. |
|paths^|array|Path Name List. |


## Parameters(****required***)
>No parameters required.


## Headers

> **Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| Content-Type | string | support "application/json" |
| Accept | string | support "application/json" |

> **Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| token | string  | Authentication token, get from login API. |

## Response
The depth of the response may vary per application.
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|data|array| AAM Reports. |
|data.lastVerifiedResult|int||
|data.previousCompareResult|bool||
|data.lastVerifiedTime| Date | Last Verified Time. |
|data.taskName| string | Task Name. |
|data.taskTypeDisplay| string |  |
|data.pathNIStatusDetail| string | |
|data.applicationName| string | Application Name. |
|data.pathName| string | Path Name. |
|data.sourceIp| string | Source Information |
|data.sourcePort| int | Source Port. |
|data.sourceDevice| string | Source Device. |
|data.desIp| string | Destination Information. |
|data.desPort| int | Destination Port. |
|data.desDevice| string | Destination Device. |
|data.routingScheme| int | Routing Scheme. |
|data.group| string | Group. |
|data.protocolKeyWord| string | Protocol keyword. |
|data.protocol| string | Protocol number. |
|statusCode| integer | The returned status code of executing the API. |
|statusDescription| string | The explanation of the status code. |


# Full Example:
## Example 1: Get Full AAM Report
```python
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Report"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = []

try:
    response = requests.post(full_url, data = json.dumps(data), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Get AAM Report! - " + str(response.text))
except Exception as e:
    print (str(e))
```
```python
{'data': [{'lastVerifiedResult': 1, 'previousCompareResult': True, 'lastVerifiedTime': '2025-07-26T01:58:34Z', 'taskName': 'QTCM-21464 Benchmark', 'taskTypeDisplay': 'Benchmark Verify', 'pathNIStatusDetail': '', 'applicationName': 'ADT', 'pathName': 'pathHostNameWithPort', 'sourceIp': 'BJ_L2_Core_3', 'sourcePort': 1111, 'sourceDevice': 'BJ_L2_Core_3', 'desIp': 'BJ-L2-Core-A', 'desPort': 2222, 'desDevice': 'BJ-L2-Core-A', 'routingScheme': 0, 'group': '', 'protocolKeyWord': 'TCP', 'protocol': 'TCP', 'expertSettings': ''}, {'lastVerifiedResult': 1, 'previousCompareResult': True, 'lastVerifiedTime': '2025-07-26T01:58:34Z', 'taskName': 'QTCM-21464 Benchmark', 'taskTypeDisplay': 'Benchmark Verify', 'pathNIStatusDetail': '', 'applicationName': 'ADT', 'pathName': 'pathMulticast', 'sourceIp': 'BJ_L2_Core_4', 'sourceDevice': 'BJ_L2_Core_4', 'desIp': 'BJ-L2-Core-A', 'desDevice': 'BJ-L2-Core-A', 'routingScheme': 1, 'group': '234.1.1.1', 'protocolKeyWord': 'IPv4', 'protocol': 'IPv4', 'expertSettings': ''}, {'lastVerifiedResult': 2, 'previousCompareResult': False, 'lastVerifiedTime': '2025-07-26T01:58:34Z', 'problemDevice': 'Device Name:BJ-R1 Failure Category:423 Lack of related information Failure Reason:Neither the next hop IP address nor the output interface has been discovered by NetBrain.\r\nDevice Name:qapp-c3560-1 Failure Category:426 Lack of related information Failure Reason:Output interface was not found', 'taskName': 'QTCM-21464 Benchmark', 'taskTypeDisplay': 'Benchmark Verify', 'pathNIStatusDetail': 'GW2Lab Status Code:\r\nerror\r\nerror\r\n', 'intentResult': '2 Alerts', 'applicationName': 'app1_39404', 'pathName': 'path1_39404', 'sourceIp': 'GW2Lab', 'sourceDevice': 'GW2Lab', 'desIp': 'BJ-R2', 'desDevice': 'BJ-R2', 'routingScheme': 0, 'group': '', 'protocolKeyWord': 'IPv4', 'protocol': 'IPv4', 'expertSettings': ''}, {'lastVerifiedResult': 1, 'previousCompareResult': False, 'lastVerifiedTime': '2025-07-26T01:58:34Z', 'taskName': 'QTCM-21464 Benchmark', 'taskTypeDisplay': 'Benchmark Verify', 'pathNIStatusDetail': '', 'applicationName': 'app1_39404', 'pathName': 'path2_39404', 'sourceIp': 'BJ-R2', 'sourceDevice': 'BJ-R2', 'desIp': 'BJ-R3', 'desDevice': 'BJ-R3', 'routingScheme': 0, 'group': '', 'protocolKeyWord': 'IPv4', 'protocol': 'IPv4', 'expertSettings': ''}, {'lastVerifiedResult': 2, 'previousCompareResult': False, 'lastVerifiedTime': '2025-07-26T01:58:33Z', 'problemDevice': 'Device Name:BJ-R1 Failure Category:423 Lack of related information Failure Reason:Neither the next hop IP address nor the output interface has been discovered by NetBrain.\r\nDevice Name:qapp-c3560-1 Failure Category:426 Lack of related information Failure Reason:Output interface was not found', 'taskName': 'QTCM-21464 Benchmark', 'taskTypeDisplay': 'Benchmark Verify', 'pathNIStatusDetail': '', 'applicationName': 'app2_39404', 'pathName': 'path3_39404', 'sourceIp': 'GW2Lab', 'sourceDevice': 'GW2Lab', 'desIp': 'BJ-R3', 'desDevice': 'BJ-R3', 'routingScheme': 0, 'group': '', 'protocolKeyWord': 'IPv4', 'protocol': 'IPv4', 'expertSettings': ''}, {'lastVerifiedResult': 2, 'lastVerifiedTime': '2025-07-26T01:58:33Z', 'problemDevice': 'Device Name:158.4.0.14 Failure Category:201 Gateway Issue Failure Reason:Gateway device was not found', 'taskName': 'QTCM-21464 Benchmark', 'taskTypeDisplay': 'Benchmark Verify', 'pathNIStatusDetail': '', 'applicationName': 'AA', 'pathName': 'Extreme_ACL_per', 'sourceIp': '158.4.0.14', 'sourceDevice': '158.4.0.14', 'desIp': '158.4.0.18', 'desDevice': '158.4.0.18', 'routingScheme': 0, 'group': '', 'protocolKeyWord': 'IPv4', 'protocol': 'IPv4', 'expertSettings': ''}, {'lastVerifiedResult': 2, 'lastVerifiedTime': '2025-07-26T01:58:33Z', 'problemDevice': 'Device Name:158.4.1.67 Failure Category:201 Gateway Issue Failure Reason:Gateway device was not found', 'taskName': 'QTCM-21464 Benchmark', 'taskTypeDisplay': 'Benchmark Verify', 'pathNIStatusDetail': '', 'applicationName': 'AA', 'pathName': 'ASA_ACL per_any', 'sourceIp': '158.4.1.67', 'sourceDevice': '158.4.1.67', 'desIp': '158.4.1.75', 'desDevice': '158.4.1.75', 'routingScheme': 0, 'group': '', 'protocolKeyWord': 'IPv4', 'protocol': 'IPv4', 'expertSettings': ''}, {'lastVerifiedResult': 0, 'taskTypeDisplay': '', 'pathNIStatusDetail': '', 'applicationName': 'Hello11', 'pathName': 'test', 'sourceIp': '192.168.1.1', 'sourceDevice': 'qapp-c3560-1', 'desIp': '192.168.1.3', 'desDevice': '192.168.1.3', 'routingScheme': 0, 'group': '', 'protocolKeyWord': 'IP', 'protocol': 'IP', 'expertSettings': ''}, {'lastVerifiedResult': 0, 'taskTypeDisplay': '', 'pathNIStatusDetail': '', 'applicationName': 'Hello11', 'pathName': 'testAPIPath1033', 'sourceIp': '172.24.10.10', 'sourceDevice': 'BJ-R2', 'desIp': 'BJ-R1', 'desDevice': 'BJ-R1', 'routingScheme': 0, 'group': '', 'protocolKeyWord': 'IPv4', 'protocol': 'IPv4', 'expertSettings': ''}, {'lastVerifiedResult': 0, 'taskTypeDisplay': '', 'pathNIStatusDetail': '', 'applicationName': 'Hello11', 'pathName': 'testAPIPath102', 'sourceIp': '172.24.10.10', 'sourceDevice': 'BJ-R2', 'desIp': 'BJ-R1', 'desDevice': 'BJ-R1', 'routingScheme': 0, 'group': '', 'protocolKeyWord': 'IPv4', 'protocol': 'IPv4', 'expertSettings': ''}, {'lastVerifiedResult': 0, 'taskTypeDisplay': '', 'pathNIStatusDetail': '', 'applicationName': 'Hello11', 'pathName': 'testAPIPath203', 'sourceIp': 'BJ-R3', 'sourceDevice': 'BJ-R3', 'desIp': 'GW2Lab', 'desDevice': 'GW2Lab', 'routingScheme': 0, 'group': '', 'protocolKeyWord': 'IPv4', 'protocol': 'IPv4', 'expertSettings': ''}, {'lastVerifiedResult': 0, 'taskTypeDisplay': '', 'pathNIStatusDetail': '', 'applicationName': 'Hello11', 'pathName': 'testAPIPath101', 'sourceIp': '172.24.10.10', 'sourceDevice': 'BJ-R2', 'desIp': 'BJ-R1', 'desDevice': 'BJ-R1', 'routingScheme': 0, 'group': '', 'protocolKeyWord': 'IPv4', 'protocol': 'IPv4', 'expertSettings': ''}, {'lastVerifiedResult': 0, 'taskTypeDisplay': '', 'pathNIStatusDetail': '', 'applicationName': 'testApp1', 'pathName': 'testPath1', 'sourceIp': '10.10.0.17', 'sourceDevice': '10.10.0.17', 'desIp': '10.10.19.11', 'desDevice': '10.10.19.11', 'routingScheme': 0, 'group': '', 'protocolKeyWord': 'IPv4', 'protocol': 'IPv4', 'expertSettings': ''}, {'lastVerifiedResult': 0, 'taskTypeDisplay': '', 'pathNIStatusDetail': '', 'applicationName': 'testApp1', 'pathName': 'testPath2', 'sourceIp': '10.10.0.17', 'sourceDevice': '10.10.0.17', 'desIp': '10.10.19.11', 'desDevice': '10.10.19.11', 'routingScheme': 0, 'group': '', 'protocolKeyWord': 'IPv4', 'protocol': 'IPv4', 'expertSettings': ''}, {'lastVerifiedResult': 0, 'taskTypeDisplay': '', 'pathNIStatusDetail': '', 'applicationName': 'testApp2', 'pathName': 'testPath1', 'sourceIp': '10.10.0.17', 'sourceDevice': '10.10.0.17', 'desIp': '10.10.19.11', 'desDevice': '10.10.19.11', 'routingScheme': 0, 'group': '', 'protocolKeyWord': 'IPv4', 'protocol': 'IPv4', 'expertSettings': ''}, {'lastVerifiedResult': 0, 'taskTypeDisplay': '', 'pathNIStatusDetail': '', 'applicationName': 'testApp2', 'pathName': 'testPath2', 'sourceIp': '10.10.0.17', 'sourceDevice': '10.10.0.17', 'desIp': '10.10.19.11', 'desDevice': '10.10.19.11', 'routingScheme': 0, 'group': '', 'protocolKeyWord': 'IPv4', 'protocol': 'IPv4', 'expertSettings': ''}], 'statusCode': 790200, 'statusDescription': 'Success.'}

```

## Example 2: Get Partial AAM Report
`testApp2` has two paths - `testPath1` and `testPath2`. <br>
In the below example, only `testPath1` is matched, hence only the matched Path information (i.e. `testPath1`) of the Application will be returned.

```python
full_url = nb_url + "/ServicesAPI/API/V3/AAM/Report"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

data = [
    {
        'application': 'testApp2',
        'paths': ['testPath1', 'path2'] # There is no path named 'path2'
    }
]
try:
    response = requests.post(full_url, data = json.dumps(data), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print("Failed to Get AAM Report! - " + str(response.text))
except Exception as e:
    print (str(e))
```
```python
{
  "data": [
    {
      "lastVerifiedResult": 0,
      "taskTypeDisplay": "",
      "pathNIStatusDetail": "",
      "applicationName": "testApp2",
      "pathName": "testPath1",
      "sourceIp": "10.10.0.17",
      "sourceDevice": "10.10.0.17",
      "desIp": "10.10.19.11",
      "desDevice": "10.10.19.11",
      "routingScheme": 0,
      "group": "",
      "protocolKeyWord": "IPv4",
      "protocol": "IPv4",
      "expertSettings": ""
    }
  ],
  "statusCode": 790200,
  "statusDescription": "Success."
}
```

# cURL Code from Postman
This cURL command is based on Example 1.
```python
curl -X POST \
  "http://192.168.36.19/ServicesAPI/API/V3/AAM/Report" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "cache-control: no-cache" \
  -H "token: 1333a5eb-5e13-4d85-b813-8f87f80fb970"
```
