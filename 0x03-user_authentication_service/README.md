# User Authentication Service
Creating a user authentication service using Flask is a great project for learning about web development and RESTful API design. Below, is a guide you on how to declare API routes in a Flask app while incorporating user authentication.
### Overview of a Flask Application

A basic Flask application usually involves the following components:

-   **Flask**: The web framework.
-   **Routes**: URL endpoints that users can access.
-   **Views**: Functions that return responses when routes are accessed.

### Setting Up Your Flask App

1.  **Install Flask**:  
    If you haven't already, install Flask and other necessary libraries:
    ```
    pip install Flask Flask-JWT-Extended Flask-SQLAlchemy
    ```
 2.   **Project Structure**:  
Create the following structure for your project:
		```
		/your_project  
		    |-- app.py  
		    |-- models.py  
		    |-- config.py
		```


### **Steps to Declare API Routes in Flask**

1.  **Import Flask and Necessary Modules:** Start by importing the Flask module and any additional modules you might need.

	```
	from flask import Flask, jsonify, request
	```
2. **Initialize the Flask App:** Create an instance of the Flask application.
	```
	app = Flask(__name__)
	```
3. **Declare API Routes:** Use the `@app.route` decorator to define routes for specific endpoints and HTTP methods.

	- **Basic Route:**
		```
		@app.route('/')
		def home():
		    return "Welcome to the User Authentication Service!"
		```

	- **Route with HTTP Methods:** Specify the HTTP methods (e.g., `GET`, `POST`, `PUT`, `DELETE`) allowed for the route.
		```
		@app.route('/login', methods=['POST'])
		def login():
		    # Handle login logic
		    return jsonify({"message": "Login successful"}
		```
	- **Route with Dynamic Segments:** Use `<variable>` to capture parts of the URL as parameters.

		```
		@app.route('/user/<int:user_id>', methods=['GET'])
		def get_user(user_id):
		    # Fetch and return user details
		    return jsonify({"user_id": user_id, "name": "John Doe"})
		```

4. **Run the App:** Run the app using the `run()` method.
	```
	if __name__ == '__main__':
	    app.run(debug=True)
	```
### **Key Points to Remember:**

-   **Route Syntax:** `@app.route('/path', methods=['HTTP_METHODS'])`
    -   Default HTTP method is `GET` if not specified.
-   **Dynamic Segments:** Use `<type:variable>` for dynamic URL components (e.g., `<int:id>`).
-   **Returning Data:** Use `jsonify()` to return JSON responses, especially for APIs.
-   **Testing Routes:** You can test the endpoints using tools like **Postman**, **curl**, or a browser (for `GET` routes).

This structure ensures your Flask app can handle RESTful API requests effectively.

## -   How to get and set cookies
In Flask, you can **get** and **set cookies** using the `request` and `Response` objects or through helper functions like `make_response`.


### **Getting Cookies**

To retrieve a cookie, use the `request.cookies` object. This allows you to access the cookie by its name.

#### Example:
```
from flask import Flask, request

app = Flask(__name__)

@app.route('/get-cookie')
def get_cookie():
    user = request.cookies.get('user')  # Get cookie named 'user'
    if user:
        return f"Welcome back, {user}!"
    return "No cookie found!"

if __name__ == '__main__':
    app.run(debug=True)
```
-   **Key Points:**
    -   Use `request.cookies.get('cookie_name')` to fetch the cookie value.
    -   If the cookie does not exist, `get()` returns `None` by default.

### **Setting Cookies**

To set a cookie, modify the response object using the `set_cookie()` method.

#### Example:
```
from flask import Flask, make_response

app = Flask(__name__)

@app.route('/set-cookie')
def set_cookie():
    resp = make_response("Cookie has been set!")
    resp.set_cookie('user', 'George', max_age=3600)  # Set cookie 'user' with value 'George'
    return resp

if __name__ == '__main__':
    app.run(debug=True)
```
-   **Key Parameters:**
    -   `key`: The name of the cookie.
    -   `value`: The value to store in the cookie.
    -   `max_age`: The duration (in seconds) for which the cookie is valid. If omitted, the cookie is a session cookie.
    -   `httponly`: Set `True` to restrict JavaScript from accessing the cookie.
