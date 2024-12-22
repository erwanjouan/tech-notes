---
title: "Oauth 2.0 and OpenID Connect"
date: 2020-08-17T14:44:39+02:00
draft: false
---

![type:video](https://www.youtube.com/embed/996OiexHze0)

### Introduction

- Homegrown solutions for web authentication are not ideal because of :
  - Security
  - Maintenance

- There is a lot of confusion around OAuth :
  - Terminology / Jargon
  - Incorrect advice
      - Online resources may contradict itself

From the list of pre-2010 solutions, SAML is still valid. It allows a multi-system platform to plug its authentication mechanism to a single directory service (most of the time Active Directory in Corporate world).

**Delegated Authorization** is a concept originated from these 2010 years that is the root of Oauths concepts. It is implemented in websites that allows user to connect with Google/Facebook account.

First implementations of this mechanism were poorly executed. 

E.g. YELP inscription requiring *real* Gmail User/Password, to grab all contacts and send emails to them.

A better way to do this would be the OAuth flow:

```mermaid
graph LR;
    A[<b>yelp.com</b><br/>Connect With Google] --> B(accounts.google.com<br/>user<br/>password)
    B --> C(accounts.google.com<br/>Allow Yelp to see contacts?)
    C --> D(<b>yelp.com/callback</b><br/>getting contacts<br/>by calling API<br/>accounts.google.com)
```

In this scenario, password is only provided to Google and never to Yelp.

Note: OAuth 2.0 is widely used on Internet whereas OAuth 1.0 is now deprecated.

### Oauth Terminology

- Resource Owner: You and me. I have some contacts, I am the owner of this resource data. I am the one that clicks on "Yes, I allow this application to have access to these resources."
- Client: A way to refer to the application that wants to access to resources (Yelp in this example)
- Authorization server: System that handles permissions to resource (accounts.google.com)
- Resource server: API that holds the data that the client wants to get to (google contacts API)
- Authorization grant: The thing that proves that the user has clicked YES I approve.
- Redirect URI: Aka Callback where should I end up at the end of the flow.
- Access token: The key to let the client do what he should do.

```mermaid
sequenceDiagram
    note over client: yelp.com<br/>Redirect URI:<br/>yelp.com/callback<br/>Response type:<br/>code<br/> (Authorization code)
    client->>Authorization_Server: Go to Authorization Server
    note over Authorization_Server: accounts.google.com<br/>User:<br/>Password:
    Authorization_Server->>client: Back to client with authorization code (200 or 403)
    client->>Authorization_Server: Exchange authorization code for access token
    Authorization_Server->>client: Here is your access token
    note over Resource_Server: contacts.google.com
    client->>Resource_Server: Talk with Access Token
```

- Scope: The Authorization Server has a list of scope that it understands
    - contacts.read
    - contacts.delete
    - email.read
    ...
    Client is suppose to present to Resource Owner the list of scopes that will be used.
- Consent: Explicit clearance from the Resource owner



Question : Why do we need to get a code instead of retrieving the token directly ? 

- More Terminology:
  - Front channel (less secure channel)
      - Browser related, which code security cannot be 100% guaranteed
      - Used for code exchange
  - Back channel (highly secure channel)
      - E2E TLS from backend to API
      - Used for token exchange
      - Backends related

OAuth Authorization Code Workflow Expliciting Scopes

```mermaid
sequenceDiagram
    alt Front Channel (browser)
        note over client: yelp.com<br/>Redirect URI:<br/>yelp.com/callback<br/>Response type:<br/>code<br/> (Authorization code)<br/>Scope:<br/>profile.contacts
        client->>Authorization_Server: Go to Authorization Server
        note over Authorization_Server: accounts.google.com<br/>User:<br/>Password:
        Authorization_Server->>client: Back to client with authorization code (200 or 403)
    end
    alt Back Channel (backends)
        client->>Authorization_Server: Exchange authorization code for access token
        Authorization_Server->>client: Here is your access token
        note over Resource_Server: contacts.google.com
    end
    client->>Resource_Server: Talk with Access Token scoped with profile.contacts
```


OAuth 2.0 authorization code flow (before token/code exchange) is passed through the **front** channel. It consist in redirects which are in HTTP query parameters.
A secret key is also provided by Authorization provider along with http code. A man-in the middle attack cannot have access to this secret key, hence cannot trigger the token echange, which happens on the back channel.
Access token is never used by browser, it is exchanged from client backend to Authorization server.

Starting the flow:

```
https://accounts.google.com/o/oauth2/v2/auth?
    client_id=815946762465-612h4f8eeoio8c24cbkaafcjb5bbjcpo.apps.googleusercontent.com&
    redirect_uri=https://oauthdebugger.com/debug&
    scope=profile&
    response_type=code&
    state=foobar
```

- When authenticating with Google, the following elements are provided :
  - ClientID (Front Channel)
  - CLientSecret (Back Channel during token exchange)
  Which enables authentication on Authorization Server

[OAuth 2.0 Debugger](https://oauthdebugger.com/)

Authorize URI (required):
https://accounts.google.com/o/oauth2/v2/auth
Redirect URI (required):
https://oauthdebugger.com/debug
ClientID:
815946762465-612h4f8eeoio8c24cbkaafcjb5bbjcpo.apps.googleusercontent.com
Scope:
Profile

The OAuth Debugger only enables the Front Channel exchange, next token exchange is required on Back Channel, using the secret key.

Error code sent in callback in case of wrong parameters:

```
https://yelp.com/callback?
    error=access_denied&
    error_description=User did not consent
```
```
https://yelp.com/callback?
    code=sdfdsfsdfzeFZFZEDFCsdcsdc&
    state=foobar
```

In case everything went ok, an exchange of back code to access token is triggered towards :
```
POST www.googleapis.com/oauth2/v4/token
    Content-Type: application/x-www-form-url-encoded
    code=DFDfsdfdsdfsdfzefezggnghj&
    client_id=815946762465-612h4f8eeoio8c24cbkaafcjb5bbjcpo.apps.googleusercontent.com&
    client_secret=secret123&
    grant_type=authorization_code
```

Finally, the resource server is requested via API call with Authorization header:
```
GET api.google.com/some/endpoint
Authorization: Bearer zaeazerzertertrdsfsderfgercvbgjhjkhujghfgjuykyukuyyuttyj
```

There are several patterns depending on systems architecture and the availability of a backend channel:
(e.g React, Angular SPA with no backend)

|Pattern Name|Front Channel|Back Channel|Example|
|---|---|---|---|
|Authorization Code|X|X|WebApp with server backend, Native Mobile App|
|Implicit|X||Javascript SPA|
|Resource owner password credentials||X||
|Client credentials||X|Microservices and APIs|

#### OAuth Implicit Flow

Token is exchanged on Front Channel (less secure because token is exposed to browser code)

```mermaid
sequenceDiagram
    alt Front Channel (browser)
        note over client: yelp.com<br/>Redirect URI:<br/>yelp.com/callback<br/>Response type:<br/>token<br/>Scope:<br/>profile contacts
        client->>Authorization_Server: Go to Authorization Server
        note over Authorization_Server: accounts.google.com<br/>User:<br/>Password:
        Authorization_Server->>client: Back to redirect URI with token
    end
```

### OpenID Connect

#### Problems with OAuth 2.0 for authentication

Near 2014, OAuth 2.0 was used for a lot of identity use cases it was not designed for:

|Use Case|Function|Protocol|
|---|---|---|
|Simple login|Authentication|OAuth 2.0|
|Simple sign-on across sites|Authentication|OAuth 2.0|
|Mobile app login|Authentication|OAuth 2.0|
|Delegated authorization|Authorization|OAuth 2.0|

- But OAuth 2.0 was primarly designed for 
  - authorization (provides a scoped temporary token, permissions)
  - and not authentication (user information is not revealed to client at all)

You should not use OAuth for authentication because there is no standard way to access user's information.

Facebook, Microsoft implemented hacks to get user information, not interoperable, which is bad.

A need for a standardized layer on top of OAuth was clearly needed to deal with authentication:
- **OpenID Connect**

#### OpenID Connect

- OpenID Connect is an extension to OAuth 2.0 adds :
  - ID Token (user's information)
  - UserInfo endpoint for getting more user info
  - Standard set of scopes
  - Standardized implementation

OpenID Connect authorization code flow

The client initiates a request to the Authorization Server with scope: openid 

```
Redirect URL: yelp.com/callback
Response type: code
scope: openid profile
```
At the end of the flow, oclient can exchange its code for:
- access token
- ID token
more info can be obtained on /userinfo endpoint

As it is a plain JWT token, ID token can be decoded on [JSON Web Token website](https://www.jsonwebtoken.io).

#### Anatomy of JWT ID token

Header
.
Payload (claims)
.
Signature

Signature ensures that the token has not been modified on flight.


### Identity uses cases today

|Use Case|Function|Protocol|
|---|---|---|
|Simple login|Authentication|OpenID Connect|
|Simple sign-on across sites|Authentication|OpenID Connect|
|Mobile app login|Authentication|OpenID Connect|
|Delegated authorization|Authorization|OAuth 2.0|

