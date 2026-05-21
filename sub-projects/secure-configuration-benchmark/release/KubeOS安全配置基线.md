# KubeOS安全配置基线 v1.0

|版本|修订说明|修订时间|访问链接|
| ------------ | ------------ | ------------ | ------------ |
|1.0|初始修订|2026年5月|本文档|

## 1 三方安全软件

### 1.1 SLEM 5 must implement an endpoint security tool.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Adding endpoint security tools can provide the capability to automatically take actions in response to malicious behavior, which can provide additional agility in reacting to network threats. These tools also often include a reporting capability to provide network awareness of the system, which may not otherwise exist in an organization's systems management regime.

**规则影响：**

无

**检查方法：**


确认是否安装终端安全工具

**修复方法：**


安装终端安全工具

### 1.2 Vendor-packaged SLEM 5 security patches and updates must be installed and up to date.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Timely patching is critical for maintaining the operational availability, confidentiality, and integrity of information technology (IT) systems. However, failure to keep SLEM 5 and application software patched is a common mistake made by IT professionals. New patches are released frequently, and it is often difficult for even experienced system administrators (SAs) to keep up with of all the new patches. When new weaknesses in a SLEM 5 exist, patches are usually made available by the vendor to resolve the problems. If the most recent security patches and updates are not installed, unauthorized users may take advantage of weaknesses in the unpatched software. The lack of prompt attention to patching could result in a system compromise.

**规则影响：**

无

**检查方法：**


检测补丁是否已打？检测某软件包是否是bugfix/update版本？

**修复方法：**


无该场景，不涉及

### 1.3 SLEM 5 must use vlock to allow for session locking.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

A session lock is a temporary action taken when a user stops work and moves away from the immediate physical vicinity of the information system but does not want to log out because of the temporary nature of the absence.

The session lock is implemented at the point where session activity can be determined.

Regardless of where the session lock is determined and implemented, once invoked, the session lock must remain in place until the user reauthenticates. No other activity aside from reauthentication must unlock the system.

**规则影响：**

无

**检查方法：**


```bash
执行vlock -v命令，若存在返回值则pass，否则fail
```

**修复方法：**


在镜像制作过程中安装kbd组件

### 1.4 SLEM 5 must have the packages required for multifactor authentication to be installed.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Using an authentication device, such as a Common Access Card (CAC) or token separate from the information system, ensures that even if the information system is compromised, that compromise will not affect credentials stored on the authentication device.

Multifactor solutions that require devices separate from information systems gaining access include, for example, hardware tokens providing time-based or challenge-response authenticators and smart cards such as the U.S. Government Personal Identity Verification (PIV) card and the DOD CAC.

A privileged account is defined as an information system account with authorizations of a privileged user.

Remote access is access to DOD nonpublic information systems by an authorized user (or an information system) communicating through an external, nonorganization-controlled network. Remote access methods include, for example, dial-up, broadband, and wireless.

This requirement only applies to components where this is specific to the function of the device or has the concept of an organizational user (e.g., VPN, proxy capability). This does not apply to authentication for the purpose of configuring the device itself (management).

**规则影响：**

无

**检查方法：**


```bash
使用如下命令排查
# rpm qa | grep pam_pkcs11
# rpm qa | grep nss
# rpm qa | grep nss-util
# rpm qa | grep pcsc-ccid
# rpm qa | grep pcsc-lite
# rpm qa | grep pcsc-tools
# rpm qa | grep opensc
若包存在则满足
```

**修复方法：**


在镜像制作时添加、pam_pkcs11、nss、nss-util、pcsc-ccid、 pcsc-lite、pcsc-tools、opensc

## 2 openEuler-release

### 2.1 SLEM 5 must display the Standard Mandatory DOD Notice and Consent Banner before granting any local or remote connection to the system.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Display of a standardized and approved use notification before granting access to SLEM 5 ensures privacy and security notification verbiage used is consistent with applicable federal laws, Executive Orders, directives, policies, regulations, standards, and guidance.

System use notifications are required only for access via logon interfaces with human users and are not required when such human interfaces do not exist.

The banner must be formatted in accordance with applicable DOD policy. Use the following verbiage for SLEM 5 that can accommodate banners of 1300 characters:

"You are accessing a U.S. Government (USG) Information System (IS) that is provided for USG-authorized use only.

By using this IS (which includes any device attached to this IS), you consent to the following conditions:

-The USG routinely intercepts and monitors communications on this IS for purposes including, but not limited to, penetration testing, COMSEC monitoring, network operations and defense, personnel misconduct (PM), law enforcement (LE), and counterintelligence (CI) investigations.

-At any time, the USG may inspect and seize data stored on this IS.

-Communications using, or data stored on, this IS are not private, are subject to routine monitoring, interception, and search, and may be disclosed or used for any USG-authorized purpose.

-This IS includes security measures (e.g., authentication and access controls) to protect USG interests--not for your personal benefit or privacy.

-Notwithstanding the above, using this IS does not constitute consent to PM, LE or CI investigative searching or monitoring of the content of privileged communications, or work product, related to personal representation or services by attorneys, psychotherapists, or clergy, and their assistants. Such communications and work product are private and confidential. See User Agreement for details."

**规则影响：**

无

**检查方法：**


```bash
grep -i "<如下文本>" /etc/issue
：确认/etc/issue中的内容是否包含如下内容(国防部权利声明)：存在则pass，否则fail
You are accessing a U.S. Government (USG) Information System (IS) that is provided for USG-authorized use only.

By using this IS (which includes any device attached to this IS), you consent to the following conditions:

-The USG routinely intercepts and monitors communications on this IS for purposes including, but not limited to, penetration testing, COMSEC monitoring, network operations and defense, personnel misconduct (PM), law enforcement (LE), and counterintelligence (CI) investigations.

-At any time, the USG may inspect and seize data stored on this IS.

-Communications using, or data stored on, this IS are not private, are subject to routine monitoring, interception, and search, and may be disclosed or used for any USG-authorized purpose.

-This IS includes security measures (e.g., authentication and access controls) to protect USG interests--not for your personal benefit or privacy.

-Notwithstanding the above, using this IS does not constitute consent to PM, LE or CI investigative searching or monitoring of the content of privileged communications, or work product, related to personal representation or services by attorneys, psychotherapists, or clergy, and their assistants. Such communications and work product are private and confidential. See User Agreement for details.
```

**修复方法：**


/etc/issue中加入如下内容(国防部权利声明)：
You are accessing a U.S. Government (USG) Information System (IS) that is provided for USG-authorized use only.

By using this IS (which includes any device attached to this IS), you consent to the following conditions:

-The USG routinely intercepts and monitors communications on this IS for purposes including, but not limited to, penetration testing, COMSEC monitoring, network operations and defense, personnel misconduct (PM), law enforcement (LE), and counterintelligence (CI) investigations.

-At any time, the USG may inspect and seize data stored on this IS.

-Communications using, or data stored on, this IS are not private, are subject to routine monitoring, interception, and search, and may be disclosed or used for any USG-authorized purpose.

-This IS includes security measures (e.g., authentication and access controls) to protect USG interests--not for your personal benefit or privacy.

-Notwithstanding the above, using this IS does not constitute consent to PM, LE or CI investigative searching or monitoring of the content of privileged communications, or work product, related to personal representation or services by attorneys, psychotherapists, or clergy, and their assistants. Such communications and work product are private and confidential. See User Agreement for details.

### 2.2 SLEM 5 must be a vendor-supported release.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

A SLEM 5 release is considered supported if the vendor continues to provide security patches for the product. With an unsupported release, it will not be possible to resolve security issues discovered in the system software.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若存在返回值，则pass，否则fail
cat /etc/os-release | grep -i KubeOS
```

**修复方法：**


os发行版本默认带上，不应该通过其他方式修改，不涉及

## 3 kernel

### 3.1 SLEM 5 must restrict access to the kernel message buffer.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Restricting access to the kernel message buffer limits access only to root. This prevents attackers from gaining additional system information as a nonprivileged user.

**规则影响：**

无

**检查方法：**


```bash
使用命令检查kernel.dmesg_restrict配置值，若返回值为kernel.dmesg_restrict = 1，则pass，否则fail：
# sysctl kernel.dmesg_restrict
kernel.dmesg_restrict = 1
```

**修复方法：**


```bash
修改/etc/sysctl.conf 文件，将kernel.dmesg_restrict配置值为1：
# vim /etc/sysctl.conf
kernel.dmesg_restrict = 1
```

### 3.2 Address space layout randomization (ASLR) must be implemented by SLEM 5 to protect memory from unauthorized code execution.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Some adversaries launch attacks with the intent of executing code in nonexecutable regions of memory or in memory locations that are prohibited. Security safeguards employed to protect memory include, for example, data execution prevention and address space layout randomization. Data execution prevention safeguards can either be hardware enforced or software enforced, with hardware providing the greater strength of mechanism.

Examples of attacks are buffer overflow attacks.

**规则影响：**

无

**检查方法：**


```bash
使用命令检查kernel.randomize_va_space配置值，若返回值为kernel.randomize_va_space = 2，则pass，否则fail：
# sysctl kernel.randomize_va_space
kernel.randomize_va_space = 2
```

**修复方法：**


```bash
修改/etc/sysctl.d/99-stig.conf 中kernel.randomize_va_space=2：
# sudo sh -c 'echo "kernel.randomize_va_space=2" >> /etc/sysctl.d/99-stig.conf'
# sysctl kernel.randomize_va_space
kernel.randomize_va_space = 2
```

### 3.3 SLEM 5 must implement kptr-restrict to prevent the leaking of internal kernel addresses.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Some adversaries launch attacks with the intent of executing code in nonexecutable regions of memory or in memory locations that are prohibited. Security safeguards employed to protect memory include, for example, data execution prevention and address space layout randomization. Data execution prevention safeguards can either be hardware enforced or software enforced, with hardware providing the greater strength of mechanism.

Examples of attacks are buffer overflow attacks.

**规则影响：**

无

**检查方法：**


```bash
使用命令检查kernel.kptr_restrict配置值，若返回值为kernel.kptr_restrict = 1，则pass，否则fail：
# sysctl kernel.kptr_restrict
kernel.kptr_restrict = 1
```

**修复方法：**


```bash
修改/etc/sysctl.d/99-stig.conf 中kernel.kptr_restrict=2：
# sudo sh -c 'echo "kernel.kptr_restrict=1" >> /etc/sysctl.d/99-stig.conf'
# sysctl kernel.kptr_restrict
kernel.kptr_restrict = 1
```

### 3.4 SLEM 5 must not forward Internet Protocol version 4 (IPv4) source-routed packets.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Source-routed packets allow the source of the packet to suggest that routers forward the packet along a different path than configured on the router, which can be used to bypass network security measures. This requirement applies only to the forwarding of source-routed traffic, such as when IPv4/IPv6 forwarding is enabled and the system is functioning as a router.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为net.ipv4.conf.all.accept_source_route = 0，则pass，否则fail：
# sysctl net.ipv4.conf.all.accept_source_route
```

**修复方法：**


```bash
将系统配置为禁用 IPv4 源路由，运行以下命令：
# sudo sysctl -w net.ipv4.conf.all.accept_source_route=0
如果“0”不是系统的默认值，请在“/etc/sysctl.d/99-stig.conf”中添加或更新以下行：
# sudo sh -c 'echo "net.ipv4.conf.all.accept_source_route=0" >> /etc/sysctl.d/99-stig.conf'
# sudo sysctl --system
```

### 3.5 SLEM 5 must not forward Internet Protocol version 4 (IPv4) source-routed packets by default.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Source-routed packets allow the source of the packet to suggest that routers forward the packet along a different path than configured on the router, which can be used to bypass network security measures. This requirement applies only to the forwarding of source-routed traffic, such as when IPv4 forwarding is enabled and the system is functioning as a router.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为net.ipv4.conf.default.accept_source_route = 0，则pass，否则fail：
# sysctl net.ipv4.conf.default.accept_source_route
```

**修复方法：**


```bash
将系统配置为禁用 IPv4 默认源路由，运行以下命令：
# sudo sysctl -w net.ipv4.conf.default.accept_source_route=0
如果“0”不是系统的默认值，请在“/etc/sysctl.d/99-stig.conf”中添加或更新以下行：
# sudo sh -c 'echo "net.ipv4.conf.default.accept_source_route=0" >> /etc/sysctl.d/99-stig.conf'
# sudo sysctl --system
```

### 3.6 SLEM 5 must prevent Internet Protocol version 4 (IPv4) Internet Control Message Protocol (ICMP) redirect messages from being accepted.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

ICMP redirect messages are used by routers to inform hosts that a more direct route exists for a particular destination. These messages modify the host's route table and are unauthenticated. An illicit ICMP redirect message could result in a man-in-the-middle attack.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为net.ipv4.conf.all.accept_redirects = 0，则pass，否则fail：
# sysctl net.ipv4.conf.all.accept_redirects
```

**修复方法：**


```bash
将系统配置为不接受 IPv4 ICMP 重定向消息，运行以下命令：
# sudo sysctl -w net.ipv4.conf.all.accept_redirects=0
如果“0”不是系统的默认值，请在“/etc/sysctl.d/99-stig.conf”中添加或更新以下行：
# sudo sh -c 'echo "net.ipv4.conf.all.accept_redirects=0" >> /etc/sysctl.d/99-stig.conf'

# sudo sysctl --system
```

### 3.7 SLEM 5 must not allow interfaces to accept Internet Protocol version 4 (IPv4) Internet Control Message Protocol (ICMP) redirect messages by default.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

ICMP redirect messages are used by routers to inform hosts that a more direct route exists for a particular destination. These messages modify the host's route table and are unauthenticated. An illicit ICMP redirect message could result in a man-in-the-middle attack.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为net.ipv4.conf.default.accept_redirects = 0，则pass，否则fail：
# sysctl net.ipv4.conf.default.accept_redirects
```

**修复方法：**


```bash
将系统配置为默认不接受 IPv4 ICMP 重定向消息，运行以下命令：
# sudo sysctl -w net.ipv4.conf.default.accept_redirects=0
如果“0”不是系统的默认值，请在“/etc/sysctl.d/99-stig.conf”中添加或更新以下行：
# sudo sh -c 'echo "net.ipv4.conf.default.accept_redirects=0" >> /etc/sysctl.d/99-stig.conf'
# sudo sysctl --system
```

### 3.8 SLEM 5 must not send Internet Protocol version 4 (IPv4) Internet Control Message Protocol (ICMP) redirects.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

ICMP redirect messages are used by routers to inform hosts that a more direct route exists for a particular destination. These messages contain information from the system's route table, possibly revealing portions of the network topology.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为net.ipv4.conf.all.send_redirects = 0，则pass，否则fail：
# sysctl net.ipv4.conf.all.send_redirects
```

**修复方法：**


```bash
将系统配置不允许接口执行IPv4 ICMP重定向，运行以下命令：
# sudo sysctl -w net.ipv4.conf.all.send_redirects=0
如果“0”不是系统的默认值，请在“/etc/sysctl.d/99-stig.conf”中添加或更新以下行：
# sudo sh -c 'echo "net.ipv4.conf.all.send_redirects=0" >> /etc/sysctl.d/99-stig.conf'
# sudo sysctl --system
```

### 3.9 SLEM 5 must not allow interfaces to send Internet Protocol version 4 (IPv4) Internet Control Message Protocol (ICMP) redirect messages by default.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

ICMP redirect messages are used by routers to inform hosts that a more direct route exists for a particular destination. These messages contain information from the system's route table, possibly revealing portions of the network topology.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为net.ipv4.conf.default.send_redirects = 0，则pass，否则fail：
# sysctl net.ipv4.conf.default.send_redirects
```

**修复方法：**


```bash
将系统配置为默认不允许接口执行IPv4 ICMP重定向，运行以下命令：
# sudo sysctl -w net.ipv4.conf.default.send_redirects=0
如果“0”不是系统的默认值，请在“/etc/sysctl.d/99-stig.conf”中添加或更新以下行：
# sudo sh -c 'echo "net.ipv4.conf.default.send_redirects=0" >> /etc/sysctl.d/99-stig.conf'
```

### 3.10 SLEM 5 must not be performing Internet Protocol version 4 (IPv4) packet forwarding unless the system is a router.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Routing protocol daemons are typically used on routers to exchange network topology information with other routers. If this software is used when not required, system network information may be unnecessarily transmitted across the network.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为net.ipv4.ip_forward = 0，则pass，否则fail：
# sysctl net.ipv4.ip_forward
```

**修复方法：**


```bash
将系统配置为不执行 IPv4 数据包转发，运行以下命令：
# sudo sysctl -w net.ipv4.ip_forward=0
如果“0”不是系统的默认值，请在“/etc/sysctl.d/99-stig.conf”中添加或更新以下行：
# sudo sh -c 'echo "net.ipv4.ip_forward=0" >> /etc/sysctl.d/99-stig.conf'
# sudo sysctl --system
```

### 3.11 SLEM 5 must be configured to use TCP syncookies.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Denial of service (DoS) is a condition in which a resource is not available for legitimate users. When this occurs, the organization either cannot accomplish its mission or must operate at degraded capacity. 

Managing excess capacity ensures that sufficient capacity is available to counter flooding attacks. Employing increased capacity and service redundancy may reduce the susceptibility to some DoS attacks. Managing excess capacity may include, for example, establishing selected usage priorities, quotas, or partitioning.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为net.ipv4.tcp_syncookies = 1，则pass，否则fail：
# sysctl net.ipv4.tcp_syncookies
```

**修复方法：**


```bash
将系统配置为使用 IPv4 TCP syncookies，运行以下命令：
# sudo sysctl -w net.ipv4.tcp_syncookies=1
如果“1”不是系统的默认值，请在“/etc/sysctl.d/99-stig.conf”中添加或更新以下行：
# sudo sh -c 'echo "net.ipv4.tcp_syncookies=1" >> /etc/sysctl.d/99-stig.conf'
# sudo sysctl --system
```

### 3.12 SLEM 5 must not forward Internet Protocol version 6 (IPv6) source-routed packets.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Source-routed packets allow the source of the packet to suggest that routers forward the packet along a different path than configured on the router, which can be used to bypass network security measures. This requirement applies only to the forwarding of source-routed traffic, such as when IPv4 forwarding is enabled and the system is functioning as a router.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为net.ipv6.conf.all.accept_source_route = 0，则pass，否则fail：
# sysctl net.ipv6.conf.all.accept_source_route
```

**修复方法：**


```bash
将系统配置为禁用 IPv6 源路由，运行以下命令：
# sudo sysctl -w net.ipv6.conf.all.accept_source_route=0
如果“0”不是系统的默认值，请在“/etc/sysctl.d/99-stig.conf”中添加或更新以下行：
# sudo sh -c 'echo "net.ipv6.conf.all.accept_source_route=0" >> /etc/sysctl.d/99-stig.conf'
# sudo sysctl --system
```

### 3.13 SLEM 5 must not forward Internet Protocol version 6 (IPv6) source-routed packets by default.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Source-routed packets allow the source of the packet to suggest that routers forward the packet along a different path than configured on the router, which can be used to bypass network security measures. This requirement applies only to the forwarding of source-routed traffic, such as when IPv4 forwarding is enabled and the system is functioning as a router.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为net.ipv6.conf.default.accept_source_route = 0，则pass，否则fail：
# sysctl net.ipv6.conf.default.accept_source_route
```

**修复方法：**


```bash
将系统配置为禁用 IPv6 默认源路由，运行以下命令：
# sudo sysctl -w net.ipv6.conf.default.accept_source_route=0
如果“0”不是系统的默认值，请在“/etc/sysctl.d/99-stig.conf”中添加或更新以下行：
# sudo sh -c 'echo "net.ipv6.conf.default.accept_source_route=0" >> /etc/sysctl.d/99-stig.conf'
# sudo sysctl --system
```

### 3.14 SLEM 5 must prevent Internet Protocol version 6 (IPv6) Internet Control Message Protocol (ICMP) redirect messages from being accepted.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

ICMP redirect messages are used by routers to inform hosts that a more direct route exists for a particular destination. These messages modify the host's route table and are unauthenticated. An illicit ICMP redirect message could result in a man-in-the-middle attack.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为net.ipv6.conf.all.accept_redirects = 0，则pass，否则fail：
# sysctl net.ipv6.conf.all.accept_redirects
```

**修复方法：**


```bash
将系统配置为不接受 IPv6 ICMP 重定向消息，运行以下命令：
# sudo sysctl -w net.ipv6.conf.all.accept_redirects=0
如果“0”不是系统的默认值，请在“/etc/sysctl.d/99-stig.conf”中添加或更新以下行：
# sudo sh -c 'echo "net.ipv6.conf.all.accept_redirects=0" >> /etc/sysctl.d/99-stig.conf'
# sudo sysctl --system
```

### 3.15 SLEM 5 must not allow interfaces to accept Internet Protocol version 6 (IPv6) Internet Control Message Protocol (ICMP) redirect messages by default.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

ICMP redirect messages are used by routers to inform hosts that a more direct route exists for a particular destination. These messages modify the host's route table and are unauthenticated. An illicit ICMP redirect message could result in a man-in-the-middle attack.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为net.ipv6.conf.default.accept_redirects = 0，则pass，否则fail：
# sysctl net.ipv6.conf.default.accept_redirects
```

**修复方法：**


```bash
将系统配置为默认不接受 IPv6 ICMP 重定向消息，运行以下命令：
# sudo sysctl -w net.ipv6.conf.default.accept_redirects=0
如果“0”不是系统的默认值，请在“/etc/sysctl.d/99-stig.conf”中添加或更新以下行：
# sudo sh -c 'echo "net.ipv6.conf.default.accept_redirects=0" >> /etc/sysctl.d/99-stig.conf'
# sudo sysctl --system
```

### 3.16 SLEM 5 must not be performing Internet Protocol version 6 (IPv6) packet forwarding unless the system is a router.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Routing protocol daemons are typically used on routers to exchange network topology information with other routers. If this software is used when not required, system network information may be unnecessarily transmitted across the network.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为net.ipv6.conf.all.forwarding = 0，则pass，否则fail：
sysctl net.ipv6.conf.all.forwarding
```