### **Deleting Cookies**

To delete a cookie, set its expiration time to a past date using `set_cookie()`.

#### Example:
```
@app.route('/delete-cookie')
def delete_cookie():
    resp = make_response("Cookie has been deleted!")
    resp.set_cookie('user', '', max_age=0)  # Remove the 'user' cookie
    return resp
```
### **Key Notes:**

1.  **`request.cookies`**:
    -   Access cookies sent by the client.
2.  **`set_cookie()`**:
    -   Modify the `Response` object to add a cookie.
3.  **Secure Flags**:
    -   Use `secure=True` to ensure cookies are only sent over HTTPS.
    -   Use `samesite='Strict'` or `'Lax'` to control cross-site cookie sharing.

**Full Example:**
```
from flask import Flask, request, make_response

app = Flask(__name__)

@app.route('/set-cookie')
def set_cookie():
    resp = make_response("Cookie has been set!")
    resp.set_cookie('user', 'George', max_age=3600, httponly=True)
    return resp

@app.route('/get-cookie')
def get_cookie():
    user = request.cookies.get('user')
    if user:
        return f"Hello, {user}!"
    return "No cookie found!"

@app.route('/delete-cookie')
def delete_cookie():
    resp = make_response("Cookie has been deleted!")
    resp.set_cookie('user', '', max_age=0)
    return resp

if __name__ == '__main__':
    app.run(debug=True)
```
This setup demonstrates how to manage cookies in Flask.
## -   How to retrieve request form data

To retrieve form data in a Flask application, use the `request.form` object. This object contains the data submitted via an HTTP POST request with a `Content-Type` of `application/x-www-form-urlencoded` or `multipart/form-data` (common for form submissions)

### **Retrieving Form Data**

1.  **Import Required Modules:** Import `Flask` and `request` from `flask`

	```
	from flask import Flask, request
	```
2.   **Access `request.form`:** Use `request.form` to access the submitted form data by its field name.
### **Example: Retrieving Form Data**

#### HTML Form:
```
<form action="/submit" method="post">
  <label for="username">Username:</label>
  <input type="text" id="username" name="username">
  
  <label for="password">Password:</label>
  <input type="password" id="password" name="password">
  
  <button type="submit">Submit</button>
</form>
```
**Flask Code:**
```
app = Flask(__name__)

@app.route('/submit', methods=['POST'])
def submit():
    # Retrieve form data
    username = request.form.get('username')  # Retrieve 'username'
    password = request.form.get('password')  # Retrieve 'password'

    return f"Username: {username}, Password: {password}"

if __name__ == '__main__':
    app.run(debug=True)
```
**Key Points:**

-   `request.form.get('field_name')`: Safely retrieves the value of a form field by name. Returns `None` if the field does not exist.
-   `request.form['field_name']`: Directly retrieves the value but raises a `KeyError` if the field is missing.
### **Handling Optional Fields**

To avoid errors when a field is optional, always use `.get()` with a default value:

```
optional_field = request.form.get('optional_field', 'default_value')
```
### **Retrieving All Form Data**

You can iterate through all form data:
```
@app.route('/submit', methods=['POST'])
def submit():
    form_data = request.form.to_dict()  # Converts form data to a dictionary
    return f"Form Data: {form_data}"
```
### **Supporting Multiple Methods**

To handle both `GET` and `POST` in a single route:
```
@app.route('/submit', methods=['GET', 'POST'])
def submit():
    if request.method == 'POST':
        username = request.form.get('username')
        return f"Form submitted with username: {username}"
    return "Please submit the form."
```
### **Working with File Uploads**

If your form includes file inputs, use `request.files` to handle them:
```
@app.route('/upload', methods=['POST'])
def upload():
    file = request.files['file']
    file.save(f"./uploads/{file.filename}")
    return "File uploaded successfully!"
```
**Full Example:**
```
from flask import Flask, request

app = Flask(__name__)

@app.route('/submit', methods=['POST'])
def submit():
    username = request.form.get('username')
    password = request.form.get('password')
    return f"Username: {username}, Password: {password}"

if __name__ == '__main__':
    app.run(debug=True)
```
## -   How to return various HTTP status codes

