# Guidelines for Security Design in openEuler Community
This specification references heavily a wide range of industry standards and best practices in open source software development, with the consideration of business development trends, formulates directions for security design of software modules in openEuler community, aiming to avoid high risks in software products.

# 1. Identification & Authentication
1.1 All machine-to-machine interfaces for cross-network transmission should have an authentication mechanism, and the authentication process should be performed on the server side.

**Note**: Cross-network interfaces need to support identity authentication to prevent attackers from spoofing.

1.2 A user should be identified and authenticated on the **server** side for each request to the server.

**Note**: Unauthorized URL access is a typical web security vulnerability. By exploiting this vulnerability, an attacker can easily bypass the system's security control and access system resources or invoke system functionalities without being authorized. In order to prevent users from requesting and executing some web pages beyond their authority by directly entering the URL, the server needs to authenticate the user when he/she requests a certain URL.

1.3 All data received from untrusted data sources are checked on the **server** side for validity by checking its size, type, length, and special characters; and any data that fails the verification should be rejected.

**Note**: It is required to prevent attackers from intercepting and tampering with requests through a proxy that is trying to bypass the validity verification for the clients, and hence all data verification should be performed on the server side.

1.4 There should be no functionalities that can bypass system security mechanisms (including authentication, access control, logging and auditing) to access the system or data on it.

**Note**: If there were a functionality that can bypass the system security mechanisms to access the system or data, once it get known by malicious persons, it would be exploited to perform unauthorized operations, causing dangerous impacts on the system and its users.

**Example**: Negative examples of this kind of bypass include hidden accounts, hidden passwords, hidden key combinations for system access, hidden protocols, hidden debugging commands/ports and other secret access methods, unmanaged user accounts and so on. A password is required by default to access openEuler systems even in single-user mode to prevent physically accessible attackers from obtaining root privilege.

1.5 According to the principle of least privilege, programs should be run with an account of as low as possible privileges.

**Note**: System accounts such as "root", "administrator", "supervisor" or other privileged accounts should not be used to run programs; use accounts with ordinary privileges instead.

# 2. Secure Transmission
2.1 To transmit sensitive data or key business data across networks, use secure transmission protocols, or encrypt the data before transmission.

**Note**: Data transmitted across networks is prone to be stolen or tampered with by attackers, and hence should be protected if the data is important.

**Example**: In project ***iSulad***, the user name and password are transmitted over HTTPS channel, encrypted before sending and decrypted after receiving.

2.2 Protocols including SSL2.0, SSL3.0, TLS1.0, and TLS1.1 should not be used for secure transmission; use TLS1.2 or TLS1.3 instead as they are more secure than their predecessors.

**Note**: SSL2.0 and SSL3.0 were banned by the IETF in March 2011 and June 2015 respectively, as they contain many security issues. The symmetric encryption algorithm used in TLS1.0 supports only RC4 and only in CBC mode of the block cipher algorithm. RC4 has been recognized as insecure and has been obsoleted by the IETF in all TLS versions. The CBC mode of the symmetric algorithm has the problem of predictable IV, thus vulnerable to the BEAST (Browser Exploit Against SSL/TLS) attack. Therefore, it is recommended to use TLS1.2 or TLS1.3.

# 3. Protection of sensitive/private data
3.1 Credentials (such as passwords, keys, etc.) should not be stored in the system as plaintext, and should be protected with encryption. If the credential need not be recovered to its plaintext, it should be encrypted using irreversible encryption algorithms; otherwise, it should be encrypted with reversible encryption algorithms and in secure encryption mode.

**Example**: AES256-CFB is used in project ***iSulad*** to encrypt passwords. SHA-512 is used instead by default in openEuler.

# 4. Encryption algorithms & key management
4.1 Do not use proprietary, non-standard cryptographic algorithms.

**Note**: There are several reasons for the prohibition. First, it is difficult to guarantee security strength of cryptographic algorithms designed by non-cryptography experts; Second, this kind of algorithms has not been technically analyzed and verified and may contain unknown flaws. Moreover, proprietary algorithms violate the design principle of openness and transparency. So, open-sourced cipher suites are recommended, such as OpenSSL, OpenSSH, etc.

4.2 Insecure cipher algorithms should not be used, use strong ones instead.

**Note**: With the development of cryptography technology and the improvement of computing power, some cipher algorithms are no longer suitable for today's security domains. Those weak algorithms include the symmetric encryption algorithms DES, RC4, etc., the asymmetric encryption algorithms RSA 1024, etc., the hash algorithms SHA-0, SHA-1, MD2, MD4, MD5, etc., key agreement algorithm DH-1024, etc. It is recommended to use AES-256, RSA-3072, SHA-256, PBKDF2 or stronger cryptographic algorithms instead.

**Example**: The default encryption algorithms used in OpenSSH server are: AES128-CTR, AES192-CTR, AES256-CTR, CHACHA20-POLY1305, AES128-GCM, and AES256-GCM.

4.3 The keys used to encrypt data should not be hardcoded in the code.

**Note**: Keys must be able to be replaced in order to reduce the risk of leakage during their long-term use.

**Example**: The ***iSulad*** project uses AES256, and the key can be changed in the configuration file.

4.4 The random numbers used in cryptographic algorithms should be cryptographically secure.

**Note**: Random numbers are used to generate IVs, salts, keys, etc., which are generally used in cryptographic algorithms. Insecure random numbers make keys, IVs, etc. fully or partially predictable, weakening algorithm strength. Cryptographically secure random numbers require that their unpredictability must be guaranteed. Secure random numbers should be generated by a secure random number generator, such as the RAND_bytes() function in the OpenSSL library, the device file /dev/random in the Linux operating system, and the random number generator (RNG) module in a TPM chip.
