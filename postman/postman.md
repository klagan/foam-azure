# Postman

## Sample AAD pre-request script

> **Note**: there are expected environment variables for this script to work

```json
prerequisite script
const echoPostRequest = {
  url: 'https://login.microsoftonline.com/'+pm.environment.get('tenantId')+'/oauth2/v2.0/token',
  method: 'POST',
  header: 'Content-Type: application/x-www-form-urlencoded',
      body: {
          mode: 'urlencoded',
          urlencoded: [
            {key: "grant_type", value: "password", disabled: false},
            {key: "username", value: pm.environment.get('userName'), disabled: false},
            {key: "password", value: pm.environment.get('password'), disabled: false},
            {key: "client_Id", value: pm.environment.get('clientId'), disabled: false},
            {key: "clientSecret", value: pm.environment.get('clientSecret'), disabled: false},
            {key: "scope", value: 'openid offline_access '+ pm.environment.get('apiId') + '/.default', disabled: false}
        ]
      }
};

console.log(pm.environment.get('userName'))
console.log(pm.environment.get('currentAccessToken'))

var getToken = true;

//if (!pm.environment.get('accessTokenExpiry') || 
//    !pm.collectionVariables.get('currentAccessToken')) {
//    console.log('Token or expiry date are missing')
//} else if (pm.environment.get('accessTokenExpiry') <= (new Date()).getTime()) {
//    console.log('Token is expired')
//} else {
//    getToken = false;
//    console.log('Token and expiry date are all good');
//}

if (getToken === true) {
    pm.sendRequest(echoPostRequest, function (err, res) {
    console.log(err ? err : res.json());
        if (err === null) {
            pm.test("Token request", function () {
                pm.expect(res.code).to.not.be.oneOf([400,500]);
});
            console.log('Saving the token and expiry date')
            var responseJson = res.json();
            pm.collectionVariables.set('currentAccessToken', responseJson.access_token)
            console.log(responseJson.access_token)
    
            var expiryDate = new Date();
            expiryDate.setSeconds(expiryDate.getSeconds() + responseJson.expires_in);
            pm.environment.set('accessTokenExpiry', expiryDate.getTime());
        }
    });
}

```

[[dotnet.md]]