**修复方法：**


```bash
将系统配置为不执行 IPv6 数据包转发，运行以下命令：
# sudo sysctl -w net.ipv6.conf.all.forwarding=0
如果“0”不是系统的默认值，请在“/etc/sysctl.d/99-stig.conf”中添加或更新以下行：
# sudo sh -c 'echo "net.ipv6.conf.all.forwarding=0" >> /etc/sysctl.d/99-stig.conf'
# sudo sysctl --system
```

### 3.17 SLEM 5 must not be performing Internet Protocol version 6 (IPv6) packet forwarding by default unless the system is a router.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Routing protocol daemons are typically used on routers to exchange network topology information with other routers. If this software is used when not required, system network information may be unnecessarily transmitted across the network.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为net.ipv6.conf.default.forwarding = 0，则pass，否则fail：
sysctl net.ipv6.conf.default.forwarding
```

**修复方法：**


```bash
将系统配置为默认不执行 IPv6 数据包转发，运行以下命令：
# sudo sysctl -w net.ipv6.conf.default.forwarding=0
如果“0”不是系统的默认值，请在“/etc/sysctl.d/99-stig.conf”中添加或更新以下行：
# sudo sh -c 'echo "net.ipv6.conf.default.forwarding=0" >> /etc/sysctl.d/99-stig.conf'
# sudo sysctl --system
```

## 4 kdump

### 4.1 SLEM 5 kernel core dumps must be disabled unless needed.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Kernel core dumps may contain the full contents of system memory at the time of the crash. Kernel core dumps may consume a considerable amount of disk space and may result in denial of service (DoS) by exhausting the available space on the target file system partition.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回kdump不存在 Unit kdump.service could not be found，或查询服务状态为disable，则pass，否则fail：
systemctl status kdump.service
```

**修复方法：**


```bash
使用以下命令禁用kdump.service：
# sudo systemctl disable kdump.service
```

## 5 yum

### 5.1 SLEM 5 must remove all outdated software components after updated versions have been installed.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Previous versions of software components that are not removed from the information system after updates have been installed may be exploited by adversaries. Some information technology products may remove older versions of software automatically from the information system.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w 'clean_requirements_on_remove=True' /etc/yum.conf，若返回值为clean_requirements_on_remove=True，则pass，否则fail
```

**修复方法：**


无该场景，不涉及

## 6 分区&文件系统

### 6.1 A separate file system must be used for SLEM 5 user home directories (such as /home or an equivalent).

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

The use of separate file systems for different paths can protect the system from failures resulting from a file system becoming full or failing.

**规则影响：**

无

**检查方法：**


```bash
执行mount | grep "/home "，若存在返回值，则pass，否则fail
```

**修复方法：**


在镜像制作时配置

### 6.2 SLEM 5 must use a separate file system for /var.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

The use of separate file systems for different paths can protect the system from failures resulting from a file system becoming full or failing.

**规则影响：**

无

**检查方法：**


```bash
执行mount | grep "/var "，若存在返回值，则pass，否则fail
```

**修复方法：**


在镜像制作时配置

### 6.3 SLEM 5 must use a separate file system for the system audit data path.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

The use of separate file systems for different paths can protect the system from failures resulting from a file system becoming full or failing.

**规则影响：**

无

**检查方法：**


```bash
执行mount | grep " /var/log/audit"，若存在返回值，则pass，否则fail
```

**修复方法：**


在镜像制作时配置

### 6.4 SLEM 5 file systems that are being imported via Network File System (NFS) must be mounted to prevent files with the setuid and setgid bit set from being executed.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

The "nosuid" mount option causes the system to not execute "setuid" and "setgid" files with owner privileges. This option must be used for mounting any file system not containing approved "setuid" and "setguid" files. Executing files from untrusted file systems increases the opportunity for unprivileged users to attain unauthorized administrative access.

**规则影响：**

无

**检查方法：**


```bash
执行mount | grep "/persist/nfs" | grep nosuid，若存在返回值，则pass，否则fail
```

**修复方法：**


在镜像制作时配置

### 6.5 SLEM 5 file systems that are being imported via Network File System (NFS) must be mounted to prevent binary files from being executed.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

The "noexec" mount option causes the system to not execute binary files. This option must be used for mounting any file system not containing approved binary files, as they may be incompatible. Executing files from untrusted file systems increases the opportunity for unprivileged users to attain unauthorized administrative access.

**规则影响：**

无

**检查方法：**


```bash
执行mount | grep "/persist/nfs" | grep noexec，若存在返回值，则pass，否则fail
```

**修复方法：**


在镜像制作时配置

### 6.6 SLEM 5 file systems that are used with removable media must be mounted to prevent files with the setuid and setgid bit set from being executed.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

The "nosuid" mount option causes the system to not execute "setuid" and "setgid" files with owner privileges. This option must be used for mounting any file system not containing approved "setuid" and "setguid" files. Executing files from untrusted file systems increases the opportunity for unprivileged users to attain unauthorized administrative access.

**规则影响：**

无

**检查方法：**


不涉及，无需检测。

**修复方法：**

无

### 6.7 SLEM 5 file systems that contain user home directories must be mounted to prevent files with the setuid and setgid bit set from being executed.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

The "nosuid" mount option causes the system to not execute setuid and setgid files with owner privileges. This option must be used for mounting any file system not containing approved setuid and setguid files. Executing files from untrusted file systems increases the opportunity for unprivileged users to attain unauthorized administrative access.

**规则影响：**

无

**检查方法：**


```bash
执行for X in `awk -F: '($3>=1000)&&($7 !~ /nologin/){print $6}' /etc/passwd`; do findmnt -nkT $X; done | sort -r，若返回值中存在nosuid，则pass，否则fail
```

**修复方法：**


在镜像制作时配置

### 6.8 SLEM 5 must disable the file system automounter unless required.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Automatically mounting file systems permits easy introduction of unknown devices, thereby facilitating malicious activity.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回autofs不存在 ，或查询服务状态为inactive和disabled，则pass，否则fail：
systemctl status autofs
```

**修复方法：**


```bash
使用以下命令禁用autofs：
# sudo systemctl stop autofs
# sudo systemctl disable autofs
```

### 6.9 All SLEM 5 persistent disk partitions must implement cryptographic mechanisms to prevent unauthorized disclosure or modification of all information that requires at-rest protection.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

SLEM 5 handling data requiring data-at-rest protections must employ cryptographic mechanisms to prevent unauthorized disclosure and modification of the information at rest.

Selection of a cryptographic mechanism is based on the need to protect the integrity of organizational information. The strength of the mechanism is commensurate with the security category and/or classification of the information. Organizations have the flexibility to either encrypt all information on storage devices (i.e., full disk encryption) or encrypt specific data structures (e.g., files, records, or fields).

**规则影响：**

无

**检查方法：**


```bash
通过使用磁盘加密来防止所有需要静态保护的信息被未经授权的披露或修改。

使用以下命令验证系统分区是否全部已加密：
> sudo blkid
/dev/sda1: "UUID=26d4a101-7f48-4394-b730-56dc00e65f64" TYPE="crypto_LUKS"
/dev/sda2: "UUID=f5b8a790-14cb-4b82-882d-707d52f27765" TYPE="crypto_LUKS"
/dev/sda3: "UUID=f2d86128-f975-478d-a5b0-25806c900eac" TYPE="crypto_LUKS"
系统中存在的每个持久性磁盘分区必须为 "crypto_LUKS" 类型。如果除启动分区或伪文件系统（如 /proc 或 /sys）或临时文件系统（如 tmpfs）之外的任何分区不是 "crypto_LUKS" 类型，请要求管理员说明这些分区是如何加密的。如果没有证据表明这些分区已加密，则视为不符合项。
```

**修复方法：**


在镜像制作时添加

## 7 文件权限

### 7.1 SLEM 5 must have directories that contain system commands set to a mode of 755 or less permissive.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If SLEM 5 were to allow any user to make changes to software libraries, then those changes might be implemented without undergoing the appropriate testing and approvals that are part of a robust change management process.

This requirement applies to SLEM 5 with software libraries that are accessible and configurable, as in the case of interpreted languages. Software libraries also include privileged programs which execute with escalated privileges. Only qualified and authorized individuals must be allowed to obtain access to information system components for purposes of initiating changes, including upgrades and modifications.

**规则影响：**

无

**检查方法：**


```bash
使用命令验证系统命令目录是否具有“755”模式或更宽松的权限，若无返回值，则pass，否则fail：
# find -L  /usr/local/bin /usr/local/sbin -perm /022 -type d -exec stat -c ""%n %a"" '{}' \;
```

**修复方法：**


```bash
使用系统命令，防止未经授权的访问。运行以下命令：
# sudo find -L  /usr/local/bin /usr/local/sbin -perm /022 -type f -exec chmod 755 '{}' \;
# sudo transactional-update shell
# sudo find -L  /bin /sbin /usr/bin /usr/sbin -perm /022 -type f -exec chmod 755 '{}' \;
# exit
# sudo reboot
```

### 7.2 SLEM 5 must have system commands set to a mode of 755 or less permissive.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If SLEM 5 were to allow any user to make changes to software libraries, then those changes might be implemented without undergoing the appropriate testing and approvals that are part of a robust change management process.

This requirement applies to SLEM 5 with software libraries that are accessible and configurable, as in the case of interpreted languages. Software libraries also include privileged programs which execute with escalated privileges. Only qualified and authorized individuals must be allowed to obtain access to information system components for purposes of initiating changes, including upgrades and modifications.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令，验证系统命令目录是否具有“755”模式或更宽松的权限，若无返回值，则pass，否则fail：
# find -L  /usr/local/bin /usr/local/sbin -perm /022 -type d -exec stat -c ""%n %a"" '{}' \;
```

**修复方法：**


```bash
使用系统命令，防止未经授权的访问。运行以下命令：
# sudo find -L  /usr/local/bin /usr/local/sbin -perm /022 -type f -exec chmod 755 '{}' \;
# sudo transactional-update shell
# sudo find -L  /bin /sbin /usr/bin /usr/sbin -perm /022 -type f -exec chmod 755 '{}' \;
# exit
# sudo reboot
```

### 7.3 SLEM 5 library directories must have mode 755 or less permissive.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If SLEM 5 were to allow any user to make changes to software libraries, then those changes might be implemented without undergoing the appropriate testing and approvals that are part of a robust change management process.

This requirement applies to SLEM 5 with software libraries that are accessible and configurable, as in the case of interpreted languages. Software libraries also include privileged programs which execute with escalated privileges. Only qualified and authorized individuals must be allowed to obtain access to information system components for purposes of initiating changes, including upgrades and modifications.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令，验证系统范围内共享库目录“/lib”、“/lib64”、“/usr/lib”和“/usr/lib64”是否具有“755”模式或更宽松的权限若无返回值，则pass，否则fail：
# find /lib /lib64 /usr/lib /usr/lib64 -perm /022 -type d -exec stat -c "%n %a" '{}' \;
```

**修复方法：**


```bash
配置lib库文件，防止未经授权的访问。运行以下命令：
# sudo transactional-update shell
# sudo find /lib /lib64 /usr/lib /usr/lib64 -perm /022 -type f -exec chmod 755 '{}' \；
# exit
# sudo reboot
```

### 7.4 SLEM 5 library files must have mode 755 or less permissive.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If SLEM 5 were to allow any user to make changes to software libraries, then those changes might be implemented without undergoing the appropriate testing and approvals that are part of a robust change management process.

This requirement applies to SLEM 5 with software libraries that are accessible and configurable, as in the case of interpreted languages. Software libraries also include privileged programs which execute with escalated privileges. Only qualified and authorized individuals must be allowed to obtain access to information system components for purposes of initiating changes, including upgrades and modifications.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，系统范围内共享库文件的权限模式是否为0755或更宽松，若无返回值，则pass，否则fail：
# sudo find /lib /lib64 /usr/lib /usr/lib64 -type f -name '*.so*' -perm /022 -exec stat -c "%n %a" {} +
```

**修复方法：**


```bash
配置系统范围内共享库文件，其权限模式应为0755或更宽松。可使用以下命令进行设置：
# sudo transactional-update shell
# sudo find /lib /lib64 /usr/lib /usr/lib64 -type f -name '*.so*' -perm /022 -exec chmod go-w {} +
# exit
# sudo reboot
```

### 7.5 All SLEM 5 local interactive user home directories must have mode 750 or less permissive.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Excessive permissions on local interactive user home directories may allow unauthorized access to user files by other users.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令，验证系统中本地交互式用户所分配的主目录是否具有“750”或更宽松的模式，如果 "/etc/passwd" 中引用的主目录不具有“750”或更宽松的模式，则fail：
# ls -ld $（awk -F: '($3>=1000）&&($7!~ /nologin/){print $6}' /etc/passwd))
```

**修复方法：**


```bash
使用以下命令更改目录权限：
# sudo chmod 750 {目标目录}
```

### 7.6 All SLEM 5 local initialization files must have mode 740 or less permissive.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Local initialization files are used to configure the user's shell environment upon logon. Malicious modification of these files could compromise accounts upon logon.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令，验证系统中本地初始化文件的权限模式是否为“740”或更严格的权限限制，如果任何本地初始化文件的权限模式比“740”更宽松，则fail：
> sudo ls -al {用户目录}/.* | more
```

**修复方法：**


```bash
使用以下命令更改文件权限：
# sudo chmod 750 {目标文件}
```

### 7.7 SLEM 5 SSH daemon public host key files must have mode 644 or less permissive.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If a public host key file is modified by an unauthorized user, the SSH service may be compromised.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值中第一列权限均小于等于644，则pass，否则fail：
# find /etc/ssh -name 'ssh_host*key.pub' -exec stat -c "%a %n" {} \;
```

**修复方法：**


```bash
使用以下命令，将 "/etc/ssh" 下的公共主机密钥文件的权限模式更改为 "644"：
# sudo chmod 644 /etc/ssh/ssh_host*key.pub
```

### 7.8 SLEM 5 SSH daemon private host key files must have mode 640 or less permissive.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If an unauthorized user obtains the private SSH host key file, the host could be impersonated.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值中第一列权限均小于等于640，则pass，否则fail：
# find /etc/ssh -name 'ssh_host*key' -exec stat -c "%a %n" {} \;
```

**修复方法：**


```bash
使用以下命令，将系统中 "/etc/ssh" 下的 SSH 守护进程私有主机密钥文件的权限模式设置为 "640"：

# sudo chmod 640 /etc/ssh/ssh_host*key
```

### 7.9 SLEM 5 library files must be owned by root.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If SLEM 5 were to allow any user to make changes to software libraries, then those changes might be implemented without undergoing the appropriate testing and approvals that are part of a robust change management process.

This requirement applies to SLEM 5 with software libraries that are accessible and configurable, as in the case of interpreted languages. Software libraries also include privileged programs which execute with escalated privileges. Only qualified and authorized individuals must be allowed to obtain access to information system components for purposes of initiating changes, including upgrades and modifications.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若无返回值，则pass，否则fail：
# find /lib /lib64 /usr/lib /usr/lib64 -type f -name '*.so*' ! -user root -exec stat -c "%n %U" {} +
```

**修复方法：**


```bash
使用命令配置系统范围内共享库文件，这些文件包含在“/lib”、“/lib64”、“/usr/lib”和“/usr/lib64”目录中，将所有这些文件的所有权更改为 root：
# sudo transactional-update shell
# sudo find /lib /lib64 /usr/lib /usr/lib64 -type f -name '*.so*'! -user root -exec chown root {} +
# exit
# sudo reboot
```

### 7.10 SLEM 5 library files must be group-owned by root.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If SLEM 5 were to allow any user to make changes to software libraries, then those changes might be implemented without undergoing the appropriate testing and approvals that are part of a robust change management process.

This requirement applies to SLEM 5 with software libraries that are accessible and configurable, as in the case of interpreted languages. Software libraries also include privileged programs which execute with escalated privileges. Only qualified and authorized individuals must be allowed to obtain access to information system components for purposes of initiating changes, including upgrades and modifications.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若无返回值，则pass，否则fail：
# find /lib /lib64 /usr/lib /usr/lib64 -type f -name '*.so*' ! -group root -exec stat -c "%n %G" {} +
```

**修复方法：**


```bash
配置系统范围内共享库文件，这些文件包含在“/lib”、“/lib64”、“/usr/lib”和“/usr/lib64”目录中。使用以下命令将这些文件的所有权更改为组为root：
# sudo transactional-update shell
# sudo find /lib /lib64 /usr/lib /usr/lib64 -type f -name '*.so*'! -group root -exec chown :root {} +
# exit
# sudo reboot
```

### 7.11 SLEM 5 library directories must be owned by root.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If SLEM 5 were to allow any user to make changes to software libraries, then those changes might be implemented without undergoing the appropriate testing and approvals that are part of a robust change management process.

This requirement applies to SLEM 5 with software libraries that are accessible and configurable, as in the case of interpreted languages. Software libraries also include privileged programs which execute with escalated privileges. Only qualified and authorized individuals must be allowed to obtain access to information system components for purposes of initiating changes, including upgrades and modifications.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若无返回值，则pass，否则fail：
# find /lib /lib64 /usr/lib /usr/lib64 ! -user root -type d -exec stat -c "%n %U" '{}' \;
```

**修复方法：**


```bash
配置库目录，以防止未经授权的访问。运行以下命令：
# sudo transactional-update shell
# sudo find /lib /lib64 /usr/lib /usr/lib64! -user root -type d -exec chown root '{}' \；
# exit
# sudo reboot
```

### 7.12 SLEM 5 library directories must be group-owned by root.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If SLEM 5 were to allow any user to make changes to software libraries, then those changes might be implemented without undergoing the appropriate testing and approvals that are part of a robust change management process.

This requirement applies to SLEM 5 with software libraries that are accessible and configurable, as in the case of interpreted languages. Software libraries also include privileged programs which execute with escalated privileges. Only qualified and authorized individuals must be allowed to obtain access to information system components for purposes of initiating changes, including upgrades and modifications.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若无返回值，则pass，否则fail：
# find /lib /lib64 /usr/lib /usr/lib64 ! -group root -type d -exec stat -c "%n %G" '{}' \;
```

**修复方法：**


```bash
配置库目录，以防止未经授权的访问。运行以下命令：
# sudo transactional-update shell
# sudo find /lib /lib64 /usr/lib /usr/lib64! -group root -type d -exec chgrp root '{}' \；
# exit
# sudo reboot
```

### 7.13 SLEM 5 must have system commands owned by root.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If SLEM 5 were to allow any user to make changes to software libraries, then those changes might be implemented without undergoing the appropriate testing and approvals that are part of a robust change management process.

This requirement applies to SLEM 5 with software libraries that are accessible and configurable, as in the case of interpreted languages. Software libraries also include privileged programs which execute with escalated privileges. Only qualified and authorized individuals must be allowed to obtain access to information system components for purposes of initiating changes, including upgrades and modifications.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若无返回值，则pass，否则fail：
# find -L  /usr/local/bin /usr/local/sbin ! -user root -type f -exec stat -c "%n %U" '{}' \;
```

**修复方法：**


```bash
配置系统命令，防止未经授权的访问。运行以下命令：
# sudo transactional-update shell
# sudo find -L /bin /sbin /usr/bin /usr/sbin! -user root -type f -exec chown root '{}' \；
# exit
# sudo reboot
```

### 7.14 SLEM 5 must have system commands group-owned by root or a system account.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If SLEM 5 were to allow any user to make changes to software libraries, then those changes might be implemented without undergoing the appropriate testing and approvals that are part of a robust change management process.

This requirement applies to SLEM 5 with software libraries that are accessible and configurable, as in the case of interpreted languages. Software libraries also include privileged programs which execute with escalated privileges. Only qualified and authorized individuals must be allowed to obtain access to information system components for purposes of initiating changes, including upgrades and modifications.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若无返回值，则pass，否则fail：
# find -L  /usr/local/bin /usr/local/sbin! -group root -type f -exec stat -c "%n %G" '{}' \;
```

**修复方法：**


```bash
配置系统命令，防止未经授权的访问。运行以下命令：
# sudo transactional-update shell
# sudo find -L /bin /sbin /usr/bin /usr/sbin! -user root -type f -exec chown root '{}' \；
# exit
# sudo reboot
```

### 7.15 SLEM 5 must have directories that contain system commands owned by root.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If SLEM 5 were to allow any user to make changes to software libraries, then those changes might be implemented without undergoing the appropriate testing and approvals that are part of a robust change management process.

This requirement applies to SLEM 5 with software libraries that are accessible and configurable, as in the case of interpreted languages. Software libraries also include privileged programs which execute with escalated privileges. Only qualified and authorized individuals must be allowed to obtain access to information system components for purposes of initiating changes, including upgrades and modifications.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若无返回值，则pass，否则fail：
# find -L  /usr/local/bin /usr/local/sbin ! -user root -type d -exec stat -c "%n %U" '{}' \;
```

**修复方法：**


