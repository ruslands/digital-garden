# iOS Single Sign-On (SSO): Sign in with Apple and Facebook Login

## Table of Contents
1. [Introduction](#introduction)
2. [Sign in with Apple](#sign-in-with-apple)
3. [Facebook Login](#facebook-login)
4. [Configuration](#configuration)
5. [Troubleshooting](#troubleshooting)
6. [Resources](#resources)

---

## Introduction

Single Sign-On (SSO) allows users to authenticate with your iOS application using their existing accounts from Apple or Facebook. This guide covers the implementation, configuration, and best practices for both Sign in with Apple and Facebook Login on iOS.

**Key Benefits:**
- Improved user experience (no need to create new accounts)
- Reduced friction in the sign-up process
- Enhanced security through trusted providers
- Compliance with platform requirements (Apple requires Sign in with Apple for apps using third-party sign-in)

---

## Sign in with Apple

### Overview

Sign in with Apple is Apple's authentication service that allows users to sign in to apps and websites using their Apple ID. It provides privacy-focused authentication with options to hide email addresses.

### Authentication Response Structure

When a user successfully authenticates with Sign in with Apple, you receive an `ASAuthorizationAppleIDCredential` object containing:

```json
{
  "identifier": "000670.a3b1fd1c2fe445668bc407ac40429ade.1531",
  "identityToken": "ZXlKcmFXUWlPaUptYURaQ2N6aERJaXdpWVd4bklqb2lVbE15TlRZaWZRLmV5SnBjM01pT2lKb2RIUndjem92TDJGd2NHeGxhV1F1WVhCd2JHVXVZMjl0SWl3aVlYVmtJam9pWTI5dExtMWxkR0ZpYjJ4cGMyMHVZMnh2ZFdRdWFXOXpJaXdpWlhod0lqb3hOekE1TVRJeE5ERTVMQ0pwWVhRaU9qRTNNRGt3TXpVd01Ua3NJbk4xWWlJNklqQXdNRFkzTUM1aE0ySXhabVF4WXpKbVpUUTBOVFkyT0dKak5EQTNZV00wTURReU9XRmtaUzR4TlRNeElpd2lZMTlvWVhOb0lqb2laRlY2WVc0ME5IRlVja3RWVjNKU1ExaHdMWFZpZHlJc0ltVnRZV2xzSWpvaWRHOXBZMmhyYVc1aExtNWhkR0ZzYVdGQVoyMWhhV3d1WTI5dElpd2laVzFoYVd4ZmRtVnlhV1pwWldRaU9uUnlkV1VzSW1GMWRHaGZkR2x0WlNJNk1UY3dPVEF6TlRBeE9Td2libTl1WTJWZmMzVndjRzl5ZEdWa0lqcDBjblZsZlEuWXdkS1JnX3ZPRDlvWkltbUxXb185OGRVMFRPdF9JR0wxU3hZcm43cDBHSkF1SzlfRDFBQ2doM242eGNpNXZwcldPUjA5eEhndnNESTc3LVVUSVNpWXNoOGFWSWlEYjk2SGpuMmhkV0pIeHBteXBfMmFiSGsweDdsR0hiSXNvNHpyYjR5UU1wbGFlR09mMmd2YTJNTDRYaEZEZl83ZVAzd2VWTUU1YWpQZTBxNHFoa0NwTXZYcmJZZWFJQWFNUE10dlluR1BvTVIyLWRZOWk5bVVsQWRVYkxQTjJaSUpMYkhVNWpkMUw0WHJpWWdBNlh2TUJXaWU1ZFhpMDR2bzE4NFAxUUE3SHl2ZU55Tl94aTk4ZWJ3TGEwU3F3YVJVV3pGTE51UEtfNFNra2poWmduUi11YzVrejJoV1V4NjhvQ0FtVXhmTk0yS2duc0VtTkQ5YnFpQXN3",
  "fullame": {
    "familyName": "Toichkina",
    "givenName": "Natalia"
  },
  "authorizationCode": "Y2NiOWU1MTQ1YmE2ODQ0NjNiNTQ0ZmU3YzFkYjdmY2QxLjAucnd4cS5mbGw0dEhZVnB5UjFLSGhvMS0wb0tB"
}
```

### Important Notes About User Information

**Critical Behavior**: You can only retrieve the full name and email for the first time. If these are the required information for your registration process, make sure you save it before beginning your registration process and remove it once your registration is confirmed.

**Why This Happens**: This behaves correctly. User info is only sent in the `ASAuthorizationAppleIDCredential` upon initial user sign up. Subsequent logins to your app using Sign in with Apple with the same account do not share any user info and will only return a user identifier in the `ASAuthorizationAppleIDCredential`.

**Best Practice**: It is recommended that you securely cache the initial `ASAuthorizationAppleIDCredential` containing the user info until you can validate that an account has successfully been created on your server.

### Key Components

#### 1. Identifier

**Description**: A unique value that Apple assigned to the end-user.

**Usage**: 
- Utilize this value — not an email or login name — when you store this user on your server
- The provided value will exactly match across all devices that the user owns
- Apple will provide the user with the same value for all of the apps associated with your Team ID
- Any app a user runs receives the same ID, meaning you already possess all their information on your server and don't need to ask the user to provide it

**Implementation**: Your server's database likely already stores some other identifier for the user. Simply add a new column to your user type table which holds the Apple-provided identifier. Your server-side code will then check that column first for a match. If not found, revert to your existing login or registration flows, such as using an email address or login name.

#### 2. Identity Token (id_token)

**Description**: An `id_token` is a JSON Web Token (JWT) that contains claims about the identity of the authenticated user. It provides information such as the user's unique identifier, email address (if granted by the user), and other user attributes.

**Usage**: 
- The `id_token` is typically used by the client application (your app) to verify the identity of the user who has authenticated through Sign in with Apple
- You can decode and validate the `id_token` to ensure that it was issued by Apple's authorization server and that the user's identity is valid

**Purpose**: Used for verifying the user's identity on the client side.

#### 3. Authorization Code (code)

**Description**: The "code" is a short-lived authorization code that the client application receives as part of the OAuth 2.0 authorization flow when the user authenticates with Apple's authentication service.

**Purpose**: The main purpose of the "code" is to be exchanged for an access token and optionally a refresh token. It is a one-time-use code that is sent back to your application's redirect URI.

**Usage**: Your application then sends this code, along with its client ID and client secret, to Apple's token endpoint to obtain an access token.

### Authentication Flow

Here's a simplified flow of how these components work together:

1. **User Initiates Sign In**: The user initiates the Sign in with Apple process in your application
2. **Redirect to Apple**: Your application redirects the user to Apple's authentication service
3. **User Authenticates**: The user logs in or authenticates using their Apple ID
4. **Authorization Code Returned**: After successful authentication, Apple's service returns an authorization code to your application's redirect URI
5. **Exchange Code for Token**: Your application then makes a secure server-to-server request to Apple's token endpoint, exchanging the authorization code for an access token and, optionally, a refresh token
6. **Use Access Token**: The access token can be used to access the user's data or perform actions on their behalf, depending on the permissions granted by the user

**Note**: The `id_token` is typically included in the response from Apple's authentication service and is used for verifying the user's identity. It is separate from the "code," which is used for obtaining access to the user's data.

### Response Mode: form_post

**Description**: The string `"response_mode=form_post"` appears to be related to web development and web applications, specifically in the context of OAuth 2.0 and OpenID Connect authentication protocols.

**What It Does**: In OAuth 2.0 and OpenID Connect, the `response_mode` parameter is used to specify how the authorization server should return the authorization response to the client application after the user has granted or denied consent. The `form_post` value for the `response_mode` parameter indicates that the authorization server should send the response data as a form post request to the client's specified redirect URI.

**Why Use It**: This is often used when the response data, such as an authorization code or an identity token, needs to be sent securely to the client without exposing it in the URL, which could be seen in the browser's address bar or logged in various server logs. Using a form post allows for a more secure transmission of this sensitive information.

**Summary**: When you see `"response_mode=form_post"` in the context of OAuth 2.0 or OpenID Connect, it means that the authorization server will send the authorization response data to the client application using a form post request. This is done to enhance security and protect sensitive information during the authentication process.

---

## Facebook Login

### Overview

Starting with Facebook iOS SDK v17.0, Facebook introduced changes to tokens, making the use of Limited Login mandatory in most cases. The traditional `accessToken` from the SDK can now only be obtained if the user has granted tracking permission through the system UI `ATTrackingManager`.

### Token Types

#### 1. Standard Access Token (configuration.tracking = .enabled)

**Behavior**:
- Works as before if the application received permission from the user through `ATTrackingManager`
- Works only inside the application itself if the application did not receive tracking permission
- Cannot be validated on the backend even for validation purposes
- Can only be used within the iOS application

**When to Use**: When you only need the token for in-app operations and the user has granted tracking permission.

#### 2. JWT Token (configuration.tracking = .limited)

**Behavior**:
- Validated on the backend
- Includes current values of profile fields that the user granted permission for
- Contains permissions that the user granted
- Does not allow making requests to REST API

**When to Use**: When you need a token that can be validated on the backend and includes user profile information.

**Implementation**: To get a token that can be validated on the backend, you need to pass `.limited` in the `configuration.tracking` field during login in the SDK. This will perform a special limited login and return a token of a different type — JWT, which should be sent to the backend. The backend can validate this token and confirm that it is indeed a valid Facebook token.

**Limitations**: The backend cannot use this token for requests to Graph API. Inside the JWT, all profile fields that the user granted permission for will already be "embedded" — but the set of permissions during limited login is limited, which is why it's called "limited."

### Migration Guide

**Summary**:
- Standard token (`configuration.tracking = .enabled`) works as before if the app received permission from the user through `ATTrackingManager`
- Standard token (`configuration.tracking = .enabled`) works only inside the app itself if the app did not receive tracking permission
- JWT token (`configuration.tracking = .limited`) is validated on the backend, includes current values of profile fields, permissions granted by the user, and does not allow making requests to REST API
- For migration from standard token to JWT: on the client, you need to change the login call and profile data retrieval, and on the backend — the token validation logic

---

## Configuration

### Sign in with Apple Configuration

#### Step 1: Create Primary App ID

1. Go to "Identifiers" category in Apple Developer Portal
2. Create your primary App ID
3. In the "Capabilities" section, check the box "Sign In with Apple"
4. Configure a URL to receive user account events (optional but recommended)
5. **Important**: Click the blue "Save" button in the upper right corner

#### Step 2: Create Service ID

1. Go back to "Identifiers" category
2. Use the dropdown in the upper right to select "Service IDs"
3. Create a new Service ID
4. Check the box "Sign in with Apple" that appears on the bottom of the section
5. Click "Configure"
6. **Important**: In the top part of the dialog box, you must choose the App ID created in Step 1. If you cannot see it or it says "No App ID available," go back and check the configuration on your App ID
7. Enter your domains:
   - **Domains and subdomains**: Enter your domains WITHOUT protocol (https://)
   - **Return URL**: Enter your full, exact return URL that the browser can redirect to after sign in, this time with protocol and full path

#### Step 3: Generate Secret Key

Depending on your use case, you next probably need to generate a secret key for the app and sign a JWT token with it.

### Configuration Values

**Environment Variables**:
```
APPLE_TEAM_ID = "J94K5F48MM"
APPLE_KEY_ID = "V9T4U3SV72"
APPLE_PRIVATE_KEY = '-----BEGIN PRIVATE KEY--...'
APPLE_CLIENT_ID = "com.metabolism.cloud.ios"
```

**Service Configuration Table**:

| Type                          | Value                             |
| ----------------------------- | --------------------------------- |
| AppID                         | `cloud.metabolism.client`         |
| ServiceID (Same as client_id) | `cloud.metabolism.client.service` |
| Key                           | `66DUCBLX7J`                      |

**Web Apple Sign In Configuration**:

Create serviceID with redirect and host URLs:

**Development Environment**:
- Domain: `development.metabolism.cloud`
- Redirect URLs:
  - `https://development.metabolism.cloud/api/v1/auth/apple/callback`
  - `https://development.metabolism.cloud/auth/callback`

**Staging Environment**:
- Domain: `staging.metabolism.cloud`
- Redirect URLs:
  - `https://staging.metabolism.cloud/api/v1/auth/apple/callback`
  - `https://staging.metabolism.cloud/auth/callback`

**Production Environment**:
- Domain: `metabolism.cloud`
- Redirect URLs:
  - `https://metabolism.cloud/api/v1/auth/apple/callback`
  - `https://metabolism.cloud/api/auth/callback`

### Key Creation

**Location**: [Apple Developer - Auth Keys](https://developer.apple.com/account/resources/authkeys/list)

### Email Sources Registration

**Location**: [Apple Developer - Services Configuration](https://developer.apple.com/account/resources/services/configure)

**Note**: You need to create AppID for both production and development environments.

**Important**: Your Services ID (e.g., `com.example.webapp`) should be used as the value of `client_id`.

---

## Troubleshooting

### Common Issues

#### Issue: "No App ID available" in Service ID Configuration

**Solution**: 
1. Go back to your App ID configuration
2. Make absolutely sure you checked the "Sign In with Apple" box
3. Click "Save" in the upper right corner
4. Don't waste your time trying to fill it in if you don't see your App ID in the top dropdown

#### Issue: User Information Not Available on Subsequent Logins

**Solution**: This is expected behavior. User info is only available on the first sign-in. You must:
1. Cache the initial `ASAuthorizationAppleIDCredential` containing user info
2. Store it securely until account creation is confirmed
3. Remove it once registration is confirmed

#### Issue: Token Validation Fails on Backend

**Solution**: 
- For Sign in with Apple: Ensure you're using the correct `identityToken` (JWT) and validating it properly
- For Facebook: Use Limited Login (`configuration.tracking = .limited`) to get a JWT token that can be validated on the backend

---

## Resources

### Sign in with Apple Documentation

- [Request an Authorization to the Sign in with Apple Server](https://developer.apple.com/documentation/sign_in_with_apple/request_an_authorization_to_the_sign_in_with_apple_server)
- [Authenticating Users with Sign in with Apple](https://developer.apple.com/documentation/sign_in_with_apple/sign_in_with_apple_rest_api/authenticating_users_with_sign_in_with_apple)
- [SignInResponse](https://developer.apple.com/documentation/sign_in_with_apple/signinresponsei)
- [Generate and Validate Tokens](https://developer.apple.com/documentation/sign_in_with_apple/generate_and_validate_tokens)
- [ClientConfig RedirectURI](https://developer.apple.com/documentation/sign_in_with_apple/clientconfigi/3230952-redirecturi/)
- [Apple Developer Forums - Thread 122536](https://developer.apple.com/forums/thread/122536)
- [Apple Developer Forums - Thread 120239](https://developer.apple.com/forums/thread/120239)
- [Apple Developer Search - redirect_uri](https://developer.apple.com/search/?q=redirect_uri)
- [Apple OpenID Configuration](https://appleid.apple.com/.well-known/openid-configuration)
- [Google OpenID Configuration](https://accounts.google.com/.well-known/openid-configuration)

### Community Resources

- [Sign in with Apple using Django - Identifiers and Keys](https://github.com/truffls/sign-in-with-apple-using-django/blob/master/identifiers-and-keys.md)
- [Sign in with Apple - Gist](https://gist.github.com/aamishbaloch/2f0e5d94055e1c29c0585d2f79a8634e)
- [Sign in with Apple using Django - Backend](https://github.com/truffls/sign-in-with-apple-using-django/blob/master/backend.md)
- [How to Configure Sign in with Apple - Medium](https://medium.com/identity-beyond-borders/how-to-configure-sign-in-with-apple-77c61e336003)
- [Apple Developer Forums - Thread 117221](https://developer.apple.com/forums/thread/117221)

### Facebook Login Documentation

- [Facebook Limited Login - Token Validation](https://developers.facebook.com/docs/facebook-login/limited-login/token/validating)
- [Facebook Limited Login - iOS](https://developers.facebook.com/docs/facebook-login/limited-login/ios/)
- [Facebook Limited Login - Permissions](https://developers.facebook.com/docs/facebook-login/limited-login/permissions)

---

## Best Practices

1. **Always Cache Initial User Info**: Store the initial `ASAuthorizationAppleIDCredential` securely until account creation is confirmed
2. **Use Identifier, Not Email**: Always use the Apple-provided identifier for user identification, not email addresses
3. **Validate Tokens on Backend**: Always validate tokens on your backend server, never trust client-side tokens alone
4. **Handle Token Expiration**: Implement proper token refresh mechanisms for both Apple and Facebook tokens
5. **Respect User Privacy**: Only request the minimum permissions necessary for your app's functionality
6. **Test Both Flows**: Test both first-time sign-in and subsequent logins to ensure proper handling
7. **Implement Fallback**: Always have a fallback authentication method in case SSO fails
