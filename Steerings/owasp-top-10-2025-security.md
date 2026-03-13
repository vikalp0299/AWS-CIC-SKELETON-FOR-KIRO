---
inclusion: auto
---

# OWASP Top 10 2025 Security Guidelines

This steering file provides security guidance based on the OWASP Top 10 2025. Follow these guidelines when writing, reviewing, or modifying code.

## A01:2025 - Broken Access Control

### Description
Access control failures allow users to act outside their intended permissions, leading to unauthorized data access, modification, or deletion. Common issues: bypassing checks via URL tampering, insecure direct object references, missing API access controls, privilege escalation, JWT manipulation, CORS misconfiguration, and force browsing.

### How to Prevent
- Except for public resources, deny by default.
- Implement access control mechanisms once and reuse them throughout the application, including minimizing Cross-Origin Resource Sharing (CORS) usage.
- Model access controls should enforce record ownership rather than allowing users to create, read, update, or delete any record.
- Unique application business limit requirements should be enforced by domain models.
- Disable web server directory listing and ensure file metadata (e.g., .git) and backup files are not present within web roots.
- Log access control failures, alert admins when appropriate (e.g., repeated failures).
- Implement rate limits on API and controller access to minimize the harm from automated attack tooling.
- Stateful session identifiers should be invalidated on the server after logout. Stateless JWT tokens should be short-lived to minimize the window of opportunity for an attacker. For longer-lived JWTs, consider using refresh tokens and following OAuth standards to revoke access.
- Use well-established toolkits or patterns that provide simple, declarative access controls.

---

## A02:2025 - Security Misconfiguration

### Description
Systems configured incorrectly from a security perspective create vulnerabilities. Issues include: missing security hardening, unnecessary features enabled, default credentials unchanged, verbose error messages, disabled security features, insecure framework/library settings, and missing security headers.

### How to Prevent
- A repeatable hardening process enabling the fast and easy deployment of another environment that is appropriately locked down. Development, QA, and production environments should all be configured identically, with different credentials used in each environment. This process should be automated to minimize the effort required to set up a new secure environment.
- A minimal platform without any unnecessary features, components, documentation, or samples. Remove or do not install unused features and frameworks.
- A task to review and update the configurations appropriate to all security notes, updates, and patches as part of the patch management process (see A03 Software Supply Chain Failures). Review cloud storage permissions (e.g., S3 bucket permissions).
- A segmented application architecture provides effective and secure separation between components or tenants, with segmentation, containerization, or cloud security groups (ACLs).
- Sending security directives to clients, e.g., Security Headers.
- An automated process to verify the effectiveness of the configurations and settings in all environments.
- Proactively add a central configuration to intercept excessive error messages as a backup.
- If these verifications are not automated, they should be manually verified annually at a minimum.
- Use identity federation, short-lived credentials, or role-based access mechanisms provided by the underlying platform instead of embedding static keys or secrets in code, configuration files, or pipelines.

---

## A03:2025 - Software Supply Chain Failures

### Description
Breakdowns in building, distributing, or updating software caused by vulnerabilities or malicious changes in third-party code, tools, or dependencies. Risks include: untracked component versions, vulnerable/outdated software, lack of vulnerability scanning, weak supply chain hardening, no separation of duties, untrusted component sources, and insecure CI/CD pipelines.

### How to Prevent
- Centrally generate and manage the Software Bill of Materials (SBOM) of your entire software.
- Track not just your direct dependencies, but their (transitive) dependencies, and so on.
- Reduce attack surface by removing unused dependencies, unnecessary features, components, files, and documentation.
- Continuously inventory the versions of both client-side and server-side components (e.g., frameworks, libraries) and their dependencies using tools like OWASP Dependency Track, OWASP Dependency Check, retire.js, etc.
- Continuously monitor sources like Common Vulnerability and Exposures (CVE), National Vulnerability Database (NVD), and Open Source Vulnerabilities (OSV) for vulnerabilities in the components you use. Use software composition analysis, software supply chain, or security-focused SBOM tools to automate the process. Subscribe to alerts for security vulnerabilities related to components you use.
- Only obtain components from official (trusted) sources over secure links. Prefer signed packages to reduce the chance of including a modified, malicious component (see A08:2025-Software and Data Integrity Failures).
- Deliberately choose which version of a dependency you use and upgrade only when there is need.
- Monitor for libraries and components that are unmaintained or do not create security patches for older versions. If patching is not possible, consider migrating to an alternative. If that is not possible, consider deploying a virtual patch to monitor, detect, or protect against the discovered issue.
- Update your CI/CD, IDE, and any other developer tooling regularly
- Avoid deploying updates to all systems simultaneously. Use staged rollouts or canary deployments to limit exposure in case a trusted vendor is compromised.
- Harden code repositories (no secrets, protect branches, backups), developer workstations (patching, MFA, monitoring), build servers & CI/CD (separation of duties, access control, signed builds, environment-scoped secrets, tamper-evident logs), artifacts (integrity via provenance, signing, time stamping), and infrastructure as code (managed like all code with PRs and version control).

