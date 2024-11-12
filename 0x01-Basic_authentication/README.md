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