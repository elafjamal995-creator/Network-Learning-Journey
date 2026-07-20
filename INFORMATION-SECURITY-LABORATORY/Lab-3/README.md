# Introduction to MD5

MD5, short for **Message-Digest Algorithm 5**, is a legacy cryptographic hash function used to convert data into a fixed 32-character hexadecimal string (a digital fingerprint).

### Key Characteristics:
* **Fast Performance:** MD5 is extremely fast compared to modern hashing algorithms like SHA-256.
* **Security Status:** It is considered **deprecated and insecure** for sensitive cryptographic purposes. This is due to discovered vulnerabilities that allow **Collisions** (where two completely different files or inputs produce the exact same hash value).
* **Current Use Cases:** While it is no longer secure for passwords or high-security applications, it is still commonly used in low-risk scenarios, such as verifying data integrity (**Checksums**) to ensure a file wasn't corrupted during a basic network transfer