```bash
配置系统命令，防止未经授权的访问。运行以下命令：
# sudo transactional-update shell
# sudo find -L /bin /sbin /usr/bin /usr/sbin! -user root -type d -exec chown root '{}' \；
# exit
# sudo reboot
```

### 7.16 SLEM 5 must have directories that contain system commands group-owned by root.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If SLEM 5 were to allow any user to make changes to software libraries, then those changes might be implemented without undergoing the appropriate testing and approvals that are part of a robust change management process.

This requirement applies to SLEM 5 with software libraries that are accessible and configurable, as in the case of interpreted languages. Software libraries also include privileged programs which execute with escalated privileges. Only qualified and authorized individuals must be allowed to obtain access to information system components for purposes of initiating changes, including upgrades and modifications.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若无返回值，则pass，否则fail：
# find -L  /usr/local/bin /usr/local/sbin ! -group root -type d -exec stat -c "%n %G" '{}' \;
```

**修复方法：**


```bash
配置系统命令，防止未经授权的访问。运行以下命令：
# sudo transactional-update shell
# sudo find -L /bin /sbin /usr/bin /usr/sbin! -group root -type d -exec chgrp root '{}' \；
# exit
# sudo reboot
```

### 7.17 All SLEM 5 files and directories must have a valid owner.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Unowned files and directories may be unintentionally inherited if a user is assigned the same User Identifier (UID) as the UID of the unowned files.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若无返回值，则pass，否则fail：
# find / -fstype xfs -nouser
```

**修复方法：**


```bash
两种修复方案：
1）从系统中删除所有没有有效用户的文件和目录
2）使用 "chown" 命令为系统中所有未拥有权的文件和目录分配一个有效的用户：
# sudo chown <用户> <文件>
```

### 7.18 All SLEM 5 files and directories must have a valid group owner.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Files without a valid group owner may be unintentionally inherited if a group is assigned the same Group Identifier (GID) as the GID of the files without a valid group owner.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若无返回值，则pass，否则fail：
# find / -fstype xfs -nogroup
```

**修复方法：**


```bash
两种修复方案：
1）从系统中删除所有没有有效组的文件和目录
2）使用 "chgrp" 命令为系统上所有文件和目录分配一个有效的组：
# sudo chgrp <组名> <文件名>
```

### 7.19 All SLEM 5 local interactive user home directories must be group-owned by the home directory owner's primary group.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If the Group Identifier (GID) of a local interactive user's home directory is not the same as the primary GID of the user, this would allow unauthorized access to the user's files, and users that share the same group may not be able to access files that they legitimately should.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令，验证系统中本地交互式用户所分配的主目录，是否由该用户的主 GID 所拥有的组所拥有，如果 "/etc/passwd" 中引用的用户主目录，不是由该用户的主 GID 所拥有的组所拥有，则fail：
# awk -F: '($3>=1000)&&($7!~ /nologin/){print $4, $6}' /etc/passwd
```

**修复方法：**


```bash
使用以下命令更改目录所属组：
# sudo chgrp users {目标目录}
```

### 7.20 All SLEM 5 world-writable directories must be group-owned by root, sys, bin, or an application group.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If a world-writable directory has the sticky bit set and is not group-owned by a privileged Group Identifier (GID), unauthorized users may be able to modify files created by others.

The only authorized public directories are those temporary directories supplied with the system or those designed to be temporary file repositories. The setting is normally reserved for directories used by the system and by users for temporary file storage, (e.g., /tmp), and for directories requiring global read/write access.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值的第3列和第4列均为root，则pass，否则fail：
# find / -perm -002 -type d -exec ls -lLd {} \;
```

**修复方法：**


```bash
将系统中所有具有全局可写权限的目录的所属组更改为root，可以使用以下命令：
# sudo chgrp root <目录>
```

### 7.21 The sticky bit must be set on all SLEM 5 world-writable directories.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Preventing unauthorized information transfers mitigates the risk of information, including encrypted representations of information, produced by the actions of prior users/roles (or the actions of processes acting on behalf of prior users/roles) from being available to any current users/roles (or current processes) that obtain access to shared system resources (e.g., registers, main memory, and hard disks) after those resources have been released back to information systems. The control of information in shared resources is also commonly referred to as object reuse and residual information protection.

This requirement generally applies to the design of an information technology product, but it can also apply to the configuration of particular information system components that are, or use, such products. This can be verified by acceptance/validation processes in DOD or other government agencies.

There may be shared resources with configurable protections (e.g., files in storage) that may be assessed on specific information system components.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值的第1列是"drwxrwxrwt."，则pass，否则fail：
# find / \( -path /.snapshots -o -path /sys -o -path /proc \) -prune -o -perm -002 -type d -exec ls -lLd {} \;
```

**修复方法：**


```bash
使用以下命令为所有全局可写目录设置粘滞位（以“/tmp”目录为例）：
# sudo chmod 1777 /tmp
注：对于每个全局可写目录，将上述命令中的“/tmp”替换为尚未设置粘滞位的全局可写目录。
```

### 7.22 SLEM 5 must prevent unauthorized users from accessing system error messages.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Only authorized personnel should be aware of errors and the details of the errors. Error messages are an indicator of an organization's operational state or can identify SLEM 5 or platform. Additionally, Personally Identifiable Information (PII) and operational information must not be revealed through error messages to unauthorized personnel or their designated representatives.

The structure and content of error messages must be carefully considered by the organization and development team. The extent to which the information system is able to identify and handle error conditions is guided by organizational policy and operational requirements.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为/var/log/messages root:root 640，则pass，否则fail：
# stat -c "%n %U:%G %a" /var/log/messages
```

**修复方法：**


```bash
使用以下命令更改/var/log/messages文件权限：
# sudo chmod 640 /var/log/messages
```

### 7.23 SLEM 5 must generate error messages that provide information necessary for corrective actions without revealing information that could be exploited by adversaries.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Any operating system providing too much information in error messages risks compromising the data and security of the structure, and content of error messages needs to be carefully considered by the organization.

Organizations carefully consider the structure/content of error messages. The extent to which information systems are able to identify and handle error conditions is guided by organizational policy and operational requirements. Information that could be exploited by adversaries includes, for example, erroneous logon attempts with passwords entered by mistake as the username, mission/business information that can be derived from (if not stated explicitly by) information recorded, and personal information, such as account numbers, social security numbers, and credit card numbers.

The /var/log/btmp, /var/log/wtmp, and /var/log/lastlog files have group write and global read permissions to allow for the lastlog function to perform. Limiting the permissions beyond this configuration will result in the failure of functions that rely on the lastlog database.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若无返回值，则pass，否则fail：
# find /var/log -perm /137 ! -name '*[bw]tmp' ! -name '*lastlog' -type f -exec stat -c "%n %a" {} \;
```

**修复方法：**


```bash
使用以下命令将 /var/log 目录下所有日志文件的权限设置为“640”或更严格的权限：
# sudo find /var/log -perm /137 ! -name '*[bw]tmp' ! -name '*lastlog' -type f -exec chmod 640 '{}' \；
```

### 7.24 All SLEM 5 local interactive user home directories defined in the /etc/passwd file must exist.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If a local interactive user has a home directory defined that does not exist, the user may be given access to the / directory as the current working directory upon logon. This could create a denial of service (DoS) because the user would not be able to access their logon configuration files, and it may give them visibility to system files they normally would not be able to access.

**规则影响：**

无

**检查方法：**


```bash
存疑：
明确用户名后，执行stat -c "%n %U:%G %a"检测文件权限。
```

**修复方法：**


```bash
使用以下命令对用户主目录进行修改：
# sudo mkdir /home/{用户名}
# sudo chown {用户名} /home/{用户名}
# sudo chgrp users /home/{用户名}
# sudo chmod 0750 /home/{用户名}
```

### 7.25 All SLEM 5 local initialization files must not execute world-writable programs.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If user start-up files execute world-writable programs, especially in unprotected directories, they could be maliciously modified to destroy user files or otherwise compromise the system at the user level. If the system is compromised at the user level, it is easier to elevate privileges to eventually compromise the system at the root and network level.

**规则影响：**

无

**检查方法：**


```bash
执行
find / -xdev -perm -002 -type f -exec ls -ld {} \;
若该命令无返回值，则pass，否则fail
```

**修复方法：**


```bash
运行chmod命令，设置系统初始化脚本中不包含全局可写的配置文件：
将文件权限设为755，即：-rwxr-xr-x，表示文件所有者拥有读、写、执行权限，所属组及其他用户仅有读、执行权限。
# chmod 755 <file>
```

## 8 网络

### 8.1 SLEM 5 must be configured to prohibit or restrict the use of functions, ports, protocols, and/or services as defined in the Ports, Protocols, and Services Management (PPSM) Category Assignments List (CAL) and vulnerability assessments.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

To prevent unauthorized connection of devices, unauthorized transfer of information, or unauthorized tunneling (i.e., embedding of data types within data types), organizations must disable or restrict unused or unnecessary physical and logical ports/protocols on information systems.

Additionally, operating system remote access functionality must have the capability to immediately disconnect current users remotely accessing the information system and/or disable further remote access. The speed of disconnect or disablement varies based on the criticality of mission functions and the need to eliminate immediate or future remote access to organizational information systems.

**规则影响：**

无

**检查方法：**


```bash
通过运行以下命令，检查防火墙配置，若返回值中存在”Active: active“，并且存在“firewalld.service; enabled“，则pass，否则fail：
systemctl status firewalld.service
```

**修复方法：**


```bash
使用以下命令，启动firewalld.service：
# sudo systemctl start firewalld.service
# sudo systemctl enable firewalld.service --now
```

## 9 chrony

### 9.1 SLEM 5 clock must, for networked systems, be synchronized to an authoritative DOD time source at least every 24 hours.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Inaccurate time stamps make it more difficult to correlate events and can lead to an inaccurate analysis. Determining the correct time a particular event occurred on a system is critical when conducting forensic analysis and investigating system events. Sources outside the configured acceptable allowance (drift) may be inaccurate.

Synchronizing internal information system clocks provides uniformity of time stamps for information systems with multiple system clocks and systems connected over a network.

Organizations should consider endpoints that may not have regular access to the authoritative time server (e.g., mobile, teleworking, and tactical endpoints).

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为server 0.us.pool.ntp.mil maxpoll 16，则pass，否则fail：
grep maxpoll /etc/chrony.conf
```

**修复方法：**


```bash
使用以下命令，修改配置文件，增加或修改以下文字：
# vim /etc/chrony.conf
server <time_source> maxpoll 16
```

## 10 网卡配置

### 10.1 SLEM 5 must not have network interfaces in promiscuous mode unless approved and documented.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Network interfaces in promiscuous mode allow for the capture of all network traffic visible to the system. If unauthorized individuals can access these applications, it may allow then to collect information such as logon IDs, passwords, and key exchanges between systems.

If the system is being used to perform a network troubleshooting function, the use of these tools must be documented with the information system security officer (ISSO) and restricted to only authorized personnel.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若无返回值，则pass，否则fail：
ip link | grep -i promisc
```

**修复方法：**


```bash
使用以下命令将接口的混杂模式设置为关闭：
# sudo ip link set dev <设备名称> promisc off
```

## 11 openssh

### 11.1 SLEM 5 must display the Standard Mandatory DOD Notice and Consent Banner before granting access via SSH.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Display of a standardized and approved use notification before granting access to SLEM 5 ensures privacy and security notification verbiage used is consistent with applicable federal laws, Executive Orders, directives, policies, regulations, standards, and guidance.

System use notifications are required only for access via logon interfaces with human users and are not required when such human interfaces do not exist.

The banner must be formatted in accordance with applicable DOD policy. Use the following verbiage for SLEM 5 that can accommodate banners of 1300 characters:

"You are accessing a U.S. Government (USG) Information System (IS) that is provided for USG-authorized use only.

By using this IS (which includes any device attached to this IS), you consent to the following conditions:

-The USG routinely intercepts and monitors communications on this IS for purposes including, but not limited to, penetration testing, COMSEC monitoring, network operations and defense, personnel misconduct (PM), law enforcement (LE), and counterintelligence (CI) investigations.

-At any time, the USG may inspect and seize data stored on this IS.

-Communications using, or data stored on, this IS are not private, are subject to routine monitoring, interception, and search, and may be disclosed or used for any USG-authorized purpose.

-This IS includes security measures (e.g., authentication and access controls) to protect USG interests--not for your personal benefit or privacy.

-Notwithstanding the above, using this IS does not constitute consent to PM, LE or CI investigative searching or monitoring of the content of privileged communications, or work product, related to personal representation or services by attorneys, psychotherapists, or clergy, and their assistants. Such communications and work product are private and confidential. See User Agreement for details."

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为Banner /etc/issue/，则pass，否则fail：
grep -w '^Banner /etc/issue/' /etc/ssh/sshd_config
```

**修复方法：**


```bash
使用以下命令修改/etc/ssh/sshd_config文件，增加或修改以下文字，然后重启sshd服务：
# vim /etc/ssh/sshd_config
Banner /etc/issue/
# sudo systemctl restart sshd.service
```

### 11.2 SLEM 5 must be configured so that all network connections associated with SSH traffic terminate after becoming unresponsive.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Terminating an unresponsive SSH session within a short time period reduces the window of opportunity for unauthorized personnel to take control of a management session enabled on the console or console port that has been left unattended. In addition, quickly terminating an idle SSH session will also free up resources committed by the managed network element.

Terminating network connections associated with communications sessions includes, for example, deallocating associated TCP/IP address/port pairs at the operating system level and deallocating networking assignments at the application level if multiple application sessions are using a single operating system-level network connection. This does not mean the operating system terminates all sessions or network access; it only ends the unresponsive session and releases the resources associated with that session.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为ClientAliveCountMax 1，则pass，否则fail：
grep -w '^ClientAliveCountMax 1' /etc/ssh/sshd_config
```

**修复方法：**


```bash
使用以下命令修改/etc/ssh/sshd_config文件，增加或修改以下文字，然后重启sshd服务：
# vim /etc/ssh/sshd_config
ClientAliveCountMax 1
# sudo systemctl restart sshd.service
```

### 11.3 SLEM 5 must be configured so that all network connections associated with SSH traffic are terminated after 10 minutes of becoming unresponsive.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Terminating an unresponsive SSH session within a short time period reduces the window of opportunity for unauthorized personnel to take control of a management session enabled on the console or console port that has been left unattended. In addition, quickly terminating an idle SSH session will also free up resources committed by the managed network element. 

Terminating network connections associated with communications sessions includes, for example, deallocating associated TCP/IP address/port pairs at the operating system level and deallocating networking assignments at the application level if multiple application sessions are using a single operating system-level network connection. This does not mean that the operating system terminates all sessions or network access; it only ends the unresponsive session and releases the resources associated with that session.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为ClientAliveInterval 600，则pass，否则fail：
grep -w '^ClientAliveInterval 600' /etc/ssh/sshd_config
```

**修复方法：**


```bash
使用以下命令修改/etc/ssh/sshd_config文件，增加或修改以下文字，然后重启sshd服务：
# vim /etc/ssh/sshd_config
ClientAliveInterval 600
# sudo systemctl restart sshd.service
```

### 11.4 SLEM 5 SSH daemon must disable forwarded remote X connections for interactive users, unless to fulfill documented and validated mission requirements.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

The security risk of using X11 forwarding is that the client's X11 display server may be exposed to attack when the SSH client requests forwarding. A system administrator may have a stance in which they want to protect clients that may expose themselves to attack by unwittingly requesting X11 forwarding, which can warrant a ''no'' setting.

X11 forwarding should be enabled with caution. Users with the ability to bypass file permissions on the remote host (for the user's X11 authorization database) can access the local X11 display through the forwarded connection. An attacker may then be able to perform activities such as keystroke monitoring if the ForwardX11Trusted option is also enabled.

If X11 services are not required for the system's intended function, they should be disabled or restricted as appropriate to the system's needs.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为X11Forwarding no，则pass，否则fail：
grep -w '^X11Forwarding no' /etc/ssh/sshd_config
```

**修复方法：**


```bash
使用以下命令修改/etc/ssh/sshd_config文件，增加或修改以下文字，然后重启sshd服务：
# vim /etc/ssh/sshd_config
ClientAliveInterval 600
# sudo systemctl restart sshd.service
```

### 11.5 SLEM 5 must deny direct logons to the root account using remote access via SSH.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

To ensure individual accountability and prevent unauthorized access, organizational users must be individually identified and authenticated.

A group authenticator is a generic account used by multiple individuals. Use of a group authenticator alone does not uniquely identify individual users. Examples of the group authenticator is the Unix OS "root" user account, the Windows "Administrator" account, the "sa" account, or a "helpdesk" account.

For example, the Unix and Windows SLEM 5 offer a "switch user" capability, allowing users to authenticate with their individual credentials and, when needed, "switch" to the administrator role. This method provides for unique individual authentication prior to using a group authenticator.

Users (and any processes acting on behalf of users) need to be uniquely identified and authenticated for all accesses other than those accesses explicitly identified and documented by the organization, which outlines specific user actions that can be performed on SLEM 5 without identification or authentication.

Requiring individuals to be authenticated with an individual authenticator prior to using a group authenticator allows for traceability of actions, as well as adding an additional level of protection of the actions that can be taken with group account knowledge.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为PermitRootLogin no，则pass，否则fail：
grep -w '^PermitRootLogin no' /etc/ssh/sshd_config
```

**修复方法：**


```bash
使用以下命令修改/etc/ssh/sshd_config文件，增加或修改以下文字：
# vim /etc/ssh/sshd_config
PermitRootLogin no
```

### 11.6 SLEM 5 must log SSH connection attempts and failures to the server.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Remote access services, such as those providing remote access to network devices and information systems, which lack automated monitoring capabilities, increase risk and make remote user access management difficult at best.

Remote access is access to DOD nonpublic information systems by an authorized user (or an information system) communicating through an external, nonorganization-controlled network. Remote access methods include, for example, dial-up, broadband, and wireless.

Automated monitoring of remote access sessions allows organizations to detect cyberattacks and also ensure ongoing compliance with remote access policies by auditing connection activities of remote access capabilities, such as Remote Desktop Protocol (RDP), on a variety of information system components (e.g., servers, workstations, notebook computers, smartphones, and tablets).

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为LogLevel VERBOSE，则pass，否则fail：
grep -w '^LogLevel VERBOSE' /etc/ssh/sshd_config
```

**修复方法：**


```bash
使用以下命令修改/etc/ssh/sshd_config文件，增加或修改以下文字：
# vim /etc/ssh/sshd_config
LogLevel VERBOSE
```

### 11.7 SLEM 5 must display the date and time of the last successful account logon upon an SSH logon.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Providing users with feedback on when account accesses via SSH last occurred facilitates user recognition and reporting of unauthorized account use.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为PrintLastLog yes，则pass，否则fail：
grep -w '^PrintLastLog yes' /etc/ssh/sshd_config
```

**修复方法：**


```bash
使用以下命令修改/etc/ssh/sshd_config文件，增加或修改以下文字：
# vim /etc/ssh/sshd_config
PrintLastLog yes
```

### 11.8 SLEM 5 SSH daemon must be configured to not allow authentication using known hosts authentication.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Configuring this setting for the SSH daemon provides additional assurance that remote logon via SSH will require a password, even in the event of misconfiguration elsewhere.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为IgnoreUserKnownHosts yes，则pass，否则fail：
grep -w '^IgnoreUserKnownHosts yes' /etc/ssh/sshd_config
```

**修复方法：**


```bash
使用以下命令修改/etc/ssh/sshd_config文件，增加或修改以下文字：
# vim /etc/ssh/sshd_config
gnoreUserKnownHosts yes
```

### 11.9 SLEM 5 SSH daemon must perform strict mode checking of home directory configuration files.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If other users have access to modify user-specific SSH configuration files, they may be able to log on to the system as another user.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为StrictModes yes，则pass，否则fail：
grep -w '^StrictModes yes' /etc/ssh/sshd_config
```

**修复方法：**


```bash
使用以下命令修改/etc/ssh/sshd_config文件，增加或修改以下文字：
# vim /etc/ssh/sshd_config
StrictModes yes
```

### 11.10 SLEM 5, for PKI-based authentication, must enforce authorized access to the corresponding private key.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If the private key is discovered, an attacker can use the key to authenticate as an authorized user and gain access to the network infrastructure.

The cornerstone of the PKI is the private key used to encrypt or digitally sign information.

If the private key is stolen, this will lead to the compromise of the authentication and nonrepudiation gained through PKI because the attacker can use the private key to digitally sign documents and pretend to be the authorized user.

Both the holders of a digital certificate and the issuing authority must protect the computers, storage devices, or whatever they use to keep the private keys.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若返回值为Load key "/etc/ssh/ssh_host_dsa_key": Permission denied，则pass，否则fail：
ssh-keygen -y -f /etc/ssh/ssh_host_dsa_key
```

**修复方法：**


```bash
使用以下命令创建一个新的私蒥和公钥对，该密钥对使用密码：
# sudo ssh-keygen -n <密码短语>
```

### 11.11 SLEM 5 must have SSH installed to protect the confidentiality and integrity of transmitted information.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

Without protection of the transmitted information, confidentiality and integrity may be compromised because unprotected communications can be intercepted and either read or altered. 

This requirement applies to both internal and external networks and all types of information system components from which information can be transmitted (e.g., servers, mobile devices, notebook computers, printers, copiers, scanners, and facsimile machines). Communication paths outside the physical protection of a controlled boundary are exposed to the possibility of interception and modification. 

