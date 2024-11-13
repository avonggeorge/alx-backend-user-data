# Basic authentication
### **What Authentication Means**

-   **Definition**: Authentication is the process of verifying the identity of a user or system to ensure that they are who they claim to be.
-   **Purpose**: It protects resources by confirming that only authorized users can access certain data or services.
-   **Examples**: Common forms of authentication include passwords, biometrics (fingerprint, facial recognition), and tokens (like OTPs).
### **What Base64 Is**

-   **Definition**: Base64 is a binary-to-text encoding scheme that converts binary data into a text format using only 64 characters (A-Z, a-z, 0-9, +, /).
-   **Usage**: Base64 is commonly used to encode data so it can be safely transmitted over media designed to handle text. For example, it’s used to encode binary files (like images) in emails or data in URL parameters.
-   **Note**: Base64 encoding doesn’t encrypt data, so it’s not secure on its own—it’s just a way of representing data.
### **How to Encode a String in Base64**
-   **Encoding Credentials**: The format for Basic Authentication credentials is `"username:password"`, which is then Base64-encoded.
-   **Example**:
    -   Username: `user`
    -   Password: `pass`
    -   Combined: `user:pass`
    -   Base64-encoded: `dXNlcjpwYXNz`
    -   Final Header: `Authorization: Basic dXNlcjpwYXNz`
-   **Note**: Base64 is not encryption—it only encodes the credentials to make them ASCII-compatible. Anyone intercepting the header can easily decode it to see the original credentials.
-   **Process**:
    -   First, convert the string into bytes.
    -   Use a Base64 encoding function to convert the bytes to a Base64 string.
-   **Example** (Python):
    
```
import base64
encoded = base64.b64encode("your_string".encode("utf-8"))
print(encoded.decode("utf-8"))  # Outputs encoded Base64 string
```
### **What Basic Authentication Means**

-   **Definition**: Basic authentication is a simple authentication scheme built into HTTP, where the client sends the username and password encoded in Base64 with each request.
-   **How it Works**:
    -   The client encodes their username and password in Base64 and includes it in the `Authorization` header.
    -   The server decodes the credentials and verifies them before granting access.
-   **Security Note**: Basic Authentication isn’t secure on its own because it transmits credentials in a reversible form (Base64). Therefore, it’s usually used with HTTPS to encrypt the transmission.

-   **Purpose**: It provides a straightforward way for a client (like a browser or application) to prove its identity to the server without managing a session.
### **How It Works**

-   When a client wants to access a protected resource, it sends an HTTP request to the server.
-   The server responds with a `401 Unauthorized` status, indicating that the resource requires authentication and includes a `WWW-Authenticate` header specifying "Basic" authentication.
-   The client then encodes the username and password in Base64 format and sends them back in the `Authorization` header.


### **How to Send the Authorization Header**

-   **Process**:
    -   The `Authorization` header is formatted as `"Authorization: Basic <Base64-encoded-credentials>"`.
    -   **Example**: Suppose your username is "user" and password is "pass".
        -   Encode the credentials as `user:pass` in Base64 (e.g., `dXNlcjpwYXNz`).
        -   Send it in the header: `Authorization: Basic dXNlcjpwYXNz`.
-   **Example with Curl**:
```
curl -H "Authorization: Basic dXNlcjpwYXNz" http://yourapi.com/resource
```


### **Security Concerns**

-   **Not Encrypted**: Basic Authentication transmits credentials as plain text, making it vulnerable to interception if sent over an insecure connection.
-   **Use with HTTPS**: To protect credentials, Basic Authentication should always be used over HTTPS. HTTPS encrypts the transmission, preventing credentials from being easily intercepted.
-   **Lacks Session Management**: Basic Authentication does not handle session management, so the client sends credentials with every request, which can lead to repetitive transmission of sensitive data.
###  **Advantages and Disadvantages**

-   **Advantages**:
    -   Simple and easy to implement; built directly into HTTP.
    -   No need for cookies, sessions, or complex login flows.
    -   Compatible with many clients, browsers, and applications.
-   **Disadvantages**:
    -   Not secure unless paired with HTTPS.
    -   Requires sending credentials with every request.
    -   Provides no way to manage session timeout or expiration without re-sending credentials each time.

###  **Example Code**

Here’s how you might use Basic Authentication in a few different contexts:

-   **Using Curl**:
```
curl -u username:password https://api.example.com/resource
```
- **In Python (using Requests)**:
```
import requests
response = requests.get('https://api.example.com/resource', auth=('username', 'password'))
print(response.text)
```
### Summary

Basic Authentication is a quick and easy way to authenticate HTTP requests by sending Base64-encoded credentials in each request. While convenient, it should only be used over HTTPS due to security limitations. It’s generally suitable for APIs or applications where simplicity is essential and where HTTPS can guarantee the security of transmitted data.

## Other Types of Authentication
There are various authentication methods beyond Basic Authentication, each suited to different applications, security needs, and architectures. Here are some common ones:

### 1. **Token-Based Authentication**

-   **Overview**: After a user logs in, the server generates a token, which the client includes in the `Authorization` header for each request. Tokens can carry information about the user and expiration time, making them more secure than Basic Authentication.
-   **Examples**:
    -   **JWT (JSON Web Token)**: Popular in REST APIs, JWT is a self-contained token format that includes claims about the user. Tokens are signed, making them tamper-proof, and can include expiration times.
    -   **OAuth2 Tokens**: Used in OAuth2, an open standard for authorization, tokens grant specific permissions to users without exposing their passwords.
