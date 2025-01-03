## Vulnerabilities
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
    - <ins>Easily guessable security questions</ins>.
        - **Challenge**: <a href="https://github.com/SarahZimmermann-Schmutzler/juice_shop_challenges/blob/main/bjoern's_favorite_pet">Björn's Favorite Pet</a>
    - Password reset links with no expiration or insufficient security measures.