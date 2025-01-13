# OWASP Juice Shop Challenges
In this repository I am gradually making **my solutions to the OWASP Juice Shop challenges** available and presenting the security vulnerabilities behind them.  
  
The content is created as part of my training at the **Developer Academy** and is used exclusively for teaching purposes.  

## Table of Contents
1. <a href="#introducing-the-owasp-juice-shop">Introducing the OWASP Juice Shop</a>   
    - <a href="#what-is-the-owasp-juice-shop">What is a OWASP Juice Shop?</a>
    - <a href="#what-are-the-owasp-top-ten">What are the OWASP Top Ten?</a>
2. <a href="#what-vulnerabilities-lurk-in-web-applications">What vulnerabilities lurk in web applications?</a>  
    - <a href="#1-improper-input-validation">Improper Input Validation</a>
    - <a href="#2-broken-authentication">Broken Authentication</a>
    - <a href="#3-xss---cross-site-scripting">XSS - Cross-Site Scripting</a>
3. <a href="#list-of-solved-challenges-so-far">List of solved challenges so far</a> 


## Introducing the OWASP Juice Shop
### What is the OWASP Juice Shop?
The OWASP Juice Shop is not a juice shop and you can not buy juice or anything else there. But it keeps the promise of being a really bad online shop. So bad that you can even learn something - about **Web Application Security Risks**.

> OWASP Juice Shop is probably the most modern and sophisticated insecure web application! It can be used in security trainings, awareness demos, CTFs and as a guinea pig for security tools! Juice Shop encompasses vulnerabilities from the entire OWASP Top Ten along with many other security flaws found in real-world applications!
<a href="https://owasp.org/www-project-juice-shop/">More Information</a>.

You want to solve the tasks by yourself? <a href="https://github.com/juice-shop/juice-shop">More Information</a>

### What are the OWASP Top Ten?
The OWASP Top Ten is a list of the most common and critical web application security risks. It is created and regularly updated by the Open Web Application Security Project (OWASP) community. The list serves as a guide for developers, security analysts and companies to improve the security of their applications.

> Companies should adopt this document and start the process of ensuring that their web applications minimize these risks. Using the OWASP Top 10 is perhaps the most effective first step towards changing the software development culture within your organization into one that produces more secure code.
<a href="https://owasp.org/www-project-top-ten/">More Information</a>.



## What vulnerabilities lurk in web applications?
The information about the security vulnerabilities was provided by **ChatGPT**.  
  
You will find the links to the appropriate challenges in the text.

### <ins>1) Improper Input Validation</ins>
**Improper Input Validation** is a security vulnerability that occurs when a system does not adequately or correctly validate user inputs before processing them. The lack of proper validation can lead to errors, unexpected behavior, or exploitation by malicious actors. By implementing robust input validation, many attack vectors can be mitigated early, as malicious inputs will be identified and blocked before they reach sensitive parts of the system.

### Potential Impacts
1) **Injection Attacks**:
    - SQL Injection: Inserting malicious SQL commands.
    - Command Injection: Executing system commands.
    - <ins>Privilege Escalation</ins>: Using crafted input to assign higher privileges (e.g. admin rights).
        - **Challenge**: <a href="https://github.com/SarahZimmermann-Schmutzler/juice_shop_challenges/blob/main/admin_registration">Admin Registration</a>
2) **Cross-Site Scripting (XSS)**:
    - Injecting malicious JavaScript into web applications.
3) **Buffer Overflow**:
    - Exploiting memory limits by exceeding allocated buffer sizes.
4) **Path Traversal**:
    - Accessing unauthorized files by manipulating file paths.
5) **Denial of Service (DoS)**:
    - Overloading or crashing a system with large or malicious inputs.

### Examples of Improper Input Validation:
- **Missing Length Checks**: Allowing excessively long inputs that can cause buffer overflows.
- **Unfiltered Special Characters**: Accepting characters like ', ", <, and > that can be exploited for injections or XSS.
- **Direct Use of User Input**: Passing input directly to databases, files, or APIs without validation.