---

## A04:2025 - Cryptographic Failures

### Description
Failures related to lack of cryptography, weak cryptography, or leaked cryptographic keys. All data in transit should be encrypted at transport layer. Sensitive data (passwords, credit cards, health records, PII) requires extra protection at rest and in transit. Issues include: weak algorithms, default/weak keys, keys in source code, missing encryption enforcement, improper certificate validation, insecure IVs, weak randomness, deprecated hash functions (MD5, SHA1), and exploitable crypto errors.

### How to Prevent
- Classify and label data processed, stored, or transmitted by an application. Identify which data is sensitive according to privacy laws, regulatory requirements, or business needs.
- Store your most sensitive keys in a hardware or cloud-based HSM.
- Use well-trusted implementations of cryptographic algorithms whenever possible.
- Don't store sensitive data unnecessarily. Discard it as soon as possible or use PCI DSS compliant tokenization or even truncation. Data that is not retained cannot be stolen.
- Make sure to encrypt all sensitive data at rest.
- Ensure up-to-date and strong standard algorithms, protocols, and keys are in place; use proper key management.
- Encrypt all data in transit with protocols >= TLS 1.2 only, with forward secrecy (FS) ciphers, drop support for cipher block chaining (CBC) ciphers, support quantum key change algorithms. For HTTPS enforce encryption using HTTP Strict Transport Security (HSTS). Check everything with a tool.
- Disable caching for responses that contain sensitive data. This includes caching in your CDN, web server, and any application caching (eg: Redis).
- Apply required security controls as per the data classification.
- Do not use unencrypted protocols such as FTP, and STARTTLS. Avoid using SMTP for transmitting confidential data.
- Store passwords using strong adaptive and salted hashing functions with a work factor (delay factor), such as Argon2, yescrypt, scrypt or PBKDF2-HMAC-SHA-512. For legacy systems using bcrypt, get more advice at OWASP Cheat Sheet: Password Storage
- Initialization vectors must be chosen appropriate for the mode of operation. This could mean using a CSPRNG (cryptographically secure pseudo random number generator). For modes that require a nonce, the initialization vector (IV) does not need a CSPRNG. In all cases, the IV should never be used twice for a fixed key.
- Always use authenticated encryption instead of just encryption.
- Keys should be generated cryptographically randomly and stored in memory as byte arrays. If a password is used, then it must be converted to a key via an appropriate password base key derivation function.
- Ensure that cryptographic randomness is used where appropriate and that it has not been seeded in a predictable way or with low entropy. Most modern APIs do not require the developer to seed the CSPRNG to be secure.
- Avoid deprecated cryptographic functions, block building methods and padding schemes, such as MD5, SHA1, Cipher Block Chaining Mode (CBC), PKCS number 1 v1.5.
- Ensure settings and configurations meet security requirements by having them reviewed by security specialists, tools designed for this purpose, or both.
- You need to prepare now for post quantum cryptography (PQC), see reference (ENISA) so that high risk systems are safe no later than the end of 2030.

---

## A05:2025 - Injection

### Description
Application flaws allowing untrusted user input to be sent to an interpreter (browser, database, command line) causing execution of that input as commands. Vulnerable when: user data not validated/filtered/sanitized, dynamic queries without parameterization, unsanitized ORM parameters, or hostile data directly concatenated. Common types: SQL, NoSQL, OS command, ORM, LDAP, EL/OGNL injection, and XSS.

