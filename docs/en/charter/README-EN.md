# Security Committee

English | [简体中文](../../../README.md)

This document describes the responsibilities, organizational structure, operation mode, and related processes of the Security Committee.



## Mission

The openEuler Security Committee (SC) receives and responds to openEuler security issues, provides community security guidance, and carries out security governance. It is built to enhance the security of openEuler products and development environment.



## Responsibilities

+ Assist in fixing vulnerabilities: Ensure that known vulnerabilities are fixed in a timely manner. Provide patches for software package maintainers to help users fix vulnerabilities before virus attack. The patches include vulnerability detection and fixing tools.
+ Respond to security issues: Respond to reported security issues, track the handling progress, and disclose the reported issues in the community based on the security issue disclosure policy.
+ Popularize secure coding rules: Strive to create documentation or development tools to help the developers avoid common pitfalls in the software development process. We will also answer questions encountered during development and use.
+ Participate in code review: Help discover vulnerabilities in code in advance through code review.


## Member

The member list of security committee and the roles of members is maintained in [MEMBERS](MEMBERS-EN.md).

## Meeting Time

- 4:00-5:30 (GMT+8) every other Wednesday through WeLink Meeting



### How to Contact Us

We are responsible for product security release. Please use the correct contact information to obtain timely response.

| List/Group| Type| Function|
| -------------------------------------- | ------- | ------------------------------------------------------------ |
| openeuler-security@openeuler.org       | Private | openEuler security disclosure mailbox. This list is closely monitored and categorized by the PSC. For details, see [Security Disclosure Guide](../vulnerability-management-process/security-disclosure-en.md).|
| release-managers-private@openeuler.org | Private | This is a private communication email especially for release managers. For other users, please subscribe to openeuler-security@openeuler.org. To discuss security issues during the release, release managers must use this private email.|
| security-discuss-private@openeuler.org | Private | Private internal discussion email of the SC. For other users, please subscribe to openeuler-security@openeuler.org.|



### Secure Release Process

For details about how to report security issues and obtain security patches, see [Security Disclosure Guide](../vulnerability-management-process/security-disclosure-en.md).

For details about the security handling process and security policies of the openEuler community, see [Security Handling Process](../vulnerability-management-process/security-process-en.md).



## Community Discussion and Support

Visit https://openEuler.org/en to learn how to interact with the openEuler community.



## Code of Conduct

It is subject to the constraints of **openEuler Code of Conduct**.


## Repository Structure Overview

The openEuler Security Committee repositories maintain files through the following structure. When submitting code, please follow these conventions to place files in appropriate locations. If new folders need to be created, add basic descriptions in the ```index.md``` index file.

```
security-committee
 ┣ assets
 ┣ docs
 ┣ sub-projects
 ┣ MEMBERS.md
 ┣ README.md
 ┗ security-strategy-overview.md
 ```
 
+ ```assets```: Stores non-engineering resource files, including images used in documentation and public keys of Security Committee members.  
+ ```docs```: Hosts documentation describing community security processes, including governance guidelines, vulnerability management, and reporting procedures. Files are categorized in both Chinese and English.  
+ ```sub-projects```: Contains sub-projects operated/incubated by the Security Committee. For projects involving code repositories or SIG (Special Interest Group) operations, store them in ```sub-projects```. For documentation-only projects, use ```docs```.  
+ ```root```: Stores foundational information such as READMEs, community security policies, and member lists to help developers understand the big picture.  

### Index File Conventions

Each subdirectory (e.g., ```assets```, ```docs```) maintains an ```index.md``` file at its root. This file describes the contents of the current folder to ensure clarity and navigability.  

## Security Committee Operated Projects

Projects maintained and operated under the Security Committee are stored in the `sub-projects` directory. Currently maintained projects include:

+ ```SecChain```: Supply Chain Security Maturity Assessment Model  
+ ```security-configuration-benchmark```: openEuler Security Configuration Benchmark  

These projects are maintained under the guidance of the Security Committee to enhance openEuler's security ecosystem.  