# Personal data
Personally identifiable information (PII)  is any information about an individual maintained by an agency, including (1) any information that can be used to distinguish or trace an individual‘s identity, such as name, social security number, date and place of birth, mother‘s maiden name, or biometric records; and (2) any other information that is linked or linkable to an individual, such as medical, educational, financial, and employment information.
### Examples of Personally Identifiable Information (PII)

PII includes any information that can uniquely identify an individual, such as:

-   **Full name**
-   **Email address**
-   **Phone number**
-   **Social Security Number (SSN)**
-   **Address**
-   **Bank account numbers**
-   **Medical records**
-   **Biometric data** (like fingerprints, facial recognition data)

In general, any data that can identify an individual either alone or when combined with other data can be considered PII.
## How to Implement a Log Filter to Obfuscate PII Fields

To protect PII in logs, we can implement a log filter to mask or obfuscate sensitive data. Here's a simple example of how to do this in Python:
```
import re
import logging

class PIILogFilter(logging.Filter):
    def filter(self, record):
        # Replace email addresses with "****@****.com"
        record.msg = re.sub(r'\b[\w.-]+@[\w.-]+\.\w{2,4}\b', '****@****.com', record.msg)
        # Replace phone numbers with "***-***-****"
        record.msg = re.sub(r'\b\d{3}[-.\s]?\d{3}[-.\s]?\d{4}\b', '***-***-****', record.msg)
        # Additional obfuscation rules can be added here
        return True

# Set up logger
logger = logging.getLogger()
logger.addFilter(PIILogFilter())
logger.setLevel(logging.INFO)

# Example usage
logger.info("User email: john.doe@example.com, Phone: 123-456-7890")
```
##  How to Encrypt a Password and Check the Validity of an Input Password

To securely handle passwords, we should use a hashing function with salt to encrypt passwords. Here’s a common approach using the `bcrypt` library in Python:
```
import bcrypt

# Encrypting a password
def hash_password(password):
    salt = bcrypt.gensalt()
    hashed = bcrypt.hashpw(password.encode(), salt)
    return hashed

# Checking password validity
def check_password(hashed_password, user_input):
    return bcrypt.checkpw(user_input.encode(), hashed_password)

# Example usage
hashed_pw = hash_password("securePassword123")
is_valid = check_password(hashed_pw, "securePassword123")
```
In this example, `hash_password` generates a salt and hashes the password, while `check_password` compares an input password to the stored hashed password.

### How to Authenticate to a Database Using Environment Variables

Environment variables are used to securely store sensitive information like database credentials. Here’s an example of connecting to a database using environment variables in Python:
```
import os
import mysql.connector

# Set environment variables (typically done outside code, e.g., in bash or .env file)
os.environ['DB_USER'] = 'your_db_user'
os.environ['DB_PASSWORD'] = 'your_secure_password'
os.environ['DB_HOST'] = 'localhost'
os.environ['DB_NAME'] = 'your_database_name'

# Retrieve environment variables
db_user = os.getenv('DB_USER')
db_password = os.getenv('DB_PASSWORD')
db_host = os.getenv('DB_HOST')
db_name = os.getenv('DB_NAME')

# Connect to database
connection = mysql.connector.connect(
    user=db_user,
    password=db_password,
    host=db_host,
    database=db_name
)

# Example query
cursor = connection.cursor()
cursor.execute("SELECT * FROM users;")
print(cursor.fetchall())
```
In practice, avoid hard-coding credentials by loading them from environment variables, which can be managed using `.env` files with libraries like `python-dotenv` for a secure and organized approach.

With a solid understanding of these principles, you’ll be well-prepared to explain and implement secure practices around PII handling, password management, and database authentication!