### How to Prevent
- The preferred option is to use a safe API, which avoids using the interpreter entirely, provides a parameterized interface, or migrates to Object Relational Mapping Tools (ORMs). Note: Even when parameterized, stored procedures can still introduce SQL injection if PL/SQL or T-SQL concatenates queries and data or executes hostile data with EXECUTE IMMEDIATE or exec().
- Use positive server-side input validation. This is not a complete defense as many applications require special characters, such as text areas or APIs for mobile applications.
- For any residual dynamic queries, escape special characters using the specific escape syntax for that interpreter. Note: SQL structures such as table names, column names, and so on cannot be escaped, and thus user-supplied structure names are dangerous. This is a common issue in report-writing software.

---

## A06:2025 - Insecure Design

### Description
Missing or ineffective security control design. Different from insecure implementation - design flaws cannot be fixed by perfect implementation as needed security controls were never created. Caused by lack of business risk profiling and failure to determine required security design level. Requires: gathering requirements and resource management, creating secure design, and having secure development lifecycle.

### How to Prevent
- Establish and use a secure development lifecycle with AppSec professionals to help evaluate and design security and privacy-related controls
- Establish and use a library of secure design patterns or paved-road components
- Use threat modeling for critical parts of the application such as authentication, access control, business logic, and key flows
- User threat modeling as an educational tool to generate a security mindset
- Integrate security language and controls into user stories
- Integrate plausibility checks at each tier of your application (from frontend to backend)
- Write unit and integration tests to validate that all critical flows are resistant to the threat model. Compile use-cases and misuse-cases for each tier of your application.
- Segregate tier layers on the system and network layers, depending on the exposure and protection needs
- Segregate tenants robustly by design throughout all tiers

---

## A07:2025 - Authentication Failures

### Description
Attackers trick systems into recognizing invalid users as legitimate. Weaknesses include: permitting credential stuffing/brute force attacks, allowing default/weak passwords, accepting breached credentials, weak password recovery, plain text/weakly hashed passwords, missing/ineffective MFA, session ID exposure in URLs, session ID reuse after login, improper session/token invalidation, and incorrect credential scope assertion.

### How to Prevent
- Where possible, implement and enforce use of multi-factor authentication to prevent automated credential stuffing, brute force, and stolen credential reuse attacks.
- Where possible, encourage and enable the use of password managers, to help users make better choices.
- Do not ship or deploy with any default credentials, particularly for admin users.
- Implement weak password checks, such as testing new or changed passwords against the top 10,000 worst passwords list.
- During new account creation and password changes validate against lists of known breached credentials (eg: using haveibeenpwned.com).
- Align password length, complexity, and rotation policies with National Institute of Standards and Technology (NIST) 800-63b's guidelines in section 5.1.1 for Memorized Secrets or other modern, evidence-based password policies.
- Do not force human beings to rotate passwords unless you suspect breach. If you suspect breach, force password resets immediately.
- Ensure registration, credential recovery, and API pathways are hardened against account enumeration attacks by using the same messages for all outcomes ("Invalid username or password.").
- Limit or increasingly delay failed login attempts but be careful not to create a denial of service scenario. Log all failures and alert administrators when credential stuffing, brute force, or other attacks are detected or suspected.
- Use a server-side, secure, built-in session manager that generates a new random session ID with high entropy after login. Session identifiers should not be in the URL, be securely stored in a secure cookie, and invalidated after logout, idle, and absolute timeouts.
- Ideally, use a premade, well-trusted system to handle authentication, identity, and session management. Transfer this risk whenever possible by buying and utilizing a hardened and well tested system.
- Verify the intended use of provided credentials, e.g. for JWTs validate aud, iss claims and scopes

---

## A08:2025 - Software or Data Integrity Failures

### Description
Code and infrastructure not protecting against invalid/untrusted code or data being treated as trusted. Examples: relying on plugins/libraries from untrusted sources/CDNs, insecure CI/CD without integrity checks, pulling unverified code/artifacts, auto-updates without integrity verification, and insecure deserialization of attacker-modifiable data.