In Flask, you can return various HTTP status codes alongside your response using the `Response` object, helper functions like `make_response`, or by simply adding the status code to the return statement.
### **Methods to Return HTTP Status Codes**

#### **1. Directly Specifying Status Code**

You can return a tuple containing the response body and the status code.

```
from flask import Flask

app = Flask(__name__)

@app.route('/success')
def success():
    return "Success", 200  # 200 OK

@app.route('/not-found')
def not_found():
    return "Not Found", 404  # 404 Not Found

if __name__ == '__main__':
    app.run(debug=True)
```
#### **2. Using `make_response`**

The `make_response` function allows you to create a `Response` object and specify the status code explicitly.
```
from flask import Flask, make_response

app = Flask(__name__)

@app.route('/created')
def created():
    resp = make_response("Resource Created")
    resp.status_code = 201  # 201 Created
    return resp

if __name__ == '__main__':
    app.run(debug=True)
```
#### **3. Using Flask Helper Methods**

Flask provides helper functions like `jsonify` to create a JSON response and include a status code.
```
from flask import Flask, jsonify

app = Flask(__name__)

@app.route('/unauthorized')
def unauthorized():
    return jsonify({"error": "Unauthorized access"}), 401  # 401 Unauthorized

if __name__ == '__main__':
    app.run(debug=True)
```
### **Common HTTP Status Codes**

Here are some commonly used HTTP status codes:
| **Status Code** | **Name**                 | **Description**                               |
|------------------|--------------------------|-----------------------------------------------|
| `200`            | OK                      | The request succeeded.                        |
| `201`            | Created                 | The resource was successfully created.        |
| `204`            | No Content              | The request succeeded, but no content to return. |
| `400`            | Bad Request             | The server could not understand the request.  |
| `401`            | Unauthorized            | Authentication is required.                   |
| `403`            | Forbidden               | The server refuses to fulfill the request.    |
| `404`            | Not Found               | The requested resource was not found.         |
| `405`            | Method Not Allowed      | The HTTP method is not allowed for the resource. |
| `409`            | Conflict                | There is a conflict with the current state of the resource. |
| `410`            | Gone                    | The resource is no longer available.          |
| `429`            | Too Many Requests       | The user has sent too many requests in a given amount of time. |
| `500`            | Internal Server Error   | The server encountered an unexpected error.   |
| `502`            | Bad Gateway             | The server received an invalid response from an upstream server. |
| `503`            | Service Unavailable     | The server is currently unable to handle the request. |
| `504`            | Gateway Timeout         | The server did not receive a timely response from an upstream server. |



### **Custom HTTP Status Messages**

You can return a custom message with a status code.
```
@app.route('/custom-error')
def custom_error():
    return "Something went wrong!", 500  # Custom error message with 500 status code
```
### **Setting Headers Along with Status Codes**

You can include custom headers while returning a response.
```
@app.route('/custom-header')
def custom_header():
    resp = make_response("Here is a custom header")
    resp.status_code = 200
    resp.headers['X-Custom-Header'] = 'CustomValue'
    return resp
```
### **Using Abbreviations for HTTP Status Codes**

Flask provides constants for HTTP status codes via `flask-api`.
```
from flask_api import status

@app.route('/gone')
def gone():
    return "This resource is gone!", status.HTTP_410_GONE
```
**Complete Example**
```
from flask import Flask, jsonify, make_response

app = Flask(__name__)

@app.route('/ok')
def ok():
    return "Everything is fine!", 200

@app.route('/not-found')
def not_found():
    return jsonify({"error": "Resource not found"}), 404

@app.route('/server-error')
def server_error():
    resp = make_response("Internal Server Error")
    resp.status_code = 500
    return resp

if __name__ == '__main__':
    app.run(debug=True)
```
This approach allows flexibility to handle any HTTP status codes efficiently in Flask.