Protecting the confidentiality and integrity of organizational information can be accomplished by physical means (e.g., employing physical distribution systems) or by logical means (e.g., employing cryptographic techniques). If physical means of protection are employed, logical means (cryptography) do not have to be employed, and vice versa.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若存在返回值则pass，否则fail
# find / -name ssh-keygen
```

**修复方法：**


在镜像制作过程中安装openssh组件

### 11.12 SLEM 5 must use SSH to protect the confidentiality and integrity of transmitted information.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

Without protection of the transmitted information, confidentiality and integrity may be compromised because unprotected communications can be intercepted and either read or altered.

This requirement applies to both internal and external networks and all types of information system components from which information can be transmitted (e.g., servers, mobile devices, notebook computers, printers, copiers, scanners, and facsimile machines). Communication paths outside the physical protection of a controlled boundary are exposed to the possibility of interception and modification.

Protecting the confidentiality and integrity of organizational information can be accomplished by physical means (e.g., employing physical distribution systems) or by logical means (e.g., employing cryptographic techniques). If physical means of protection are employed, logical means (cryptography) do not have to be employed, and vice versa.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若返回值中包含active，则pass，否则fail
# systemctl status sshd.service | grep -i active
```

**修复方法：**


```bash
使用如下方法启用服务，并配置永久生效：
# systemctl start sshd
# systemctl enable sshd
```

### 11.13 SLEM 5 must not allow unattended or automatic logon via SSH.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

Failure to restrict system access via SSH to authenticated users negatively impacts SLEM 5 security.

**规则影响：**

无

**检查方法：**


```bash
执行如下2条命令：若存在返回值则pass，否则fail
# grep -w "^PermitEmptyPasswords no" /etc/ssh/sshd_config
# grep -w "^PermitUserEnvironment no" /etc/ssh/sshd_config
```

**修复方法：**


修改/etc/ssh/sshd_config文件，配置PermitUserEnvironment字段为no，重启sshd服务：

### 11.14 SLEM 5 must implement DOD-approved encryption to protect the confidentiality of SSH remote connections.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

Without confidentiality protection mechanisms, unauthorized individuals may gain access to sensitive information via a remote access session.

Remote access is access to DOD nonpublic information systems by an authorized user (or an information system) communicating through an external, nonorganization-controlled network. Remote access methods include, for example, dial-up, broadband, and wireless.

Encryption provides a means to secure the remote connection to prevent unauthorized access to the data traversing the remote access connection (e.g., RDP), thereby providing a degree of confidentiality. The encryption strength of a mechanism is selected based on the security categorization of the information.

The system will attempt to use the first cipher presented by the client that matches the server list. Listing the values "strongest to weakest" is a method to ensure the use of the strongest cipher available to secure the SSH connection.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若存在返回值则pass，否则fail
# grep -w "^Ciphers aes256-ctr,aes192-ctr,aes128-ctr" /etc/ssh/sshd_config
```

**修复方法：**


```bash
修改/etc/ssh/sshd_config文件，增加配置，执行服务重载
# vim /etc/ssh/sshd_config
Ciphers aes256-ctr,aes192-ctr,aes128-ctr
# systemctl restart sshd
```

### 11.15 SLEM 5 SSH daemon must be configured to only use Message Authentication Codes (MACs) employing FIPS 140-2/140-3 approved cryptographic hash algorithms.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

Without cryptographic integrity protections, information can be altered by unauthorized users without detection.

Remote access (e.g., RDP) is access to DOD nonpublic information systems by an authorized user (or an information system) communicating through an external, nonorganization-controlled network. Remote access methods include, for example, dial-up, broadband, and wireless.

Cryptographic mechanisms used for protecting the integrity of information include, for example, signed hash functions using asymmetric cryptography enabling distribution of the public key to verify the hash information while maintaining the confidentiality of the secret key used to generate the hash.

The system will attempt to use the first hash presented by the client that matches the server list. Listing the values "strongest to weakest" is a method to ensure the use of the strongest hash available to secure the SSH connection.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若存在返回值则pass，否则fail
# grep -w "^MACs hmac-sha2-512,hmac-sha2-256" /etc/ssh/sshd_config
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/ssh/sshd_config
hmac-sha2-512,hmac-sha2-256
```

### 11.16 SLEM 5 SSH server must be configured to use only FIPS 140-2/140-3 validated key exchange algorithms.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

Without cryptographic integrity protections provided by FIPS 140-2/140-3 validated cryptographic algorithms, information can be viewed and altered by unauthorized users without detection.

The system will attempt to use the first algorithm presented by the client that matches the server list. Listing the values "strongest to weakest" is a method to ensure the use of the strongest algorithm available to secure the SSH connection.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若存在返回值则pass，否则fail
# grep -w "^KexAlgorithms ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group-exchange-sha256" /etc/ssh/sshd_config
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/ssh/sshd_config
KexAlgorithms ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group-exchange-sha256
```

### 11.17 There must be no .shosts files on SLEM 5.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

The .shosts files are used to configure host-based authentication for individual users or the system via SSH. Host-based authentication is not sufficient for preventing unauthorized access to the system as it does not require interactive identification and authentication of a connection request or for the use of two-factor authentication.

**规则影响：**

无

**检查方法：**


```bash
执行 find / \( -path /.snapshots -o -path /sys -o -path /proc \) -prune -o -name '.shosts' -print
若存在返回值则fail，否则pass
```

**修复方法：**


```bash
删除任何 ".shosts" 文件
rm /<文件路径>/.shosts
```

### 11.18 There must be no shosts.equiv files on SLEM 5.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

The shosts.equiv files are used to configure host-based authentication for the system via SSH. Host-based authentication is not sufficient for preventing unauthorized access to the system, as it does not require interactive identification and authentication of a connection request, or for the use of two-factor authentication.

**规则影响：**

无

**检查方法：**


```bash
执行find /etc -name shosts.equiv
若存在返回值则fail，否则pass
```

**修复方法：**


```bash
删除任何 "shosts.equiv" 文件
rm /<文件路径>/shosts.equiv
```

### 11.19 SLEM 5 must not allow unattended or automatic logon via the graphical user interface (GUI).

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

Failure to restrict system access to authenticated users negatively impacts SLEM 5 security.

**规则影响：**

无

**检查方法：**


```bash
检查系统是否开启图形用户界面，如果未开启则该项为pass，否则执行如下命令，如果返回值中包含某条输出结果不是"no"，则fail，否则pass：
# grep -i ^DISPLAYMANAGER_AUTOLOGIN /etc/sysconfig/displaymanager
     DISPLAYMANAGER_AUTOLOGIN=""

# grep -i ^DISPLAYMANAGER_PASSWORD_LESS_LOGIN /etc/sysconfig/displaymanager
     DISPLAYMANAGER_PASSWORD_LESS_LOGIN="no"
```

**修复方法：**


```bash
关闭图形界面功能，或者通过如下命令修改配置：
# vim /etc/sysconfig/displaymanager
将包含DISPLAYMANAGER_AUTOLOGIN或DISPLAYMANAGER_PASSWORD_LESS_LOGIN的条目均改为"no"
```

## 12 网卡设置

### 12.1 SLEM 5 wireless network adapters must be disabled unless approved and documented.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without protection of communications with wireless peripherals, confidentiality and integrity may be compromised because unprotected communications can be intercepted and either read, altered, or used to compromise SLEM 5.

This requirement applies to wireless peripheral technologies (e.g., wireless mice, keyboards, displays, etc.) used with a SLEM 5. Wireless peripherals (e.g., Wi-Fi/Bluetooth/IR keyboards, mice, pointing devices, and Near Field Communications [NFC]) present a unique challenge by creating an open, unsecured port on a computer. Wireless peripherals must meet DOD requirements for wireless data transmission and be approved for use by the AO. Even though some wireless peripherals, such as mice and pointing devices, do not ordinarily carry information that need to be protected, modification of communications with these wireless peripherals may be used to compromise SLEM 5. Communication paths outside the physical protection of a controlled boundary are exposed to the possibility of interception and modification.

Protecting the confidentiality and integrity of communications with wireless peripherals can be accomplished by physical means (e.g., employing physical barriers to wireless radio frequencies) or by logical means (e.g., employing cryptographic techniques). If physical means of protection are employed, then logical means (cryptography) do not have to be employed, and vice versa. If the wireless peripheral is only passing telemetry data, encryption of the data may not be required.

**规则影响：**

无

**检查方法：**


```bash
KubeOS不支持无线网络使用以下命令验证是否启用了无线网络适配器：
# wicked show all…
wlan0 up
link: #3, state up, mtu 1500
type: wireless, hwaddr 06:00:00:00:00:02
config: wicked:xml:/etc/wicked/ifconfig/wlan0.xml
leases: ipv4 dhcp granted
addr: ipv4 10.0.0.101/16 [dhcp]
route: ipv4 default via 10.0.0.1 proto dhcp
如果配置了无线接口且未在 AO（授权官员）处记录和批准，这将被视为一个发现项，无需检测。
```

**修复方法：**


```bash
配置禁用所有无线网络接口，使用以下命令：
对于每个类型为无线的接口，将该接口置于“down”状态：
 > sudo wicked ifdown wlan0
对于每个类型为无线且配置类型为“compat:suse：”的接口，删除关联的文件：
 > sudo rm /etc/sysconfig/network/ifcfg-wlan0
对于每个类型为无线的接口，对于每个类型为“wicked:xml：”的配置，删除关联的文件或从文件中删除该接口的配置。
 > sudo rm /etc/wicked/ifconfig/wlan0.xml
```

## 13 usb-storage

### 13.1 SLEM 5 must disable the USB mass storage kernel module.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without identifying devices, unidentified or unknown devices may be introduced, thereby facilitating malicious activity.

Peripherals include but are not limited to such devices as flash drives, external storage, and printers.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令验证：
> grep usb-storage /etc/modprobe.d/50-blacklist.conf
blacklist usb-storage
如果该行被注释掉或该行缺失，则视为发现项。
```

**修复方法：**


在“/etc/modprobe.d/50-blacklist.conf”文件中添加或修改以下行：
blacklist usb-storage

## 14 用户账号&口令

### 14.1 All SLEM 5 local interactive user accounts, upon creation, must be assigned a home directory.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If local interactive users are not assigned a valid home directory, there is no place for the storage and control of files they should own.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若该命令无返回值，则fail，否则pass：
grep -i "^CREATE_HOME yes" /etc/login.defs
```

**修复方法：**


```bash
将系统配置为为所有新的本地交互用户分配主目录。
在 "/etc/login.defs" 文件中添加或修改以下行：
# vim /etc/login.defs
CREATE_HOME yes
```

### 14.2 SLEM 5 default permissions must be defined in such a way that all authenticated users can only read and modify their own files.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Setting the most restrictive default permissions ensures that when new accounts are created, they do not have unnecessary access.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若该命令无返回值，则fail，否则pass：
grep -i "^UMASK 077" /etc/login.defs
```

**修复方法：**


```bash
将系统配置为定义所有已认证用户的默认权限，以确保这些用户只能读取和修改自己的文件。
在 "/etc/login.defs" 文件中添加或修改以下行：
# vim /etc/login.defs
UMASK 077
```

### 14.3 SLEM 5 shadow password suite must be configured to enforce a delay of at least five seconds between logon prompts following a failed logon attempt.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Limiting the number of logon attempts over a certain time interval reduces the chances that an unauthorized user may gain access to an account.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查，若该命令无返回值，则fail，否则pass：
grep -wi "^fail_delay 5" /etc/login.defs
```

**修复方法：**


```bash
配置系统，以确保在登录尝试失败后，再次出现登录提示之前，至少有五秒的延迟。
在 "/etc/login.defs" 文件中添加或修改以下行：
# vim /etc/login.defs
FAIL_DELAY 5
```

### 14.4 All SLEM 5 local interactive users must have a home directory assigned in the /etc/passwd file.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If local interactive users are not assigned a valid home directory, there is no place for the storage and control of files they should own.

**规则影响：**

无

**检查方法：**


```bash
使用以下命令检查本地交互用户是否已分配主目录，如无返回则pass：
getent passwd | while IFS=: read user _ _ _ _ homedir shell; do
    if grep -qxF "$shell" /etc/shells 2>/dev/null; then
        if [ ! -d "$homedir" ]; then
            echo "$user: fail"
        fi
    fi
done
```

**修复方法：**


```bash
为所有当前未分配主目录的系统本地交互用户分配主目录。
通过 `usermod` 命令为用户分配主目录：
# sudo usermod -d {用户家目录} {用户名}
```

### 14.5 All SLEM 5 local interactive user initialization files executable search paths must contain only paths that resolve to the users' home directory.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

The executable search path (typically the PATH environment variable) contains a list of directories for the shell to search to find executables. If this path includes the current working directory (other than the user's home directory), executables in these directories may be executed instead of system commands. This variable is formatted as a colon-separated list of directories. If there is an empty entry, such as a leading or trailing colon or two consecutive colons, this is interpreted as the current working directory. If deviations from the default system search path for the local interactive user are required, they must be documented with the information system security officer (ISSO).

**规则影响：**

无

**检查方法：**


```bash
执行grep -i path= /home/<username>/.*
若该命令的返回值为：/home/<username>/.bash_profile:PATH=$PATH:$HOME/.local/bin:$HOME/bin，则pass，否则fail
```

**修复方法：**


修改/home/<username>/.bash_profile文件，设置正确的PATH环境变量：
PATH=$PATH:$HOME/.local/bin:$HOME/bin

### 14.6 SLEM 5 must automatically expire temporary accounts within 72 hours.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Temporary accounts are privileged or nonprivileged accounts established during pressing circumstances, such as new software or hardware configuration or an incident response, where the need for prompt account activation requires bypassing normal account authorization procedures. If any inactive temporary accounts are left enabled on the system and are not either manually removed or automatically expired within 72 hours, the security posture of the system will be degraded and exposed to exploitation by unauthorized users or insider threat actors. 

Temporary accounts are different from emergency accounts. Emergency accounts, also known as "last resort" or "break glass" accounts, are local logon accounts enabled on the system for emergency use by authorized system administrators to manage a system when standard logon methods are failing or not available. Emergency accounts are not subject to manual removal or scheduled expiration requirements.

The automatic expiration of temporary accounts may be extended as needed by the circumstances, but it must not be extended indefinitely. A documented permanent account should be established for privileged users who need long-term maintenance accounts.

**规则影响：**

无

**检查方法：**


```bash
执行chage -l <temporary_account_name> | grep -E '(Password|Account) expires'，若Password expires和Account expires的值距离系统时间在72h以内，则pass，否则fail
```

**修复方法：**


```bash
运行以下命令，设置临时账号有效期72小时：
# chage -E $(date -d +3days +%Y-%m-%d) <temporary_account_name>
```

### 14.7 SLEM 5 must never automatically remove or disable emergency administrator accounts.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Emergency administrator accounts, also known as "last resort" or "break glass" accounts, are local logon accounts enabled on the system for emergency use by authorized system administrators to manage a system when standard logon methods are failing or not available. Emergency accounts are not subject to manual removal or scheduled expiration requirements.

**规则影响：**

无

**检查方法：**


```bash
执行chage -l <emergency_administrator_account_name> | grep -E '(Password|Account) expires'，若Password expires和Account expires的值是never或者一个超大的日期，则pass，否则fail
```

**修复方法：**


```bash
运行以下命令，使紧急管理员账户永远不会自动删除或禁用：
# chage -I -1 -M 99999 <emergency_administrator_account_name>
```

### 14.8 SLEM 5 must not have unnecessary accounts.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Accounts providing no operational purpose provide additional opportunities for system compromise. Unnecessary accounts include user accounts for individuals not requiring access to the system and application accounts for applications not installed on the system.

**规则影响：**

无

**检查方法：**


执行more /etc/passwd
若返回值中存在账户名与系统文档账户名中实际不符的，则fail，否则pass

**修复方法：**


```bash
运行以下命令，删除系统文档不需要的账户：
# userdel <username>
```

### 14.9 SLEM 5 must not have unnecessary account capabilities.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Accounts providing no operational purpose provide additional opportunities for system compromise. Therefore all necessary noninteractive accounts should not have an interactive shell assigned to them.

**规则影响：**

无

**检查方法：**


```bash
执行awk -F: 'NR==FNR { shells[$1]=1; next } $3 > 1000 && ($7 in shells) ' /etc/shells /etc/passwd
若存在返回值则fail，否则pass
```

**修复方法：**


```bash
运行以下命令，为特定的非交互式用户账户禁用交互式shell：
# usermod --shell /sbin/nologin <username>
```

### 14.10 SLEM 5 must disable account identifiers (individuals, groups, roles, and devices) after 35 days of inactivity after password expiration.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Inactive identifiers pose a risk to systems and applications because attackers may exploit an inactive identifier and potentially obtain undetected access to the system. Owners of inactive accounts will not notice if unauthorized access to their user account has been obtained.

SLEM 5 must track periods of inactivity and disable application identifiers after 35 days of inactivity.

**规则影响：**

无

**检查方法：**


```bash
执行grep -i '^INACTIVE=35' /etc/default/useradd
，若有返回值，则pass，否则fail
```

**修复方法：**


修改/etc/default/useradd文件，添加或修改配置：
INACTIVE=35

### 14.11 SLEM 5 must not have duplicate User IDs (UIDs) for interactive users.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

To ensure accountability and prevent unauthenticated access, interactive users must be identified and authenticated to prevent potential misuse and compromise of the system.

Interactive users include organizational employees or individuals the organization deems to have equivalent status of employees (e.g., contractors). Interactive users (and processes acting on behalf of users) must be uniquely identified and authenticated to all accesses, except for the following: 

1) Accesses explicitly identified and documented by the organization. Organizations document specific user actions that can be performed on the information system without identification or authentication; and

2) Accesses that occur through authorized use of group authenticators without individual authentication. Organizations may require unique identification of individuals in group accounts (e.g., shared privilege accounts) or for detailed accountability of individual activity.

**规则影响：**

无

**检查方法：**


```bash
执行awk -F ":" 'list[$3]++{print $1, $3}' /etc/passwd，若无返回值，则pass，否则fail
```

**修复方法：**


```bash
假设存在UID重复的用户，且已确认该用户是无用的僵尸用户，则使用userdel <username>命令删除该用户
# userdel <username>
原则上用户UID是唯一的，若存在重复UID，需要使用useradd、usermod、userdel工具来管理用户。
```

### 14.12 SLEM 5 must display the date and time of the last successful account logon upon logon.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Providing users with feedback on when account accesses last occurred facilitates user recognition and reporting of unauthorized account use.

**规则影响：**

无

**检查方法：**


执行grep "session required pam_lastlog.so showfailed" /etc/pam.d/login
，若有返回值，则pass，否则fail

**修复方法：**


修改/etc/pam.d/login文件，在头部配置：
session required pam_lastlog.so showfailed

### 14.13 SLEM 5 must initiate a session lock after a 15-minute period of inactivity.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

A session time-out lock is a temporary action taken when a user stops work and moves away from the immediate physical vicinity of the information system but does not log out because of the temporary nature of the absence.

Rather than relying on the users to manually lock their SLEM 5 session prior to vacating the vicinity, SLEM 5 needs to be able to identify when a user's session has idled and take action to initiate the session lock.

The session lock is implemented at the point where session activity can be determined and/or controlled.

**规则影响：**

无

**检查方法：**


```bash
执行如下3条命令，如均有返回值则pass，否则fail：
1.grep -w "^TMOUT=900" /etc/profile.d/autologout.sh
2.grep -w "^readonly TMOUT" /etc/profile.d/autologout.sh
3.grep -w "^export TMOUT" /etc/profile.d/autologout.sh
```

**修复方法：**


```bash
修改/etc/profile.d/autologout.sh文件，增加或修改配置：
TMOUT=900
readonly TMOUT
export TMOUT
```

### 14.14 SLEM 5 must lock an account after three consecutive invalid access attempts.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

By limiting the number of failed access attempts, the risk of unauthorized system access via user password guessing, otherwise known as brute-forcing, is reduced. Limits are imposed by locking the account.

The pam_tally2.so module maintains a count of attempted accesses. This includes username entry into a logon field as well as password entry. With counting access attempts, it is possible to lock an account without presenting a password into the password field. This should be taken into consideration as it poses as an avenue for denial of service (DoS).

**规则影响：**

无

**检查方法：**


```bash
执行如下2条命令，如均有返回值则pass，否则fail：
1.grep -w "^auth required pam_faillock.so onerr=fail silent audit deny=3" /etc/pam.d/common-auth
2.grep -w "^account required pam_faillock.so" /etc/pam.d/common-account
```

**修复方法：**


修改/etc/pam.d/common-auth文件，增加或修改配置：
auth required pam_faillock.so onerr=fail silent audit deny=3
account required pam_faillock.so

### 14.15 SLEM 5 must enforce a delay of at least five seconds between logon prompts following a failed logon attempt via pluggable authentication modules (PAM).

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Limiting the number of logon attempts over a certain time interval reduces the chances that an unauthorized user may gain access to an account.

**规则影响：**

无

**检查方法：**


执行grep “auth required pam_faildelay.so delay=5000000“ /etc/pam.d/common-auth，若有返回值，则pass，否则fail

**修复方法：**


修改/etc/pam.d/common-auth文件，增加或修改配置：
auth required pam_faildelay.so delay=5000000

### 14.16 SLEM 5 must use the invoking user's password for privilege escalation when using "sudo".

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

The sudoers security policy requires that users authenticate themselves before they can use sudo. When sudoers requires authentication, it validates the invoking user's credentials. If the rootpw, targetpw, or runaspw flags are defined and not disabled, by default the operating system will prompt the invoking user for the "root" user password.

For more information on each of the listed configurations, reference the sudoers(5) manual page.

**规则影响：**

无

**检查方法：**


```bash
执行grep -rE '^Defaults !(targetpw|rootpw|runaspw)$' /etc/sudoers /etc/sudoers.d/，若返回值存在targetpw，rootpw，runaspw，则pass，否则fail
# grep -rE '^Defaults !(targetpw|rootpw|runaspw)$' /etc/sudoers /etc/sudoers.d/
Defaults !targetpw
Defaults !rootpw
Defaults !runaspw
```

**修复方法：**


修改/etc/sudoers文件，添加或修改如下3行配置：
Defaults !targetpw
Defaults !rootpw
Defaults !runaspw

### 14.17 SLEM 5 must reauthenticate users when changing authenticators, roles, or escalating privileges.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without reauthentication, users may access resources or perform tasks for which they do not have authorization.

When SLEM 5 provides the capability to change user authenticators, change security roles, or escalate a functional capability, it is critical the user reauthenticate.

**规则影响：**

无

**检查方法：**


```bash
1.执行grep -i "^nopasswd" /etc/sudoers，若存在返回值，则fail，否则pass
# grep -i "^nopasswd" /etc/sudoers