### Mitigation and Prevention:
1) **Whitelist Validation**: Only allow specific, expected input values.
2) **Escape and Sanitize Input**:
    - Use escaping for SQL, HTML, XML, etc.
3) **Limit Input Size**:
    - Define maximum length and valid ranges for input data.
4) **Use Secure APIs**:
    - Use prepared statements for database interactions.
5) **Regular Security Testing**:
    - Conduct code reviews and penetration testing to identify vulnerabilities.
6) **Utilize Secure Frameworks**:
    - Modern frameworks often include built-in mechanisms to prevent improper input handling.

### <ins>2) Broken Authentication</ins>
**Broken Authentication** is a security vulnerability where an application’s authentication mechanisms are poorly implemented or configured, allowing attackers to take over user accounts or access protected resources. This vulnerability is listed in the OWASP Top 10 and represents a serious threat to web applications.  
A successful attack on authentication mechanisms can grant attackers full access to user accounts and their sensitive data. In the worst-case scenario, an attacker could gain administrative privileges and compromise the entire application.

### Causes of Broken Authentication
1) **Weak Password Implementation**:
    - Allowing weak or easily guessable passwords.
    - No enforcement of password complexity or length requirements.
2) **Lack of Protection Against Brute-Force or Credential Stuffing Attacks**:
    - No limit on login attempts.
    - No delay for repeated failed login attempts.
    - Allowing the reuse of passwords from compromised databases.
3) **Insecure Password Storage**:
    - Storing passwords in plaintext.
    - Using weak hashing algorithms like MD5 or SHA-1.
4) **Session Management Weaknesses**:
    - Session IDs are not invalidated after logout or timeout.
    - Session IDs are predictable or easy to guess.
    - No protection against session hijacking (e.g., missing Secure or HttpOnly flags on cookies).
5) **Insecure Password Recovery Mechanisms**:
    - <ins>Easily guessable security questions or their answers</ins>.
        - **Challenge**: <a href="https://github.com/SarahZimmermann-Schmutzler/juice_shop_challenges/blob/main/bjoern's_favorite_pet">Björn's Favorite Pet</a>
    - Password reset links with no expiration or insufficient security measures.

### Possible Attacks
1) **Credential Stuffing**:
    - Attackers use a list of usernames and passwords from data breaches to log into an application.
2) **Brute-Force Attacks**:
    - Attackers systematically try different passwords until the correct one is found.
3) **Session Hijacking**:
    - Attackers steal an active session ID to gain access to a victim’s account.
4) **Man-in-the-Middle Attacks**:
    - Credentials are transmitted insecurely over unencrypted channels (e.g., HTTP instead of HTTPS).
5) **Password Reset Manipulation**:
    - Attackers guess security questions or manipulate password reset mechanisms.

### Preventive Measures Against Broken Authentication
1) Enforce Strong Password Policies:
    - Require password complexity and a minimum length.
    - Enable multi-factor authentication (MFA).
2) Limit Login Attempts:
    - Lock accounts after several failed attempts.
    - Introduce delays for repeated requests (rate-limiting).
3) Secure Password Storage:
    - Hash passwords using strong algorithms such as bcrypt, Argon2, or PBKDF2.
    - Add salts to prevent rainbow table attacks.
4) Secure Session Management:
    - Invalidate session IDs after logout or timeout.
    - Set cookies with Secure, HttpOnly, and SameSite flags.
5) Defend Against Credential Stuffing:
    - Check passwords against known breach databases (e.g., using the Have I Been Pwned API).
    - Use captchas or additional authentication steps.
6) Monitoring and Logging:
    - Detect and respond to unusual login patterns (e.g., multiple failed login attempts).
7) Regular Security Testing:
    - Conduct penetration tests to identify and fix vulnerabilities.

### <ins>3) XSS - Cross-Site Scripting</ins>
**XSS (Cross-Site Scripting)** is a common security vulnerability in web applications that allows attackers to inject malicious code (often JavaScript) into web pages viewed by other users. This vulnerability arises due to inadequate validation or sanitization of user input.

