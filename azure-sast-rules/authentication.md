---
description: >-
  This repo contains all  types of security best practices for handling
  authentication
---

# 🔐 Authentication

## 1. How to handle secure password while connecting with the Database ?

Instead of hardcoding passwords, using `env` variable would be much better . Let's take example with Django and python&#x20;

### Problem&#x20;

* Hard code passwords can lead to security vulnerability which is significant security risk&#x20;
* Flexibility issue : can't be modified  password without modifying code or client side&#x20;
* Version control Issue : storing hardcoded password in VS repo, with multiple access can lead to security risk&#x20;

```python
// Non compliant code 


# settings.py

DATABASES = {
    'postgresql_db': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'quickdb',
        'USER': 'sonarsource',
        'PASSWORD': '', # Noncompliant
        'HOST': 'localhost',
        'PORT': '5432'
    }
}


```

```python
// complaint solution


# settings.py
import os

DATABASES = {
    'postgresql_db': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'quickdb',
        'USER': 'sonarsource',
        'PASSWORD': os.getenv('DB_PASSWORD'),
        'HOST': 'localhost',
        'PORT': '5432'
    }
}
```

Let's take another DB example for MySQL  connection&#x20;

```sql
// Non Compliant  code

from mysql.connector import connection

connection.MySQLConnection(host='localhost', user='sonarsource', password='')
```

```sql
// Compliant code

from mysql.connector import connection
import os

db_password = os.getenv('DB_PASSWORD')
connection.MySQLConnection(host='localhost', user='sonarsource', password=db_password)
```

_**References**_&#x20;

|       |                                                                                                                                                                      |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CWE   | [https://cwe.mitre.org/data/definitions/521](https://cwe.mitre.org/data/definitions/521)                                                                             |
| OWASP | [https://owasp.org/Top10/A07\_2021-Identification\_and\_Authentication\_Failures/](https://owasp.org/Top10/A07\_2021-Identification\_and\_Authentication\_Failures/) |

