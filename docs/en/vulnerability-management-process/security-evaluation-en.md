###Severity assessment – how we score vulnerabilities

This topic describes the severity assessment solutions and typical cases used by openEuler to address security issues.

##Definition of security issues

Security problems are a type of vulnerability, which refers to the flaws or weaknesses in the design, implementation, operation, and management of the system, which can be exploited by external threats to violate the security policy of the system and affect the confidentiality, integrity, and availability of the system. An attacker can exploit these vulnerabilities to deny a user access to computing resources or execute arbitrary code on a user's computer. Because viruses and worms can exploit these vulnerabilities to spread from computer to computer, security issues pose a significant risk to users. Security issues can be broken down into two basic types:


    + Denial of Service
    + Arbitrary code execution is allowed

#Denial of Service

Denial of access to computing resources is a denial of service security issue, such as crashing a running program or consuming a lot of memory, disk space, or CPU time. However, it is important to note that the program contains a variety of issues that can sometimes cause the program to crash, but if the crash is not reproducible in a consistent way, then these problems will not be considered a denial of service security issue. The security issue of denial of service is something that can be "consistently" and "reliably" reproducible, and an attacker can use it to deliberately crash a running program.

#A typical case of a denial-of-service security issue

   + A specially crafted email that crashes the client
   + The specific network data that crashes the caregiver
   + HTML The specific HTML that crashes the browser

#A typical case of non-denial-of-service security issues

   + Crashing the text editor by performing very complex user input operations
   + Crashing a computer through an action or action of physical access

##Arbitrary code execution is allowed

In general, allowing arbitrary code execution poses more security problems than denial of service. Allowing arbitrary code execution may allow a malicious user to gain control of a computer, or allow the spread of viruses or worms. During the normal execution of a program, there will be a predictable execution process, but a malicious input program may change the execution process and run code of the attacker's choice.

   + Remote code execution: Whenever a computer is connected to a network, it is possible for a remote att     ack  er to remotely exploit arbitrary code execution issues to execute malicious code. This means tha     tthe at  tacker does not have to approach or log in to the compromised computer. For example, worms i     nfect countl  ess computers connected to the Internet through the Internet. Malicious crackers also o     ften exploit remo  te code execution issues to gain unauthorized access to computer systems.

   + Local code execution: A local code execution issue that can cause an attacker to run code when a user     ex  ecutes a specific command, or to escalate privileges by exploiting a SUID program. 

In general, these issues are found in a variety of commands, but if the user needs to be asked to execute certain commands, then these errors are not considered a security issue. For example, a local command execution requires elevated privileges to run code.

##openEuler security issue level

openEuler's product security issue rating uses the Common Vulnerability Scoring System (CVSS) to evaluate the impact of security issues found in openEuler. A prioritized risk assessment can help you understand and upgrade your systems so you can make informed decisions about these issues that are right for your operating environment.

##Common Vulnerability Score (CVSS)

The Common Vulnerability Scoring System (CVSS) provides guidance for the basic scoring of vulnerabilities and describes the severity in detail by scoring various aspects of the vulnerability, including: access, access complexity, confidentiality, integrity, and availability. CVSS levels are used as a criterion to identify key indicators of defects, but openEuler does not just use CVSS levels to prioritize bug fixes. For the priority of those bugs that have been fixed, openEuler will refer to the four-point scale to give a score based on the overall impact.

The CVSS v3 Foundation Metrics cover all aspects of vulnerability impact assessment

   + Attack Vector (AV): The "remote capability" of the attack and how the vulnerability can be exploited      under that capability
    
   + Attack Complexity (AC): The difficulty of the attack and the difficulty of obtaining the factors requ     ired for a successful attack
    
   + User Interaction (UI): Determines whether an attack is automated or requires human involvement
    
   + Required Permissions (PR): The level of user authentication required for the attack to succeed
    
   + Scope(s): Determines whether an attacker can affect components with different permission levels
    
   + Confidentiality (C): Determine whether data can be disclosed to non-authorized parties; If so, to wha     tlevel
    
   + Integrity (I): A measure of the trustworthiness of the data and the trustworthiness that the data wil     l not be modified by unauthorized users
    
   + Availability (A): Whether authorized users are available when they need access to data or services Th     e formula converts these metrics into a single numerical score ranging from 0.0 (no risk) to 10.0 (hi     ghest risk). For a detailed description of the basic metrics, see Common Vulnerability Scoring System     v3.0: User Guide

##Differences between NVD and openEuler scores

Since software packages are provided by multiple vendors, and the CVSS basic score is related to the version number, method, platform, and compilation method of the software provided by these vendors, it is sometimes difficult for openEuler to directly use the scores of third-party vulnerability libraries (such as NVD), because these scores only provide a single CVSS base score for each vulnerability, which may vary greatly when applied to openEuler.

For example, because the Firefox application is also available for Microsoft Windows (users typically run Firefox with administrator privileges), the NVD rated Firefox's vulnerabilities as having the highest impact. However, the openEuler version uses a Low Impact score because Firefox generally runs as an unprivileged user. For these reasons, we recommend that you use the CVSS basic score provided by openEuler as much as possible.

If you believe that the CVSS v3 base score for a particular vulnerability is incorrect, please let us know and we will be happy to discuss the issue and update the score if needed

##Typical case description

To be added
