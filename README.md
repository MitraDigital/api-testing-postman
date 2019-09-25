# api-testing-postman

### Run the script in local environment
1. Install Node.js
2. Install Newman from npm globally on the system
`$ npm install -g newman`
3. Update following configuration in test.postman_environment.json file
`consumerKey, consumerSecret, apiContext, hostUrl`
4. Execute the following command from script directory to run the test
`newman run APITest_collection.json -e Environment1.json`
5. To Avoid SSL certificate validation error in local environment use following command
 `newman run APITest_collection.json -e Environment1.json --insecure`