2.执行grep -i "^!authenticate" /etc/sudoers，若存在返回值，则fail，否则pass
# grep -i "^!authenticate" /etc/sudoers
```

**修复方法：**


```bash
使用sed命令删除/etc/sudoers文件中的"NOPASSWD"或"!authenticate"账号：
# sed -i '/NOPASSWD/d' /etc/sudoers
# sed -i '/!authenticate/d' /etc/sudoers
```

### 14.18 SLEM 5 must require reauthentication when using the "sudo" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without reauthentication, users may access resources or perform tasks for which they do not have authorization.

When operating systems provide the capability to escalate a functional capability, it is critical the organization requires the user to reauthenticate when using the "sudo" command.

If the value is set to an integer less than 0, the user's time stamp will not expire and the user will not have to reauthenticate for privileged actions until the user's session is terminated.

**规则影响：**

无

**检查方法：**


```bash
执行grep -ir 'timestamp_timeout' /etc/sudoers /etc/sudoers.d，若返回值中存在:Defaults timestamp_timeout=0，则pass，否则fail
# grep -ir 'timestamp_timeout' /etc/sudoers /etc/sudoers.d
/etc/sudoers:Defaults timestamp_timeout=0
```

**修复方法：**


修改/etc/sudoers文件，添加或修改配置：
Defaults timestamp_timeout=0

### 14.19 SLEM 5 must restrict privilege elevation to authorized personnel.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

The sudo command allows a user to execute programs with elevated (administrator) privileges. It prompts the user for their password and confirms the request to execute a command by checking a file, called sudoers. If the "sudoers" file is not configured correctly, any user defined on the system can initiate privileged actions on the target system.

**规则影响：**

无

**检查方法：**


```bash
执行grep -iw 'ALL' /etc/sudoers /etc/sudoers.d/*，若返回值中不存在如下两行配置项，则pass，否则fail
ALL     ALL=(ALL) ALL
ALL     ALL=(ALL:ALL) ALL
# grep -iw 'ALL' /etc/sudoers /etc/sudoers.d/*
/etc/sudoers:root       ALL=(ALL)       ALL
```

**修复方法：**


删除/etc/sudoers文件中的如下配置行：
ALL     ALL=(ALL) ALL
ALL     ALL=(ALL:ALL) ALL

### 14.20 SLEM 5 must specify the default "include" directory for the /etc/sudoers file.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

The "sudo" command allows authorized users to run programs (including shells) as other users, system users, and root. The "/etc/sudoers" file is used to configure authorized "sudo" users as well as the programs they are allowed to run. Some configuration options in the "/etc/sudoers" file allow configured users to run programs without reauthenticating. Use of these configuration options makes it easier for one compromised account to be used to compromise other accounts.

It is possible to include other sudoers files from within the sudoers file currently being parsed using the @include and @includedir directives. For compatibility with sudo versions prior to 1.9.1, #include and #includedir are also accepted. When sudo reaches this line it will suspend processing of the current file (/etc/sudoers) and switch to the specified file/directory. Once the end of the included file(s) is reached, the rest of /etc/sudoers will be processed. Files that are included may themselves include other files. A hard limit of 128 nested include files is enforced to prevent include file loops.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w "^#includedir /etc/sudoers.d" /etc/sudoers
，若存在返回值，则pass，否则fail
# grep -w "^#includedir /etc/sudoers.d" /etc/sudoers
#includedir /etc/sudoers.d
```

**修复方法：**


```bash
修改/etc/sudoers文件，添加或修改如下配置行：
#includedir /etc/sudoers.d
```

### 14.21 SLEM 5 must enforce passwords that contain at least one uppercase character.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Use of a complex password helps increase the time and resources required to compromise the password. Password complexity, or strength, is a measure of the effectiveness of a password in resisting attempts at guessing and brute-force attacks.

Password complexity is one factor of several that determines how long it takes to crack a password. The more complex the password, the greater the number of possible combinations that need to be tested before the password is compromised.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w "^password requisite pam_pwquality.so ucredit=-1" /etc/pam.d/common-password
，若存在返回值，则pass，否则fail
# grep -w "^password requisite pam_pwquality.so ucredit=-1" /etc/pam.d/common-password
password requisite pam_cracklib.so ucredit=-1
```

**修复方法：**


修改/etc/pam.d/common-password文件，添加或修改配置：
password requisite pam_pwquality.so ucredit=-1

### 14.22 SLEM 5 must enforce passwords that contain at least one lowercase character.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Use of a complex password helps increase the time and resources required to compromise the password. Password complexity, or strength, is a measure of the effectiveness of a password in resisting attempts at guessing and brute-force attacks.

Password complexity is one factor of several that determines how long it takes to crack a password. The more complex the password, the greater the number of possible combinations that need to be tested before the password is compromised.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w "^password requisite pam_pwquality.so lcredit=-1" /etc/pam.d/common-password
，若存在返回值，则pass，否则fail
```

**修复方法：**


修改/etc/pam.d/common-password文件，添加或修改配置：
password requisite pam_pwquality.so lcredit=-1

### 14.23 SLEM 5 must enforce passwords that contain at least one numeric character.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Use of a complex password helps increase the time and resources required to compromise the password. Password complexity, or strength, is a measure of the effectiveness of a password in resisting attempts at guessing and brute-force attacks.

Password complexity is one factor of several that determines how long it takes to crack a password. The more complex the password, the greater the number of possible combinations that need to be tested before the password is compromised.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w "^password requisite pam_pwquality.so dcredit=-1" /etc/pam.d/common-password
，若存在返回值，则pass，否则fail
```

**修复方法：**


修改/etc/pam.d/common-password文件，添加或修改配置：
password requisite pam_pwquality.so dcredit=-1

### 14.24 SLEM 5 must enforce passwords that contain at least one special character.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Use of a complex password helps increase the time and resources required to compromise the password. Password complexity or strength is a measure of the effectiveness of a password in resisting attempts at guessing and brute-force attacks.

Password complexity is one factor in determining how long it takes to crack a password. The more complex the password, the greater the number of possible combinations that need to be tested before the password is compromised.

Special characters are not alphanumeric. Examples include: ~ ! @ # $ % ^ *.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w "^password requisite pam_pwquality.so ocredit=-1" /etc/pam.d/common-password
，若存在返回值，则pass，否则fail
```

**修复方法：**


修改/etc/pam.d/common-password文件，添加或修改配置：
password requisite pam_pwquality.so ocredit=-1

### 14.25 SLEM 5 must prevent the use of dictionary words for passwords.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If SLEM 5 allows the user to select passwords based on dictionary words, this increases the chances of password compromise by increasing the opportunity for successful guesses and brute-force attacks.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w "^password requisite pam_pwquality.so" /etc/pam.d/common-password
，若存在返回值，则pass，否则fail
```

**修复方法：**


修改/etc/pam.d/common-password文件，添加或修改配置：
password requisite pam_pwquality.so

### 14.26 SLEM 5 must employ passwords with a minimum of 15 characters.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

The shorter the password, the lower the number of possible combinations that need to be tested before the password is compromised.

Password complexity, or strength, is a measure of the effectiveness of a password in resisting attempts at guessing and brute-force attacks. Password length is one factor of several that helps determine strength and how long it takes to crack a password. Use of more characters in a password helps exponentially increase the time and/or resources required to compromise the password.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w "^password requisite pam_pwquality.so minlen=15" /etc/pam.d/common-password
，若存在返回值，则pass，否则fail
```

**修复方法：**


修改/etc/pam.d/common-password文件，添加或修改配置：
password requisite pam_pwquality.so minlen=15

### 14.27 SLEM 5 must require the change of at least eight of the total number of characters when passwords are changed.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If SLEM 5 allows the user to consecutively reuse extensive portions of passwords, this increases the chances of password compromise by increasing the window of opportunity for attempts at guessing and brute-force attacks.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w "^password requisite pam_pwquality.so difok=8" /etc/pam.d/common-password
，若存在返回值，则pass，否则fail
```

**修复方法：**


修改/etc/pam.d/common-password文件，添加或修改配置：
password requisite pam_pwquality.so difok=8

### 14.28 SLEM 5 must not allow passwords to be reused for a minimum of five generations.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Password complexity, or strength, is a measure of the effectiveness of a password in resisting attempts at guessing and brute-force attacks. If the information system or application allows the user to consecutively reuse their password when that password has exceeded its defined lifetime, the end result is a password that is not changed as per policy requirements.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w "^password requisite pam_pwhistory.so remember=5 use_authtok" /etc/pam.d/common-password
，若存在返回值，则pass，否则fail
```

**修复方法：**


修改/etc/pam.d/common-password文件，添加或修改配置：
password requisite pam_pwhistory.so remember=5 use_authtok

### 14.29 SLEM 5 must configure the Linux Pluggable Authentication Modules (PAM) to only store encrypted representations of passwords.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Passwords need to be protected at all times, and encryption is the standard method for protecting passwords. If passwords are not encrypted, they can be plainly read (i.e., clear text) and easily compromised.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w "^password required pam_unix.so sha512" /etc/pam.d/common-password
，若存在返回值，则pass，否则fail
```

**修复方法：**


修改/etc/pam.d/common-password文件，添加或修改配置：
password required pam_unix.so sha512

### 14.30 SLEM 5 must employ user passwords with a minimum lifetime of 24 hours (one day).

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Enforcing a minimum password lifetime helps prevent repeated password changes to defeat the password reuse or history enforcement requirement. If users are allowed to immediately and continually change their password, the password could be repeatedly changed in a short period of time to defeat the organization's policy regarding password reuse.

**规则影响：**

无

**检查方法：**


```bash
执行awk -F: '$4 < 1 {print $1 ":" $4}' /etc/shadow
若返回值中<username>的值为1，则pass，否则fail
```

**修复方法：**


```bash
执行passwd命令设置账户的口令修改间隔时间为1天
# passwd -n 1 <username>
```

### 14.31 SLEM 5 must employ user passwords with a maximum lifetime of 60 days.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Any password, no matter how complex, can eventually be cracked. Therefore, passwords need to be changed periodically. If SLEM 5 does not limit the lifetime of passwords and force users to change their passwords, there is the risk that SLEM 5 passwords could be compromised.

**规则影响：**

无

**检查方法：**


```bash
执行awk -F: '$5 > 60 || $5 == "" {print $1 ":" $5}' /etc/shadow
若返回值中<username>的值为60，则pass，否则fail
```

**修复方法：**


```bash
执行passwd命令设置账户的口令有效期为60天：
# passwd -x 60 <username>
```

### 14.32 SLEM 5 must employ a password history file.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Password complexity, or strength, is a measure of the effectiveness of a password in resisting attempts at guessing and brute-force attacks. If the information system or application allows the user to consecutively reuse their password when that password has exceeded its defined lifetime, the end result is a password that is not changed as per policy requirements.

**规则影响：**

无

**检查方法：**


```bash
执行find /etc/security -name opasswd，若存在返回值，则pass，否则fail
```

**修复方法：**


```bash
按照以下命令创建密码历史记录文件：
创建文件：
# touch /etc/security/opasswd
设置所有权权限：
# chown root:root /etc/security/opasswd
设置访问权限：
# chmod 0600 /etc/security/opasswd
```

### 14.33 SLEM 5 must employ FIPS 140-2/140-3 approved cryptographic hashing algorithm for system authentication (login.defs).

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Unapproved mechanisms that are used for authentication to the cryptographic module are not verified and therefore cannot be relied on to provide confidentiality or integrity, and DOD data may be compromised.

SLEM 5 using encryption are required to use FIPS 140-2/140-3 compliant mechanisms for authenticating to cryptographic modules.

FIPS 140-2/140-3 is the current standard for validating that mechanisms used to access cryptographic modules use authentication that meets DOD requirements. This allows for Security Levels 1, 2, 3, or 4 for use on a general-purpose computing system.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w "^ENCRYPT_METHOD SHA512 " /etc/login.defs
，若存在返回值，则pass，否则fail
```

**修复方法：**


修改/etc/login.defs文件，添加或修改配置：
ENCRYPT_METHOD SHA512

### 14.34 SLEM 5 must be configured to create or update passwords with a minimum lifetime of 24 hours (one day).

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Enforcing a minimum password lifetime helps prevent repeated password changes to defeat the password reuse or history enforcement requirement. If users are allowed to immediately and continually change their password, the password could be repeatedly changed in a short period of time to defeat the organization's policy regarding password reuse.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w "^PASS_MIN_DAYS 1 " /etc/login.defs
，若存在返回值，则pass，否则fail
```

**修复方法：**


修改/etc/login.defs文件，添加或修改配置：
PASS_MIN_DAYS 1

### 14.35 SLEM 5 must be configured to create or update passwords with a maximum lifetime of 60 days.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Any password, no matter how complex, can eventually be cracked. Therefore, passwords need to be changed periodically. If SLEM 5 does not limit the lifetime of passwords and force users to change their passwords, there is the risk that SLEM 5 passwords could be compromised.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w "^PASS_MAX_DAYS   7 " /etc/login.defs
，若存在返回值，则pass，否则fail
```

**修复方法：**


修改/etc/login.defs文件，添加或修改配置：
PASS_MAX_DAYS   7

### 14.36 SLEM 5 must implement multifactor authentication for access to privileged accounts via pluggable authentication modules (PAM).

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Using an authentication device, such as a Common Access Card (CAC) or token that is separate from the information system, ensures that even if the information system is compromised, that compromise will not affect credentials stored on the authentication device.

Multifactor solutions that require devices separate from information systems gaining access include, for example, hardware tokens providing time-based or challenge-response authenticators and smart cards such as the U.S. Government Personal Identity Verification (PIV) card and the DOD CAC.

A privileged account is defined as an information system account with authorizations of a privileged user.

Remote access is access to DOD nonpublic information systems by an authorized user (or an information system) communicating through an external, nonorganization-controlled network. Remote access methods include, for example, dial-up, broadband, and wireless.

This requirement only applies to components where this is specific to the function of the device or has the concept of an organizational user (e.g., VPN, proxy capability). This does not apply to authentication for the purpose of configuring the device itself (management).

**规则影响：**

无

**检查方法：**


```bash
执行grep -w "^auth sufficient pam_pkcs11.so" /etc/pam.d/common-auth
 ，若存在返回值，则pass，否则fail
```

**修复方法：**


修改/etc/pam.d/common-auth文件，添加或修改配置：
auth sufficient pam_pkcs11.so

### 14.37 SLEM 5 must implement certificate status checking for multifactor authentication.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Using an authentication device, such as a Common Access Card (CAC) or token separate from the information system, ensures credentials stored on the authentication device will not be affected if the information system is compromised.

Multifactor solutions that require devices separate from information systems to gain access include hardware tokens providing time-based or challenge-response authenticators, and smart cards such as the U.S. Government Personal Identity Verification (PIV) card and the DOD CAC.

A privileged account is defined as an information system account with authorizations of a privileged user.

Remote access is access to DOD nonpublic information systems by an authorized user (or an information system) communicating through an external, nonorganization-controlled network. Remote access methods include, for example, dial-up, broadband, and wireless.

This requirement only applies to components with device-specific functions, or for organizational users (e.g., VPN, proxy capability). This does not apply to authentication for the purpose of configuring the device itself (management).

**规则影响：**

无

**检查方法：**


```bash
执行grep use_pkcs11_module /etc/pam_pkcs11/pam_pkcs11.conf | awk '/pkcs11_module coolkey {/,/}/' /etc/pam_pkcs11/pam_pkcs11.conf | grep cert_policy，若返回结果为cert_policy = ca,ocsp_on,signature,crl_auto;则pass，否则fail
```

**修复方法：**


修改 /etc/pam_pkcs11/pam_pkcs11.conf文件，修改配置：保证所有的cert_policy值中包含ocsp_on配置。

### 14.38 If Network Security Services (NSS) is being used by SLEM 5 it must prohibit the use of cached authentications after one day.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If cached authentication information is out of date, the validity of the authentication information may be questionable.

**规则影响：**

无

**检查方法：**


```bash
执行grep -i "^memcache_timeout" /etc/sssd/sssd.conf
若返回结果为memcache_timeout = 86400，则pass，否则fail
```

**修复方法：**


修改/etc/sssd/sssd.conf文件，添加或修改配置：
在"[nss]"下配置
memcache_timeout = 86400

### 14.39 SLEM 5 must configure the Linux Pluggable Authentication Modules (PAM) to prohibit the use of cached offline authentications after one day.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If cached authentication information is out of date, the validity of the authentication information may be questionable.

**规则影响：**

无

**检查方法：**


执行grep "offline_credentials_expiration" /etc/sssd/sssd.conf
若返回结果为offline_credentials_expiration = 1，则pass，否则fail

**修复方法：**


修改/etc/sssd/sssd.conf文件，添加或修改配置：
在"[pam]"下配置
offline_credentials_expiration = 1

### 14.40 SLEM 5, for PKI-based authentication, must validate certificates by constructing a certification path (which includes status information) to an accepted trust anchor.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without path validation, an informed trust decision by the relying party cannot be made when presented with any certificate not already explicitly trusted.

A trust anchor is an authoritative entity represented via a public key and associated data. It is used in the context of public key infrastructures, X.509 digital certificates, and DNSSEC.

When there is a chain of trust, usually the top entity to be trusted becomes the trust anchor; it can be, for example, a Certification Authority (CA). A certification path starts with the subject certificate and proceeds through a number of intermediate certificates up to a trusted root certificate, typically issued by a trusted CA.

This requirement verifies that a certification path to an accepted trust anchor is used for certificate validation and that the path includes status information. Path validation is necessary for a relying party to make an informed trust decision when presented with any certificate not already explicitly trusted. Status information for certification paths includes certificate revocation lists or online certificate status protocol responses. Validation of the certificate status information is out of scope for this requirement.

**规则影响：**

无

**检查方法：**


执行grep cert_policy /etc/pam_pkcs11/pam_pkcs11.conf
若返回结果为cert_policy = ca,ocsp_on,signature,crl_auto;，则pass，否则fail

**修复方法：**


修改 /etc/pam_pkcs11/pam_pkcs11.conf文件，添加或修改配置：
cert_policy = ca,oscp_on,signature,crl_auto

### 14.41 SLEM 5 must be configured to not overwrite Pluggable Authentication Modules (PAM) configuration on package changes.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

The "pam-config" command line utility automatically generates a system PAM configuration as packages are installed, updated, or removed from the system. "pam-config" removes configurations for PAM modules and parameters that it does not know about. It may render ineffective PAM configuration by the system administrator and thus impact system security.

**规则影响：**

无

**检查方法：**


```bash
执行find /etc/pam.d/ -type l -iname "common-*"
若无返回结果，则pass，否则fail
```

**修复方法：**


```bash
备份PAM配置文件到，使用以下命令删除PAM配置文件的软链接：
# sudo sh -c 'for X in /etc/pam.d/common-*-pc; do cp -ivp --remove-destination $X ${X:0:-3}; done'
```

### 14.42 SLEM 5 root account must be the only account with unrestricted access to the system.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

If an account other than root also has a User Identifier (UID) of "0", it has root authority, giving that account unrestricted access to the entire SLEM 5. Multiple accounts with a UID of "0" afford an opportunity for potential intruders to guess a password for a privileged account.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若存在返回值则pass，否则fail
# awk -F: '$3 == 0 {print $1}' /etc/passwd | grep root
```

**修复方法：**


```bash
直接修改/etc/passwd中root账号的GID字段为0，然后重启系统
# usermod -g 0 root
```

### 14.43 SLEM 5 must not be configured to allow blank or null passwords.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

Passwords need to be protected at all times, and encryption is the standard method for protecting passwords. If passwords are not encrypted, they can be plainly read (i.e., clear text) and easily compromised.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若存在返回值则fail，否则pass
# pam_unix.so /etc/pam.d/* | grep nullok
```

**修复方法：**


```bash
检查如下配置文件，若存在nullok配置，需要删除：
# vim /etc/pam.d/password-auth
auth        sufficient    pam_unix.so nullok try_first_pass

# vim /etc/pam.d/password-auth
password    sufficient    pam_unix.so sha512 shadow nullok try_first_pass use_authtok

# vim /etc/pam.d/system-auth
auth        sufficient    pam_unix.so nullok try_first_pass

# vim /etc/pam.d/system-auth
password    sufficient    pam_unix.so sha512 shadow nullok try_first_pass use_authtok
```

### 14.44 SLEM 5 must not have accounts configured with blank or null passwords.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

If an account has an empty password, anyone could log on and run commands with the privileges of that account. Accounts with empty passwords should never be used in operational environments.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令。若存在返回值则fail，否则pass
# awk -F: '!$2 {print $1}' /etc/shadow
```

**修复方法：**


```bash
通过如下命令删除查询到的使用空密码的账户：
查询：
# awk -F: '!$2 {print $1}' /etc/shadow
删除：
# vim /etc/login.defs
```

### 14.45 SLEM 5 must employ FIPS 140-2/140-3-approved cryptographic hashing algorithms for system authentication.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

The system must use a strong hashing algorithm to store the password. The system must use a sufficient number of hashing rounds to ensure the required level of entropy.

Passwords need to be protected at all times, and encryption is the standard method for protecting passwords. If passwords are not encrypted, they can be plainly read (i.e., clear text) and easily compromised.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令。若存在返回值则pass，否则fail
# grep -w "^ENCRYPT_METHOD SHA512" /etc/login.defs
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/login.defs
ENCRYPT_METHOD SHA512
```

### 14.46 SLEM 5 shadow password suite must be configured to use a sufficient number of hashing rounds.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

The system must use a strong hashing algorithm to store the password. The system must use a sufficient number of hashing rounds to ensure the required level of entropy.

Passwords need to be protected at all times, and encryption is the standard method for protecting passwords. If passwords are not encrypted, they can be plainly read (i.e., clear text) and easily compromised.

**规则影响：**

无

**检查方法：**


```bash
执行如下2条命令：若存在返回值则pass，否则fail
# grep -w "^SHA_CRYPT_MAX_ROUNDS 5000" /etc/login.defs
# grep -w "^SHA_CRYPT_MIN_ROUNDS 5000" /etc/login.defs
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/login.defs
SHA_CRYPT_MIN_ROUNDS 5000
SHA_CRYPT_MAX_ROUNDS 5000
```

## 15 SELinux

### 15.1 SLEM 5 must enable the SELinux targeted policy.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without verification of the security functions, security functions may not operate correctly and the failure may go unnoticed. Security function is defined as the hardware, software, and/or firmware of the information system responsible for enforcing the system security policy and supporting the isolation of code and data on which the protection is based. Security functionality includes, but is not limited to, establishing system accounts, configuring access authorizations (i.e., permissions, privileges), setting events to be audited, and setting intrusion detection parameters.

This requirement applies to operating systems performing security function verification/testing and/or systems and environments that require this functionality.

**规则影响：**

无

**检查方法：**


```bash
执行grep -i "SELINUXTYPE=targeted" /etc/selinux/config，若有返回值，则pass，否则fail
```

**修复方法：**


修改/etc/selinux/config文件，增加或修改配置：
SELINUXTYPE=targeted

### 15.2 SLEM 5 must prevent nonprivileged users from executing privileged functions, including disabling, circumventing, or altering implemented security safeguards/countermeasures.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Preventing nonprivileged users from executing privileged functions mitigates the risk that unauthorized individuals or processes may gain unnecessary access to information or privileges.

Privileged functions include, for example, establishing accounts, performing system integrity checks, or administering cryptographic key management activities. Nonprivileged users are individuals who do not possess appropriate authorizations. Circumventing intrusion detection and prevention mechanisms or malicious code protection mechanisms are examples of privileged functions that require protection from nonprivileged users.

**规则影响：**

无

**检查方法：**


```bash
执行semanage login -l | more 检测:
1.所有管理员必须映射到“sysadm_u”、“staff_u”或组织定义的适当定制的受限角色。
2.所有授权的非管理员用户必须映射到“user_u”角色。
满足以上两点，则pass，否则fail
```

**修复方法：**


```bash
1.所有管理员必须映射到“sysadm_u”、“staff_u”或组织定义的适当定制的受限角色。
2.所有授权的非管理员用户必须映射到“user_u”角色。
3.修改selinux配置：
使用以下命令将新用户映射到“sysadm_u”角色：
# semanage login -a -s sysadm_u <username>

使用以下命令将现有用户映射到“sysadm_u”角色：
# semanage login -m -s sysadm_u <username>

使用以下命令将新用户映射到“staff_u”角色：
# semanage login -a -s staff_u <username>

使用以下命令将现有用户映射到“staff_u”角色：
# semanage login -m -s staff_u <username>

使用以下命令将新用户映射到“user_u”角色：
# semanage login -a -s user_u <username>

使用以下命令将现有用户映射到“user_u”角色：
# semanage login -m -s user_u <username>
```

### 15.3 SLEM 5 must use a Linux Security Module configured to enforce limits on system services.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

Without verification of the security functions, security functions may not operate correctly and the failure may go unnoticed. Security function is defined as the hardware, software, and/or firmware of the information system responsible for enforcing the system security policy and supporting the isolation of code and data on which the protection is based. Security functionality includes, but is not limited to, establishing system accounts, configuring access authorizations (i.e., permissions, privileges), setting events to be audited, and setting intrusion detection parameters.

This requirement applies to operating systems performing security function verification/testing and/or systems and environments that require this functionality.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若存在返回值则pass，否则fail
# getenforce | grep Enforcing
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/selinux/config
SELINUX=Enforcing
# setfiles -c /etc/selinux/targeted/policy/policy.33 /etc/selinux/targeted/contexts/files/file_contexts  /

重启系统后生效
# reboot
```

## 16 aide

### 16.1 SLEM 5 must use a file integrity tool to verify correct operation of all security functions.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without verification of the security functions, security functions may not operate correctly, and the failure may go unnoticed. Security function is defined as the hardware, software, and/or firmware of the information system responsible for enforcing the system security policy and supporting the isolation of code and data on which the protection is based. Security functionality includes, but is not limited to, establishing system accounts, configuring access authorizations (i.e., permissions, privileges), setting events to be audited, and setting intrusion detection parameters.

This requirement applies to SLEM 5 performing security function verification/testing and/or systems and environments that require this functionality.

**规则影响：**

无

**检查方法：**


```bash
执行/usr/sbin/aide -h| grep "Usage: aide \[options\] command"，若存在返回值则pass，否则fail
```

**修复方法：**


```bash
协议安装aide组件：
# yum -y install aide
```

### 16.2 SLEM 5 file integrity tool must be configured to verify Access Control Lists (ACLs).

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

ACLs can provide permissions beyond those permitted through the file mode and must be verified by file integrity tools.

**规则影响：**

无

**检查方法：**


执行如下4条命令，若均存在返回值则pass，否则fail
1.grep "^FIPSR = p+i+n+u+g+s+m+c+acl+selinux+xattrs+sha256" /etc/aide.conf
2.grep "^DIR = p+i+n+u+g+acl+selinux+xattrs" /etc/aide.conf
3.grep "^PERMS = p+i+u+g+acl+selinux" /etc/aide.conf
4.grep "^DATAONLY =  p+n+u+g+s+acl+selinux+xattrs+sha256" /etc/aide.conf

**修复方法：**


修改/etc/aide.conf文件，添加或修改配置：
FIPSR = p+i+n+u+g+s+m+c+acl+selinux+xattrs+sha256
DIR = p+i+n+u+g+acl+selinux+xattrs
PERMS = p+i+u+g+acl+selinux
DATAONLY =  p+n+u+g+s+acl+selinux+xattrs+sha256

### 16.3 SLEM 5 file integrity tool must be configured to verify extended attributes.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Extended attributes in file systems are used to contain arbitrary data and file metadata with security implications.

**规则影响：**

无

**检查方法：**


执行如下3条命令，若均存在返回值则pass，否则fail
1.grep "^FIPSR = p+i+n+u+g+s+m+c+acl+selinux+xattrs+sha256" /etc/aide.conf
2.grep "^DIR = p+i+n+u+g+acl+selinux+xattrs" /etc/aide.conf
3.grep "^DATAONLY =  p+n+u+g+s+acl+selinux+xattrs+sha256" /etc/aide.conf

**修复方法：**


修改/etc/aide.conf文件，添加或修改配置：
FIPSR = p+i+n+u+g+s+m+c+acl+selinux+xattrs+sha256
DIR = p+i+n+u+g+acl+selinux+xattrs
DATAONLY =  p+n+u+g+s+acl+selinux+xattrs+sha256

### 16.4 SLEM 5 file integrity tool must be configured to protect the integrity of the audit tools.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Protecting the integrity of the tools used for auditing purposes is a critical step toward ensuring the integrity of audit information. Audit information includes all information (e.g., audit records, audit settings, and audit reports) needed to successfully audit information system activity.

Audit tools include but are not limited to vendor-provided and open-source audit tools needed to successfully view and manipulate audit information system activity and records. Audit tools include custom queries and report generators.

It is not uncommon for attackers to replace the audit tools or inject code into the existing tools to provide the capability to hide or erase system activity from the audit logs.

To address this risk, audit tools must be cryptographically signed to provide the capability to identify when the audit tools have been modified, manipulated, or replaced. An example is a checksum hash of the file or files.

**规则影响：**

无

**检查方法：**


执行grep /usr/sbin/au /etc/aide.conf命令，如果返回的这七行中的任何一行未按下述所示显示、被注释掉或缺失，则视为fail，否则pass
/usr/sbin/auditctl p+i+n+u+g+s+b+acl+selinux+xattrs+sha512
/usr/sbin/auditd p+i+n+u+g+s+b+acl+selinux+xattrs+sha512
/usr/sbin/ausearch p+i+n+u+g+s+b+acl+selinux+xattrs+sha512
/usr/sbin/aureport p+i+n+u+g+s+b+acl+selinux+xattrs+sha512
/usr/sbin/autrace p+i+n+u+g+s+b+acl+selinux+xattrs+sha512
/usr/sbin/audispd p+i+n+u+g+s+b+acl+selinux+xattrs+sha512
/usr/sbin/augenrules p+i+n+u+g+s+b+acl+selinux+xattrs+sha512

**修复方法：**


```bash
修改/etc/aide.conf文件，添加或修改配置：
# audit tools
/usr/sbin/auditctl p+i+n+u+g+s+b+acl+selinux+xattrs+sha512
/usr/sbin/auditd p+i+n+u+g+s+b+acl+selinux+xattrs+sha512
/usr/sbin/ausearch p+i+n+u+g+s+b+acl+selinux+xattrs+sha512
/usr/sbin/aureport p+i+n+u+g+s+b+acl+selinux+xattrs+sha512
/usr/sbin/autrace p+i+n+u+g+s+b+acl+selinux+xattrs+sha512
/usr/sbin/audispd p+i+n+u+g+s+b+acl+selinux+xattrs+sha512
/usr/sbin/augenrules p+i+n+u+g+s+b+acl+selinux+xattrs+sha512
```

### 16.5 Advanced Intrusion Detection Environment (AIDE) must verify the baseline SLEM 5 configuration at least weekly.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Unauthorized changes to the baseline configuration could make the system vulnerable to various attacks or allow unauthorized access to SLEM 5. Changes to SLEM 5 configurations can have unintended side effects, some of which may be relevant to security.

Detecting such changes and providing an automated response can help avoid unintended, negative consequences that could ultimately affect the security state of SLEM 5. SLEM 5's information system security manager (ISSM)/information system security officer (ISSO) and system administrator (SA) must be notified via email and/or monitoring system trap when there is an unauthorized modification of a configuration item.

**规则影响：**

无

**检查方法：**


```bash
执行grep -R aide /etc/crontab /etc/cron.*，若有返回值，则pass，否则fail
```

**修复方法：**


修改/etc/cron.weekly/aide文件，添加或修改配置：
space_left  = 25%

### 16.6 SLEM 5 must notify the system administrator (SA) when Advanced Intrusion Detection Environment (AIDE) discovers anomalies in the operation of any security functions.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If anomalies are not acted on, security functions may fail to secure the system.

Security function is defined as the hardware, software, and/or firmware of the information system responsible for enforcing the system security policy and supporting the isolation of code and data on which the protection is based. Security functionality includes, but is not limited to, establishing system accounts, configuring access authorizations (i.e., permissions, privileges), setting events to be audited, and setting intrusion detection parameters.

Notifications provided by information systems include messages to local computer consoles and/or hardware indications, such as lights.

This capability must take into account operational requirements for availability for selecting an appropriate response. The organization may choose to shut down or restart the information system upon security function anomaly detection.

**规则影响：**

无

**检查方法：**


```bash
执行grep -i "aide" /etc/cron.*/aide，若有返回值，则pass，否则fail
```

**修复方法：**


```bash
修改/etc/cron.daily/aide文件，添加或修改配置：
0 0 * * * /usr/sbin/aide --check | /bin/mail -s "$HOSTNAME - Daily AIDE integrity check run" root@example_server_name.mil
```

## 17 日志审计

### 17.1 SLEM 5 must offload rsyslog messages for networked systems in real time and offload standalone systems at least weekly.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Information stored in one location is vulnerable to accidental or incidental deletion or alteration.

Offloading is a common process in information systems with limited audit storage capacity.

**规则影响：**

无

**检查方法：**


```bash
1.执行grep -i "^*.* @<日志接收端服务器IP>:514" /etc/rsyslog.conf
若存在返回值则pass，否则fail

