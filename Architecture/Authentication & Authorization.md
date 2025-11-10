# Authentication & Authorization: Security Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Single Sign-On (SSO)](#single-sign-on-sso)
3. [Authentication Protocols](#authentication-protocols)
4. [OAuth 2.0](#oauth-20)
5. [OpenID Connect (OIDC)](#openid-connect-oidc)
6. [SAML](#saml)
7. [HTTP Authentication Schemes](#http-authentication-schemes)
8. [Identity Providers](#identity-providers)
9. [CORS and Authorization](#cors-and-authorization)
10. [Resources](#resources)

---

## Introduction

Authentication and authorization are fundamental security concepts in software development. **Authentication** verifies who a user is, while **authorization** determines what they're allowed to do.

**Key Concepts:**
- **Authentication**: "Who are you?" - Verifying user identity
- **Authorization**: "What can you do?" - Determining user permissions
- **Identity Provider (IdP)**: System that manages user identities
- **Service Provider (SP)**: Application that relies on IdP for authentication

---

## Single Sign-On (SSO)

### What is SSO?

**Single Sign-On (SSO)** is a convenient authentication method that allows users to access multiple applications and systems using just one login.

**From a Developer's Perspective:**
SSO is when an application (service provider) relies on a third-party trusted application, known as an identity provider, to authenticate users.

**From a User's Perspective:**
SSO is when a user logs into an app once and is then automatically logged in to all the other apps that make use of the same SSO authentication mechanism.

**Key Requirement:**
An SSO mechanism is called single sign-on only if, once you're logged in, you can access all company-approved applications and websites without having to log in again.

### SSO vs Same Sign-On

**Single Sign-On:**
- One login provides access to all applications
- Authentication token is passed seamlessly between applications
- User doesn't need to re-authenticate

**Same Sign-On (Credential Vaulting):**
- Works more as a credential vaulting or password manager mechanism
- You may have and use the same credentials
- But they must be provided each time you access a different application
- Not true SSO, but uses shared credentials

### SSO Solutions

| Solution      | Website                                  | Description                                         |
| ------------- | ---------------------------------------- | --------------------------------------------------- |
| **authentik** | [goauthentik.io](https://goauthentik.io) | Open-source identity provider                       |
| **Keycloak**  | -                                        | Open-source identity and access management          |
| **Authelia**  | [authelia.com](https://www.authelia.com) | Open-source authentication and authorization server |

### SSO Implementation Methods

SSO can be implemented via different protocols and methods:

1. **Enterprise Single Sign-On (ESSO)**
   - Corporate SSO solutions
   - Typically uses directory services

2. **Web Single Sign-On (WebSSO)**
   - Browser-based SSO
   - Common for web applications

3. **Federated Identity**
   - Cross-domain identity federation
   - Allows sharing identities across organizations

4. **OpenID Connect (OIDC)**
   - Modern authentication protocol
   - Built on OAuth 2.0

5. **OAuth 2.0**
   - Authorization framework
   - Can be used for SSO

6. **Kerberos-Based SSO**
   - Network authentication protocol
   - Common in enterprise environments

7. **Smart-card Authentication**
   - Physical token-based authentication
   - High security environments

8. **Security Assertion Markup Language (SAML)**
   - XML-based authentication protocol
   - Enterprise SSO standard

### Directory Server Authentication

**Directory Server Authentication (Same-Sign-On):**
- Systems requiring authentication for each application
- Uses the same credentials from a directory server
- User must authenticate for each application separately

**Single Sign-On:**
- Single authentication provides access to multiple applications
- Authentication token is passed seamlessly to configured applications
- User authenticates once

---

## Authentication Protocols

### LDAP (Lightweight Directory Access Protocol)

**Definition:**
LDAP is a protocol that helps users find data about organizations, persons, and more.

**Main Goals:**
1. **Store data** in the LDAP directory
2. **Authenticate users** to access the directory

**Purpose:**
LDAP is the protocol or communication process that enables users to access a network resource through a directory service.

**Use Cases:**
- Corporate directory services
- User authentication
- Organizational data lookup

### SAML (Security Assertion Markup Language)

**Definition:**
SAML is a standard authentication (and occasionally authorization) protocol which is most often used to simplify access to web applications via single sign-on (SSO).

**Key Characteristics:**
- **Umbrella Standard**: Covers federation, identity management, and single sign-on (SSO)
- **Browser-Based**: Activates Single Sign-On (SSO) for browser-based applications
- **XML-Based Format**: Uses XML for authentication and authorization processes

**Implementation Note:**
There are multiple ways to implement SSO. One of the most popular is through an XML-based open standard called Security Assertion Markup Language (SAML). The SAML specification is flexible and has a number of options to cover a range of possible cases. No two vendors implement the spec the same way. In other words, it's not implemented consistently and there are several "flavors" of SAML. As a result, it's rife with opportunities for security issues.

**Components:**
- Identity Provider (IdP)
- Service Provider (SP)
- SAML assertions (XML documents)

---

## OAuth 2.0

### What is OAuth 2.0?

**OAuth 2.0 (Open Authorization)** is an open standard for token-based authentication and authorization which is used to provide single sign-on (SSO).

**Key Purpose:**
Allows an end user's account information to be used by third-party services, such as Facebook, without exposing the user's password.

**In Russian:**
OAuth 2.0 - это протокол авторизации, позволяющий выдать одному сервису (приложению) права на доступ к ресурсам пользователя на другом сервисе. Протокол избавляет от необходимости доверять приложению логин и пароль, а также позволяет выдавать ограниченный набор прав, а не все сразу.

**Authorization vs Authentication:**
OAuth 2.0 является протоколом авторизации, то есть позволяет выдать права на действия, которые приложение сможет производить от лица пользователя. При этом пользователь после авторизации может вообще не участвовать в процессе выполнения действий.

**Example:**
If you're logged into Google and used those credentials for Hootsuite, you've used OAuth.

### OAuth 2.0 Flow

**Token Management:**

1. **Login Response:**
   - When you log in, the server sends 2 tokens in response to the client:
     - **Access Token**: Short expiry time
     - **Refresh Token**: Long expiry time

2. **Client Storage:**
   - The client (Front end) stores:
     - **Refresh token** in local storage
     - **Access token** in cookies

3. **API Calls:**
   - The client uses an access token for calling APIs
   - When the access token expires, the client:
     - Picks the refresh token from local storage
     - Calls auth server API to get a new token

4. **Token Refresh:**
   - The auth server has an API exposed that:
     - Accepts refresh token
     - Checks for its validity
     - Returns a new access token

5. **Logout:**
   - Once the refresh token expires, the user will be logged out

### OAuth 2.0 vs SAML

**Comparison:**
Both applications can be used for web single sign-on (SSO), but:
- **SAML** tends to be specific to a user
- **OAuth** tends to be specific to an application

---

## OpenID Connect (OIDC)

### What is OIDC?

**OpenID Connect (OIDC)** is an authentication protocol.

**Relationship to OAuth 2.0:**
- OIDC and SAML are both authentication protocols that allow identity providers (IdP) to implement user validation and access control
- **OIDC replaced SAML** in many modern applications
- **OIDC is built on top of OAuth 2.0**

### How OIDC Works

**Components:**
- **IdP (Identity Provider)**: Like Okta, Google, or Active Directory
- **SP (Service Provider)**: Application that relies on IdP for authentication

**Process:**
- An IdP maintains a database of user identity information
- A service provider (SP) relies on this information to authenticate a user, sometimes only once for multiple applications (single sign-on)
- Both OIDC and SAML are standards that define how this information flows between these two parties

**Goal:**
The end goal for both is the same: user authentication. But the underlying methodology to achieve the goal is different.

### OIDC vs OAuth 2.0

**OIDC:**
- OpenID Connect is another specification, based on OAuth 2.0
- It extends OAuth 2.0, specifying things that are relatively ambiguous in OAuth 2.0
- Tries to make it more interoperable

**Examples:**
- **Google login** uses OpenID Connect (which underneath uses OAuth 2.0)
- **Facebook login** doesn't support OpenID Connect. It has its own flavor of OAuth 2.0

### OIDC Scopes

**Definition:**
OIDC scopes define the claims (the user attributes) that an application can have access to.

**Process:**
- The IdP maintains a list of acceptable scopes
- An application can choose which to request, depending on its needs
- After a user explicitly consents to sharing their details (which includes the scopes), the IdP makes the scopes available to the application

**Setup Requirements:**
- The IdP must assign a secret and client-ID to the RP (Relying Party)
- The RP must share the endpoint it wants to receive codes and/or tokens on

### OpenID (Legacy)

**Historical Note:**
There was also an "OpenID" specification (not "OpenID Connect"). That tried to solve the same thing as OpenID Connect, but was not based on OAuth 2.0. So, it was a complete additional system. It is not very popular or used nowadays.

---

## HTTP Authentication Schemes

### Basic Authentication

**Definition:**
Basic and Digest authentication schemes are dedicated to authentication using a username and a secret (see [RFC7616](https://www.rfc-editor.org/rfc/rfc7616) and [RFC7617](https://www.rfc-editor.org/rfc/rfc7617)).

**How It Works:**
Basic authentication transmits credentials as user ID/password pairs, encoded using base64. The client sends HTTP requests with the `Authorization` header that contains the word `Basic` followed by a space and a base64-encoded string `username:password`.

**Security Note:**
- Credentials are base64-encoded (not encrypted)
- Should only be used over HTTPS
- Vulnerable to interception if used over HTTP

### Bearer Authentication

**Definition:**
Bearer authentication scheme is dedicated to authentication using a token and is described by the [RFC6750](https://www.rfc-editor.org/rfc/rfc6750). Even if this scheme comes from an OAuth 2.0 specification, you can still use it in any other context where tokens are exchanged between a client and a server.

**How It Works:**
Bearer authentication (also called token authentication) has security tokens called bearer tokens. The name "Bearer authentication" can be understood as "give access to the bearer of this token." The bearer token is a cryptic string, usually generated by the server in response to a login request. The client must send this token in the Authorization header when making requests to protected resources.

**Format:**
```
Authorization: Bearer <token>
```

**Token Characteristics:**
No matter how they are created, tokens are always:
- **Encoded**: Usually base64 or similar
- **Signed**: To prevent tampering
- **Rarely Encrypted**: Most tokens are signed, not encrypted

---

## Identity Providers

### What is an IdP?

**Identity Provider (IdP)** maintains a database of user identity information. A service provider (SP) relies on this information to authenticate a user, sometimes only once for multiple applications (single sign-on).

**Examples:**
- Okta
- Google
- Active Directory
- Microsoft Azure AD

### Microsoft Active Directory

**Microsoft's Active Directory (AD)** is an identity and access management (IAM) platform.

**Use Cases:**
- Enterprise user management
- Windows domain authentication
- Centralized user directory

---

## State Parameter

**Definition:**
The `state` parameter allows you to restore the previous state of your application.

**Purpose:**
The state parameter preserves some state objects set by the client in the Authorization request and makes it available to the client in the response.

**Security Benefit:**
Helps prevent CSRF (Cross-Site Request Forgery) attacks by allowing the client to verify that the response corresponds to its original request.

**Resources:**
- [Auth0: State Parameters](https://auth0.com/docs/secure/attack-protection/state-parameters)
- [Auth0: React Quickstart](https://auth0.com/docs/quickstart/spa/react)
- [AWS Cognito: Login Endpoint](https://docs.aws.amazon.com/cognito/latest/developerguide/login-endpoint.html)

---

## CORS and Authorization

### CORS (Cross-Origin Resource Sharing)

**What is CORS?**
CORS is a mechanism that allows web pages to make requests to a different domain than the one serving the web page.

**Resources:**
- [Codecademy: What is CORS](https://www.codecademy.com/articles/what-is-cors)
- [Wikipedia: CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)

### CORS Testing Examples

**Preflight Request (OPTIONS):**
```bash
curl -v --request OPTIONS 'https://vpakete.com/crm/warehouse/all' \
  --header 'Origin: http://localhost:3000' \
  --header 'Access-Control-Request-Method: GET' \
  --header 'Authorization: Basic OTc0NWY3OmI1NmQ3ZWNkNjI='
```

**Alternative CORS Test:**
```bash
curl -H "Origin: http://example.com" \
  -H "Access-Control-Request-Method: POST" \
  -H "Access-Control-Request-Headers: X-Requested-With" \
  -H "Authorization: Basic OTc0NWY3OmI1NmQ3ZWNkNjI=" \
  -X OPTIONS --verbose https://vpakete.by/crm/warehouse/all
```

**Checking CORS Headers:**
```bash
curl -I -X OPTIONS \
  -H "Origin: http://EXAMPLE.COM" \
  -H 'Access-Control-Request-Method: POST' \
  https://vpakete.by/crm/warehouse/all 2>&1 | grep 'Access-Control-Allow-Origin'
```

### Authorization Resources

- [Habr: Authorization Article](https://habr.com/en/company/dataart/blog/262817/)

---

## Resources

### Authentication & Authorization

- [Auth0: State Parameters](https://auth0.com/docs/secure/attack-protection/state-parameters)
- [Auth0: React Quickstart](https://auth0.com/docs/quickstart/spa/react)
- [AWS Cognito: Login Endpoint](https://docs.aws.amazon.com/cognito/latest/developerguide/login-endpoint.html)

### CORS

- [Codecademy: What is CORS](https://www.codecademy.com/articles/what-is-cors)
- [Wikipedia: CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)

### Protocols

- [RFC7616: HTTP Digest Authentication](https://www.rfc-editor.org/rfc/rfc7616)
- [RFC7617: HTTP Basic Authentication](https://www.rfc-editor.org/rfc/rfc7617)
- [RFC6750: OAuth 2.0 Bearer Token Usage](https://www.rfc-editor.org/rfc/rfc6750)

### Shell Parameter Expansion

- [GNU Bash Manual](http://www.gnu.org/software/bash/manual/bash.html) - The '$' character introduces parameter expansion, command substitution, or arithmetic expansion.

---

## Summary

Authentication and authorization are critical security components:

- **SSO**: Enables single login for multiple applications
- **OAuth 2.0**: Authorization protocol for third-party access
- **OIDC**: Modern authentication protocol built on OAuth 2.0
- **SAML**: XML-based enterprise SSO protocol
- **HTTP Authentication**: Basic and Bearer token schemes
- **CORS**: Enables cross-origin requests securely

**Key Takeaways:**
- Authentication verifies identity; authorization determines permissions
- SSO improves user experience and security
- OAuth 2.0 is for authorization; OIDC adds authentication
- Tokens are encoded and signed, rarely encrypted
- CORS must be properly configured for cross-origin requests

Understanding these concepts is essential for building secure, user-friendly applications.
