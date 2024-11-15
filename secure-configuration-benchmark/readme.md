# 安全配置基线

在当今的数字化环境中，操作系统安全已成为企业和个人必须面对的挑战，运行环境的复杂性和不一致性使得安全加固、核查、巡检等工作变得异常艰巨。

为了应对日益增长的网络安全风险，保护企业信息系统安全，实现操作系统安全配置的规范化、标准化、制度化，提升语言交流的一致性和管理的便捷度，各大OS及安全扫描厂商联合openEuler社区，参考国内外标准，结合业界优秀实践，打造了满足国内外桌面及服务器市场高等级安全诉求的 **《openEuler安全配置基线》** ，该基线包含了操作系统完整的安全配置要求和建议。可以帮助用户在投入最少工作量的情况下，显著提升系统的整体安全防护能力，满足国内外各种安全配置核查诉求。基线建设期间得到了启明星辰、统信、麒麟软件、麒麟信安、超聚变、中科微澜、天翼云、绿盟等合作伙伴的大力支持，收获了大量宝贵意见和建议。

openEuler社区当前已完成安全配置基线建设，并实现了业界知名开源工具openSCAP的适配。社区将继续联合上下游各大厂商，结合行业场景，共同推动更加直观、易用的配置扫描、加固脚本、安全巡检等工具的不断建设和完善，推动全产业链快速达成整体安全合规。

openEuler最新发布的安全配置基线点此获取：[openEuler安全配置基线](https://gitee.com/openeuler/security-committee/blob/master/secure-configuration-benchmark/release/openEuler%E5%AE%89%E5%85%A8%E9%85%8D%E7%BD%AE%E5%9F%BA%E7%BA%BF.md)

# 范围

本安全配置基线适用于openEuler操作系统，兼容各种硬件架构平台。

# 工具
openEuler基于开源能力，构建相应的安全配置核查能力，主要涉及如下两个开源软件：
1. openSCAP：[https://gitee.com/src-openeuler/openscap](https://gitee.com/src-openeuler/openscap)
2. scap-security-guide：[https://gitee.com/src-openeuler/scap-security-guide](https://gitee.com/src-openeuler/scap-security-guide)

如果需要使用其对openEuler的配置安全进行核查，可按如下步骤进行操作：
1. 安装工具（假设使用openEuler系统，并已配置正确的yum源）。
    ``` 
    # dnf install openscap -y
    # dnf install scap-security-guide -y
    ```
2. 调用oscap命令对当前系统进行核查，其中scan_results.xml和scan_report.html是核查结果文档，将详细列出每一条规范的检查结果。
    ```
    # oscap xccdf eval --profile xccdf_org.ssgproject.content_profile_standard --results scan_results.xml --report scan_report.html /usr/share/xml/scap/ssg/content/ssg-openeuler2203-ds.xml
    ```
3.调用oscap命令对当前系统进行加固，其中scan_results.xml和scan_report.html是加固结果文档，将详细列出每一条规范的加固结果。
    ```
    # oscap xccdf eval --remediate --profile xccdf_org.ssgproject.content_profile_standard --report scan_report.html --results scan_results.xml /usr/share/xml/scap/ssg/content/ssg-openeuler2203-ds.xml
    ```
**说明：**

1、当前工具有个别条目受限于技术及规范特点（如依赖于主观判断的条目），无法通过工具进行自动化核查，工具检查结果将显示notchecked。

2、上述命令中“ssg-openeuler2203-ds.xml”是openEuler 22.03LTS版本对应的配置文件，可以替换为其他对应版本的配置文件，此处只是举例，不再赘述。

3、安全加固条目支持加固客观项，部分主观条目无法自动加固，仍需用户手动修改
# 声明

- 安全配置基线并不是系统安全的全部内容，符合所有安全配置基线的操作系统也并不代表绝对安全；
- 部分安全配置基线同业务需要可能存在冲突，比如基线要求系统中不应启用FTP等非安全传输协议，但业务存在必须使用FTP的场景，所以应按需选择和使用，并评估相应的安全风险；
- 本基线仅用于指导不同的用户，基于自身不同的背景及需要，选择部分基线或全部基线内容，对openEuler系统进行配置，从而提升操作系统基础安全能力，以满足各种审计、合规、安全研究、业务运营等的需要；
- 本基线内容随社区发展不断建设完善，使用者可根据openEuler发行版本，基于自身业务考虑，选取基线中的规范项子集，发布相应的安全配置规范要求。

# FAQ
1、基于openeuler默认安装环境执行安全加固时，会出现如下error项（加固失败）

|失败项|失败原因|解决方案|
| ------------ | ------------ | ------------ |
|  Configure System Cryptography Policy |  update-crypto-policies命令不存在 | 请安装crypto-policies后执行加固命令  |
|Limit Password Reuse|该配置项需要管理员先通过authselect配置文件，因此需要先使用authselect select选择配置 |使用authselect list列出可用配置文件，authselect select 选择配置文件（注：如果默认的authselect配置文件不能满足特定需求，建议使用自定义的authselect配置文件。）|
|Lock Accounts After Failed Password Attempts|该配置项需要管理员先通过authselect配置文件，因此需要先使用authselect select选择配置 |使用authselect list列出可用配置文件，authselect select 选择配置文件（注：如果默认的authselect配置文件不能满足特定需求，建议使用自定义的authselect配置文件。）|
|Set Lockout Time for Failed Password Attempts|该配置项需要管理员先通过authselect配置文件，因此需要先使用authselect select选择配置 |使用authselect list列出可用配置文件，authselect select 选择配置文件（注：如果默认的authselect配置文件不能满足特定需求，建议使用自定义的authselect配置文件。）|
|Ensure PAM Enforces Password Requirements - Authentication Retry Prompts Permitted Per-Session|该配置项需要管理员先通过authselect配置文件，因此需要先使用authselect select选择配置 |使用authselect list列出可用配置文件，authselect select 选择配置文件（注：如果默认的authselect配置文件不能满足特定需求，建议使用自定义的authselect配置文件。）|
|Privilege escalation command audit rules should be configured|不能自动扫描|请手动检查|
|Verify ip6tables Enabled if Using IPv6|防火墙不能同时开启|NA|
|Enable haveged service|haveged.service找不到，需要用户先安装haveged|yum install haveged|

2、基于openeuler默认安装环境执行安全扫描时，即使配置不符合openeuler规范时扫描也会显示pass项。

|通过项|通过原因|解决方案|
| ------------ | ------------ | ------------ |
|Disable SSH Support for Rhosts RSA Authentication|openssh版本>=7.4时sshd服务会忽略相关配置。在配置相关参数的场景下，扫码脚本会先检查openssh版本，如果版本>=7.4，会直接显示检查通过 |无需解决|
|Allow Only SSH Protocol 2|openssh版本>=7.4时sshd服务会忽略相关配置。在配置相关参数的场景下，扫码脚本会先检查openssh版本，如果版本>=7.4，会直接显示检查通过 |无需解决|
