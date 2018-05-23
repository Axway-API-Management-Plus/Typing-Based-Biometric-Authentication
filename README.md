# Typing Based Biometric Authentication
A sample API and test suite to test out TypingDNA's REST APIs for typing based biometric authentication.

## Descrpition
The TypingDNA (https://api.typingdna.com) team has a very neat suite that supports 2FA (two factor authentication) by enabling biometric authentication based on keyboard typing patterns. Included in this are tools such as:

- A sample biometric typing pattern "fingerprint" generator: https://www.typingdna.com/docs/generate-typing-patterns-tool.html
- Some great documentation for the REST API: https://api.typingdna.com/index.html
- A sample code repo: https://github.com/TypingDNA

Using these resources, we were able to register their APIs in the Axway API Manager, create a front end Swagger definition document, and configure a test repository in POSTMAN. These artifacts, along with the required certificates, can be found in the GitHub repo source directory: https://github.com/Axway-API-Management-Plus/Typing-Based-Biometric-Authentication/tree/master/src

## Value
How might one use this in their organization? Often the biggest challenge with implementing new factors of authentication is access to the hardware and middleware, and installation on all devices. The TypingDNA method allows for biometric authentication with your standard keyboard, orchestrated via REST APIs rather than middleware.

Taking this, the Axway API Manager can register the APIs to allow for ease of discover, implementation into new APIs/application, use as additional fine grain authorization as an abstraction layer in front of pre-existing resources, and more. The Axway API Management Suite allows you to remove the requirement to give developers your enterprise API Secret keys or manage their own, by virtualizing access to the TypingDNA REST APIs, and allowing a different method of authentication for consumption through your Axway API Portal. Finally, by registering the APIs in Axway API Manager, you get a Swagger 2.0 definitation document created for easy consumption by developer applications and to support API-first strategies.

## Deploy the API

The first step would be to go to TypingDNA's site (https://www.typingdna.com/) and click the "Try API for Free" button to get your own API developer keys, or just go direct to their signup page (https://www.typingdna.com/clients/signup).

Once you have your key, you can read their documentation, and then proceed to import their API definition via the Axway API Management created JSON artifact. To do this, log into API Manager and choose "API --> BE API --> New API --> Import Swagger API". Choose the /src/TypingDNA_BEAPI.json swagger definition included, or get it via URL: https://github.com/Axway-API-Management-Plus/Typing-Based-Biometric-Authentication/blob/master/src/TypingDNA_BEAPI.json

From here, look at your newly imported API and change the name as you see fit. Once finished and saved, choose "API --> FE API --> New API --> New from Back End" and select the TypingDNA API. Once this loads into the new FE API screen there are a few changes you will need to make.

1. Set the resource path on the Inbound tab. For this, I used '/typingdna'. If you are using the sample Postman repoistory, you may want to use this as well, as it is set up against this URI, or you can simply change the URL in the sample requests.
2. You will need to set up Inbound Security on the same tab. For this, I simply used Passthrough for my initial testing to ensure credentials did not need managed in the Postman samples, but if you wish, you can configure security of the virtualized API here.
3. On the outbound tab, you need to specify your API Key details. The way the TypingDNA API is configured, it takes your API Key and Secret in as an Authorization Basic header. So here you will actually select HTTP Basic as your outbound authentication profile, and specify your key and secret as username and password.
4. The Trusted Certificates tab automatically populates the trusted certificate and issuer chain based on the remote domain, however, there seems to be some redirection when testing this. This requires trust of an additional certificate and chain for an Azure web service. Add the three included certificates from the /src file to the trusted certificates tab.
5. Save and publish the API.


## Test the API

At this point you should have a working API. If you go back to the FE API navigation, click on the TypingDNA API and check the API Methods, you should see endpoints similar in nature to this: https://api-env.demo.axway.com:8065/typingdna/user/{id}

There are a number of ways to test this, but for these purposes, you might want to start with the Postman collection included. To do so, open up Postman, click Import, and pull in the TypingDNA-PostmanCollection.json file from the /src directory. This should give you samples for the services we have defined, and an additonal one we did not as well. From here we can register a user by typing pattern, check the user was registered, and then authenticate them with the following steps.

1. For simplicity sake, rather than developing a new FE for this, we can use the TypingDNA test tool. Navigate to https://www.typingdna.com/docs/generate-typing-patterns-tool.html, enter some text, and click "Get Typing Patterns". For my testing, I used: "The quick brown fox jumps over the lazy dog." An oldie, but goodie.
2. Grab the "Sametext Pattern" value and hop over to Postman. We will now use the 'Save Typing Pattern' POST method to create a user entry. In the URL, replace ":id" with a user ID, such as "testuser1". On the Body tab, paste the typing pattern you copied into the "tp" key value field to populate the form data. Note: Since we have saved our API keys in API Manager, no authentication information needs to be supplied at this time, meaning end users/developers do not need your API Key! Additional security can be added in the FE API via the Inbound Security dialog. Click Send, and you should get back a 200 Ok.
```
{
    "message": "Done",
    "success": 1,
    "status": 200
}
```
3. After this completes, try the Check User GET method by replacing the ":id" with your user ID. You should get back a 200 with a "count" of "1", as shown here:
```
{
    "message": "Done",
    "success": 1,
    "count": 1,
    "mobilecount": 0,
    "type": "All",
    "status": 200
}
```
4. The next step is to verify the users biometric match. Hop back over to the typing pattern generation tool from step one, and get a new pattern by typing the same phrase. I like do to a hard refresh (shift + F5) on the page to wipe the old pattern first to ensure a fresh one is generated. 
5. Once more in Postman, use the Verify Typing Pattern POST request. Replace the ":id" in the URI with the user you wish to verify. In the Body tab, replace the "tp" value with the new typing pattern. You may modify the quality parameter as well if you wish. When you click send, you should get back a verificatio with a score out of 100 to denote the level of assurance, as seen here:
```
{
    "message": "Done",
    "success": 1,
    "result": 1,
    "score": 100,
    "confidence_interval": 9,
    "status": 200
}
```

### Next Steps

At this point, hopefully you have a working API. This could be integrated into your user enrollment process, with new Axway API Management Inbound Security policies being created to incorporate this functionality. It could be part of general login processes or additonal verification for secure operations. 

From an application development stand point, why not talk to Axway about Axway Appcelerator Titanium and how the SDK could ehance your mobile application security as well?

Feel free to reach out to Axway for more information on how this could be used to enhance your Identity and Access Managmenet (IdAM) initiatives within your enterprise!