-   **Use Case**: APIs, single-page applications, mobile apps.

### 2. **Session-Based Authentication**

-   **Overview**: A classic, stateful approach where a server creates and stores a session ID after the user logs in. The session ID is stored as a cookie and sent with each request. The server uses this ID to look up the user's session and authorize the request.
-   **Use Case**: Traditional web applications with server-side rendering, where persistent sessions are beneficial.

### 3. **OAuth2**

-   **Overview**: OAuth2 is an authorization protocol that allows third-party applications to request limited access to a user’s resources without exposing the user’s password. OAuth2 involves an access token granted after user consent.
-   **Examples**:
    -   **Authorization Code Grant**: Used for web apps where an authorization code is exchanged for a token after user consent.
    -   **Implicit Grant**: Used in browser-based apps, where the access token is directly provided to the app after consent.
-   **Use Case**: Integrating with third-party APIs like Google, Facebook, or GitHub for authentication and access to user data.

### 4. **Multi-Factor Authentication (MFA)**

-   **Overview**: MFA adds additional layers of security by requiring multiple forms of verification, such as a password plus a one-time code (OTP) sent via SMS, email, or generated by an authenticator app.
-   **Types**:
    -   **Two-Factor Authentication (2FA)**: A common form of MFA using two factors, such as a password and OTP.
    -   **Biometric Factors**: Fingerprint, face recognition, or voice as a second factor.
-   **Use Case**: High-security applications, banking, and environments where strong verification is needed.

### 5. **Biometric Authentication**

-   **Overview**: Uses unique biological characteristics to verify identity, such as fingerprints, facial recognition, or retinal scans.
-   **Examples**:
    -   **Fingerprint Scanning**: Common on mobile devices.
    -   **Face Recognition**: Used by systems like Apple’s Face ID.
    -   **Retinal Scanning or Voice Recognition**: Less common but used in specialized secure environments.
-   **Use Case**: Mobile device unlocking, secure facilities, high-security apps.

### 6. **API Key Authentication**

-   **Overview**: The client includes an API key in each request, which the server uses to identify and authorize the request. API keys are generated by the server and are unique to each client.
-   **Usage**: Not highly secure by itself, but often paired with other methods (like IP whitelisting) for added security.
-   **Use Case**: Public APIs, services requiring lightweight authentication.

### 7. **SAML (Security Assertion Markup Language)**

-   **Overview**: SAML is an XML-based protocol that allows identity providers (IdPs) to send user authentication data to service providers (SPs). SAML is common in single sign-on (SSO) scenarios.
-   **Use Case**: Enterprise applications, single sign-on for cloud and on-premise applications in enterprise environments.

### 8. **Single Sign-On (SSO)**

-   **Overview**: SSO enables a user to log in once and access multiple related systems without re-authenticating. SSO can use protocols like OAuth2, SAML, or OpenID Connect.
-   **Examples**:
    -   **Google SSO**: Login once with Google, and access many third-party sites.
    -   **OpenID Connect**: A protocol built on OAuth2 for authentication, commonly used for SSO.
-   **Use Case**: Corporate environments, ecosystems with multiple services that share user identity.

### 9. **Certificate-Based Authentication**

-   **Overview**: Uses digital certificates as authentication factors. Certificates, like SSL/TLS certificates, are issued by a trusted authority and contain the user’s public key.
-   **Use Case**: Secure client-server communication, VPNs, and enterprise environments where strong, verifiable identity is essential.

### 10. **Passwordless Authentication**

-   **Overview**: Authentication without passwords, typically using magic links, OTPs, or biometrics.
-   **Examples**:
    -   **Magic Links**: A user receives a link via email, which logs them in when clicked.
    -   **OTP (One-Time Password)**: A time-based code is generated and sent to the user for authentication.
-   **Use Case**: Reducing the security risks and friction associated with passwords, mobile apps, and applications focused on user convenience.

### Summary

Different authentication methods provide varying levels of security, scalability, and usability. Selecting the right authentication method depends on the application's requirements, sensitivity of the data, and ease of use for the end-user.


# Simple API

Simple HTTP API for playing with `User` model.


## Files

### `models/`

- `base.py`: base of all models of the API - handle serialization to file
- `user.py`: user model

### `api/v1`

- `app.py`: entry point of the API
- `views/index.py`: basic endpoints of the API: `/status` and `/stats`
- `views/users.py`: all users endpoints


## Setup

```
$ pip3 install -r requirements.txt
```


## Run

```
$ API_HOST=0.0.0.0 API_PORT=5000 python3 -m api.v1.app
```


## Routes

- `GET /api/v1/status`: returns the status of the API
- `GET /api/v1/stats`: returns some stats of the API
- `GET /api/v1/users`: returns the list of users
- `GET /api/v1/users/:id`: returns an user based on the ID
- `DELETE /api/v1/users/:id`: deletes an user based on the ID
- `POST /api/v1/users`: creates a new user (JSON parameters: `email`, `password`, `last_name` (optional) and `first_name` (optional))
- `PUT /api/v1/users/:id`: updates an user based on the ID (JSON parameters: `last_name` and `first_name`)