2.执行grep -i "^*.* @@<日志接收端服务器IP>:514" /etc/rsyslog.conf
若存在返回值则pass，否则fail
```

**修复方法：**


修改/etc/rsyslog.conf文件，添加或修改配置：
*.* @<日志接收端服务器IP>:514
*.* @@<日志接收端服务器IP>:514

### 17.2 SLEM 5 must have the auditing package installed.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without establishing what type of events occurred, the source of events, where events occurred, and the outcome of events, it would be difficult to establish, correlate, and investigate the events leading up to an outage or attack.

Audit record content that may be necessary to satisfy this requirement includes, for example, time stamps, source and destination addresses, user/process identifiers, event descriptions, success/fail indications, filenames involved, and access control or flow control rules invoked.

Associating event types with detected events in SLEM 5 audit logs provides a means of investigating an attack, recognizing resource utilization or capacity thresholds, or identifying an improperly configured SLEM 5.

**规则影响：**

无

**检查方法：**


```bash
执行auditctl -v，若有返回值，则pass，否则fail
# auditctl -v
auditctl version 3.1.2
```

**修复方法：**


```bash
使用如下命令安装audit组件：
# yum -y install audit
```

### 17.3 SLEM 5 audit records must contain information to establish what type of events occurred, the source of events, where events occurred, and the outcome of events.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without establishing what type of events occurred, the source of events, where events occurred, and the outcome of events, it would be difficult to establish, correlate, and investigate the events leading up to an outage or attack.

Audit record content that may be necessary to satisfy this requirement includes, for example, time stamps, source and destination addresses, user/process identifiers, event descriptions, success/fail indications, filenames involved, and access control or flow control rules invoked.

Associating event types with detected events in SLEM 5 audit logs provides a means of investigating an attack, recognizing resource utilization or capacity thresholds, or identifying an improperly configured SLEM 5.

**规则影响：**

无

**检查方法：**


```bash
1.执行systemctl is-active auditd.service，若返回值为active，则pass，否则fail
# systemctl is-active auditd.service
active
2.执行systemctl is-enabled auditd.service，若返回值为enabled，则pass，否则fail
# systemctl is-enabled auditd.service
enabled
```

**修复方法：**


```bash
使用如下命令启用auditd服务：
# systemctl enable auditd.service
# systemctl start auditd.service
```

### 17.4 The audit-audispd-plugins package must be installed on SLEM 5.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Information stored in one location is vulnerable to accidental or incidental deletion or alteration.

Offloading is a common process in information systems with limited audit storage capacity.

The auditd service does not include the ability to send audit records to a centralized server for management directly. However, it can use a plug-in for audit event multiplexor to pass audit records to a remote server.

**规则影响：**

无

**检查方法：**


```bash
使用grep命令查看配置：
# grep -iw "^active = yes " /etc/audit/plugins.d/au-remote.conf
active = yes
```

**修复方法：**


修改/etc/audit/plugins.d/au-remote.conf文件，添加或修改配置：
active = yes

### 17.5 SLEM 5 must allocate audit record storage capacity to store at least one week of audit records when audit records are not immediately sent to a central audit record storage facility.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

To ensure SLEM 5 has a sufficient storage capacity in which to write the audit logs, SLEM 5 must be able to allocate audit record storage capacity.

The task of allocating audit record storage capacity is usually performed during initial installation of SLEM 5.

**规则影响：**

无

**检查方法：**


```bash
使用df -h /var/log/audit/命令检查本地审计日志是否预留足够的磁盘空间，若返回值中Use%那一列下面的数值超过50%，则fail，否则pass
# df -h /var/log/audit/
Filesystem                Size  Used Avail Use% Mounted on
/dev/mapper/euleros-root   45G  5.5G   37G  14% /
```

**修复方法：**


```bash
根据业务实际场景，删除或转移备份历史旧日志（保留当前正在写入的audit.log），为新的日志写入预留空间:
# systemctl stop auditd
# rm -rf /var/log/audit/audit.log.*.gz //删除或转移备份审计日志
# systemctl start auditd
```

### 17.6 SLEM 5 auditd service must notify the system administrator (SA) and information system security officer (ISSO) immediately when audit storage capacity is 75 percent full.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

If security personnel are not notified immediately when storage volume reaches 75 percent utilization, they are unable to plan for audit record storage capacity expansion.

**规则影响：**

无

**检查方法：**


```bash
使用grep命令查看配置：
# grep -iw "^space_left = 25% " /etc/audit/auditd.conf
space_left = 25%
```

**修复方法：**


修改/etc/audit/auditd.conf文件，添加或修改配置：
space_left  = 25%

### 17.7 SLEM 5 audit system must take appropriate action when the audit storage volume is full.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

It is critical that when SLEM 5 is at risk of failing to process audit logs as required, it takes action to mitigate the failure. Audit processing failures include software/hardware errors, failures in the audit capturing mechanisms, and audit storage capacity being reached or exceeded. Responses to audit failure depend on the nature of the failure mode.

When availability is an overriding concern, other approved actions in response to an audit failure are as follows: 

1) If the failure was caused by the lack of audit record storage capacity, SLEM 5 must continue generating audit records if possible (automatically restarting the audit service if necessary), overwriting the oldest audit records in a first-in-first-out manner.

2) If audit records are sent to a centralized collection server and communication with this server is lost or the server fails, SLEM 5 must queue audit records locally until communication is restored or until the audit records are retrieved manually. Upon restoration of the connection to the centralized collection server, action should be taken to synchronize the local audit data with the collection server.

**规则影响：**

无

**检查方法：**


```bash
使用grep命令查看配置：
# grep "^disk_full_action = HALT" /etc/audit/auditd.conf
disk_full_action = HALT
```

**修复方法：**


修改/etc/audit/auditd.conf文件，添加或修改配置：
disk_full_action = HALT

### 17.8 SLEM 5 must offload audit records onto a different system or media from the system being audited.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Information stored in one location is vulnerable to accidental or incidental deletion or alteration.

Offloading is a common process in information systems with limited audit storage capacity.

**规则影响：**

无

**检查方法：**


```bash
使用grep命令查看配置：
# grep -i "^network_failure_action = syslog"/etc/audit/audisp-remote.conf
network_failure_action = syslog
```

**修复方法：**


修改/etc/audit/audisp-remote.conf文件，添加或修改配置：
network_failure_action = syslog

### 17.9 Audispd must take appropriate action when SLEM 5 audit storage is full.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Information stored in one location is vulnerable to accidental or incidental deletion or alteration.

Offloading is a common process in information systems with limited audit storage capacity.

**规则影响：**

无

**检查方法：**


```bash
使用grep命令查看配置：
# grep -i "^disk_full_action = syslog" /etc/audisp/audisp-remote.conf
disk_full_action = syslog
```

**修复方法：**


修改/etc/audit/audisp-remote.conf文件，添加或修改配置：
disk_full_action = syslog

### 17.10 SLEM 5 must protect audit rules from unauthorized modification.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without the capability to restrict which roles and individuals can select which events are audited, unauthorized personnel may be able to prevent the auditing of critical events. Misconfigured audits may degrade the system's performance by overwhelming the audit log. Misconfigured audits may also make it more difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

**规则影响：**

无

**检查方法：**


```bash
1.执行stat -c "%n %U:%G %a" /var/log/audit，若返回值为/var/log/audit root:root 600，则pass，否则fail
2.执行stat -c "%n %U:%G %a" /var/log/audit/audit.log，若返回值为/var/log/audit/audit.log root:root 600，则pass，否则fail
3.执行stat -c "%n %U:%G %a" /etc/audit/audit.rules，若返回值为/etc/audit/audit.rules root:root 640，则pass，否则fail
4.执行stat -c "%n %U:%G %a" /etc/audit/rules.d/audit.rules，若返回值为/etc/audit/rules.d/audit.rules root:root 640，则pass，否则fail
```

**修复方法：**


```bash
1.使用chmod设置如下文件的权限：
# chmod 600 /var/log/audit
# chmod 600 /var/log/audit/audit.log
# chmod 640 /etc/audit/audit.rules
# chmod 640 /etc/audit/rules.d/audit.rules
2.使用chown设置如下文件的拥有者和属组：
# chown root:root /var/log/audit
# chown root:root /var/log/audit/audit.log
# chown root:root /etc/audit/audit.rules
# chown root:root /etc/audit/rules.d/audit.rules
```

### 17.11 SLEM 5 audit tools must have the proper permissions configured to protect against unauthorized access.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Protecting audit information includes identifying and protecting the tools used to view and manipulate log data. Protecting audit tools is necessary to prevent unauthorized operation on audit information.

SLEM 5 providing tools to interface with audit information will leverage user permissions and roles identifying the user accessing the tools and the corresponding rights the user enjoys to make access decisions regarding the access to audit tools.

Audit tools include, but are not limited to, vendor-provided and open-source audit tools needed to view and manipulate audit information system activity and records. Audit tools include custom queries and report generators.

**规则影响：**

无

**检查方法：**


```bash
(oe无/usr/sbin/audispd文件)

