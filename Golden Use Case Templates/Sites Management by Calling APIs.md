
# Sites and Devices In Sites Modification
This use case focuses on Site Management APIs. 

Overview: <br>
> 1. Create multiple sites with different types
> 2. Add and replace devices in the site
> 3. Commit the site changes
> 4. Delete site transaction (optional)
> 5. Logout of NetBrain

This use case utilizes 13 APIs.

**[Step 1: Use Case Preparation](#step-1-use-case-preparation)**
> 1a. import all useful modules and create global variables<br>
> 1b. call login API<br>
> 1c. call specify_a_working_domain API<br>

**[Step 2: Create One Transaction For Site Modification](#step-2-create-one-transaction-for-site-modification)**
> 2a. call create_site_transaction API<br>
> 2b. call site_transaction_heartbeat API <br>

**[Step 3: Create Site For Site Modification](#step-3-create-site-for-site-modification)**
> 3a. call create_site API<br>
> 3b. call create_a_leaf_site API <br>

**[Step 4: Modify Devices In Site](#step-4-modify-devices-in-site)**
> 4a. call add_site_device API<br>
> 4b. call get_site_devices API <br>
> 4c. call replace_site_devices API <br>

**[Step 5: Implement All Modifications To System](#step-5-implement-all-modifications-to-system)**
> 5a. call commit_site_transactionn API

**[Step 6: Remove Site Transaction (Optional)](#step-6-remove-site-transaction-optional)**
> 6a. call remove_site_transaction API

**[Step 7: Logout of NetBrain System](#step-7-logout-of-netbrain-system)**
> 7a. call logout API

## Step 1: Use Case Preparation

***1a. import all useful modules and create global variables***
> Remember to change the `nb_url` to user's own working url.

***1b. call login API***
>Call the login API with `username` and `password` as inputs. As response, we will get the authentication `token` as the fixed input in following API callings. 
For more details, please refer to [Login API](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Login%20API.md)

***1c. call specify_a_working_domain API***
>With the above steps, we have completed the full login process; we have joined the NetBrain system by calling APIs with records of tenantId and domainId. <br>
If the user doesn't know the ID of corresponding tenant and domain, please fully complete step 1-4 in use case #1.

> We will now proceed to use the NetBrain functions.
For more details on this API, please refer to [Specify A Working Domain API](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Specify%20A%20Working%20Domain%20API.md) 



```python
# import python modules 
import requests
import time
import urllib3
import pprint
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

nb_url = "http://192.168.28.79"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'} 
TenantName = "Initial Tenant"
DomainName = "Support and Service"
username = "gdluserTest"
password = "123456"
tenantId = "fb24f3f0-81a7-1929-4b8f-99106c23fa5b"
domainId = "850ff5e9-c639-404d-85a3-d920dbee509c"
```


```python
# call login API
body = {
    "username" : username,
    "password" : password
}

login_URL = nb_url + "/ServicesAPI/API/V1/Session"

def login(login_URL, body, headers):
    try:
        # Do the HTTP request
        response = requests.post(login_URL, headers=headers, data = json.dumps(body), verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            js = response.json()
            return (js["token"])
        else:
            return ("Failed to Get Token! - " + str(response.text))
    except Exception as e:
        return (str(e))
    
token = login(login_URL, body, headers)
print(token) # print out the authentication token.
```
API Response: 

    639afa4a-0fb1-4005-b540-ad037ed131d9
    


```python
# call specify_a_working_domain API

specify_a_working_domain_url = nb_url + "/ServicesAPI/API/V1/Session/CurrentDomain"

def specify_a_working_domain(tenantId, domainId, specify_a_working_domain_url, headers, token):
    headers["Token"] = token
    body = {
        "tenantId": tenantId,
        "domainId": domainId
    }
    
    try:
        # Do the HTTP request
        response = requests.put(specify_a_working_domain_url, data=json.dumps(body), headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            result = response.json()
            return ("Working Domain Specified Successfully, with domainId: " + domainId)
            
        elif response.status_code != 200:
            return ("Login Failed! - " + str(response.text))

    except Exception as e: print (str(e))

res =  specify_a_working_domain(tenantId, domainId, specify_a_working_domain_url, headers, token)
print (res)
```
API Response: 

    Working Domain Specified Successfully, with domainId: 850ff5e9-c639-404d-85a3-d920dbee509c
    

## Step 2: Create One Transaction For Site Modification

***2a. call create_site_transaction API***
>All site modification operations must be executed in a transaction. In another word, the user should create a transaction before starting any other site changes. 
e.g. create site, move devices.

>After site modification, the user must <i>explicitly commit</i> the operations.

>Note that a site transaction will <i>lock</i> the entire NetBrain system for site change operations. To prevent a system-wide deadlock due to client exception or negligence, if no follow-up operations or heartbeat is sent within a 30-seconds timeframe, another could invalidate this transaction and create a new transaction which cannot used by the current session.

>Alternatively, you can also call [Remove Site Transaction API](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/REST%20APIs%20Documentation/Site%20Management/Remove%20Site%20Transaction%20API.md) to free up the site operation. Removing the transaction also lets the user discard any site change operations since the beginning of a transaction, or called rollback. <br>
Please refer to Step 6 for more details on how to delete the site transaction.

>Please refer to [Create A Site Transaction API](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/REST%20APIs%20Documentation/Site%20Management/Create%20A%20Site%20Transaction%20API.md) for more details.

***2b. call site_transaction_heartbeat API***
>This API sends a hearbeat signal to the server to keep a transaction alive.

>Failed to do so will cause the transaction to be discarded by the system, if no other site change operations have been sent to the server via the current session within 30 seconds. For more details on this API, please refer to [Site Transaction Heartbeat API](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/REST%20APIs%20Documentation/Site%20Management/Site%20Transaction%20Heartbeat%20API.md).


```python
# call create_site_transaction API
create_a_transaction_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/Transactions"

def create_a_transaction(create_a_transaction_url, headers, token):
    try:
        response = requests.post(create_a_transaction_url, headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()
            print (result)
        else:
            print ("Failed to Create Site Transaction! - " + str(response.text))

    except Exception as e:
        print (str(e)) 

result = create_a_transaction(create_a_transaction_url, headers, token)
result
```
API Response: 

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    


```python
# call site_transaction_heartbeat API

site_transaction_heartbeat_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/Transactions/Heartbeat"

def site_transaction_heartbeat(headers, token):
    headers["Token"] = token

    try:
        response = requests.post(site_transaction_heartbeat_url, headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()
            print (result)
        else:
            print ("Failed to Set Site Transaction Heartbeat! - " + str(response.text))

    except Exception as e:
        print (str(e))
        
result = site_transaction_heartbeat(headers, token)
result
```
API Response: 

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

## Step 3: Create Site For Site Modification
***3a. call create_site API***
>After creating the transaction for site modification, we are going to create a site via [Create Site API](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/REST%20APIs%20Documentation/Site%20Management/Create%20Site%20API.md).
>Note that<br>
>>a) a new site will be created as a parent site if a site doesn't have its parent site in current system.<br>
>>b) this API will replace the ImportSiteTree in 7.0b.<br>
>>c) this API call needs to be invoked in a site transaction.

***3b. call create_a_leaf_site API***
>By now, we have created two parent sites. Now, we will call [Create A Leaf Site API](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/REST%20APIs%20Documentation/Site%20Management/Create%20A%20Leaf%20Site%20API.md) to create a container site. If a parent site doesn't exist in current system, please make sure to create it before creating its child site.


```python
create_site_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites"

sitePath1 = "My Network/America"
isContainer1 = True

sitePath2 = "My Network/America/Burlington"
isContainer2 = False

body = {
   "sites": [
                {
                    "sitePath" : sitePath1,
                    "isContainer": isContainer1
                },
                {
                    "sitePath" : sitePath2,
                    "isContainer": isContainer2
                }
            ]
        }

def create_site(create_site_url, headers, token, body):
    headers["Token"] = token
    try:
        response = requests.post(create_site_url, data = json.dumps(body), headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()
            print (result)
        else:
            print ("Failed to Create Site! - " + str(response.text))

    except Exception as e:
        print (str(e))
        
result = create_site(create_site_url, headers, token, body)
result
```
API Response: 

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    


```python
create_a_leaf_site_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/Leaf"

sitePath = "My Network/America/Burlington/Netbrain"

body = {
            "sitePath" : sitePath       
        }

def create_a_leaf_site(create_a_leaf_site_url, headers, token, body):
    try:
        response = requests.post(create_a_leaf_site_url, data = json.dumps(body), headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()
            print (result)
        else:
            print ("Failed to Create Leaf Site! - " + str(response.text))

    except Exception as e:
        print (str(e))
        
result = create_a_leaf_site(create_a_leaf_site_url, headers, token, body)
result
```
API Response: 

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

## Step 4: Modify Devices In Site
***4a. call add_site_device API***
>After we have created all sites, we will start to import devices into our sites via [Add Site Devices API](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/REST%20APIs%20Documentation/Site%20Management/Add%20Site%20Devices%20API.md). 
This will add devices into the site specified by site path or Id. 
All devices will be marked as manually added type.

>We will add two device lists to two individually created sites respectively. In this sub-step involves two parts;
>>1) Add 4 devices into the site with site path: "My Network/America/Burlington/Netbrain"
>>2) Add 4 devices (which are different with first part) to another site with site path: "My Network/America".

***4b. call get_site_devices API***
>In order to confirm that we have correctly added the devices to each sites, we will call [Get Site Devices API](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/REST%20APIs%20Documentation/Site%20Management/Get%20Site%20Devices%20API.md) to output the details of each site. <br>
The result will include all devices belonging to each sites specified by site name. <br>
Note that the siteID must be a leaf site ID (not root or container site). Otherwise, error will return.

***4c. call replace_site_devices API***
> In this step, we will focus on changing the device groups in a site, via [Replace Site Devices API](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/REST%20APIs%20Documentation/Site%20Management/Replace%20Site%20Devices%20API.md).
This API will remove all existing devices from the specified site and add new devices via information indicated in the device parameters.


```python
# call add_site_device API

add_site_device_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/Devices"

sitePath = "My Network/America/Burlington/Netbrain"
devices = ["AS20001", "AS20002", "AS20003", "AS30000"]

body = {
           "sitePath" : sitePath,
           "Devices": devices
        } 

def add_site_device(add_site_device_url, headers, token, body):
    headers["Token"] = token
    try:
        response = requests.post(add_site_device_url, data = json.dumps(body), headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()
            print (result)
        else:
            print ("Failed to Add Devices! - " + str(response.text))

    except Exception as e:
        print (str(e))
        
result = add_site_device(add_site_device_url, headers, token, body)
result
```
API Response: 

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    


```python
sitePath1 = "My Network/America"
devices1 = ["R1", "R10", "R11", "R10"]

body1 = {
           "sitePath" : sitePath1,
           "Devices": devices1
        } 

result1 = add_site_device(add_site_device_url, headers, token, body1)
result1
```
API Response: 

    Devices added Fail! - {"statusCode":791006,"statusDescription":"leaf site 6397dc66-429e-4e32-a5f1-0e3d3b72ba7e does not exist."}
    


```python
#call get_site_devices API

get_site_devices_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/Devices"

sitePath = "My Network/ASIA"

data = {
           "sitePath" : sitePath
            # "sitId" : sitId
     }    

def get_site_devices(get_site_devices_url, headers, token, data):
    headers["Token"] = token
    try:
        response = requests.get(get_site_devices_url, params = data, headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()["devices"]
            print (result)
        else:
            print ("Failed to Get Site Devices! - " + str(response.text))

    except Exception as e:
        print (str(e))
        
result = get_site_devices(get_site_devices_url, headers, token, data)
result
```
API Response: 

    [{'id': '71e07730-1247-4f5f-acbc-2b3428f8d0cf', 'mgmtIP': '10.18.19.18', 'hostname': 'R18'}]
    


```python
sitePath1 = "My Network/MPLS Core"

data1 = {
           "sitePath" : sitePath1
            # "sitId" : sitId
     }

result1 = get_site_devices(get_site_devices_url, headers, token, data1)
#print(str(len(result1)))
result1

```
API Response: 

    [{'id': '1d48d218-06cf-4657-af2c-39796946122b', 'mgmtIP': '123.10.1.1', 'hostname': 'R4'}, {'id': '497b25bd-1f8c-4bfa-80be-49ab692ce4d4', 'mgmtIP': '123.10.1.10', 'hostname': 'R3'}, {'id': '5c3d72d6-d0f2-41f4-8b1e-5762dff6e55a', 'mgmtIP': '123.10.1.22', 'hostname': 'R6'}, {'id': '6d62e420-af59-4ee3-948d-54df60fe05ca', 'mgmtIP': '123.10.1.6', 'hostname': 'R5'}, {'id': '81229708-571a-419a-a10d-9481661718a4', 'mgmtIP': '123.10.1.2', 'hostname': 'R1'}, {'id': 'a8652884-7701-5e84-b4d8-cc03652490e5', 'hostname': 'ISP'}, {'id': 'b98f107a-622e-4985-8f95-f5b541f699f3', 'mgmtIP': '123.7.7.7', 'hostname': 'R7'}, {'id': 'f190b385-676f-4579-ad6d-700122a21caf', 'mgmtIP': '123.10.1.17', 'hostname': 'R2'}]
    


```python
#call replace_site_devices API

replace_site_devices_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/Devices"

sitePath = "My Network/MPLS Core"

devices = ["R1", "R10", "R11", "R12"]
#devicesOri = ["R4", "R3", "R6", "R5", "R1", "ISP", "R7", "R2"]

body = {
           "sitePath" : sitePath,
           "Devices": devices
    }   

def replace_site_devices(replace_site_devices_url, headers, token, body):
    headers["Token"] = token
    try:
        response = requests.put(replace_site_devices_url, data = json.dumps(body), headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()
            print (result)
        else:
            print ("Failed to Add Devices! - " + str(response.text))

    except Exception as e:
        print (str(e))
        
result = replace_site_devices(replace_site_devices_url, headers, token, body)
result
```
API Response: 

    {'statusCode': 790200, 'statusDescription': 'Success.'}


## Step 5: Implement All Modifications To System
>To finalize our changes, we will commit the changes made in the previous steps by calling [Commit Site Transaction API](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/REST%20APIs%20Documentation/Site%20Management/Commit%20Site%20Transaction%20API.md).

>Without this API, all modifications made in previous steps are all considered pending and will be discarded. <br>
In order to update the whole structure, we must commit the site transaction. <br>
In other words, every time a user creates a site transaction to modify sites, one must end with the calling of [Commit Site Transaction API](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/REST%20APIs%20Documentation/Site%20Management/Commit%20Site%20Transaction%20API.md) to update the entire workflow.


```python
rebuildSite = False

body = {"rebuildSite" : rebuildSite}

commit_site_transactionn_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/Transactions"

def commit_site_transactionn(commit_site_transactionn_url, headers, token, rebuildSite):
    headers["Token"] = token
    try:
        response = requests.put(commit_site_transactionn_url, data = json.dumps(body), headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()
            print (result)
        else:
            print ("Failed to Commit Site Transaction! - " + str(response.text))

    except Exception as e:
        print (str(e))
        
result = commit_site_transaction(commit_site_transactionn_url, headers, token, rebuildSite)
result
```
API Response: 

    {'statusCode': 790200, 'statusDescription': 'Success.'}


## Step 6: Remove Site Transaction (Optional)
>As mentioned in Step 2, to avoid a site lockup, we will call [Remove Site Transaction API](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/REST%20APIs%20Documentation/Site%20Management/Remove%20Site%20Transaction%20API.md) to free up the site operation. <br> 
Removing the transaction also lets the user discard any site change operations since the beginning of a transaction, or called rollback.

```python
remove_site_transaction_full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/Transactions"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

def remove_site_transaction(remove_site_transaction_full_url, headers, token):
    try:
        response = requests.delete(remove_site_transaction_full_url, headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()
            print (result)
        else:
            print ("Site transaction remove Failed! - " + str(response.text))

    except Exception as e:
        print (str(e)) 

result = remove_site_transaction(remove_site_transaction_full_url, headers, token)
result
```
API Response:
```
    {'statusCode': 790200, 'statusDescription': 'Success.'}
```




## Step 7: Logout of NetBrain System
>We will finish this use case by calling [Logout API](https://github.com/NetBrainAPI/NetBrain-REST-API-R12/blob/main/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Logout%20API.md) to log out of NetBrain.

```python
logout_url = nb_url + "/ServicesAPI/API/V1/Session"

def logout(logout_url, token, headers):
    headers["token"] = token
    
    try:
        # Do the HTTP request
        response = requests.delete(logout_url, headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            js = response.json()
            return (js)
        else:
            return ("Session Logout Failed! - " + str(response.text))

    except Exception as e:
        return (str(e))

logout = logout(logout_url, token, headers)
logout
```

API Response: 


    {'statusCode': 790200, 'statusDescription': 'Success.'}

