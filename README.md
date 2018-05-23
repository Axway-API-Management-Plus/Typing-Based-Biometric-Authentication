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


New API --> Import Swagger API. Choose the swagger definition included in the /src file, or get it via URL: https://github.com/Axway-API-Management-Plus/Typing-Based-Biometric-Authentication/blob/master/src/TypingDNA_swagger.json

