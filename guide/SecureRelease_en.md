
## Network Security Quality Requirements for Releases
To ensure the network security quality of the releases, we recommend that you complete the following security-related checks before a release is released

**Catalog**

+ [Pass the Virus Scan](#pass-the-virus-scan)
+ [Pass Vulnerability Assessment](#pass-vulnerability-scan)
+ [Pass Secure Compilation Options Scan](#pass-secure-compilation-options-scan)
+ [Pass Security Test Baseline Casees Validation](#pass-security-test-baseline-casees-validation)
+ [Pass Open Source Code Snippets Citation Scan](#pass-open-source-code-snippets-citation-scan)
+ [Pass Compliance Check of Open Source Software Licenses](#pass-compliance-check-of-open-source-software-licenses)


### Pass the Virus Scan

**Objective**

Make sure that the release does not contain any virus.

**Adopted Criteria**

There must be no suspected virus in the virus scan results. If there is a suspected virus, it needs to be cleaned up or clarified as a false positive.

**Object**
Virus scan report and possible clarification documentation

**Provider**

Tester Group

(If there is a suspected virus in the scan results, the development team is responsible for solving the problem or clarifying the false alarm.)

**Inspector**

Security Committee



### **Pass Vulnerability Assessment**

**Objective**

Make sure that all vulnerabilities in the released version are fixed or assessed.

**Adopted Criteria**

1. All vulnerabilities that can be repaired must be fixed

2. Vulnerabilities that cannot be repaired need to be reviewed and filed by the Security Committee (including CVE vulnerabilities that have no solution in the industry)

**Object**
Vulnerability list, all vulnerabilities are in the fixed or filed state

**Provider**

Tester Group

(If there are vulnerabilities that have not been fixed in the scan results, the development team needs to fix them or explain the reason why they cannot be repaired to the Security Committee for review and filing.)

**Inspector**

Security Committee



### **Pass Secure Compilation Options Scan**

**Objective**

Apply secure compilation options to reduce security risks

**Adopted Criteria**

1. The security compilation option "Requirement Class" in the scan results has been implemented, and the "Recommendation Class" is not required.

2. The requirements that are not suitable for modification shall be reviewed and filed by the Security Committee.

**Object**
Secure Compilation Options Scanning Report and posible Record Information

**Provider**

Tester Group

(If there is a security compilation option that has not been applied in the scan results, the development team needs to modify it, and if it is not suitable for modification, prepare materials to the Security Committee for review and filing.)

**Inspector**

Security Committee



### Pass Security Test Baseline Casees Validation

**Objective**

Ensure that the security test baseline cases passes

**Adopted Criteria**

All security test baseline use cases are executed

**Object**
Conclusion of the test report

**Provider**

Tester Group

**Inspector**

Security Committee



### Pass Open Source Code Snippets Citation Scan

**Objective**

There is no open source code snippets citation to the source code of the released version

**Adopted Criteria**

There is no open source code snippets ciatation issue after the scan result is confirmed.

**Object**
The open source code snippets ciatation to the scan results and confirmation results

**Provider**

Tester Group

**Inspector**

Compliance SIG Group



### Pass Compliance Check of Open Source Software Licenses

**Objective**

Ensure that all software in the release package has an open source software usage statement and that the content is correct

**Adopted Criteria**

All open source software in the release package has a corresponding open source software usage statement, and the content is correct

**Object**
Open source software usage declaration file

**Provider**

Development Group

**Inspector**

Compliance SIG Group