### How XSS Works: Source and Sink
- **Source**: The entry point for untrusted data. This can include URL parameters, form fields, HTTP headers, cookies, or any user-provided input.
- **Sink**: The execution point where the untrusted data is improperly handled or rendered. Examples include inserting data directly into the DOM, using eval(), or rendering it in an HTML context.

### Types of XSS
1) Reflected XSS (Non-Persistent):
    - The malicious code originates from the Source (e.g., user input or query parameters) and is immediately reflected in the server's response without being stored.
        - <ins>Example</ins>: A search field that displays the entered query without filtering it before rendering in the HTML.
        
2) Stored XSS (Persistent):
    - The malicious code is stored on the server (e.g., in a database) and delivered to users every time they view the affected page. Here, the Source is the attacker's input, and the Sink is where the code is rendered for users.
        - <ins>Example</ins>: A forum post where an attacker embeds a script tag that executes when others view the post.
        - **Challenge**: <a href="https://github.com/SarahZimmermann-Schmutzler/juice_shop_challenges/blob/main/client_side_xss_protection">Client-side XSS Protection</a>

3) DOM-Based XSS:
    - This type of XSS occurs entirely in the client-side browser. The attack manipulates the Source (e.g., URL parameters or client-side variables) and leverages a vulnerable Sink (e.g., dynamically updating the DOM) to execute malicious code.
        - <ins>Example</ins>: A JavaScript function that takes unvalidated user input from the URL and directly injects it into the DOM.

### Risks of XSS
- **Session Cookie Theft**: Attackers can steal cookies to impersonate the victim.
- **Content Manipulation**: Attackers can alter web page content or redirect users to phishing pages.
- **Execution of Malicious Actions**: Attackers can perform actions on behalf of the victim, such as submitting forms or making unauthorized requests.
- **Malware Distribution**: XSS can be used to spread malware by injecting malicious scripts.

### Mitigating XSS
1) Input Validation and Sanitization:
    - Validate user input at the Source and sanitize it to remove malicious content.
    - Use escaping and encoding (e.g., convert < to &lt;) before rendering data in the Sink.

2) Content Security Policy (CSP):
    - Implement a CSP to restrict the execution of unauthorized scripts or loading of untrusted resources.

3) Contextual Output Encoding:
    - Use encoding mechanisms appropriate for the Sink (e.g., HTML encoding for HTML contexts, JavaScript escaping for JavaScript contexts).

4) Security Libraries:
    - Use libraries such as OWASP’s ESAPI or built-in framework functions to handle data safely between Source and Sink.

5) Cookie Security:
    - Ensure cookies are set with HttpOnly and Secure flags to reduce the impact of XSS.

6) Regular Security Testing:
    - Conduct automated and manual penetration tests to identify vulnerabilities in the flow from Source to Sink.


## List of solved challenges so far
### <ins>Three-Star-Challenges</ins>

| Name | Category | Description | Link |
| ---- | ------- | ----------- | -------- |
| Admin Registration | Improper Input Validation | Register as a user with administrator privileges. | <a href="https://github.com/SarahZimmermann-Schmutzler/juice_shop_challenges/blob/main/admin_registration">Admin Registration</a> |
| Björn's Favorite Pet | Broken Authentication | Reset the password of Bjoern's OWASP account via the Forgot Password mechanism with the original answer to his security question. | <a href="https://github.com/SarahZimmermann-Schmutzler/juice_shop_challenges/blob/main/bjoern's_favorite_pet">Björn's Favorite Pet</a> |
| Client-side XSS Protection | XSS | Perform a persisted XSS attack with ```<iframe src=\"javascript:alert(`xss`)\">``` bypassing a client-side security mechanism. | <a href="https://github.com/SarahZimmermann-Schmutzler/juice_shop_challenges/blob/main/client_side_xss_protection">Client-side XSS Protection</a> |