1.执行stat -c "%n %U:%G %a" /usr/sbin/auditctl，若返回值为/usr/sbin/auditctl root:root 750，则pass，否则fail
2.执行stat -c "%n %U:%G %a" /usr/sbin/auditd，若返回值为/usr/sbin/auditd root:root 750，则pass，否则fail
3.执行stat -c "%n %U:%G %a" /usr/sbin/ausearch，若返回值为/usr/sbin/ausearch root:root 755，则pass，否则fail
4.执行stat -c "%n %U:%G %a" /usr/sbin/aureport，若返回值为/usr/sbin/aureport root:root 755，则pass，否则fail
5.执行stat -c "%n %U:%G %a" /usr/sbin/autrace，若返回值为/usr/sbin/autrace root:root 750，则pass，否则fail
6.执行stat -c "%n %U:%G %a" /usr/sbin/augenrules，若返回值为/usr/sbin/augenrules root:root 750，则pass，否则fail
```

**修复方法：**


```bash
1.使用chmod设置如下文件的权限为750(-rwxr-x---)：
# chmod 750 /usr/sbin/auditctl
# chmod 750 /usr/sbin/auditd
# chmod 750 /usr/sbin/ausearch
# chmod 750 /usr/sbin/aureport
# chmod 750 /usr/sbin/autrace
# chmod 750 /usr/sbin/augenrules
2.使用chown设置如下文件的拥有者和属组：
# chown root:root /usr/sbin/auditctl
# chown root:root /usr/sbin/auditd
# chown root:root /usr/sbin/ausearch
# chown root:root /usr/sbin/aureport
# chown root:root /usr/sbin/autrace
# chown root:root /usr/sbin/augenrules
```

### 17.12 SLEM 5 audit tools must have the proper permissions applied to protect against unauthorized access.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Protecting audit information includes identifying and protecting the tools used to view and manipulate log data. Protecting audit tools is necessary to prevent unauthorized operation on audit information.

SLEM 5 providing tools to interface with audit information will leverage user permissions and roles identifying the user accessing the tools and the corresponding rights the user enjoys to make access decisions regarding the access to audit tools.

Audit tools include, but are not limited to, vendor-provided and open-source audit tools needed to successfully view and manipulate audit information system activity and records. Audit tools include custom queries and report generators.

**规则影响：**

无

**检查方法：**


KubeOS无/etc/permissions.local文件，无需检测。

**修复方法：**


不涉及

### 17.13 Audispd must offload audit records onto a different system or media from SLEM 5 being audited.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Information stored in one location is vulnerable to accidental or incidental deletion or alteration.

Offloading is a common process in information systems with limited audit storage capacity.

**规则影响：**

无

**检查方法：**


执行grep remote_server /etc/audisp/audisp-remote.conf若返回值中存在remote_server = <ip_address>，则pass，否则fail

手动配置能够ping通的IP地址
ping <ip_address>

**修复方法：**


根据实际情况配置远端服务器IP，修改配置文件/etc/audit/audisp-remote.conf，增加或修改remote_server = <ip_address>

### 17.14 The information system security officer (ISSO) and system administrator (SA), at a minimum, must have mail aliases to be notified of a SLEM 5 audit processing failure.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

It is critical for the appropriate personnel to be aware if a system is at risk of failing to process audit logs as required. Without this notification, the security personnel may be unaware of an impending failure of the audit capability, and system operation may be adversely affected.

Audit processing failures include software/hardware errors, failures in the audit capturing mechanisms, and audit storage capacity being reached or exceeded.

This requirement applies to each audit data storage repository (i.e., distinct information system component where audit records are stored), the centralized audit storage capacity of organizations (i.e., all audit data storage repositories combined), or both.

**规则影响：**

无

**检查方法：**


```bash
执行grep -i "^postmaster:" /etc/aliases | grep root，若返回值中postmaster的值为root，则pass，否则fail
```

**修复方法：**


修改/etc/aliases文件，添加或修改配置：
postmaster: root

### 17.15 The information system security officer (ISSO) and system administrator (SA), at a minimum, must be alerted of a SLEM 5 audit processing failure event.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

It is critical for the appropriate personnel to be aware if a system is at risk of failing to process audit logs as required. Without this notification, the security personnel may be unaware of an impending failure of the audit capability, and system operation may be adversely affected.

Audit processing failures include software/hardware errors, failures in the audit capturing mechanisms, and audit storage capacity being reached or exceeded.

This requirement applies to each audit data storage repository (i.e., distinct information system component where audit records are stored), the centralized audit storage capacity of organizations (i.e., all audit data storage repositories combined), or both.

**规则影响：**

无

**检查方法：**


执行grep action_mail /etc/audit/auditd.conf，若返回值为action_mail_acct = root，则pass，否则fail

**修复方法：**


修改/etc/audit/auditd.conf文件，添加或修改配置：
action_mail_acct = root

### 17.16 SLEM 5 must generate audit records for all uses of the "chacl" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行grep -w /usr/bin/chacl /etc/audit/rules.d/audit.rules
若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
```

**修复方法：**


```bash
修改配置文件/etc/audit/rules.d/audit.rules，增加或修改：
-a always,exit -F path=/usr/bin/chacl -F perm=x -F auid>=1000 -F auid!=unset -k prim_mod
重启auditd.service服务:
# systemctl restart auditd.service
```

### 17.17 SLEM 5 must generate audit records for all uses of the "chage" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行grep -w /usr/bin/chage /etc/audit/rules.d/audit.rules
若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
```

**修复方法：**


```bash
修改配置文件/etc/audit/rules.d/audit.rules，增加或修改：
-a always,exit -F path=/usr/bin/chage -F perm=x -F auid>=1000 -F auid!=unset -k privileged-chage
重启auditd.service服务:
# systemctl restart auditd.service
```

### 17.18 SLEM 5 must generate audit records for all uses of the "chcon" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行grep -w /usr/bin/chcon /etc/audit/rules.d/audit.rules
若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
```

**修复方法：**


```bash
修改配置文件/etc/audit/rules.d/audit.rules，增加或修改：
-a always,exit -F path=/usr/bin/chcon -F perm=x -F auid>=1000 -F auid!=unset -k prim_mod
重启auditd.service服务:
# systemctl restart auditd.service
```

### 17.19 SLEM 5 must generate audit records for all uses of the "chfn" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Reconstruction of harmful events or forensic analysis is not possible if audit records do not contain enough information.

At a minimum, the organization must audit the full-text recording of privileged commands. The organization must maintain audit trails in sufficient detail to reconstruct events to determine the cause and impact of compromise.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w /usr/bin/chfn /etc/audit/rules.d/audit.rules
若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
```

**修复方法：**


```bash
修改配置文件/etc/audit/rules.d/audit.rules，增加或修改：
-a always,exit -F path=/usr/bin/chfn -F perm=x -F auid>=1000 -F auid!=unset -k privileged-chfn
重启auditd.service服务:
# systemctl restart auditd.service
```

### 17.20 SLEM 5 must generate audit records for all uses of the "chmod" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行grep -w /usr/bin/chmod /etc/audit/rules.d/audit.rules
若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
```

**修复方法：**


```bash
修改配置文件/etc/audit/rules.d/audit.rules，增加或修改：
-a always,exit -F path=/usr/bin/chmod -F perm=x -F auid>=1000 -F auid!=unset -k prim_mod
重启auditd.service服务:
# systemctl restart auditd.service
```

### 17.21 SLEM 5 must generate audit records for a uses of the "chsh" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Reconstruction of harmful events or forensic analysis is not possible if audit records do not contain enough information.

At a minimum, the organization must audit the full-text recording of privileged commands. The organization must maintain audit trails in sufficient detail to reconstruct events to determine the cause and impact of compromise.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w /usr/bin/chsh /etc/audit/rules.d/audit.rules
若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
```

**修复方法：**


```bash
修改配置文件/etc/audit/rules.d/audit.rules，增加或修改：
-a always,exit -F path=/usr/bin/chsh -F perm=x -F auid>=1000 -F auid!=unset -k privileged-chsh
重启auditd.service服务:
# systemctl restart auditd.service
```

### 17.22 SLEM 5 must generate audit records for all uses of the "crontab" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行grep -w /usr/bin/crontab /etc/audit/rules.d/audit.rules
若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
```

**修复方法：**


```bash
修改配置文件/etc/audit/rules.d/audit.rules，增加或修改：
-a always,exit -F path=/usr/bin/crontab -F perm=x -F auid>=1000 -F auid!=unset -k privileged-crontab
重启auditd.service服务:
# systemctl restart auditd.service
```

### 17.23 SLEM 5 must generate audit records for all uses of the "gpasswd" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Reconstruction of harmful events or forensic analysis is not possible if audit records do not contain enough information.

At a minimum, the organization must audit the full-text recording of privileged commands. The organization must maintain audit trails in sufficient detail to reconstruct events to determine the cause and impact of compromise.

**规则影响：**

无

**检查方法：**


```bash
执行grep -w /usr/bin/gpasswd /etc/audit/rules.d/audit.rules
若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
```

**修复方法：**


```bash
修改配置文件/etc/audit/rules.d/audit.rules，增加或修改：
-a always,exit -F path=/usr/bin/gpasswd -F perm=x -F auid>=1000 -F auid!=unset -k privileged-gpasswd
重启auditd.service服务:
# systemctl restart auditd.service
```

### 17.24 SLEM 5 must generate audit records for all uses of the "insmod" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without the capability to generate audit records, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

The list of audited events is the set of events for which audits are to be generated. This set of events is typically a subset of the list of all events for which the system is capable of generating audit records.

DOD has defined the following list of events for which SLEM 5 will provide an audit record generation capability: 

1) Successful and unsuccessful attempts to access, modify, or delete privileges, security objects, security levels, or categories of information (e.g., classification levels);

2) Access actions, such as successful and unsuccessful logon attempts, privileged activities or other system-level access, starting and ending time for user access to the system, concurrent logons from different workstations, successful and unsuccessful accesses to objects, all program initiations, and all direct access to the information system;

3) All account creations, modifications, disabling, and terminations; and 

4) All kernel module load, unload, and restart actions.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /sbin/insmod /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-w /sbin/insmod -p x -k modules
重启服务
# systemctl restart auditd.service
```

### 17.25 SLEM 5 must generate audit records for all uses of the "kmod" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without the capability to generate audit records, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

The list of audited events is the set of events for which audits are to be generated. This set of events is typically a subset of the list of all events for which the system is capable of generating audit records.

DOD has defined the following list of events for which SLEM 5 will provide an audit record generation capability: 

1) Successful and unsuccessful attempts to access, modify, or delete privileges, security objects, security levels, or categories of information (e.g., classification levels);

2) Access actions, such as successful and unsuccessful logon attempts, privileged activities or other system-level access, starting and ending time for user access to the system, concurrent logons from different workstations, successful and unsuccessful accesses to objects, all program initiations, and all direct access to the information system;

3) All account creations, modifications, disabling, and terminations; and 

4) All kernel module load, unload, and restart actions.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /usr/bin/kmod /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-w /usr/bin/kmod -p x -k modules 
重启服务
# systemctl restart auditd.service
```

### 17.26 SLEM 5 must generate audit records for all uses of the "modprobe" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without the capability to generate audit records, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

The list of audited events is the set of events for which audits are to be generated. This set of events is typically a subset of the list of all events for which the system is capable of generating audit records.

DOD has defined the following list of events for which SLEM 5 will provide an audit record generation capability: 

1) Successful and unsuccessful attempts to access, modify, or delete privileges, security objects, security levels, or categories of information (e.g., classification levels);

2) Access actions, such as successful and unsuccessful logon attempts, privileged activities or other system-level access, starting and ending time for user access to the system, concurrent logons from different workstations, successful and unsuccessful accesses to objects, all program initiations, and all direct access to the information system;

3) All account creations, modifications, disabling, and terminations; and 

4) All kernel module load, unload, and restart actions.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /sbin/modprobe /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-w /sbin/modprobe -p x -k modules 
重启服务
# systemctl restart auditd.service
```

### 17.27 SLEM 5 must generate audit records for all uses of the "newgrp" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Reconstruction of harmful events or forensic analysis is not possible if audit records do not contain enough information.

At a minimum, the organization must audit the full-text recording of privileged commands. The organization must maintain audit trails in sufficient detail to reconstruct events to determine the cause and impact of compromise.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /usr/bin/newgrp /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F path=/usr/bin/newgrp -F perm=x -F auid>=1000 -F auid!=unset -k privileged-newgrp
重启服务
# systemctl restart auditd.service
```

### 17.28 SLEM 5 must generate audit records for all uses of the "pam_timestamp_check" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /sbin/pam_timestamp_check /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F path=/sbin/pam_timestamp_check -F perm=x -F auid>=1000 -F auid!=unset -k privileged-pam_timestamp_check
重启服务
# systemctl restart auditd.service
```

### 17.29 SLEM 5 must generate audit records for all uses of the "passwd" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Reconstruction of harmful events or forensic analysis is not possible if audit records do not contain enough information.

At a minimum, the organization must audit the full-text recording of privileged commands. The organization must maintain audit trails in sufficient detail to reconstruct events to determine the cause and impact of compromise.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /usr/bin/passwd /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F path=/usr/bin/passwd -F perm=x -F auid>=1000 -F auid!=unset -k privileged-passwd
重启服务
# systemctl restart auditd.service
```

### 17.30 SLEM 5 must generate audit records for all uses of the "rm" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /usr/bin/rm /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F path=/usr/bin/rm -F perm=x -F auid>=1000 -F auid!=unset -k prim_mod
重启服务
# systemctl restart auditd.service
```

### 17.31 SLEM 5 must generate audit records for all uses of the "rmmod" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without the capability to generate audit records, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

The list of audited events is the set of events for which audits are to be generated. This set of events is typically a subset of the list of all events for which the system is capable of generating audit records.

DOD has defined the following list of events for which SLEM 5 will provide an audit record generation capability: 

1) Successful and unsuccessful attempts to access, modify, or delete privileges, security objects, security levels, or categories of information (e.g., classification levels);

2) Access actions, such as successful and unsuccessful logon attempts, privileged activities or other system-level access, starting and ending time for user access to the system, concurrent logons from different workstations, successful and unsuccessful accesses to objects, all program initiations, and all direct access to the information system;

3) All account creations, modifications, disabling, and terminations; and 

4) All kernel module load, unload, and restart actions.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /sbin/rmmod /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-w /sbin/rmmod -p x -k modules
重启服务
# systemctl restart auditd.service
```

### 17.32 SLEM 5 must generate audit records for all uses of the "setfacl" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /usr/bin/setfacl /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F path=/usr/bin/setfacl -F perm=x -F auid>=1000 -F auid!=unset -k prim_mod
重启服务
# systemctl restart auditd.service
```

### 17.33 SLEM 5 must generate audit records for all uses of the "ssh-agent" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Reconstruction of harmful events or forensic analysis is not possible if audit records do not contain enough information.

At a minimum, the organization must audit the full-text recording of privileged commands. The organization must maintain audit trails in sufficient detail to reconstruct events to determine the cause and impact of compromise.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /usr/bin/ssh-agent /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F path=/usr/bin/ssh-agent -F perm=x -F auid>=1000 -F auid!=unset -k privileged-ssh-agent
重启服务
# systemctl restart auditd.service
```

### 17.34 SLEM 5 must generate audit records for all uses of the "ssh-keysign" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Reconstruction of harmful events or forensic analysis is not possible if audit records do not contain enough information.

At a minimum, the organization must audit the full-text recording of privileged commands. The organization must maintain audit trails in sufficient detail to reconstruct events to determine the cause and impact of compromise.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /usr/lib/ssh/ssh-keysign /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F path=/usr/lib/ssh/ssh-keysign -F perm=x -F auid>=1000 -F auid!=unset -k privileged-ssh-keysign
重启服务
# systemctl restart auditd.service
```

### 17.35 SLEM 5 must generate audit records for all uses of the "su" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /usr/bin/su /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F path=/usr/bin/su -F perm=x -F auid>=1000 -F auid!=unset -k privileged-priv_change
重启服务
# systemctl restart auditd.service
```

### 17.36 SLEM 5 must generate audit records for all uses of the "sudo" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Reconstruction of harmful events or forensic analysis is not possible if audit records do not contain enough information.

At a minimum, the organization must audit the full-text recording of privileged commands. The organization must maintain audit trails in sufficient detail to reconstruct events to determine the cause and impact of compromise.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /usr/bin/sudo /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F path=/usr/bin/sudo -F perm=x -F auid>=1000 -F auid!=unset -k privileged-sudo
重启服务
# systemctl restart auditd.service
```

### 17.37 SLEM 5 must generate audit records for all uses of the "sudoedit" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /usr/bin/sudoedit /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F path=/usr/bin/sudoedit -F perm=x -F auid>=1000 -F auid!=unset -k privileged-sudoedit
重启服务
# systemctl restart auditd.service
```

### 17.38 SLEM 5 must generate audit records for all uses of the "unix_chkpwd" or "unix2_chkpwd" commands.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /sbin/unix_chkpwd /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F path=/sbin/unix_chkpwd -F perm=x -F auid>=1000 -F auid!=unset -k privileged-unix-chkpwd
-a always,exit -F path=/sbin/unix2_chkpwd -F perm=x -F auid>=1000 -F auid!=unset -k privileged-unix2-chkpwd
重启服务
# systemctl restart auditd.service
```

### 17.39 SLEM 5 must generate audit records for all uses of the "usermod" command.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /usr/sbin/usermod /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F path=/usr/sbin/usermod -F perm=x -F auid>=1000 -F auid!=unset -k privileged-usermod
重启服务
# systemctl restart auditd.service
```

### 17.40 SLEM 5 must generate audit records for all account creations, modifications, disabling, and termination events that affect /etc/group.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Once an attacker establishes initial access to a system, the attacker often attempts to create a persistent method of reestablishing access. One way to accomplish this is for the attacker to simply create a new account. Auditing account creation mitigates this risk.

To address access requirements, SLEM 5 may be integrated with enterprise-level authentication/access/auditing mechanisms that meet or exceed access control policy requirements.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /etc/group /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-w /etc/group -p wa -k account_mod
重启服务
# systemctl restart auditd.service
```

### 17.41 SLEM 5 must generate audit records for all account creations, modifications, disabling, and termination events that affect /etc/security/opasswd.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Once an attacker establishes initial access to a system, the attacker often attempts to create a persistent method of reestablishing access. One way to accomplish this is for the attacker to simply create a new account. Auditing account creation mitigates this risk.

To address access requirements, SLEM 5 may be integrated with enterprise-level authentication/access/auditing mechanisms that meet or exceed access control policy requirements.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /etc/security/opasswd /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-w /etc/security/opasswd -p wa -k account_mod
重启服务
# systemctl restart auditd.service
```

### 17.42 SLEM 5 must generate audit records for all account creations, modifications, disabling, and termination events that affect /etc/passwd.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Once an attacker establishes initial access to a system, the attacker often attempts to create a persistent method of reestablishing access. One way to accomplish this is for the attacker to simply create a new account. Auditing account creation mitigates this risk.

To address access requirements, SLEM 5 may be integrated with enterprise-level authentication/access/auditing mechanisms that meet or exceed access control policy requirements.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /etc/passwd /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-w /etc/passwd -p wa -k account_mod
重启服务
# systemctl restart auditd.service
```

### 17.43 SLEM 5 must generate audit records for all account creations, modifications, disabling, and termination events that affect /etc/shadow.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Once an attacker establishes initial access to a system, the attacker often attempts to create a persistent method of reestablishing access. One way to accomplish this is for the attacker to simply create a new account. Auditing account creation mitigates this risk.

To address access requirements, SLEM 5 may be integrated with enterprise-level authentication/access/auditing mechanisms that meet or exceed access control policy requirements.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /etc/shadow /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-w /etc/shadow -p wa -k account_mod
重启服务
# systemctl restart auditd.service
```

### 17.44 SLEM 5 must generate audit records for all uses of the "chmod", "fchmod" and "fchmodat" system calls.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter). The system call rules are loaded into a matching engine that intercepts each syscall made by all programs on the system. Therefore, it is important to use syscall rules only when absolutely necessary since these affect performance. The more rules, the bigger the performance hit. However, the performance can be helped by combining syscalls into one rule whenever possible.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，如果 "chmod"、"fchmod" 和 "fchmodat" 系统调用没有定义 "b32" 和 "b64" 审计规则，则fail，否则pass
# grep -w chmod,fchmod,fchmodat /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F arch=b32 -S chmod,fchmod,fchmodat -F auid>=1000 -F auid!=unset -k perm_mod
-a always,exit -F arch=b64 -S chmod,fchmod,fchmodat -F auid>=1000 -F auid!=unset -k perm_mod
重启服务
# systemctl restart auditd.service
```

### 17.45 SLEM 5 must generate audit records for all uses of the "chown", "fchown", "fchownat", and "lchown" system calls.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter). The system call rules are loaded into a matching engine that intercepts each syscall made by all programs on the system. Therefore, it is very important to use syscall rules only when absolutely necessary, since these affect performance. The more rules, the bigger the performance hit. The performance can be helped, however, by combining syscalls into one rule whenever possible.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，如果返回值中的 "chown"、"fchown"、"fchownat" 和 "lchown" 系统调用定义 "b32" 和 "b64" 审计规则，则pass，否则fail
#grep -w chown,fchown,fchownat,lchown /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F arch=b32 -S chown,fchown,fchownat,lchown -F auid>=1000 -F auid!=unset -k perm_mod
-a always,exit -F arch=b64 -S chown,fchown,fchownat,lchown -F auid>=1000 -F auid!=unset -k perm_mod
重启服务
# systemctl restart auditd.service
```

