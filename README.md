# api-testing-postman

### Run the script in local environment
1. Install Node.js
2. Install Newman from npm globally on the system
`$ npm install -g newman`
3. Update following configuration in test.postman_environment.json file
`consumerKey, consumerSecret, apiContext, hostUrl`
4. Execute the following command from script directory to run the test
`newman run TokenAPITest.postman_collection.json -e test.postman_environment.json`

