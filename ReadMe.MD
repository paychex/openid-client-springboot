# Background

Paychex can serve as an Identity Provider (IDP) for your application using Paychex OpenID credentials.
The Paychex OpenID offering is compliant with [RFC 6749](https://tools.ietf.org/html/rfc6749) and
[OpenID Specs](https://openid.net/developers/specs/).

The details can be obtained at the Discovery endpoint.

`https://oidc.paychex.com/.well-known/openid-configuration`

# Pre requisites
1. Paychex Issued Oauth Client ID and Secret
1. Java 1.8+

# Build
Clone the repository and build using gradle.

`./gradlew clean build --refresh-dependencies` 

# Run
1.Update the client ID and Secret in the appliation.yml file

```yaml
        paychex:
            provider: paychex
            client-id: <Your Client ID>
            client-secret: <Your Client Secret>
```

2. Start the application 

    * Using gradle spring boot plugin
    
        `./gradlew bootRun`
        
    * Using the jar
    
        `java -jar /build/libs/openid-sample-springboot-1.0-SNAPSHOT.jar`


3. Once the application has started, you can use the OIDC flow on 

    `http://localhost:8080`
    
    You should be able to access the userinfo response
    
    ```json
    {
        "claims": {
            "sub": "<Paychex UserID>",
            "birthdate": "1970-01-01",
            "profile": "John Doe",
            "preferred_username": "john_doe",
            "given_name": "John",
            "locale": "US",
            "family_name": "Doe",
            "email": "john.doe@acme.com"
        },
        "address": {
            "formatted": null,
            "streetAddress": null,
            "locality": null,
            "region": null,
            "postalCode": null,
            "country": null
        },
        "zoneInfo": null,
        "givenName": "John",
        "fullName": null,
        "email": "john.doe@acme.com",
        "nickName": null,
        "preferredUsername": "john_doe",
        "familyName": "Doe",
        "middleName": null,
        "profile": "John Doe",
        "picture": null,
        "website": null,
        "emailVerified": null,
        "gender": null,
        "birthdate": "1970-01-01",
        "phoneNumber": null,
        "phoneNumberVerified": null,
        "updatedAt": null,
        "locale": "US",
        "subject": "<Paychex UserID>"
    }
    ```
   
# Notes

* You can override the redirect URI by specifying your own redirectURI
    ```yaml
                scope:
              - openid
              - email
              - profile
            redirect-uri: <Custom Redirect URI>
    ```
    The default from spring is
     
     `{baseUrl}/login/oauth2/code/{registrationId} `
    
    which translates to
     
     `http://localhost:8080/login/oauth2/code/paychex`

* The scope list configured is as required by the application, this can be modified to the list of scopes the application would like to access but, is always a subset of the client scopes.