# Security Committee

English | [简体中文](README.md)

This document describes the responsibilities, organizational structure, operation mode, and related processes of the Security Committee (SC).

## Mission

The openEuler SC is responsible for receiving and responding to openEuler security issue reports, provides community security guidance, and carries out community security governance. For details about the specific responsibilities and requirements, see [openEuler Community Security Assurance Policy Outline](docs/en/vulnerability-management-process/security-strategy-overview-en.md). Providing the most secure products and development environment for openEuler users is the SC mission.

## Responsibilities

+ Assistance in fixing vulnerabilities: Ensure that known vulnerabilities are fixed in a timely manner. Provide patches for software package maintainers to help users fix vulnerabilities before virus attack, including vulnerability detection and fixing tools.
+ Response to security issues: Respond to reported security issues, follow up the handling progress, and disclose the reported issues in the community based on the security issue disclosure policy.
+ Secure coding rules: Popularize secure coding rules. This is the goal of the security team. The security team strives to create documentation or development tools to help the developers prevent common pitfalls in the software development process. The security team will also answer questions encountered during development and use.
+ Participation in code review: The security team aims to help the development team proactively identify vulnerabilities through code reviews.

## Members

The list of SC members and their responsibilities are continuously maintained in [MEMBERS](docs/en/charter/MEMBERS-EN.md).

## Meeting Time

- Meetings are held from 4:00 to 5:30 every other week on Wednesdays through WeLink Meeting.

### How to Contact SC

SC is closely tied to product releases, working in with release managers to ensure that products are released securely. Please use the correct contact information to obtain timely response.

| Email                            | Type   | Purpose                                                        |
| -------------------------------------- | ------- | ------------------------------------------------------------ |
| openeuler-security@openeuler.org       | Private | openEuler security disclosure mailbox. This list is closely monitored and categorized by the PSC. For details, see [openEuler Security Disclosure](docs/en/vulnerability-management-process/security-disclosure-en.md).|
| release-managers-private@openeuler.org | Private | Private communication mailbox especially for release managers. For other users, please subscribe to openeuler-security@openeuler.org. To discuss security issues during the release, release managers must use this mailbox.|
| security-discuss-private@openeuler.org | Private | Private internal discussion mailbox of the SC. For other users, please subscribe to openeuler-security@openeuler.org.|

### Secure Release Process

For details about how to report security issues and obtain security patches, see [openEuler Security Disclosure](docs/en/vulnerability-management-process/security-disclosure-en.md).

For details about the security handling process and security policies of the openEuler community, see [Security Handling Process](docs/en/vulnerability-management-process/security-process-en.md).

## Code of Conduct

+ It is subject to the constraints of [openEuler Code of Conduct](https://gitcode.com/openeuler/community/blob/master/code-of-conduct.md).
+ It is subject to the constraints of [Code of Conduct for Members of the openEuler Security Committee](docs/zh/charter/security-committee-rules.md).

## Repository Structure Overview

The openEuler SC repository maintains files through the following structure. When committing code, please follow these conventions to place files in appropriate locations. If new folders need to be created, add basic descriptions in the `index.md` file.

```sh
security-committee
 ┣ assets
 ┣ docs
 ┣ sub-projects
 ┣ MEMBERS.md
 ┣ README.md
 ┗ security-strategy-overview.md
 ```

 + `assets`: stores non-engineering resource files, including images used in documentation and public keys of SC members.
 + `docs`: stores documentation describing community security processes, including community guidelines, vulnerability management, and reporting procedures. Files are categorized in both Chinese and English.
 + `sub-projects`: contains sub-projects operated/incubated by the SC. For projects involving engineering file saving or SIG (Special Interest Group) operations, store them in `sub-projects`. For documentation-only projects, use `docs`.
 + `root`: stores foundational information such as READMEs, community security policies, and member lists to help developers understand the big picture.

### Index Files

 In the repository structure, each subfolder such as `assets` or `docs` should maintain the `index.md` file at their root to describe file information about the respective folders.

## Security Committee Operated Projects

 The `sub-projects` folder contains projects operated and maintained by the SC. Currently maintained projects include the following:

 + `SecChain`: supply chain security maturity assessment model.
 + `security-configuration-benchmark`: openEuler security configuration benchmark.
