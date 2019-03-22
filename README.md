# postman_collection
Postman_automation_collection can be used to test the basic functionality of the all the backend API's.

## Description
This collection is made by placing all the API's in a sequence using postman aiming the maximum test coverage. Here, we write our test cases by using environment_variables and the test snippets provided by postman. After that we export our api-collection and environment to the local or server having the newman installed, we will be able to run the collection and this could also be integrated with Jenins to run the script after every deployment.

## Writing test cases
### 1. Set environvent_variable for passing the fields in next API's by getting it from response_body.
```
var jsonData = JSON.parse(responseBody);
pm.environment.set("token", jsonData.data.token);
```
### 2. Validate Status_code.
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```
### 3. Validate schema of response.
```
var schema = {
    "required": [
        "status",
        "msg",
        "code",
        "data"
    ],
    "properties": {
        "status": {
            "type": "boolean"
        },
        "msg": {
            "type": "string"
        },
        "code": {
            "type": "number"
        },
        "data": {
            "required": [
                "token",
                "email",
                "account_id",
                "profile_type",
                "profile_id",
                "forum_token"
            ],
            "properties": {
                "token": {
                    "type": "string"
                },
                "email": {
                    "type": "string"
                },
                "account_id": {
                    "type": "string"
                },
                "profile_type": {
                    "type": "number"
                },
                "profile_id": {
                    "type": "string"
                },
                "forum_token": {
                    "type": "string"
                }
            },
            "type": "object"
        }
    }
};

pm.test('Schema Validation', function() {
    var result=tv4.validateResult(jsonData, schema);
 
    if(!result.valid){
        console.log(result);
    }
 
    pm.expect(result.valid).to.be.true;
});
```
### 4. Validate if expected data is seen in the response.
```
var user_id = pm.variables.get("user_id");

pm.test("event for given user_id", function () {
    pm.expect(jsonData.data.events[0].user_id).to.deep.equal(user_id);
});
```

## Steps to run postman collection from command line,
1. Firstly install 'node.js'.
	Command -	```brew install node```

2. Then install 'newman'
	Command -	```npm install newman```

3. Export the postman collection and the environments from postman.
	
4. In terminal, goto the saved location

5. Run Collection on desired environment.
	Command - ```newman run (api_collection.json) -e (test/stage_env.json)```
	
## Visuals
<img width="814" alt="1" src="https://user-images.githubusercontent.com/43333698/54807263-b5cc7880-4ca2-11e9-92d5-b2ac7ff14050.png">
<img width="808" alt="2" src="https://user-images.githubusercontent.com/43333698/54807338-ef9d7f00-4ca2-11e9-84ca-2effb5384293.png">
<img width="802" alt="3" src="https://user-images.githubusercontent.com/43333698/54807353-ff1cc800-4ca2-11e9-9c98-d60a903f52f4.png">
<img width="814" alt="4" src="https://user-images.githubusercontent.com/43333698/54807381-0cd24d80-4ca3-11e9-89cb-e3f9be135cf5.png">

## Test Coverage
1. We are making all the api’s run in sequence to make sure, positive flow is working.
2. We have covered almost all the API’s performing CRUD operations in all the Modules except Cards and Signup.
3. This flow also checks for creating users, making location updates and events.
4. For each API - Status code, response message, required fields and their data type is being validated.

QA team will keep this updated every once in a month after the release by adding the new API's.