### How to Prevent
- Use digital signatures or similar mechanisms to verify the software or data is from the expected source and has not been altered.
- Ensure libraries and dependencies, such as npm or Maven, are only consuming trusted repositories. If you have a higher risk profile, consider hosting an internal known-good repository that's vetted.
- Ensure that there is a review process for code and configuration changes to minimize the chance that malicious code or configuration could be introduced into your software pipeline.
- Ensure that your CI/CD pipeline has proper segregation, configuration, and access control to ensure the integrity of the code flowing through the build and deploy processes.
- Ensure that unsigned or unencrypted serialized data is not received from untrusted clients and subsequently used without some form of integrity check or digital signature to detect tampering or replay of the serialized data.

---

## A09:2025 - Security Logging and Alerting Failures

### Description
Without logging/monitoring, attacks cannot be detected; without alerting, quick response is difficult. Issues include: auditable events not logged consistently, inadequate log messages, unprotected log integrity, logs not monitored, local-only storage without backup, missing/ineffective alerting thresholds, no real-time attack detection, sensitive info in logs, log injection vulnerabilities, missing error logging, inadequate alert use cases, too many false positives, and incomplete playbooks.

### How to Prevent
- Ensure all login, access control, and server-side input validation failures can be logged with sufficient user context to identify suspicious or malicious accounts and held for enough time to allow delayed forensic analysis.
- Ensure that every part of your app that contains a security control is logged, whether it succeeds or fails.
- Ensure that logs are generated in a format that log management solutions can easily consume.
- Ensure log data is encoded correctly to prevent injections or attacks on the logging or monitoring systems.
- Ensure all transactions have an audit trail with integrity controls to prevent tampering or deletion, such as append-only database tables or similar.
- Ensure all transactions that throw an error are rolled back and started over. Always fail closed.
- If your application or its users behave suspiciously, issue an alert. Create guidance for your developers on this topic so they can code against this or buy a system for this.
- DevSecOps and security teams should establish effective monitoring and alerting use cases including playbooks such that suspicious activities are detected and responded to quickly by the Security Operations Center (SOC) team.
- Add 'honeytokens' as traps for attackers into your application e.g. into the database, data, as real and/or technical user identity. As they are not used in normal business, any access generates logging data that can be alerted with nearly no false positives.
- Behavior analysis and AI support could be optionally an additional technique to support low rates of false positives for alerts.
- Establish or adopt an incident response and recovery plan, such as National Institute of Standards and Technology (NIST) 800-61r2 or later. Teach your software developers what application attacks and incidents look like, so they can report them.

---

## A10:2025 - Mishandling of Exceptional Conditions

### Description
Programs failing to prevent, detect, and respond to unusual/unpredictable situations, leading to crashes, unexpected behavior, and vulnerabilities. Caused by: missing/poor input validation, late/high-level error handling, unexpected environmental states, inconsistent exception handling, or unhandled exceptions. Results in: logic bugs, overflows, race conditions, fraudulent transactions, memory/state/resource/timing/auth/authz issues affecting confidentiality, availability, and integrity.

### How to Prevent
- Catch every possible system error directly at the place where they occur and handle it (solve the problem and ensure recovery). Include throwing an error (inform user understandably), logging the event, and issuing alerts if justified.
- Have a global exception handler in place for anything missed.
- Implement monitoring/observability tooling that watches for repeated errors or attack patterns that could trigger response, defense, or blocking.
- If part way through a transaction, roll back every part and start again (fail closed). Never attempt to recover a transaction part way through.
- Add rate limiting, resource quotas, throttling, and other limits wherever possible to prevent exceptional conditions. Nothing should be limitless.
- Consider outputting identical repeated errors (above a certain rate) as statistics showing occurrence count and timeframe, appended to original message.
- Include strict input validation (with sanitization or escaping for hazardous characters that must be accepted).
- Use centralized error handling, logging, monitoring, and alerting with a global exception handler. Handle exceptional conditions in one place, the same way each time.
- Create project security requirements for this advice, perform threat modelling and/or secure design review in design phase, perform code review or static analysis, and execute stress, performance, and penetration testing.
- Ideally, handle exceptional conditions the same way organization-wide for easier review and audit.

---

## Source

Content was rephrased for compliance with licensing restrictions. Based on [OWASP Top 10 2025](https://owasp.org/Top10/2025/).