### 17.46 SLEM 5 must generate audit records for all uses of the "creat", "open", "openat", "open_by_handle_at", "truncate", and "ftruncate" system calls.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter). The system call rules are loaded into a matching engine that intercepts each syscall made by all programs on the system. Therefore, it is very important to use syscall rules only when absolutely necessary, since these affect performance. The more rules, the bigger the performance hit. The performance can be helped, however, by combining syscalls into one rule whenever possible.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，
1.如果返回值中"creat"、"open"、"openat"、"open_by_handle_at"、"truncate" 和 "ftruncate" 系统调用的 "b32" 和 "b64" 审计规则均未定义，则fail，否则pass。
2.如果返回值中没有生成包含 "-F exit=-EPERM" 的规则，则fail，否则pass。
3.如果返回值中没有生成包含 "-F exit=-EACCES" 的规则，则fail，否则pass。
# grep -w creat,open,openat,open_by_handle_at,truncate,ftruncate /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F arch=b32 -S creat,open,openat,open_by_handle_at,truncate,ftruncate -F exit=-EPERM -F auid>=1000 -F auid!=unset -k perm_access
-a always,exit -F arch=b64 -S creat,open,openat,open_by_handle_at,truncate,ftruncate -F exit=-EPERM -F auid>=1000 -F auid!=unset -k perm_access

-a always,exit -F arch=b32 -S creat,open,openat,open_by_handle_at,truncate,ftruncate -F exit=-EACCES -F auid>=1000 -F auid!=unset -k perm_access
-a always,exit -F arch=b64 -S creat,open,openat,open_by_handle_at,truncate,ftruncate -F exit=-EACCES -F auid>=1000 -F auid!=unset -k perm_access
重启服务
# systemctl restart auditd.service
```

### 17.47 SLEM 5 must generate audit records for all uses of the "delete_module" system call.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，如果返回值中的"unload_module" 系统调用的 "b32" 和 "b64" 审计规则均未定义，则fail，否则pass
# grep -w delete_module /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F arch=b32 -S delete_module -F auid>=1000 -F auid!=unset -k unload_module
-a always,exit -F arch=b64 -S delete_module -F auid>=1000 -F auid!=unset -k unload_module
重启服务
# systemctl restart auditd.service
```

### 17.48 SLEM 5 must generate audit records for all uses of the "init_module" and "finit_module" system calls.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter). The system call rules are loaded into a matching engine that intercepts each syscall made by all programs on the system. Therefore, it is very important to use syscall rules only when absolutely necessary, since these affect performance. The more rules, the bigger the performance hit. The performance can be helped, however, by combining syscalls into one rule whenever possible.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，如果返回值中的"init_module" 和 "finit_module" 系统调用没有定义 "b32" 和 "b64" 审计规则，则fail，否则pass
# grep -w init_module,finit_module /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F arch=b32 -S init_module,finit_module -F auid>=1000 -F auid!=unset -k moduleload
-a always,exit -F arch=b64 -S init_module,finit_module -F auid>=1000 -F auid!=unset -k moduleload
重启服务
# systemctl restart auditd.service
```

### 17.49 SLEM 5 must generate audit records for all uses of the "mount" system call.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Reconstruction of harmful events or forensic analysis is not possible if audit records do not contain enough information.

At a minimum, the organization must audit the full-text recording of privileged commands. The organization must maintain audit trails in sufficient detail to reconstruct events to determine the cause and impact of compromise.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，如果返回值中的"mount" 系统调用没有定义 "b32"和"b64"审计规则，则fail，否则pass
# grep -w mount /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F arch=b32 -S mount -F auid>=1000 -F auid!=unset -k privileged-mount
-a always,exit -F arch=b64 -S mount -F auid>=1000 -F auid!=unset -k privileged-mount
重启服务
# systemctl restart auditd.service
```

### 17.50 SLEM 5 must generate audit records for all uses of the "setxattr", "fsetxattr", "lsetxattr", "removexattr", "fremovexattr", and "lremovexattr" system calls.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter). The system call rules are loaded into a matching engine that intercepts each syscall made by all programs on the system. Therefore, it is very important to use syscall rules only when absolutely necessary, since these affect performance. The more rules, the bigger the performance hit. The performance can be helped, however, by combining syscalls into one rule whenever possible.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，如果返回值中的"b32" 和 "b64" 审计规则未针对 "setxattr"、"fsetxattr"、"lsetxattr"、"removexattr"、"fremovexattr" 和 "lremovexattr" 系统调用进行定义，则fail，否则pass
# grep -w setxattr,fsetxattr,lsetxattr,removexattr,fremovexattr,lremovexattr /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F arch=b32 --a always,exit -F arch=b32 -S setxattr,fsetxattr,lsetxattr,removexattr,fremovexattr,lremovexattr -F auid>=1000 -F auid!=unset -k perm_mod
-a always,exit -F arch=b64 -S setxattr,fsetxattr,lsetxattr,removexattr,fremovexattr,lremovexattr -F auid>=1000 -F auid!=unset -k perm_mod
重启服务
# systemctl restart auditd.service
```

### 17.51 SLEM 5 must generate audit records for all uses of the "umount" system call.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Reconstruction of harmful events or forensic analysis is not possible if audit records do not contain enough information.

At a minimum, the organization must audit the full-text recording of privileged commands. The organization must maintain audit trails in sufficient detail to reconstruct events to determine the cause and impact of compromise.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，如果返回值中的"umount"系统调用没有定义 "b32" 和 "b64" 审计规则，则fail，否则pass
# grep -w umount /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F arch=b32 -S umount -F auid>=1000 -F auid!=unset -k privileged-umount
-a always,exit -F arch=b32 -S umount2 -F auid>=1000 -F auid!=unset -k privileged-umount
-a always,exit -F arch=b64 -S umount2 -F auid>=1000 -F auid!=unset -k privileged-umount
重启服务
# systemctl restart auditd.service
```

### 17.52 SLEM 5 must generate audit records for all uses of the "unlink", "unlinkat", "rename", "renameat", and "rmdir" system calls.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter). The system call rules are loaded into a matching engine that intercepts each syscall made by all programs on the system. Therefore, it is very important to use syscall rules only when absolutely necessary, since these affect performance. The more rules, the bigger the performance hit. The performance can be helped, however, by combining syscalls into one rule whenever possible.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，如果返回值中的 "b32" 和 "b64" 审计规则未针对 "unlink"、"unlinkat"、"rename"、"renameat" 和 "rmdir" 系统调用进行定义，则fail，否则pass
# grep -w unlink,unlinkat,rename,renameat,rmdir /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F arch=b32 -S unlink,unlinkat,rename,renameat,rmdir -F auid>=1000 -F auid!=unset -k perm_mod
-a always,exit -F arch=b64 -S unlink,unlinkat,rename,renameat,rmdir -F auid>=1000 -F auid!=unset -k perm_mod
重启服务
# systemctl restart auditd.service
```

### 17.53 SLEM 5 must generate audit records for all uses of privileged functions.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Misuse of privileged functions, either intentionally or unintentionally by authorized users, or by unauthorized external entities that have compromised information system accounts, is a serious and ongoing concern and can have significant adverse impacts on organizations. Auditing the use of privileged functions is one way to detect such misuse and identify the risk from insider threats and the advanced persistent threat.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，
1.如果未返回针对 "SUID" 文件的 "b32" 和 "b64" 审计规则，则fail，否则pass
2.如果未返回针对 "SGID" 文件的 "b32" 和 "b64" 审计规则，则fail，否则pass
#grep -w execve /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F arch=b32 -S execve -C uid!=euid -F euid=0 -k setuid
-a always,exit -F arch=b64 -S execve -C uid!=euid -F euid=0 -k setuid
-a always,exit -F arch=b32 -S execve -C gid!=egid -F egid=0 -k setgid
-a always,exit -F arch=b64 -S execve -C gid!=egid -F egid=0 -k setgid
重启服务
# systemctl restart auditd.service
```

### 17.54 SLEM 5 must generate audit records for all modifications to the "lastlog" file.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /var/log/lastlog /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-w /var/log/lastlog -p wa -k logins
重启服务
# systemctl restart auditd.service
```

### 17.55 SLEM 5 must generate audit records for all modifications to the "tallylog" file must generate an audit record.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
grep -w /var/log/tallylog /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-w /var/log/tallylog -p wa -k logins
重启服务
# systemctl restart auditd.service
```

### 17.56 SLEM 5 must audit all uses of the sudoers file and all files in the "/etc/sudoers.d/" directory.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Reconstruction of harmful events or forensic analysis is not possible if audit records do not contain enough information.

At a minimum, the organization must audit the full-text recording of privileged access commands. The organization must maintain audit trails in sufficient detail to reconstruct events to determine the cause and impact of compromise.

**规则影响：**

无

**检查方法：**


```bash
1.执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /etc/sudoers /etc/audit/rules.d/audit.rules
2.执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /etc/sudoers.d /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-w /etc/sudoers -p wa -k privileged-actions

-w /etc/sudoers.d -p wa -k privileged-actions
重启服务
# systemctl restart auditd.service
```

### 17.57 Successful/unsuccessful uses of "setfiles" in SLEM 5 must generate an audit record.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Reconstruction of harmful events or forensic analysis is not possible if audit records do not contain enough information.

At a minimum, the organization must audit the full-text recording of privileged commands. The organization must maintain audit trails in sufficient detail to reconstruct events to determine the cause and impact of compromise. The "setfiles" command is primarily used to initialize the security context fields (extended attributes) on one or more filesystems (or parts of them). Usually it is initially run as part of the SELinux installation process (a step commonly known as labeling).

When a user logs on, the AUID is set to the UID of the account that is being authenticated. Daemons are not user sessions and have the loginuid set to "-1". The AUID representation is an unsigned 32-bit integer, which equals "4294967295". The audit system interprets "-1", "4294967295", and "unset" in the same way.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /usr/sbin/setfiles /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F path=/usr/sbin/setfiles -F perm=x -F auid>=1000 -F auid!=unset -k privileged-unix-update
重启服务
# systemctl restart auditd.service
```

### 17.58 Successful/unsuccessful uses of "semanage" in SLEM 5 must generate an audit record.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Reconstruction of harmful events or forensic analysis is not possible if audit records do not contain enough information.

At a minimum, the organization must audit the full-text recording of privileged commands. The organization must maintain audit trails in sufficient detail to reconstruct events to determine the cause and impact of compromise. The "semanage" command is used to configure certain elements of SELinux policy without requiring modification to or recompilation from policy sources.

When a user logs on, the AUID is set to the UID of the account that is being authenticated. Daemons are not user sessions and have the loginuid set to "-1". The AUID representation is an unsigned 32-bit integer, which equals "4294967295". The audit system interprets "-1", "4294967295", and "unset" in the same way.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /usr/sbin/semanage /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F path=/usr/sbin/semanage -F perm=x -F auid>=1000 -F auid!=unset -k privileged-unix-update
重启服务
# systemctl restart auditd.service
```

### 17.59 Successful/unsuccessful uses of "setsebool" in SLEM 5 must generate an audit record.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Reconstruction of harmful events or forensic analysis is not possible if audit records do not contain enough information.

At a minimum, the organization must audit the full-text recording of privileged commands. The organization must maintain audit trails in sufficient detail to reconstruct events to determine the cause and impact of compromise. The "setsebool" command sets the current state of a particular SELinux Boolean or a list of Booleans to a given value.

When a user logs on, the AUID is set to the UID of the account that is being authenticated. Daemons are not user sessions and have the loginuid set to "-1". The AUID representation is an unsigned 32-bit integer, which equals "4294967295". The audit system interprets "-1", "4294967295", and "unset" in the same way.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /usr/sbin/setsebool /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-a always,exit -F path=/usr/sbin/setsebool -F perm=x -F auid>=1000 -F auid!=unset -k privileged-unix-update
重启服务
# systemctl restart auditd.service
```

### 17.60 SLEM 5 must generate audit records for the "/run/utmp file".

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /run/utmp /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-w /run/utmp -p wa -k login_mod
重启服务
# systemctl restart auditd.service
```

### 17.61 SLEM 5 must generate audit records for the "/var/log/btmp" file.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /var/log/btmp /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-w /var/log/btmp -p wa -k login_mod
重启服务
# systemctl restart auditd.service
```

### 17.62 SLEM 5 must generate audit records for the "/var/log/wtmp" file.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

Without generating audit records specific to the security and mission needs of the organization, it would be difficult to establish, correlate, and investigate the events relating to an incident or identify those responsible for one.

Audit records can be generated from various components within the information system (e.g., module or policy filter).

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w /var/log/wtmp /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
-w /var/log/wtmp -p wa -k login_mod
重启服务
# systemctl restart auditd.service
```

### 17.63 SLEM 5 must not disable syscall auditing.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

By default, SLEM 5 includes the "-a task,never" audit rule as a default. This rule suppresses syscall auditing for all tasks started with this rule in effect. Because the audit daemon processes the "audit.rules" file from the top down, this rule supersedes all other defined syscall rules; therefore no syscall auditing can take place on the operating system.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则pass，否则fail
# grep -i "\-a task,never" /etc/audit/rules.d/audit.rules
```

**修复方法：**


```bash
通过如下命令修改配置：
# vim /etc/audit/rules.d/audit.rules
找到并删除带"-a task,never" 配置
重启服务
# systemctl restart auditd.service
```

### 17.64 SLEM 5 must limit the number of concurrent sessions to 10 for all accounts and/or account types.

**级别：** 建议（LOW）

**适用版本：** ALL

**规则说明：**

SLEM 5 management includes the ability to control the number of users and user sessions that use a SLEM 5. Limiting the number of allowed users and sessions per user is helpful in reducing the risks related to denial-of-service (DoS) attacks.

This requirement addresses concurrent sessions for information system accounts and does not address concurrent sessions by single users via multiple system accounts. The maximum number of concurrent sessions should be defined based on mission needs and the operational environment for each system.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w "* hard maxlogins 10" /etc/security/limits.conf
```

**修复方法：**


```bash
修改/etc/security/limits.conf文件，增加或修改配置：
# vim /etc/security/limits.conf
* hard maxlogins 10
```

### 17.65 SLEM 5 must have policycoreutils package installed.

**级别：** 建议（LOW）

**适用版本：** ALL

**规则说明：**

Without verification of the security functions, security functions may not operate correctly and the failure may go unnoticed. Security function is defined as the hardware, software, and/or firmware of the information system responsible for enforcing the system security policy and supporting the isolation of code and data on which the protection is based. Security functionality includes, but is not limited to, establishing system accounts, configuring access authorizations (i.e., permissions, privileges), setting events to be audited, and setting intrusion detection parameters.

Policycoreutils contains the policy core utilities that are required for basic operation of an SELinux-enabled system. These utilities include load_policy to load SELinux policies, setfile to label filesystems, newrole to switch roles, and run_init to run /etc/init.d scripts in the proper context.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令中包含"SELinux status"，则pass，否则fail
# sestatus -v
```

**修复方法：**


在镜像制作过程中安装policycoreutils

### 17.66 SLEM 5 audit event multiplexor must be configured to use Kerberos.

**级别：** 建议（LOW）

**适用版本：** ALL

**规则说明：**

Information stored in one location is vulnerable to accidental or incidental deletion or alteration.

Allowing devices and users to connect to or from the system without first authenticating them allows untrusted access and can lead to a compromise or attack. Audit events that may include sensitive data must be encrypted prior to transmission. Kerberos provides a mechanism to provide both authentication and encryption for audit event records.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令没有返回任何输出，或者返回的行被注释掉，则fail，否则pass
# grep -w "enable_krb5 = yes" /etc/audit/audisp-remote.conf
```

**修复方法：**


```bash
修改/etc/audit/audisp-remote.conf，添加或修改配置：
# vim /etc/audit/audisp-remote.conf
enable_krb5 = yes
```

## 18 systemd

### 18.1 SLEM 5 must disable the x86 Ctrl-Alt-Delete key sequence.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

A locally logged-on user who presses Ctrl-Alt-Delete when at the console can reboot the system. If accidentally pressed, as could happen in the case of a mixed OS environment, this can create the risk of short-term loss of availability of systems due to unintentional reboot. In the graphical user interface environment, risk of unintentional reboot from the Ctrl-Alt-Delete sequence is reduced because the user will be prompted before any action is taken.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若返回值中存在masked，则pass，否则fail
# systemctl status ctrl-alt-del.target
```

**修复方法：**


```bash
通过如下命令修改配置：
# systemctl disable ctrl-alt-del.target
# systemctl mask ctrl-alt-del.target
# systemctl daemon-reload
```

## 19 grub

### 19.1 SLEM 5 with a basic input/output system (BIOS) must require authentication upon booting into single-user and maintenance modes.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

To mitigate the risk of unauthorized access to sensitive information by entities that have been issued certificates by DOD-approved PKIs, all DOD systems (e.g., web servers and web portals) must be properly configured to incorporate access control methods that do not rely solely on the possession of a certificate for access. Successful authentication must not automatically give an entity access to an asset or security boundary. Authorization procedures and controls must be implemented to ensure each authenticated entity also has a validated and current authorization. Authorization is the process of determining whether an entity, once authenticated, is permitted to access a specific asset. Information systems use access control policies and enforcement mechanisms to implement this requirement.

Access control policies include identity-based policies, role-based policies, and attribute-based policies. Access enforcement mechanisms include access control lists, access control matrices, and cryptography. These policies and mechanisms must be employed by the application to control access between users (or processes acting on behalf of users) and objects (e.g., devices, files, records, processes, programs, and domains) in the information system.

**规则影响：**

无

**检查方法：**


执行awk '/^ *password/ && $1 == "password_pbkdf2"' /boot/grub2/grub.cfg
若存在返回值则pass，否则fail

**修复方法：**


```bash
使用以下命令生成包含新密码的“grub.conf”文件：
# sudo grub2-mkconfig --output=/tmp/grub2.cfg
# sudo mv /tmp/grub2.cfg /boot/grub2/grub.cfg
```

### 19.2 SLEM 5 with Unified Extensible Firmware Interface (UEFI) implemented must require authentication upon booting into single-user mode and maintenance.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

If the system allows a user to boot into single-user or maintenance mode without authentication, any user that invokes single-user or maintenance mode is granted privileged access to all system information.

**规则影响：**

无

**检查方法：**


执行awk '/^ *password/ && $1 == "password_pbkdf2"'  /boot/efi/EFI/openEuler/grub.cfg
若存在返回值则pass，否则fail

**修复方法：**


```bash
使用以下命令生成包含新密码的“grub.conf”文件：
# grub2-mkconfig --output=/tmp/grub2.cfg
# mv /tmp/grub2.cfg /boot/efi/EFI/openEuler/grub.cfg
```

## 20 rpm

### 20.1 The SLEM 5 tool zypper must have gpgcheck enabled.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

Changes to any software components can have significant effects on the overall security of SLEM 5. This requirement ensures the software has not been tampered with and has been provided by a trusted vendor.

Accordingly, patches, service packs, device drivers, or SLEM 5 components must be signed with a certificate recognized and approved by the organization.

Verifying the authenticity of the software prior to installation validates the integrity of the patch or upgrade received from a vendor. This ensures the software has not been tampered with and that it has been provided by a trusted vendor. Self-signed certificates are disallowed by this requirement. SLEM 5 should not have to verify the software again. This requirement does not mandate DOD certificates for this purpose; however, the certificate used to verify the software must be from an approved Certification Authority (CA).

**规则影响：**

无

**检查方法：**


KubeOS无单包升级，无yum源，先和灵雀云确认升级方案

**修复方法：**


KubeOS无yum、单包升级场景，不涉及

## 21 telnet

### 21.1 SLEM 5 must not have the telnet-server package installed.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

It is detrimental for SLEM 5 to provide, or install by default, functionality exceeding requirements or mission objectives. These unnecessary capabilities or services are often overlooked, and therefore may remain unsecured. They increase the risk to the platform by providing additional attack vectors.

SLEM 5 is capable of providing a wide variety of functions and services. Some of the functions and services, provided by default, may not be necessary to support essential organizational operations (e.g., key missions and functions).

Examples of nonessential capabilities include but are not limited to games, software packages, tools, and demonstration software not related to requirements or providing a wide array of functionality not required for every mission but which cannot be disabled.

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若返回了telnetd的实际位置则fail，否则pass
# whereis telnetd
```

**修复方法：**


在镜像制作过程中删除

## 22 FIPS

### 22.1 FIPS 140-2/140-3 mode must be enabled on SLEM 5.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

Use of weak or untested encryption algorithms undermines the purposes of using encryption to protect data. SLEM 5 must implement cryptographic modules adhering to the higher standards approved by the federal government since this provides assurance they have been tested and validated.

**规则影响：**

无

**检查方法：**


执行cat /proc/sys/crypto/fips_enabled 
若返回值为1.则pass，否则fail

**修复方法：**


```bash
安装crypto-policies-scripts软件包
# yum install crypto-policies-scripts
打开系统FIPS模式
# fips-mode-setup --enable
重启系统
#reboot
```
