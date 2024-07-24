## 10 Community Vulnerability Management

The openEuler community attaches great importance to the security of community editions and assigns vulnerability management specialists to take charge of vulnerability-related affairs.

The openEuler Security Committee has developed a vulnerability handling process and policies, including detection, confirmation and assessment, fixing, and disclosure of vulnerabilities. In addition, a project named cve-manager is created in the community to implement automatic or semi-automatic community vulnerability management. For example, the project can automatically detect vulnerabilities in upstream software, check whether the vulnerability handling process meets community requirements, and automatically generate security advisories (SAs) in Common Vulnerability Reporting Format (CVRF). The following figure shows the vulnerability handling process followed by the community.

![](picture/vulnerability-handling-process.png)

### 10.1 Vulnerability Handling Scope

The openEuler community adopts differentiated vulnerability handling policies for different types of software packages. For software packages comprising the BaseOS of LTS releases, the vulnerability fixing rate is dominant and must be ensured. For software packages not included in the BaseOS nor LTS releases, there is no requirement on the vulnerability fixing rate.

| **Software Package Type** | **Vulnerability Detection** | **Vulnerability Fixing**                      | **Vulnerability Disclosure**  |
|----------------|--------------|-----------------------------------|---------------|
| LTS releases        | Supported        | Steer vulnerability fixing for software packages in the BaseOS to reach the SLA. | SAs and CVEs   |
| Non-LTS releases      | Supported        | Not required.                        | No SA or CVE |

### 10.2 Vulnerability Receiving

The openEuler community receives community-related vulnerabilities from multiple channels:

For disclosed vulnerabilities, detection tools or community contributors obtain vulnerability information and create CVE issues on Gitee to track vulnerability handling.

For undisclosed vulnerabilities, vulnerability management specialists of the openEuler Security Committee will reply to the vulnerability submitters within 24 hours after receiving vulnerability reports.

| **Receiving Channel**  | **Reporting Method**   | **Description**                                                         |
|--------------|----------------|----------------------------------------------------------------------|
| CVE-manager  | CVE issue      | Synchronizes vulnerabilities in upstream open source software and submits CVE issues using the **openeuler-ci-bot** account.     |
| Community contributor   | CVE issue      | Identifies vulnerabilities in upstream open source software and submits CVE issues. For details, see [How to Submit a CVE Issue](https://gitee.com/openeuler/security-committee/wikis/FAQ/How%20to%20Submit%20a%20CVE%20Issue). |
| Security researcher   | Security Committee mailbox | Reports vulnerabilities through encrypted emails. For details, see [Vulnerability Reporting](https://www.openeuler.org/en/security/vulnerability-reporting/).                                |

### 10.3 Vulnerability Assessment

All vulnerabilities tracked by the community are assessed using CVSS v3 that is widely used in the industry.

### 10.4 Vulnerability Fixing

The openEuler community uses differentiated fixing policies with specific timeframes based on the vulnerability severity to ensure rapid fixing of high-risk vulnerabilities. The Security Committee will also identify high-risk vulnerabilities based on factors such as public impact and exploitation of vulnerabilities and fix them within one to three days to reduce the risk of malicious exploitation. Vulnerability fixing is a complex task. Therefore, we do not guarantee that all vulnerabilities can be fixed within the time listed in the following table. If a vulnerability cannot be fixed (for example, unavailable upstream patches, excessive fixing difficulty, or special circumstances), the maintainer needs to notify vulnerability management specialists of the openEuler Security Committee for review or clarification as required.

| **Severity Rating**  | **CVSS Score**  | **Vulnerability Fixing Timeframe**  |
|---------------------------------|-----------------------|------------------|
| Critical                | 9.0–10.0              | 7 days              |
| High                      | 7.0–8.9               | 14 days             |
| Medium                    | 4.0–6.9               | 30 days             |
| Low                       | 0.1–3.9               | 30 days             |

### 10.5 Vulnerability Disclosure

The openEuler community discloses critical 0-day vulnerabilities only to downstream distributors in the restricted disclosure list within an embargo, and releases SAs and CVEs only after collaborative fixing.

- Public disclosure

Once a disclosed security vulnerability is fixed, vulnerability management specialists of the openEuler Security Committee and the Release SIG will jointly release an SA, which includes the technical details, CVE ID, CVSS score, severity, affected releases, and releases to fix the vulnerability. You can subscribe to SAs in the openEuler community by clicking the **sa-announce** link.

- Restricted disclosure (optional)

For undisclosed critical vulnerabilities enabling remote exploitation or privilege escalation, vulnerability fixing solutions will be provided to downstream distributors through restricted disclosure channels.
