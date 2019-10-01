# api-testing-postman

### Request Information

In order to test an API which is exposed through WSO2 AM, we have create series of API requests using postman.

In this sample, PhoneVerification 1.0.0 API has been used, which is the default sample api in wso2 am 2.10 or latest.

If you import the collection named ‘APITest_collection.json’, you can find  four following folders under the collection

- CreateApplication - Contains API calls to create a new application in API Store
- CreateNewSubscription - Contains the API calls to Subscribe the target API with created application
- APITest- contains the requests to test target API 
- RemoveSubscription - Contains the API calls to remove subscription and delete the - application that has been created for testing purpose.

##### Create Application

###### >  CallingDynamicClientRegStoreEndpoint: To get the client credential to get access token in order to call  restful service of wso2 store
#
```
curl -X POST \
  https://localhost:9443/client-registration/v0.11/register \
  -H 'Authorization: Basic  ZzhHWF83anFYUmJ0eUlUSEVxNXB3bE1MWXVRYTpGQVRydUNlSkxmTTBTMFNkdjNNTkNJTmowRFFh' \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: 97b6b833-56ac-4d8e-91ea-98a7509653ef' \
  -H 'cache-control: no-cache' \
  -d '{
    "callbackUrl": "www.google.lk",
    "clientName": "rest_api_store",
    "owner": "admin",
    "grantType": "password refresh_token",
    "saasApp": true
}'
```
###### > CallingDynamicClientStoreGetAccessToken: To get the access token with scope of apim:subscribe
#
```
curl -X POST \
  https://localhost:8243/token \
  -H 'Authorization: Basic ZzhHWF83anFYUmJ0eUlUSEVxNXB3bE1MWXVRYTpGQVRydUNlSkxmTTBTMFNkdjNNTkNJTmowRFFh' \
  -H 'Postman-Token: 2fab643e-7398-4d53-a7b7-09970e83d6b9' \
  -H 'cache-control: no-cache' \
  -d 'grant_type=password&username=admin&password=admin&scope=apim%3Asubscribe'
```
###### > GetApplicationID : To Verify that application with given name is not exist 
#
```
curl -X GET \
  'https://localhost:9443/api/am/store/v0.11/applications?query=apiTesApp' \
  -H 'Authorization: Bearer e85f57a6-564d-39cb-82a0-ead73d0c6657' \
  -H 'Postman-Token: 24ac8741-cfac-4d07-a6ca-fad3e2490e95' \
  -H 'cache-control: no-cache'
```

###### > CreateNewApplication: To Create a test application with given name
#
```
curl -X POST \
  https://localhost:9443/api/am/store/v0.11/applications \
  -H 'Authorization: Bearer e85f57a6-564d-39cb-82a0-ead73d0c6657' \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: 9e547879-ee9e-4eed-bbfd-c5658f81a87f' \
  -H 'cache-control: no-cache' \
  -d '{
    "throttlingTier": "Unlimited",
    "description": "sample app description",
    "name": "apiTesApp"
}'
```
###### > GenerateApplicationKey: To generate the key for test application if it doesn’t has.
#
```
curl -X POST \
  'https://localhost:9443/api/am/store/v0.11/applications/generate-keys?applicationId=dc1730fd-17ce-468a-894b-cb56bbcd6c54' \
  -H 'Authorization: Bearer e85f57a6-564d-39cb-82a0-ead73d0c6657' \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: 0c161f5d-6f37-42e6-acbe-69cb7aacbcb7' \
  -H 'cache-control: no-cache' \
  -d '{
 ```
#### Create New Subscription

###### > CallingDynamicClientRegPublisherEndpoint: To get the client credential to generate access token of publisher restful service
#
```
curl -X POST \
  https://localhost:9443/client-registration/v0.11/register \
  -H 'Authorization: Basic aXd5NERBSUxwZ0MxMHVNV3JvSnNDYjBfM0dNYTphQWlJajlrcWI2azFZZXo2cDcwZDY2X01GT3Nh' \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: fd94d5c2-99c5-4b8c-8765-38448dbb3e47' \
  -H 'cache-control: no-cache' \
  -d '{
    "callbackUrl": "www.google.lk",
    "clientName": "rest_api_publisher",
    "owner": "admin",
    "grantType": "password refresh_token",
    "saasApp": true
}'
```
###### CallingDynamicClientPublisherGetAccessToken: To get access token to invoke publisher restful service
#
```
curl -X POST \
  https://localhost:8243/token \
  -H 'Authorization: Basic aXd5NERBSUxwZ0MxMHVNV3JvSnNDYjBfM0dNYTphQWlJajlrcWI2azFZZXo2cDcwZDY2X01GT3Nh' \
  -H 'Postman-Token: 1b333e4b-6de6-4ad8-a7d1-be89dd0d2789' \
  -H 'cache-control: no-cache' \
  -d 'grant_type=password&username=admin&password=admin&scope=apim%3Aapi_view'
```
###### GetApiIDFromPublisher: To get the ID of API that need to be tested
#
```
curl -X GET \
  'https://localhost:9443/api/am/publisher/v0.11/apis?query=PhoneVerification' \
  -H 'Authorization: Bearer d15af3c4-a38d-321c-9e72-40cca8949d27' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -H 'Postman-Token: e4c3240e-bb00-4cb2-9796-3619eecb9c91' \
  -H 'cache-control: no-cache'
```
###### > AddNewSubscription: To subscribe the API
#
```
curl -X POST \
  https://localhost:9443/api/am/store/v0.11/subscriptions \
  -H 'Authorization: Bearer e85f57a6-564d-39cb-82a0-ead73d0c6657' \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: 10ed1562-ca9c-4a6f-9578-fa57a76c76e8' \
  -H 'cache-control: no-cache' \
  -d '{
    "tier": "Gold",
    "apiIdentifier": "ec85b6c7-25cd-47d2-9c87-d7a4f3f06aa9",
    "applicationId": "dc1730fd-17ce-468a-894b-cb56bbcd6c54"
}'
```

#### API Test

###### > AccessTokenGeneration: To bearer token to invoke target api
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

###### > CallPhoneVerificationAPI: To call target API
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
$ newman run APITest_collection.json -e Environment1.json
```
To avoid the SSL certificate validation error in local environment execute following command
```
$ newman run APITest_collection.json -e Environment1.json --insecure
```
