# Broken Access Control

---

Broken access controls represent a core class of vulnerability in which an application or system fails to enforce restrictions on 
sensitive data or functionality. Attackers exploit this failure to reach resources that should remain off-limits, ranging from 
individual user accounts and files to databases and administrative interfaces. The underlying causes typically include design 
shortcomings, misconfigurations, or implementation errors in the access enforcement logic.

Access control itself is the mechanism that decides which users or processes may interact with a given resource. Common models 
include Discretionary Access Control (DAC), where the resource owner grants permissions at will; Mandatory Access Control (MAC), 
which applies system-enforced policies independent of owner preference; Role-Based Access Control (RBAC), which assigns privileges 
according to organizational roles; and Attribute-Based Access Control (ABAC), which evaluates a combination of attributes such as role,
time, location, and device. Each model can be undermined when enforcement is incomplete or inconsistent.

Typical manifestations of broken access control include horizontal privilege escalation (a user reaches another user’s resources at 
the same privilege level, for example by altering a user identifier in a URL), vertical privilege escalation (a low-privilege user 
reaches higher-privilege functions, for instance by modifying a hidden field or parameter), insufficient access-control checks that 
are skipped or applied inconsistently, and insecure direct object references that expose predictable identifiers. These weaknesses can
be mitigated only by rigorous enforcement, continuous review, and targeted testing.

In the TryHackMe laboratory environment the target instance is started from the room interface and becomes reachable at 
http://<TARGET_IP>/ after a short initialization period. The application presents registration, login, and dashboard pages. 
Registration creates an ordinary user account; subsequent login produces a JSON response containing status, message, name fields, 
an is_admin flag, and a redirect_link that points to dashboard.php with an isadmin parameter. Traffic interception with Burp Suite 
reveals that the application runs on Debian Linux under Apache 2.4.38 with PHP 8.0.19 and that no security headers are present. The
isadmin parameter is therefore an obvious candidate for manipulation.

Changing the intercepted isadmin value from false to true causes the application to redirect to the otherwise hidden admin.php page,
granting a low-privilege session administrative visibility. From that page the “Admin access” checkbox for the registered account 
can be enabled, completing a vertical privilege escalation that allows revocation of other administrators’ privileges.

Prevention centers on consistent server-side enforcement of the principle of least privilege. Role definitions should map explicit
permissions to each role, parameterized queries must separate SQL syntax from user data, session lifetimes and cookie attributes must
be tightly controlled, and all input must be validated and sanitized while insecure functions such as md5 are avoided in favor of 
password_hash. Developers may consult the OWASP PHP Configuration Cheat Sheet at 
https://cheatsheetseries.owasp.org/cheatsheets/PHP_Configuration_Cheat_Sheet.html, the security section of PHP The Right Way at 
https://phptherightway.com/#security, and established secure-coding guidance for PHP.

Horizontal escalation enables lateral movement within the same privilege tier, while vertical escalation can confer full 
administrative control and, in extreme cases, compromise of the entire environment. Continuous monitoring for unauthorized activity
remains essential.

---

| Description | Code/Command |
| --- | --- |
| Role and permission definition with permission check | $roles = [ 'admin' => ['create', 'read', 'update', 'delete'], 'editor' => ['create', 'read', 'update'], 'user' => ['read'], ]; function hasPermission($userRole, $requiredPermission) { global $roles; return in_array($requiredPermission, $roles[$userRole]); } if (hasPermission('admin', 'delete')) { // Allow delete operation } else { // Deny delete operation } |
| Vulnerable SQL query using direct interpolation | $username = $_POST['username']; $password = $_POST['password']; $query = "SELECT * FROM users WHERE username='$username' AND password='$password'"; |
| Secure query using prepared statements | $username = $_POST['username']; $password = $_POST['password']; $stmt = $pdo->prepare("SELECT * FROM users WHERE username=? AND password=?"); $stmt->execute([$username, $password]); $user = $stmt->fetch(); |
| Session initialization, variable setting, and expiry check | session_start(); $_SESSION['user_id'] = $user_id; $_SESSION['last_activity'] = time(); if (isset($_SESSION['last_activity']) && (time() - $_SESSION['last_activity'] > 1800)) { session_unset(); session_destroy(); } |
| Input sanitization and secure password hashing | $username = filter_input(INPUT_POST, 'username', FILTER_SANITIZE_STRING); $password = filter_input(INPUT_POST, 'password', FILTER_SANITIZE_STRING); // Avoid: $password = md5($password); $password = password_hash($password, PASSWORD_DEFAULT); |

---

### Key Takeaways
- Understand the definition and business impact of broken access control.
- Identify the vulnerability through systematic examination of web-application functionality and traffic.
- Exploit the weakness in a controlled laboratory setting by intercepting and modifying the isadmin parameter and elevating privileges
   via the administrative interface.
- Apply defensive measures that include role-based permission checks, parameterized queries, disciplined session handling, and secure
   coding practices.
- Regularly review and test all access-control logic to confirm that enforcement remains effective.

---




