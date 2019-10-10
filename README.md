# api-testing-postman

### Request Information

In order to test an API which is exposed through WSO2 AM, we have create series of API requests using postman.

In this sample, PhoneVerification 1.0.0 API has been used, which is the default sample api in wso2 am 2.10 or latest.

If you import the collection named ‘APITest_collection.json’, you can find  four following folders under the collection

- CreateApplication - Contains API calls to create a new application in API Store
- CreateNewSubscription - Contains the API calls to Subscribe the target API with created application
- APITest- contains the requests to test target API 
- RemoveSubscription - Contains the API calls to remove subscription and delete the - application that has been created for testing purpose.



#### API Test

###### AccessTokenGeneration: To bearer token to invoke target api
#
```
curl -X POST \
  https://localhost:8243/token \
  -H 'Authorization: Basic VVRHZV9KV2VZWDhaTUdUMjB0RExZZXNSVGx3YTp4TmtDZlNjeDg0bTZ5TG9ycTVSMGs4cks2OWth' \
  -H 'Content-Type: text/plain' \
  -H 'Postman-Token: d60dbedd-35a8-4874-82ff-e437a754a9b5' \
  -H 'cache-control: no-cache' \
  -d grant_type=client_credentials
```

In postman request, we can add JavaScript code to execute during 2 events in the flow:
- Before a request is sent to the server, as a pre-request script under the Pre-request Script tab. Eg: Generating Base64 encoded basic token can be performed in Pre-request Script.
- After a response is received, as a test script under the Tests tab.

Sample Pre Request Script for token Generation as follows.

In above script after the encoding, encoded values is stored in environment variable named apiBasicToken as following, and It can be reused in request header as {{apiBasicToken}}.
postman.setEnvironmentVariable("apiBasicToken",base64) // environment variable need to be declared before accessing
Sample Test Script for Token Generation 
Using the JavaScript we can extract response and implement validations

###### CallPhoneVerificationAPI: To call target API
#
```
curl -X GET \
  'https://localhost:8243/phoneverify/1.0.0/CheckPhoneNumber?PhoneNumber=0774589632&LicenseKey=0' \
  -H 'Authorization: Bearer' \
  -H 'Postman-Token: d2984112-b7f9-42a2-bc7f-f86e68abae23' \
  -H 'cache-control: no-cache'
```

Under the APITest folder any no of request can be added for testing. and also simple test scripts also can be added to validate response.

### Execute the script

By importing collection file into postman, custom API requests  can be added under APITest folder.
In order to execute the script, navigate to the scrip folder and execute following command 
```
$ newman run src/test/APITest_collection.json -e config/Environment1.json
```
To avoid the SSL certificate validation error in local environment execute following command
```
$ newman run src/test/APITest_collection.json -e config/Environment1.json --insecure
```

#### Jenkins Setup with Newman Report

Inorder to execute postman testcases with report geneartion following pacgae need to be globally installed in jenkins server.

```
$ npm install -g newman-reporter-htmlextra
```

Configure jenkins build type as execute shell and give following command

```
newman run src/test/APITest_collection.json -e config/Environment1.json --insecure -r htmlextra --reporter-htmlextra-export report/build_Report.html
```

Build the project and find the report in following folder

```
workspace/report/build_report.html
```

if Report page is not in well format then execte following comand in jenkins script console to allow html/css plugins

```
System.setProperty( "hudson.model.DirectoryBrowserSupport.CSP" , "" )
```
