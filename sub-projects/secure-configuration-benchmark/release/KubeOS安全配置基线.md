# KubeOS安全配置基线 v1.0

| 版本 | 修订说明 | 修订时间  | 访问链接 |
| ---- | -------- | --------- | -------- |
| 1.0  | 初始修订 | 2026年5月 | 本文档   |

## 1 三方安全软件

### 1.1 KubeOS 必须确保安装终端安全工具

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

添加终端安全工具可以提供自动响应恶意行为的能力，从而在网络威胁应对方面提供额外的灵活性。这些工具通常还包含报告功能，能够为系统提供网络感知能力，而这种能力在组织的系统管理机制中可能原本并不存在。

**规则影响：**

无

**检查方法：**


确认是否安装终端安全工具

**修复方法：**


安装终端安全工具

### 1.2 KubeOS必须安装安全补丁并保持软件包更新为最新版本。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

及时打补丁对于维持信息技术系统的运行可用性、保密性和完整性至关重要。然而，未能及时更新KubeOS及应用程序软件的补丁是IT专业人员常犯的错误。新补丁发布频繁，即使经验丰富的系统管理员也难以跟上所有新补丁的节奏。当KubeOS出现新的安全漏洞时，上游通常会提供修复问题的补丁。若未安装最新的安全补丁和更新程序，未经授权的用户可能利用未修补软件中的漏洞。未能及时关注补丁更新可能导致系统遭受入侵。

**规则影响：**

无

**检查方法：**


检测补丁是否已打？检测某软件包是否是bugfix/update版本？

**修复方法：**


无该场景，不涉及

### 1.3 KubeOS 必须使用 vlock 来实现会话锁定。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

会话锁定是指用户在暂停工作且离开系统的直接物理邻近区域、但因离开具有临时性而不愿注销时所采取的临时措施。

会话锁定的实施点应位于可判定会话活动状态的位置。

无论会话锁定的判定与实施位置如何，一旦触发，该锁定必须持续有效直至用户重新完成身份认证。除重新认证外，任何其他操作均不得解除系统锁定状态。

**规则影响：**

无

**检查方法：**


```bash
执行vlock -v命令，若存在返回值则pass，否则fail
```

**修复方法：**


在镜像制作过程中安装kbd组件

### 1.4 KubeOS 必须安装支持多因素认证（MFA）所需的软件包。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

使用独立于信息系统的认证设备，可确保即使信息系统遭到入侵，存储在认证设备上的凭证也不会受到影响。

要求使用独立于信息系统的认证设备才能接入的多因素解决方案包括：提供基于时间或挑战 - 响应机制的硬件令牌，以及智能卡

特权账户是指具有特权用户授权权限的信息系统账户。

远程访问是指授权用户（或信息系统）通过外部、非组织控制的网络，访问国防部非公开信息系统的行为。远程访问方式包括拨号、宽带和无线接入等。

本要求仅适用于具备特定设备功能概念或存在组织用户概念的组件（例如 VPN、代理功能）。该要求不适用于为配置设备本身（即管理目的）而进行的身份认证。

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

### 2.1 KubeOS 必须在授予任何本地或远程连接之前，显示标准的强制通知与同意横幅。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

在授予对 KubeOS 的访问权限之前显示标准化的已批准使用通知，可确保所使用的隐私与安全通知用语，符合适用的法律、行政命令、指令、政策、法规、标准及指南。

系统使用通知仅适用于通过具有人类用户的登录接口进行的访问；若不存在此类接口，则无需显示通知。

横幅的格式必须符合适用的政策。

**规则影响：**

无

**检查方法：**


```bash
grep -i "<如下文本>" /etc/issue
：确认/etc/issue中的内容是否包含标准化的已批准使用通知，存在则pass，否则fail
```

**修复方法：**

/etc/issue中加入符合规定的声明内容：
# vim /etc/issue

### 2.2 KubeOS 必须是厂商支持的有效发行版.

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

当厂商继续为该版本提供安全补丁时，该 KubeOS 发行版即被视为“受支持（Supported）”。若使用“不受支持（Unsupported）”的发行版，则无法修复系统软件中发现的安全漏洞

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

### 3.1 KubeOS 必须限制对内核消息缓冲区（Kernel Message Buffer）的访问权限。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

限制对内核消息缓冲区（Kernel Message Buffer）的访问权限，意味着仅允许 root 用户（即拥有超级用户权限的进程）进行访问。此举可防止攻击者以 非特权用户（Non-privileged User） 身份获取额外的系统信息（System Information）。

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

### 3.2 KubeOS 必须实施地址空间布局随机化（Address Space Layout Randomization, ASLR），以防止内存遭受未授权代码执行攻击。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

部分攻击者以在非可执行内存区域（Non-executable Memory Regions）或被禁止访问的内存地址（Prohibited Memory Locations）中执行恶意代码为目的发起攻击。用于保护内存的安全机制包括数据执行阻止（Data Execution Prevention, DEP）和地址空间布局随机化（Address Space Layout Randomization, ASLR）等。其中，数据执行阻止机制既可由硬件（Hardware）强制实施，也可由软件（Software）强制实施，而硬件强制机制通常提供更强有力的防护。

此类攻击的典型示例包括缓冲区溢出攻击（Buffer Overflow Attacks）。

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

### 3.3 KubeOS 必须启用 kptr_restrict 机制，以防止内核地址泄露。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

部分攻击者旨在非可执行内存区域（Non-executable Memory Regions）或受禁止访问的内存地址中执行恶意代码。用于保护内存的安全机制包括数据执行阻止（Data Execution Prevention, DEP）和地址空间布局随机化（Address Space Layout Randomization, ASLR）等。其中，数据执行阻止机制既可由硬件（Hardware）强制实施，也可由软件（Software）强制实施；在 Linux 环境下，硬件机制（通常依赖 CPU 的 NX bit 或 XD bit）能提供更强的防护效力。

此类攻击的典型示例包括缓冲区溢出攻击（Buffer Overflow Attacks）。

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

### 3.4 KubeOS 禁止转发 IPv4 源路由（Source-Routed）数据包。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

源路由数据包允许发送方指示路由器将数据包沿着与路由器配置路径不同的路径进行转发，攻击者可利用此机制绕过网络安全措施（如防火墙规则）。

本安全要求仅适用于源路由流量的转发场景，即当系统启用了 IPv4/IPv6 转发功能（net.ipv4.ip_forward 或 net.ipv6.conf.all.forwarding 为 1）且系统充当路由器角色时。

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

### 3.5 KubeOS 默认禁止转发 IPv4 源路由（Source-Routed）数据包。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

源路由数据包允许发送方指示路由器沿着与路由器配置路径不同的路径转发数据包，攻击者可利用此机制绕过网络安全措施（如防火墙规则）。

本安全要求仅适用于源路由流量的转发场景，即当系统启用了 IPv4 转发功能（net.ipv4.ip_forward 为 1）且系统充当路由器角色时。

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

### 3.6 KubeOS 必须禁止接收 IPv4 ICMP 重定向（ICMP Redirect）消息。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

ICMP 重定向消息由路由器发出，用于通知主机存在通往特定目的地的更优路径。此类消息会修改主机路由表，且缺乏身份认证机制。恶意（非法）的 ICMP 重定向消息可能导致中间人攻击（Man-in-the-Middle, MITM）。

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

### 3.7 KubeOS 默认不得允许网络接口接收 IPv4 ICMP 重定向消息。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

ICMP 重定向消息（ICMP Redirect）由路由器发出，旨在通知主机存在通往特定目的地的更优（更短）路径。此类消息会动态修改主机内核路由表，且由于 ICMP 协议缺乏身份认证（Unauthenticated）机制，攻击者可伪造此类消息。接收非法的 ICMP 重定向消息可能导致中间人攻击（Man-in-the-Middle, MITM），使攻击者能够劫持或篡改网络流量。

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

### 3.8 KubeOS 默认不得发送 IPv4 ICMP 重定向消息。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

ICMP 重定向消息通常由路由器发出，旨在告知主机存在通往特定目的地的更优（更短）路径。此类消息包含源自系统路由表（Route Table）的具体信息，若主机违规发送，可能会导致网络拓扑结构（Network Topology）的部分细节泄露，从而增加被攻击者探测的风险。

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

### 3.9 KubeOS 默认不得允许网络接口发送 IPv4 ICMP 重定向消息。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

ICMP 重定向消息通常由路由器发出，旨在告知主机存在通往特定目的地的更优（更短）路径。此类消息包含源自系统路由表（Route Table）的具体信息，若主机违规发送，可能会导致网络拓扑结构（Network Topology）的部分细节泄露，从而增加被攻击者探测的风险。

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

### 3.10 除非系统充当路由器角色，否则 KubeOS 默认不得执行 IPv4 数据包转发。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

路由协议守护进程（Routing Protocol Daemons，如 ospfd、bgpd 等）通常仅部署在路由器上，用于与其他路由器交换网络拓扑信息。若在非路由器场景（如普通服务器或主机）下启用该功能，将导致系统网络信息被不必要地广播或传输至网络中，增加网络拓扑被探测和攻击面扩大的风险。

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

### 3.11 KubeOS 必须配置启用 TCP SYN Cookies。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

拒绝服务攻击（DoS）是指合法用户无法访问系统资源的状态。当发生此类攻击时，组织可能无法履行其核心职能，或被迫在性能严重受损的情况下运行。

通过管理超额容量（Excess Capacity），可确保系统拥有足够的资源来抵御洪水攻击（Flooding Attacks）。增加容量和服务冗余可以降低系统对部分 DoS 攻击的敏感度。管理超额容量的措施可能包括：设定特定使用优先级、实施配额限制（Quotas）或进行资源分区（Partitioning）等。

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

### 3.12 KubeOS 不得转发源路由的 IPv6 数据包。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

源路由（Source Routing）数据包允许发送方指定数据包在网络中传输的路径，而非由路由器根据标准路由表自动决定。这种机制可能被攻击者利用，使其数据包绕过防火墙、入侵检测系统（IDS）或其他网络安全措施，从而进入受保护的网络区域。

此安全要求主要针对系统作为路由器（Router）运行时的源路由流量转发行为。若系统仅作为普通主机（Host），通常不涉及路由转发逻辑，但必须确保内核参数禁止此类行为。

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

### 3.13 请使用linux术语翻译以下英语为中文，确保语义通顺，规则明确:

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

源路由（Source Routing）数据包允许发送方指定数据包在网络中传输的路径，而非由路由器根据标准路由表自动决策。该机制可能被攻击者利用，使其数据包绕过防火墙、入侵检测系统（IDS）或其他网络安全策略，从而进入受保护的网络区域。

此安全要求主要针对系统作为路由器（Router）运行时的源路由流量转发行为。若系统作为普通主机（Host）运行，虽不涉及转发逻辑，但仍需确保内核层面默认禁止此类行为，以符合最小权限原则。

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

### 3.14 KubeOS 必须阻止接受 IPv6 ICMP 重定向消息。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

ICMP 重定向（ICMP Redirect）消息由路由器发送，用于告知主机存在到达特定目标更优（更直接）的路径。主机收到此类消息后，会更新其路由表（Routing Table）。由于 ICMP 重定向消息未经过身份验证（Unauthenticated），攻击者可伪造此类消息，诱导主机将流量错误地路由到攻击者控制的节点，从而实施中间人攻击（Man-in-the-Middle, MITM），窃取或篡改敏感数据。

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

### 3.15 KubeOS 默认不得允许网络接口接受 IPv6 ICMP 重定向消息。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

ICMP 重定向（ICMP Redirect）消息由路由器发送，旨在通知主机存在到达特定目标更优（更直接）的下一跳路径。主机若接受此类消息，将自动更新其内核路由表（Routing Table）。由于 ICMP 重定向消息缺乏身份验证机制（Unauthenticated），攻击者可伪造此类消息，诱导主机将流量错误地重定向至攻击者控制的节点，从而实施中间人攻击（Man-in-the-Middle, MITM），导致数据被窃听、篡改或阻断。

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

### 3.16 KubeOS 默认不得执行 IPv6 数据包转发，除非该系统被配置为路由器。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

路由协议守护进程（Routing Protocol Daemons）通常仅运行在路由器上，用于与其他路由器交换网络拓扑信息。如果系统在非路由器场景下（如普通主机或工作负载节点）开启了 IPv6 数据包转发功能，且未实际运行路由服务，可能会导致系统网络信息被不必要地广播或透传到网络中，增加网络暴露面及被探测攻击的风险。

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

### 3.17 KubeOS 默认不得执行 IPv6 数据包转发，除非该系统被明确配置为路由器。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

路由协议守护进程（Routing Protocol Daemons）通常仅部署于路由器节点，用于与其他路由器交换网络拓扑信息。若系统在非路由器角色（如普通计算节点或主机）上启用 IPv6 数据包转发功能，可能导致系统网络配置及拓扑信息被不必要地广播或透传至网络中，从而增加网络暴露面，提升被路由探测或攻击的风险。

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

### 4.1 除非确有必要，否则必须禁用 KubeOS 内核核心转储（Kernel Core Dumps）。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

内核核心转储可能包含系统在崩溃时的全部内存内容。内核核心转储可能会占用大量磁盘空间，并可能导致拒绝服务（DoS）攻击，从而耗尽目标文件系统分区上的可用空间。

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

### 5.1 KubeOS 在安装更新版本后，必须移除所有过时的软件组件。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

在信息系统上安装软件更新后，若未彻底移除旧版本软件组件，攻击者可能利用旧版本中已知但未修复的安全漏洞（CVE）发起攻击。某些 IT 产品可能具备自动清理旧版本软件的功能，但 KubeOS 必须确保在升级过程中，显式地清理不再需要的旧包，以消除潜在的攻击面。

**规则影响：**

无

**检查方法：**


```bash
执行grep -w 'clean_requirements_on_remove=True' /etc/yum.conf，若返回值为clean_requirements_on_remove=True，则pass，否则fail
```

**修复方法：**


无该场景，不涉及

## 6 分区&文件系统

### 6.1 KubeOS 的用户主目录（如 /home 或其等效路径）必须使用独立文件系统。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

为不同路径使用独立文件系统，可防止因文件系统空间耗尽或文件系统故障而引发的系统失效。

**规则影响：**

无

**检查方法：**


```bash
执行mount | grep "/home "，若存在返回值，则pass，否则fail
```

**修复方法：**


在镜像制作时配置

### 6.2 KubeOS 必须为 /var 目录使用独立文件系统。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

为不同路径使用独立文件系统，可防止因文件系统空间耗尽或文件系统故障而引发的系统失效。

**规则影响：**

无

**检查方法：**


```bash
执行mount | grep "/var "，若存在返回值，则pass，否则fail
```

**修复方法：**


在镜像制作时配置

### 6.3 KubeOS 必须为系统审计数据路径使用独立文件系统。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

为不同路径使用独立文件系统，可防止因文件系统空间耗尽或文件系统故障而引发的系统失效。

**规则影响：**

无

**检查方法：**


```bash
执行mount | grep " /var/log/audit"，若存在返回值，则pass，否则fail
```

**修复方法：**


在镜像制作时配置

### 6.4 通过网络文件系统（NFS）导入的 KubeOS 文件系统，必须通过挂载选项防止执行设置了 setuid 和 setgid 位的文件。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

nosuid 挂载选项可阻止系统以文件所有者（Owner）的权限执行设置了 setuid 和 setgid 位的文件。对于不包含经批准的 setuid 和 setgid 文件的文件系统，在挂载时必须使用此选项。从不受信任的文件系统执行文件，会增加未授权用户获取非法管理权限的风险。

**规则影响：**

无

**检查方法：**


```bash
执行mount | grep "/persist/nfs" | grep nosuid，若存在返回值，则pass，否则fail
```

**修复方法：**


在镜像制作时配置

### 6.5 通过网络文件系统（NFS）导入的 KubeOS 文件系统，必须挂载以防止执行二进制文件。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

noexec 挂载选项可阻止系统执行二进制文件。对于不包含经批准的二进制文件的文件系统，在挂载时必须使用此选项，因为这些文件可能不兼容。从不受信任的文件系统执行文件，会增加未授权用户获取非法管理权限的风险。

**规则影响：**

无

**检查方法：**


```bash
执行mount | grep "/persist/nfs" | grep noexec，若存在返回值，则pass，否则fail
```

**修复方法：**


在镜像制作时配置

### 6.6 使用可移动介质的 KubeOS 文件系统，必须通过挂载选项防止执行设置了 setuid 和 setgid 位的文件。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

nosuid 挂载选项可阻止系统以文件所有者（Owner）的权限执行设置了 setuid 和 setgid 位的文件。对于不包含经批准的 setuid 和 setgid 文件的文件系统，在挂载时必须使用此选项。从不受信任的文件系统（如可移动介质）执行文件，会增加未授权用户获取非法管理权限的风险。

**规则影响：**

无

**检查方法：**


不涉及，无需检测。

**修复方法：**

无

### 6.7 包含用户家目录的 KubeOS 文件系统，必须挂载以防止执行设置了 setuid 和 setgid 位的文件。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

nosuid 挂载选项可阻止系统以文件所有者（Owner）的权限执行设置了 setuid 和 setgid 位的文件。对于不包含经批准的 setuid 和 setgid 文件的文件系统，在挂载时必须使用此选项。从不可信的文件系统执行文件，会增加未授权用户获取非法管理权限的风险。

**规则影响：**

无

**检查方法：**


```bash
执行for X in `awk -F: '($3>=1000)&&($7 !~ /nologin/){print $6}' /etc/passwd`; do findmnt -nkT $X; done | sort -r，若返回值中存在nosuid，则pass，否则fail
```

**修复方法：**


在镜像制作时配置

### 6.8 除非业务必需，否则 KubeOS 必须禁用文件系统自动挂载服务（Automounter）。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

禁止启用文件系统自动挂载功能允许系统自动识别并挂载未知的外部设备。

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

### 6.9 KubeOS 所有持久化磁盘分区必须实施加密机制，以防止需要静态数据保护（Data-at-Rest Protection）的所有信息发生未授权披露或篡改。

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

KubeOS 处理需要静态数据保护的数据时，必须采用加密机制，以防止未经授权的披露和修改静态信息。 

选择加密机制的依据是保护组织信息完整性的需求。该机制的强度应与信息的安全类别和/或分类相匹配。组织可以灵活地选择对存储设备上的所有信息进行加密（即全盘加密），或对特定的数据结构（例如文件、记录或字段）进行加密。

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

### 7.1 KubeOS 包含系统命令的目录权限必须设置为 755 或更低权限（即权限值不大于 755）。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

若 KubeOS 允许任意用户修改软件库（Software Libraries），则这些修改可能在未经过健全变更管理流程（Change Management Process）所规定的适当测试与审批的情况下被实施。

此要求适用于包含可访问且可配置的软件库的 KubeOS 系统，例如解释型语言（Interpreted Languages）的库文件。软件库的概念同样涵盖具有提权（Escalated Privileges）执行能力的特权程序。仅允许经过资质认证且获得授权的个体访问信息系统组件，以执行包括升级和修改在内的变更操作。

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

### 7.2 KubeOS 系统命令的权限必须设置为 755 或更低（即权限值不大于 755）

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

若 KubeOS 允许任意用户修改软件库（Software Libraries），则这些修改可能在未经过健全变更管理流程（Change Management Process）所规定的适当测试与审批的情况下被实施。

此要求适用于包含可访问且可配置的软件库的 KubeOS 系统，例如解释型语言（Interpreted Languages）的库文件。软件库的概念同样涵盖具有提权（Escalated Privileges）执行能力的特权程序。仅允许经过资质认证且获得授权的个体访问信息系统组件，以执行包括升级和修改在内的变更操作。

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

### 7.3 KubeOS 库目录的权限必须设置为 755 或更低（即权限值不大于 755）。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

若 KubeOS 允许任意用户修改软件库（Software Libraries），则这些修改可能在未经过健全变更管理流程（Change Management Process）所规定的适当测试与审批的情况下被实施。

此要求适用于包含可访问且可配置的软件库的 KubeOS 系统，例如解释型语言（Interpreted Languages）的库文件。软件库的概念同样涵盖具有提权（Escalated Privileges）执行能力的特权程序。仅允许经过资质认证且获得授权的个体访问信息系统组件，以执行包括升级和修改在内的变更操作。

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

### 7.4 KubeOS 库文件（Library Files）的权限必须设置为 755 或更低（即权限值不大于 755）。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

若 KubeOS 允许任意用户修改软件库，则这些修改可能在未经过健全变更管理流程（Change Management Process）所规定的适当测试与审批的情况下被实施。

此要求适用于包含可访问且可配置的软件库的 KubeOS 系统，例如解释型语言（Interpreted Languages）的库文件。软件库的概念同样涵盖具有提权（Escalated Privileges）执行能力的特权程序。仅允许经过资质认证且获得授权的个体访问信息系统组件，以执行包括升级和修改在内的变更操作。

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

### 7.5 所有 KubeOS 本地交互式用户的家目录（Home Directories）权限必须设置为 750 或更低（即权限值不大于 750）。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

本地交互式用户家目录若权限设置过大，可能导致其他用户未经授权使用（Access）和访问（Access）该用户的文件。

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

### 7.6 所有 KubeOS 本地初始化文件（Local Initialization Files）的权限必须设置为 740 或更低（即权限值不大于 740）。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

本地初始化文件用于在用户登录时配置其 Shell 环境。若这些文件遭到恶意篡改，将在用户登录时导致账户凭证或环境被破坏。

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

### 7.7 KubeOS SSH 守护进程（SSH Daemon）的公钥主机密钥文件（Public Host Key Files）权限必须设置为 644 或更低（即权限值不大于 644）。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

若公钥主机密钥文件被未经授权的用户篡改，SSH 服务可能会遭到破坏。

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

### 7.8 KubeOS SSH 守护进程的私钥主机密钥文件权限必须设置为 640 或更低（即权限值不大于 640）。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

若未经授权的用户获取了 SSH 主机私钥文件，该主机可能被冒充

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

### 7.9 KubeOS 库文件（Library Files）必须由 root 用户所有。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

若 KubeOS 允许任何用户对软件库文件进行修改，这些更改可能会绕过健全变更管理流程中必要的测试与审批环节而被实施。

本要求适用于包含可访问且可配置的软件库的 KubeOS 环境（例如解释型语言环境）。软件库的范围还包括以提权（Escalated Privileges）执行的特权程序。只有具备资质且获得授权的个人才应被允许访问信息系统组件，以发起变更（包括升级和修改）。

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

### 7.10 KubeOS 库文件（Library Files）必须归属 root 用户组。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

若 KubeOS 允许任何用户对软件库文件进行修改，这些更改可能会绕过健全变更管理流程中必要的测试与审批环节而被实施。

本要求适用于包含可访问且可配置的软件库的 KubeOS 环境（例如解释型语言环境）。软件库的范围还包括以提权（Escalated Privileges）执行的特权程序。只有具备资质且获得授权的个人才应被允许访问信息系统组件，以发起变更（包括升级和修改）。

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


### 7.11 KubeOS 库目录必须由 root 用户所有.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果 KubeOS 允许任何用户更改软件库，则这些更改可能在未经适当测试和批准的情况下实施，而这些测试和批准是健全变更管理流程的一部分。

此要求适用于具有可访问和可配置软件库的 KubeOS，例如解释型语言的情况。软件库还包括以提权权限执行的特权程序。只有合格且获授权的个人才被允许获取信息系统组件的访问权限，以便发起变更，包括升级和修改。

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

### 7.12 KubeOS 库目录必须由 root 组所有。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果 KubeOS 允许任何用户更改软件库，则这些更改可能在未经适当测试和批准的情况下实施，而这些测试和批准是健全变更管理流程的一部分。

此要求适用于具有可访问和可配置软件库的 KubeOS，例如解释型语言的情况。软件库还包括以提权权限执行的特权程序。只有合格且获授权的个人才被允许获取信息系统组件的访问权限，以便发起变更，包括升级和修改。


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

### 7.13 KubeOS 必须由 root 用户所有系统命令。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果 KubeOS 允许任何用户更改软件库，则这些更改可能在未经适当测试和批准的情况下实施，而这些测试和批准是健全变更管理流程的一部分。

此要求适用于具有可访问和可配置软件库的 KubeOS，例如解释型语言的情况。软件库还包括以提权权限执行的特权程序。只有合格且获授权的个人才被允许获取信息系统组件的访问权限，以便发起变更，包括升级和修改。

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

### 7.14 KubeOS 系统命令必须由 root 或系统账户组所有。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果 KubeOS 允许任何用户更改软件库，则这些更改可能在未经适当测试和批准的情况下实施，而这些测试和批准是健全变更管理流程的一部分。

此要求适用于具有可访问和可配置软件库的 KubeOS，例如解释型语言的情况。软件库还包括以提权权限执行的特权程序。只有合格且获授权的个人才被允许获取信息系统组件的访问权限，以便发起变更，包括升级和修改。

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

### 7.15 KubeOS 包含系统命令的目录必须由 root 用户所有。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果 KubeOS 允许任何用户更改软件库，则这些更改可能在未经适当测试和批准的情况下实施，而这些测试和批准是健全变更管理流程的一部分。

此要求适用于具有可访问和可配置软件库的 KubeOS，例如解释型语言的情况。软件库还包括以提权权限执行的特权程序。只有合格且获授权的个人才被允许获取信息系统组件的访问权限，以便发起变更，包括升级和修改。

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

### 7.16 KubeOS 包含系统命令的目录必须由 root 组所有。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果 KubeOS 允许任何用户更改软件库，则这些更改可能在未经适当测试和批准的情况下实施，而这些测试和批准是健全变更管理流程的一部分。

此要求适用于具有可访问和可配置软件库的 KubeOS，例如解释型语言的情况。软件库还包括以提权权限执行的特权程序。只有合格且获授权的个人才被允许获取信息系统组件的访问权限，以便发起变更，包括升级和修改。

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

### 7.17 所有 KubeOS 文件和目录必须具有有效的所有者。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果用户被分配与未拥有文件相同的用户标识符 (UID)，则可能会无意中继承未拥有文件。

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

### 7.18 所有 KubeOS 文件和目录必须具有有效的组所有者。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果没有有效组所有者的文件被分配与没有有效组所有者的文件相同的组标识符 (GID)，则可能会无意中继承这些文件。

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

### 7.19 所有 KubeOS 本地交互式用户主目录必须由主目录所有者的主组所有。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果本地交互式用户主目录的组标识符 (GID) 与该用户的主 GID 不同，这将允许未经授权的用户访问该用户的文件，并且共享同一组的用户可能无法访问他们合法应访问的文件。

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

### 7.20 所有 KubeOS 世界可写目录必须由 root、sys、bin 或应用程序组所有。

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

### 7.21 所有 KubeOS 世界可写目录必须设置粘滞位。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

防止未经授权的信息传输可降低以下风险：先前用户/角色（或代表先前用户/角色执行的进程）产生的信息，包括信息的加密表示，在先前用户/角色（或当前进程）在释放资源后重新获得对共享系统资源（例如寄存器、主存储器和硬盘）的访问时，这些信息可能对任何当前用户/角色（或当前进程）可用。共享资源中信息的控制也常称为对象重用和残留信息保护。

此要求通常适用于信息技术产品的设计，但也可能适用于配置特定使用此类产品的信息系统组件。这可以通过国防部或其他政府机构的接受/验证过程进行验证。

可能存在具有可配置保护的共享资源（例如存储中的文件），这些资源可以在特定的信息系统组件上进行评估。

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

### 7.22 KubeOS 必须防止未经授权的用户访问系统错误消息。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

只有授权人员才应知晓错误及其详细信息。错误消息是组织运营状态的指示器，或者可以识别 KubeOS 或平台。此外，不得通过错误消息向未经授权的人员或其指定代表泄露个人身份信息 (PII) 和运营信息。

组织和发展团队必须仔细考虑错误消息的结构和内容。信息系统识别和处理错误条件的程度由组织政策和运营需求指导。

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

### 7.23 KubeOS 必须生成提供纠正行动所需信息的错误消息，同时不泄露可能被对手利用的信息。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

任何在错误消息中提供过多信息的操作系统都有风险损害数据和安全性，并且错误消息的结构和内容需要由组织仔细考虑。

组织仔细考虑错误消息的结构/内容。信息系统识别和处理错误条件的程度由组织政策和运营需求指导。可能被对手利用的信息包括，例如，错误登录尝试，其中误输入的密码作为用户名，可以从记录的信息中派生（如果未明确说明）的任务/业务信息，以及个人信息，例如账号、社会安全号码和信用卡号码。

/var/log/btmp、/var/log/wtmp 和 /var/log/lastlog 文件具有组写和全局读权限，以允许 lastlog 功能执行。限制超出此配置的权限将导致依赖 lastlog 数据库的功能失败。

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

### 7.24 /etc/passwd 文件中定义的所有 KubeOS 本地交互式用户主目录必须存在。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果本地交互式用户定义了不存在的主目录，则用户在登录时可能会被赋予 / 目录作为当前工作目录。这可能导致拒绝服务 (DoS)，因为用户将无法访问其登录配置文件，并且可能会给他们提供他们通常无法访问的系统文件的可见性。

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

### 7.25 所有 KubeOS 本地初始化文件不得执行世界可写程序。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果用户启动文件执行世界可写程序，特别是在受保护目录中，它们可能被恶意修改以破坏用户文件或以其他方式在用户级别损害系统。如果系统在用户级别被妥协，则更容易提升权限以最终在根和网络级别损害系统。

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

### 8.1 KubeOS 必须根据端口、协议和服务管理 (PPSM) 类别分配列表 (CAL) 和漏洞评估，禁止或限制使用功能、端口、协议和/或服务。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

为了防止未经授权的设备连接、未经授权的信息传输或未经授权的分隔隧道（即数据类型的嵌入），组织必须禁用或禁用信息系统上未使用或不必要的物理和逻辑端口/协议。

此外，操作系统远程访问功能必须能够立即断开当前远程访问信息系统用户的连接和/或禁用进一步的远程访问。断开连接或禁用的速度根据任务功能的紧迫性以及消除对组织信息系统的即时或未来远程访问的需要而有所不同。

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

### 9.1 KubeOS 时钟必须对于联网系统，至少每 24 小时同步一次权威国防部时间源。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

不准确的时戳使得关联事件更加困难，并可能导致不准确的分析。确定系统在特定事件发生时的正确时间对于进行取证分析和调查系统事件至关重要。超出配置的可接受允许值（漂移）之外的源可能不准确。

同步内部信息系统时钟为具有多个系统时钟和通过网络连接的系统提供统一的时戳。

组织应考虑可能没有定期访问权威时间源的端点（例如移动、远程工作和战术端点）。

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

### 10.1 除非获得批准和记录，否则 KubeOS 不得将网络接口配置为混杂模式。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

混杂模式下的网络接口允许捕获系统可见的所有网络流量。如果未经授权的个人可以访问这些应用程序，则可能允许他们收集诸如登录 ID、密码和系统之间的密钥交换等信息。

如果系统用于执行网络故障排除功能，则必须与信息系统安全官员 (ISSO) 记录这些工具的使用，并仅限于授权人员。

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

### 11.1 KubeOS 必须配置为：所有与 SSH 流量关联的网络连接在变得无响应后终止。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

在短时间内终止无响应的 SSH 会话，可减少未经授权的人员接管已在控制台或控制台端口上启用且无人看管的管理会话的机会窗口。此外，快速终止空闲的 SSH 会话也将释放由被管网络元素承诺的资源。

终止与通信会话关联的网络连接包括，例如，在操作系统级别分配关联的 TCP/IP 地址/端口对，以及在多个应用程序会话使用单个操作系统级别网络连接时，在应用程序级别分配网络资源。这并不意味着操作系统终止所有会话或网络访问；它仅结束无响应的会话并释放与该会话关联的资源。

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

### 11.2 KubeOS 必须配置为：所有与 SSH 流量关联的网络连接在变得无响应 10 分钟后终止。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

在短时间内终止无响应的 SSH 会话，可减少未经授权的人员接管已在控制台或控制台端口上启用且无人看管的管理会话的机会窗口。此外，快速终止空闲的 SSH 会话也将释放由被管网络元素承诺的资源。

终止与通信会话关联的网络连接包括，例如，在操作系统级别分配关联的 TCP/IP 地址/端口对，以及在多个应用程序会话使用单个操作系统级别网络连接时，在应用程序级别分配网络资源。这并不意味着操作系统终止所有会话或网络访问；它仅结束无响应的会话并释放与该会话关联的资源。

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

### 11.3 除非为满足文档化和验证过的任务需求，否则 KubeOS SSH 守护进程必须为交互式用户禁用转发远程 X 连接。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

使用 X11 转发的安全风险在于，当 SSH 客户端请求转发时，客户端的 X11 显示服务器可能会暴露于攻击之下。系统管理员可能持有一种立场，希望保护那些可能因无意中请求 X11 转发而暴露于攻击的客户端，这可能需要设置为“否”（禁用）。

应谨慎启用 X11 转发。能够绕过远程主机上文件权限的用户（针对用户 X11 授权数据库）可以通过转发连接访问本地 X11 显示。如果同时启用了 ForwardX11Trusted 选项，攻击者随后可能能够执行诸如键盘记录等活动。

如果 X11 服务不是系统预期功能所必需的，则应根据系统需求禁用或限制它们。

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

### 11.4 KubeOS 必须禁止通过 SSH 远程访问直接登录 root 账户。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

为确保个人责任并防止未经授权的访问，组织用户必须 individually 标识和认证。

组认证者是供多个个人使用的通用账户。单独使用组认证者无法唯一标识个人用户。组认证者的示例包括 Unix OS 的“root”用户账户、Windows 的“Administrator”账户、“sa”账户或“helpdesk”账户。

例如，Unix 和 Windows KubeOS 提供“切换用户”功能，允许用户使用其个人凭据进行认证，并在需要时“切换”到管理员角色。此方法在使用组认证者之前提供唯一的个人认证。

除组织中明确标识和记录的具体用户可在 KubeOS 上执行无需标识或认证的操作外，所有其他访问都需要对（代表用户行事的）用户（及任何进程）进行唯一标识和认证。

要求个人在使用组认证者之前使用个人认证者进行认证，允许对操作进行追溯，并为使用组账户知识可采取的操作增加额外的保护级别。

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

### 11.5 KubeOS 必须记录 SSH 连接尝试及失败到服务器。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

缺乏自动监控功能的远程访问服务（例如提供网络设备信息系统远程访问的服务）会增加风险，并使远程用户访问管理变得困难。

远程访问是指授权用户（或信息系统）通过外部、非组织控制的网络通信，对 DOD 非公开信息系统的访问。远程访问方法包括，例如，拨号、宽带和无线。

对远程访问会话的自动监控允许组织检测网络攻击，并通过审计各种信息系统组件（例如服务器、工作站、笔记本电脑、智能手机和平板电脑）上远程访问功能（如远程桌面协议 (RDP)）的连接活动，确保持续符合远程访问策略。

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

### 11.6 KubeOS 必须在 SSH 登录时显示上次成功账户登录的日期和时间。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

向用户提供通过 SSH 进行的账户访问上次发生时间的反馈，有助于用户识别和报告未经授权的账户使用。

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

### 11.7 KubeOS SSH 守护进程必须配置为不允许使用 known hosts 认证进行认证。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

为 SSH 守护进程配置此设置可提供额外保证，即使其他地方配置错误，通过 SSH 的远程登录也需要密码。

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

### 11.8 KubeOS SSH 守护进程必须对主目录配置文件执行严格模式检查。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果其他用户有权修改用户特定的 SSH 配置文件，他们可能能够以另一个用户的身份登录系统。

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

### 11.9 对于基于 PKI 的认证，KubeOS 必须强制执行对相应私钥的授权访问。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果私钥被发现，攻击者可以使用该密钥以授权用户的身份进行认证并访问网络基础设施。

PKI 的基石是用于加密或数字签名的私钥。

如果私钥被盗，这将导致通过 PKI 获得的认证和不可否认性的妥协，因为攻击者可以使用私钥对文档进行数字签名并冒充授权用户。

数字证书的持有者和颁发机构都必须保护用于保存私钥的计算机、存储设备或其他设备。

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

### 11.10 KubeOS 必须安装 SSH 以保护传输信息的机密性和完整性。

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

如果没有对传输信息的保护，机密性和完整性可能会受到损害，因为未受保护的通信可能会被拦截，并被读取或篡改。

此要求适用于内部和外部网络，以及可以从其传输信息的所有类型的信息系统组件（例如服务器、移动设备、笔记本电脑、打印机、复印机、扫描仪和传真机）。位于受控边界物理保护之外的通信路径容易受到拦截和修改的可能性。

保护组织信息的机密性和完整性可以通过物理方式（例如，采用物理分发系统）或逻辑方式（例如，采用密码技术）来实现。如果采用物理保护方式，则不必采用逻辑方式（密码学），反之亦然。

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若存在返回值则pass，否则fail
# find / -name ssh-keygen
```

**修复方法：**


在镜像制作过程中安装openssh组件

### 11.11 KubeOS 必须使用 SSH 来保护传输信息的机密性和完整性。

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

如果没有对传输信息的保护，机密性和完整性可能会受到损害，因为未受保护的通信可能会被拦截，并被读取或篡改。

此要求适用于内部和外部网络，以及可以从其传输信息的所有类型的信息系统组件（例如服务器、移动设备、笔记本电脑、打印机、复印机、扫描仪和传真机）。位于受控边界物理保护之外的通信路径容易受到拦截和修改的可能性。

保护组织信息的机密性和完整性可以通过物理方式（例如，采用物理分发系统）或逻辑方式（例如，采用密码技术）来实现。如果采用物理保护方式，则不必采用逻辑方式（密码学），反之亦然。

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

### 11.12 KubeOS 必须不允许通过 SSH 进行无人值守或自动登录。

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

未能限制通过 SSH 对经过认证用户的系统访问，会对 KubeOS 安全性产生负面影响。

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

### 11.13 KubeOS 必须实施 DOD 批准的加密以保护 SSH 远程连接的机密性。

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

如果没有机密性保护机制，未经授权的个人可能会通过远程访问会话获取敏感信息。

远程访问是指授权用户（或信息系统）通过外部、非组织控制的网络通信，对 DOD 非公开信息系统的访问。远程访问方法包括，例如，拨号、宽带和无线。

加密提供了一种手段，用于保护远程连接，以防止未经授权访问通过远程访问连接传输的数据（例如 RDP），从而提供一定程度的机密性。机制的加密强度是基于信息的安全分类选择的。

系统将尝试使用客户端提出的第一个与服务器列表匹配的密码。将值按“最强到最弱”列出是一种确保使用可用于保护 SSH 连接的最强密码的方法。

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

### 11.14 KubeOS SSH 守护进程必须配置为仅使用采用 FIPS 140-2/140-3 批准的密码哈希算法的消息认证码 (MACs)。

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

如果没有密码完整性保护，未经授权的用户可以在未检测到情况下更改信息。

远程访问（例如 RDP）是指授权用户（或信息系统）通过外部、非组织控制的网络通信，对 DOD 非公开信息系统的访问。远程访问方法包括，例如，拨号、宽带和无线。

用于保护信息完整性的密码机制包括，例如，使用非对称加密的签名哈希函数，使公共密钥能够验证哈希信息，同时保持用于生成哈希的秘密密钥的机密性。

系统将尝试使用客户端提出的第一个与服务器列表匹配的哈希。将值按“最强到最弱”列出是一种确保使用可用于保护 SSH 连接的最强哈希的方法。

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

### 11.15 KubeOS SSH 服务器必须配置为仅使用 FIPS 140-2/140-3 验证的密钥交换算法。

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

如果没有由 FIPS 140-2/140-3 验证的密码算法提供的密码完整性保护，未经授权的用户可以在未检测到情况下查看和更改信息。

系统将尝试使用客户端提出的第一个与服务器列表匹配的算法。将值按“最强到最弱”列出是一种确保使用可用于保护 SSH 连接的最强算法的方法。

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

### 11.16 KubeOS 上不得存在 .shosts 文件。

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

.shosts 文件用于通过 SSH 为单个用户或系统配置基于主机的认证。基于主机的认证不足以防止对系统的未经授权访问，因为它不要求对连接请求进行交互式标识和认证，也不要求使用双因素认证。

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

### 11.17 KubeOS 上不得存在 shosts.equiv 文件。

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

shosts.equiv 文件用于通过 SSH 为系统配置基于主机的认证。基于主机的认证不足以防止对系统的未经授权访问，因为它不要求对连接请求进行交互式标识和认证，也不要求使用双因素认证。

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

### 11.18 KubeOS 不允许通过图形用户界面 (GUI) 进行无人值守或自动登录。

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

未能将系统访问限制为经过认证的用户，会对 KubeOS 安全性产生负面影响。

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

### 12.1 除非经过批准和记录，否则 KubeOS 无线网卡必须禁用。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果没有保护与无线外围设备的通信，机密性和完整性可能会受到损害，因为未受保护的通信可能会被拦截、读取、篡改或用于损害 KubeOS。

此要求适用于与 KubeOS 一起使用的外围无线技术（例如无线鼠标、键盘、显示器等）。无线外围设备（例如 Wi-Fi/蓝牙/红外键盘、鼠标、指点设备和近场通信 [NFC]）通过创建计算机上的开放、未受保护的端口带来独特的挑战。无线外围设备必须满足 DOD 对无线数据传输的要求，并经 AO 批准使用。尽管某些无线外围设备（例如鼠标和指点设备）通常不携带需要保护的信息，但与这些无线外围设备的通信修改可能被用于损害 KubeOS。位于受控边界物理保护之外的通信路径容易受到拦截和修改的可能性。

保护与无线外围设备通信的机密性和完整性可以通过物理方式（例如，采用针对无线射频的物理屏障）或逻辑方式（例如，采用密码技术）来实现。如果采用物理保护方式，则不必采用逻辑方式（密码学），反之亦然。如果无线外围设备仅传输遥测数据，则可能不需要加密数据。

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

### 13.1 KubeOS 必须禁用 USB 大容量存储内核模块。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果没有识别设备，可能会引入未标识或未知设备，从而促进恶意活动。

外围设备包括但不限于闪存驱动器、外部存储器和打印机。

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

### 14.1 所有 KubeOS 本地交互式用户账户在创建时必须分配主目录。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果未为本地交互式用户分配有效的主目录，则没有地方存储和控制他们应拥有的文件。

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

### 14.2 KubeOS 默认权限必须定义得使所有经过认证的用户只能读取和修改自己的文件。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

设置最严格的默认权限可确保在创建新账户时，它们没有不必要的访问权限。

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

### 14.3 KubeOS shadow密码套件必须配置为在失败登录尝试后的登录提示之间强制执行至少五秒的延迟。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

限制超过一定时间间隔内的登录尝试次数，可降低未经授权的用户获得账户访问权限的机会。

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

### 14.4 所有 KubeOS 本地交互用户必须在 /etc/passwd 文件中分配一个主目录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果未为本地交互用户分配有效的home目录，则无法存储和控制他们应拥有的文件。

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

### 14.5 所有 KubeOS 本地交互式用户初始化文件的可执行搜索路径必须仅包含解析为用户主目录的路径

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

可执行文件搜索路径（通常是 PATH 环境变量）包含一个目录列表，供 shell 查找可执行文件。如果该路径包含当前工作目录（用户主目录除外），则这些目录中的可执行文件可能会被执行，而不是系统命令。该变量被格式化为以冒号分隔的目录列表。如果存在空条目，例如开头或结尾的冒号，或两个连续的冒号，则会被解释为当前工作目录。如果本地交互式用户的默认系统搜索路径需要偏离，必须与信息系统安全官（ISSO）进行记录。

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

### 14.6 KubeOS必须自动在72小时内使临时账户失效

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

临时账户是在紧急情况下（如新软件或硬件配置、事件响应等）建立的具有特权或非特权权限的账户，其创建目的是为了在需要快速激活账户时绕过正常的账户授权流程。如果系统中存在未激活的临时账户，并且这些账户在72小时内既未被手动删除也未自动失效，系统的安全态势将会被削弱，从而暴露给未经授权的用户或内部威胁行为者进行利用。

临时账户与应急账户不同。应急账户，也称为“最后手段”或“破窗”账户，是系统中为授权系统管理员在标准登录方式失效或不可用时，用于紧急管理系统的本地登录账户。应急账户不受手动删除或计划失效要求的约束。

临时账户的自动失效时间可根据具体情况进行适当延长，但不得无限期延长。对于需要长期维护账户的特权用户，应建立有文档记录的永久账户。

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

### 14.7 KubeOS不能自动删除或禁用紧急管理员帐户

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

紧急管理员帐户，是在系统上启用的本地登录帐户，供授权系统管理员在标准登录方法失败或不可用时紧急使用，以管理系统。紧急帐户不受手动删除或计划到期要求的限制。

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

### 14.8 KubeOS不能有不必要的帐户.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

没有操作目的的帐户为系统提供了更多的危害机会。不必要的帐户包括不需要访问系统的个人的用户帐户和系统上未安装的应用程序的应用程序帐户。

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

### 14.9 KubeOS不能有不必要的帐户权能.

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

没有操作目的的帐户提供了额外的系统风险。因此，所有必要的非交互式帐户不应该为其分配交互式shell。

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

### 14.10 KubeOS必须在密码到期后35天不活跃后禁用帐户标识符（个人、组、角色和设备）

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

非活动标识符会给系统和应用程序带来风险，因为攻击者可能会利用非活动标识符，并可能获得对系统的未检测的访问权限。非活动帐户的所有者不会注意到，如果对其用户帐户的未经授权的访问已获得。

KubeOS必须跟踪不活动的时间段，并在不活动35天后禁用应用程序标识符。

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

### 14.11 KubeOS对于交互式用户，不能有重复的用户ID(UID)

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

为了确保问责制和防止未经身份验证的访问，必须识别和验证交互式用户，以防止系统的潜在滥用和危害。

交互式用户包括组织雇员或组织认为具有同等雇员身份的个人（例如，承包商）。交互式用户（和代表用户行事的进程）必须唯一标识并对所有访问进行身份验证，以下情况除外：

1)由组织明确标识和记录的访问。组织记录无需识别或认证即可在信息系统上执行的特定用户操作；

2)通过授权使用组身份验证器而不进行个人身份验证而发生的访问。组织可能需要团体帐户（例如共享特权帐户）中的个人的唯一标识，或者需要对个人活动进行详细的问责。

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

### 14.12 KubeOS必须在登录时显示上次成功登录帐户的日期和时间

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

向用户提供上次帐户访问发生时间的反馈，有助于用户识别和报告未经授权的帐户使用。

**规则影响：**

无

**检查方法：**


执行grep "session required pam_lastlog.so showfailed" /etc/pam.d/login
，若有返回值，则pass，否则fail

**修复方法：**


修改/etc/pam.d/login文件，在头部配置：
session required pam_lastlog.so showfailed

### 14.13 KubeOS必须在15分钟的非活动期间后启动会话锁定

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

会话超时锁定是一种临时措施，当用户停止工作并离开信息系统的物理邻近区域，但由于暂时性离开而未注销时，系统会采取此措施。

与其依赖用户在离开前手动锁定 KubeOS 会话，KubeOS 需要能够识别用户会话何时处于空闲状态，并采取行动启动会话锁定。 

会话锁定在能够确定和/或控制会话活动的位置实现。

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

### 14.14 KubeOS 必须在连续三次无效访问尝试后锁定账户

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

通过限制失败的访问尝试次数，可以降低通过用户密码猜测（也称为暴力破解）方式未经授权访问系统的风险。限制措施是通过锁定账户来实现的。 

pam_faillock.so 模块会记录尝试访问的次数。这包括在登录字段中输入用户名以及输入密码。通过统计访问尝试次数，可以在不输入密码字段的情况下锁定账户。这一点需要加以考虑，因为它可能成为拒绝服务（DoS）攻击的一种途径。

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

### 14.15 KubeOS必须强制在通过可插拔身份验证模块（PAM）登录尝试失败后的登录提示之间至少有5秒的延迟

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

限制特定时间间隔内的登录尝试次数可以减少未经授权的用户访问帐户的机会。

**规则影响：**

无

**检查方法：**


执行grep “auth required pam_faildelay.so delay=5000000“ /etc/pam.d/common-auth，若有返回值，则pass，否则fail

**修复方法：**


修改/etc/pam.d/common-auth文件，增加或修改配置：
auth required pam_faildelay.so delay=5000000

### 14.16 KubeOS在使用“sudo”时，必须使用调用用户的密码进行提权

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

sudoers安全策略要求用户在使用sudo之前进行身份验证。当sudoers需要身份验证时，它会验证调用用户的凭据。如果定义了rootpw、targetpw或runaspw标志但未禁用，则默认情况下，操作系统将提示调用用户输入“root”用户密码。

有关列出的每个配置的更多信息，请参考sudoers(5)手册页。

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

### 14.17 KubeOS在更改身份验证、角色或提升权限时必须重新验证用户

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果没有重新身份验证，用户可以访问资源或执行他们没有授权的任务

当KubeOS提供了更改用户身份验证、更改安全角色或升级功能的功能时，用户重新身份验证至关重要

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

### 14.18  KubeOS在使用“sudo”命令时必须要求重新身份验证

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果没有重新身份验证，用户可能会访问资源或执行他们没有授权的任务。

当操作系统提供升级功能的能力时，组织要求用户在使用“sudo”命令时重新进行身份验证是至关重要的。

如果该值设置为小于0的整数，则用户的时间戳不会过期，并且用户在终止用户的会话之前不必为特权操作重新进行身份验证。

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

### 14.19 KubeOS必须限制权限提升给授权人员

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

sudo命令允许用户以提升的（管理员）权限执行程序。它提示用户输入密码，并通过检查一个名为sudoers的文件来确认执行命令的请求。如果“sudoers”文件未正确配置，则系统上定义的任何用户都可以在目标系统上启动特权操作。

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

### 14.20 KubeOS必须为/etc/sudoers文件指定默认的“include”目录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

“sudo”命令允许授权用户以其他用户、系统用户和root身份运行程序（包括shell）。“/etc/sudoers”文件用于配置授权的“sudo”用户以及允许他们运行的程序。“/etc/sudoers”文件中的某些配置选项允许已配置的用户在不重新验证的情况下运行程序。使用这些配置选项可以使一个受感染帐户更容易地用于危害其他帐户。

可以使用@include和@includeir指令从当前正在解析的sudoers文件中包含其他sudoers文件。为了与1.9.1之前的sudo版本兼容，#include和#includeir也被接受。当sudo到达此行时，它将暂停当前文件（/etc/sudoers）的处理，并切换到指定的文件/目录。一旦到达包含文件的结尾（s），将处理/etc/sudoers的其余部分。包含的文件本身可能包含其他文件。强制使用128个嵌套的包含文件的硬限制，以防止包含文件循环。

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

### 14.21 KubeOS必须强制使用至少包含一个大写字符的密码

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

使用复杂密码有助于增加破坏密码所需的时间和资源。密码复杂度，或强度，是衡量密码在抵御猜测和暴力攻击企图方面的有效性。

密码复杂度是决定破解密码所需时间的几个因素中的一个。密码越复杂，在密码被泄露之前需要测试的可能组合的数量就越多。

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

### 14.22 KubeOS必须强制使用至少包含一个小写字符的密码

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

使用复杂密码有助于增加破坏密码所需的时间和资源。密码复杂度，或强度，是衡量密码抵抗猜测和暴力攻击企图的有效性的指标。

密码复杂度是决定破解密码所需时间的几个因素中的一个。密码越复杂，在密码被泄露之前需要测试的可能组合的数量就越多。

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

### 14.23 KubeOS必须强制使用至少包含一个数字字符的密码

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

使用复杂密码有助于增加破坏密码所需的时间和资源。密码复杂度，或强度，是衡量密码抵抗猜测和暴力攻击企图的有效性的一种度量。

密码复杂度是决定破解密码所需时间的几个因素中的一个。密码越复杂，在密码被泄露之前需要测试的可能组合的数量就越多。

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

### 14.24 KubeOS必须强制使用至少包含一个特殊字符的密码
**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

使用复杂密码有助于增加破坏密码所需的时间和资源。密码复杂度或强度是衡量密码在抵御猜测和暴力攻击企图方面的有效性的指标。

密码复杂度是决定破解密码所需时间的一个因素。密码越复杂，在密码被泄露之前需要测试的可能组合的数量就越多。

特殊字符不是字母数字字符。示例包括：~ ! @ # $ % ^ *.

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

### 14.25 KubeOS必须防止使用字典单词作为密码

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果KubeOS允许用户根据字典单词选择密码，这将通过增加成功猜测和暴力破解攻击的机会来增加密码泄露的机会。

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

### 14.26 KubeOS必须使用至少15个字符的密码

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

密码越短，在密码被泄露之前需要测试的可能组合的数量就越少。

密码复杂度，或强度，是衡量密码抵抗猜测和暴力攻击企图的有效性的指标。密码长度是几个因素中的一个，有助于确定密码的强度和破解密码所需的时间。在密码中使用更多的字符有助于指数性地增加破坏密码所需的时间和/或资源。

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

### 14.27 KubeOS在修改密码时，必须要求至少修改总字符数的8个字符

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果KubeOS允许用户连续重用大量的密码，这通过增加猜测和暴力破解尝试的机会窗口来增加密码泄露的机会

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

### 14.28 KubeOS 必须禁止密码在至少五次内重复使用

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

密码复杂度或强度是衡量密码抵抗猜测和暴力攻击企图的有效性的指标。如果信息系统或应用程序允许用户在密码超过其定义的生存期时连续重复使用其密码，则最终结果是密码未按照策略要求进行更改。

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

### 14.29 KubeOS必须配置Linux可插拔身份验证模块（PAM）以仅存储密码的加密表示形式

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

任何时候都需要保护密码，加密是保护密码的标准方法。如果密码没有加密，它们可能会被明文读取（即明文）并且很容易被泄露。

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

### 14.30 KubeOS必须使用至少24小时（一天）的用户密码

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

实施最短密码有效期期有助于防止重复更改密码以破坏密码重用或历史强制要求。如果允许用户立即和持续地更改其密码，则密码可能会在短时间内被反复更改，从而破坏组织关于密码重用的策略。

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

### 14.31 KubeOS必须使用最长有效期为60天的用户密码

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

任何密码，无论多么复杂，最终都可以被破解。因此，需要定期修改密码。如果KubeOS不限制密码的有效期，并强制用户更改密码，则存在KubeOS密码可能被泄露的风险。

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

### 14.32 KubeOS必须使用密码历史记录文件

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

密码复杂度，或强度，是衡量密码抵抗猜测和暴力攻击企图的有效性的指标。如果信息系统或应用程序允许用户在密码超过其定义的有效期时连续重复使用其密码，则最终结果是密码未按照策略要求进行更改。

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

### 14.33 KubeOS必须使用FIPS 140-2/140-3认可的加密哈希算法来进行系统身份验证（login.defs）

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

用于对加密模块进行身份验证的未经批准的机制未经验证，因此不能依赖这些机制来提供机密性或完整性，并且数据可能会受到破坏。

使用加密的KubeOS需要使用符合FIPS 140-2/140-3的机制来对加密模块进行身份验证。

FIPS 140-2/140-3是验证用于访问加密模块的机制是否使用符合要求的身份验证的当前标准。这允许在通用计算系统上使用安全级别1、2、3或4。

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

### 14.34 KubeOS必须配置为创建或更新密码，其有效期至少为24小时（一天）

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

实施最短的密码有效期有助于防止重复更改密码，以违反密码重用或历史强制要求。如果允许用户立即和持续地更改其密码，则密码可能会在短时间内被反复更改，从而破坏组织关于密码重用的策略。

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

### 14.35 KubeOS必须配置为创建或更新密码，最长有效期为60天

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

任何密码，无论多么复杂，最终都可以被破解。因此，需要定期修改密码。如果KubeOS不限制密码的有效期，并强制用户更改密码，则存在KubeOS密码可能被泄露的风险。

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

### 14.36 KubeOS必须通过可插拔认证模块（PAM）实现对特权帐户的多因素认证。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

使用与信息系统分离的身份验证设备（例如通用访问卡(CAC）或令牌)，可确保即使信息系统受到破坏，该危害也不会影响身份验证设备上存储的凭据。

需要与信息系统分离的设备获得访问权限的多因素解决方案包括，例如，提供基于时间或挑战响应的验证器的硬件令牌和智能卡，如美国政府个人身份验证（PIV）卡和国防部CAC。

特权帐户定义为具有特权用户授权的信息系统帐户。

远程访问是指授权用户（或信息系统）通过外部非组织控制的网络访问国防部非公共信息系统。远程接入方式包括拨号、宽带、无线等。

此要求仅适用于特定于设备功能或具有组织用户概念（例如，VPN、代理功能）的组件。这不适用于为配置设备本身（管理）而进行的身份验证。

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

### 14.37 KubeOS必须为多因素身份验证实现证书状态检查。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

使用与信息系统分离的身份验证设备（如通用访问卡(CAC）或令牌)，可确保在信息系统受到破坏时，存储在身份验证设备上的凭据不会受到影响。

需要与信息系统分离的设备才能获得访问权限的多因素解决方案包括提供基于时间或挑战响应验证器的硬件令牌，以及诸如美国政府个人身份验证（PIV）卡和国防部CAC等智能卡。

特权帐户定义为具有特权用户授权的信息系统帐户。

远程访问是指授权用户（或信息系统）通过外部非组织控制的网络访问非公共信息系统。远程接入方式包括拨号、宽带、无线等。

此要求仅适用于具有设备特定功能的组件，或组织用户（例如，VPN、代理功能）。这不适用于为配置设备本身（管理）而进行的身份验证。

**规则影响：**

无

**检查方法：**


```bash
执行grep use_pkcs11_module /etc/pam_pkcs11/pam_pkcs11.conf | awk '/pkcs11_module coolkey {/,/}/' /etc/pam_pkcs11/pam_pkcs11.conf | grep cert_policy，若返回结果为cert_policy = ca,ocsp_on,signature,crl_auto;则pass，否则fail
```

**修复方法：**


修改 /etc/pam_pkcs11/pam_pkcs11.conf文件，修改配置：保证所有的cert_policy值中包含ocsp_on配置。

### 14.38 如果KubeOS正在使用网络安全服务（NSS），则必须在一天后禁止使用缓存的身份验证

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果缓存的认证信息过期，认证信息的有效性可能会受到质疑。

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

### 14.39 KubeOS必须配置Linux可插拔身份验证模块（PAM）以禁止在一天后使用缓存的离线身份验证

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果缓存的认证信息过期，则认证信息的有效性可能会受到质疑。

**规则影响：**

无

**检查方法：**


执行grep "offline_credentials_expiration" /etc/sssd/sssd.conf
若返回结果为offline_credentials_expiration = 1，则pass，否则fail

**修复方法：**


修改/etc/sssd/sssd.conf文件，添加或修改配置：
在"[pam]"下配置
offline_credentials_expiration = 1

### 14.40 KubeOS对于基于PKI的身份验证，必须通过构造指向可接受的信任锚的证书路径（包括状态信息）来验证证书

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果没有路径验证，则当提供任何尚未明确受信任的证书时，依赖方无法做出明智的信任决定。

信任锚是通过公钥和相关数据表示的权威实体。它用于公钥基础设施、X.509数字证书和DNSSEC的上下文中。

当存在信任链时，通常要信任的顶级实体成为信任锚；它可以是，例如，证书颁发机构（CA）。证书路径从主体证书开始，经过许多中间证书，直到可信根证书，通常由可信CA颁发。

此要求验证是否使用到接受的信任锚的证书路径进行证书验证，并且该路径是否包含状态信息。路径验证对于依赖方而言是必要的，当提供任何尚未明确受信任的证书时，可以做出知情的信任决定。

证书路径的状态信息包括证书吊销列表或在线证书状态协议响应。验证证书状态信息不在此要求的范围之内。

**规则影响：**

无

**检查方法：**


执行grep cert_policy /etc/pam_pkcs11/pam_pkcs11.conf
若返回结果为cert_policy = ca,ocsp_on,signature,crl_auto;，则pass，否则fail

**修复方法：**


修改 /etc/pam_pkcs11/pam_pkcs11.conf文件，添加或修改配置：
cert_policy = ca,oscp_on,signature,crl_auto

### 14.41 KubeOS必须配置为在包更改时不覆盖可插拔身份验证模块（PAM）配置

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

在系统中安装、更新或删除软件包时，“pam-config”命令行实用程序会自动生成系统PAM配置。“pam-config”会删除它不知道的PAM模块和参数的配置。它可能会使系统管理员的PAM配置无效，从而影响系统的安全性

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

### 14.42 KubeOS root帐户必须是唯一对系统具有不受限制访问权限的帐户

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

如果root以外的帐户也具有“0”的用户标识符（UID），则它具有root权限，从而使该帐户可以不受限制地访问整个KubeOS。UID为“0”的多个帐户为潜在入侵者提供了猜测特权帐户密码的机会。

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

### 14.43 KubeOS不能配置为允许空白或空密码

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

任何时候都需要保护密码，加密是保护密码的标准方法。如果密码没有加密，它们可能会被明文读取（即明文），并且很容易被泄露。

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

### 14.44 KubeOS不能使用配置为空白或null密码的帐户

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

如果帐户的密码为空，则任何人都可以使用该帐户的权限登录并运行命令。不应该在操作环境中使用空密码的帐户。

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

### 14.45 KubeOS必须使用FIPS 140-2/140-3认可的加密哈希算法来进行系统身份验证

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

系统必须使用强哈希算法来存储密码。系统必须使用足够数量的哈希轮数，以确保所需的熵水平。

任何时候都需要保护密码，加密是保护密码的标准方法。如果密码不加密，它们可能被明文读取（即明文），并且很容易被泄露。

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

### 14.46 KubeOS影子密码套件必须配置为使用足够数量的哈希轮数

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

系统必须使用强哈希算法来存储密码。系统必须使用足够数量的哈希轮数，以确保所需的熵水平。

任何时候都需要保护密码，加密是保护密码的标准方法。如果密码没有加密，它们可能会被明文读取（即明文），并且很容易被泄露。

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

### 15.1 KubeOS必须启用SELinux targeted策略

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果没有对安全功能进行验证，安全功能可能无法正常运行，并且可能会忽略故障。安全功能定义为信息系统的硬件、软件和/或固件，负责实施系统安全策略并支持保护所基于的代码和数据的隔离。安全功能包括但不限于建立系统帐户、配置访问授权（即权限、特权）、设置要审计的事件以及设置入侵检测参数。

此要求适用于执行安全功能验证/测试的操作系统和/或需要此功能的系统和环境。

**规则影响：**

无

**检查方法：**


```bash
执行grep -i "SELINUXTYPE=targeted" /etc/selinux/config，若有返回值，则pass，否则fail
```

**修复方法：**


修改/etc/selinux/config文件，增加或修改配置：
SELINUXTYPE=targeted

### 15.2 KubeOS必须防止非特权用户执行特权功能，包括禁用、规避或更改已实施的安全保障/对策。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

防止非特权用户执行特权功能可以降低未经授权的个人或进程可能获得对信息或特权的不必要访问的风险。

特权功能包括，例如，建立帐户，执行系统完整性检查，或管理加密密钥管理活动。非特权用户是指不拥有适当授权的个人。绕过入侵检测和防御机制或恶意代码保护机制是需要保护非特权用户的特权功能的示例。

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

### 15.3 KubeOS必须使用配置为对系统服务实施限制的Linux安全模块

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

如果没有对安全功能进行验证，安全功能可能无法正确运行，并且可能会忽略故障。安全功能被定义为信息系统的硬件、软件和/或固件，负责实施系统安全策略并支持保护所基于的代码和数据的隔离。安全功能包括但不限于建立系统帐户、配置访问授权（即权限、特权）、设置要审计的事件以及设置入侵检测参数。

此要求适用于执行安全功能验证/测试的操作系统和/或需要此功能的系统和环境。

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

### 16.1 KubeOS必须使用文件完整性工具来验证所有安全功能的正确运行

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果没有对安全功能进行验证，安全功能可能无法正确运行，并且故障可能会被忽视。安全功能定义为信息系统的硬件、软件和/或固件，负责实施系统安全策略并支持保护所基于的代码和数据的隔离。安全功能包括但不限于建立系统帐户、配置访问授权（即权限、特权）、设置要审计的事件以及设置入侵检测参数。

此要求适用于KubeOS执行安全功能验证/测试和/或需要此功能的系统和环境。

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

### 16.2 KubeOS文件完整性工具必须配置来验证访问控制列表（ACL）

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

ACL可以提供超过文件模式允许的权限，并且必须通过文件完整性工具进行验证。

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

### 16.3 KubeOS文件完整性工具需要配置，以验证扩展属性

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

文件系统中的扩展属性用于包含具有安全影响的任意数据和文件元数据。

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

### 16.4 KubeOS文件完整性工具必须配置，以保护审计工具的完整性

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

保护用于审计目的的工具的完整性是确保审计信息完整性的关键步骤。审计信息包括成功审计信息系统活动所需的所有信息（例如，审计记录、审计设置和审计报告）。

审计工具包括但不限于供应商提供的和成功查看和操作审计信息系统活动和记录所需的开源审计工具。审核工具包括自定义查询和报告生成器。

攻击者更换审计工具或向现有工具注入代码，以提供隐藏或清除审计日志中的系统活动的功能，这种情况并不罕见。

为了解决这种风险，审计工具必须进行加密签名，以提供识别审计工具何时被修改、操作或替换的功能。例如，文件的校验和哈希。

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

### 16.5 高级入侵检测环境（AIDE）必须至少每周验证一次KubeOS的基线配置

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

对基线配置的未经授权的更改可能会使系统容易受到各种攻击，或允许未经授权的访问KubeOS。对KubeOS配置的更改可能会产生意想不到的副作用，其中一些可能与安全有关。

检测此类更改并提供自动响应有助于避免可能最终影响KubeOS安全状态的意外的负面后果。KubeOS的信息系统安全管理员（ISSM）/信息系统安全官（ISSO）和系统管理员（SA）必须在未经授权的配置项修改时通过电子邮件和/或监控系统得到通知。

**规则影响：**

无

**检查方法：**


```bash
执行grep -R aide /etc/crontab /etc/cron.*，若有返回值，则pass，否则fail
```

**修复方法：**


修改/etc/cron.weekly/aide文件，添加或修改配置：
space_left  = 25%

### 16.6 KubeOS必须在高级入侵检测环境（AIDE）发现任何安全功能运行中的异常时通知系统管理员（SA）

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不对异常进行操作，安全功能可能无法保护系统。

安全功能定义为信息系统的硬件、软件和/或固件，负责实施系统安全策略并支持保护所基于的代码和数据的隔离。安全功能包括但不限于建立系统帐户、配置访问授权（即权限、特权）、设置要审计的事件以及设置入侵检测参数。

信息系统提供的通知包括本地计算机控制台和/或硬件指示（例如指示灯）的消息。

这种能力必须考虑到可用性的操作要求，以选择适当的响应。组织可以选择在检测到安全功能异常时关闭或重新启动信息系统。

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

### 17.1 KubeOS必须实时卸载联网系统的rsyslog消息，并至少每周卸载独立系统

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

存储在一个位置的信息容易被意外或偶然的删除或更改。

卸载是审计存储容量有限的信息系统中的常见过程。

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

### 17.2 KubeOS必须安装审计包

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不确定发生了什么类型的事件、事件的来源、事件发生的位置和事件的结果，就很难建立、关联和调查导致中断或攻击的事件。

满足此要求可能需要的审计记录内容包括，例如时间戳、源地址和目标地址、用户/进程标识符、事件描述、成功/失败指示、涉及的文件名以及调用的访问控制或流控制规则。

将事件类型与KubeOS审核日志中检测到的事件相关联，可以提供调查攻击、识别资源利用率或容量阈值或识别配置不当的KubeOS的方法。

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

### 17.3 KubeOS审计记录必须包含信息，以确定发生了什么类型的事件、事件的来源、事件发生的位置以及事件的结果

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不确定发生了什么类型的事件、事件的来源、事件发生的位置和事件的结果，就很难建立、关联和调查导致中断或攻击的事件。

满足此要求可能需要的审计记录内容包括，例如时间戳、源地址和目标地址、用户/进程标识符、事件描述、成功/失败指示、涉及的文件名以及调用的访问控制或流控制规则。

将事件类型与KubeOS审核日志中检测到的事件相关联，可以提供调查攻击、识别资源利用率或容量阈值或识别配置不当的KubeOS的方法。

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

### 17.4 在KubeOS上必须安装audit-audispd-plugins包

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

存储在一个位置的信息容易被意外或偶然的删除或更改。

卸载是审计存储容量有限的信息系统中的常见过程。

auditd服务不包括将审核记录直接发送到集中服务器进行管理的功能。但是，它可以使用审计事件多路复用器的插件将审计记录传递到远程服务器。

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

### 17.5 当审计记录没有立即发送到中央审计记录存储设施时，KbeOS必须分配审计记录存储容量以存储至少一周的审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

为了确保KubeOS有足够的存储容量来写入审计日志，KbeOS必须能够分配审计记录存储容量。

分配审计记录存储容量的任务通常在KubeOS初始安装时执行。

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

### 17.6 KubeOS auditd服务必须在审计存储容量已满75%时立即通知系统管理员（SA）和信息系统安全官（ISSO）

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果在存储卷达到75%利用率时没有立即通知安全人员，他们就无法规划审计记录存储容量扩展。

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

### 17.7 KubeOS审计系统必须在审计存储卷已满时采取适当的操作

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

关键是当KubeOS面临无法按要求处理审核日志的风险时，它会采取措施来缓解故障。审核处理失败包括软件/硬件错误、审核捕获机制故障以及达到或超过审核存储容量。对审核失败的响应取决于失败模式的性质。

如果可用性是首要考虑的问题，则为响应审核失败而采取的其他经批准的措施如下：

1)如果故障是由于缺乏审计记录存储容量导致的，则KubeOS必须在可能的情况下继续生成审计记录（必要时自动重启审计服务），以先进先出的方式覆盖最旧的审计记录。

2)如果审计记录被发送到集中收集服务器，并且与此服务器的通信丢失或服务器发生故障，则KubeOS必须在本地对审计记录进行排队，直到通信恢复或直到手动检索审计记录。恢复与集中采集服务器的连接后，应采取措施将本地审计数据与采集服务器同步。

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




### 17.8 KubeOS 必须将审计记录卸载到与被审计系统不同的系统或介质上

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

存储在一个位置的信息容易受到意外或偶然性的删除或篡改。卸载是审计存储容量有限的信息系统中常见的处理流程。

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

### 17.9 当 KubeOS 审计存储已满时，Audispd 必须采取适当的措施

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

存储在一个位置的信息容易受到意外或偶然性的删除或篡改。卸载是审计存储容量有限的信息系统中常见的处理流程。

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

### 17.10 KubeOS 必须保护审计规则免受未经授权的修改

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果无法限制哪些角色和个人可以选择审计哪些事件，未经授权的人员就可能阻止对关键事件的审计。配置不当的审计可能会因审计日志过载而降低系统性能。配置不当的审计还可能使建立、关联和调查与事件相关的事件，或识别事件责任人变得更加困难。

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

### 17.11 KubeOS 审计工具必须配置适当的权限，以防止未经授权的访问

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

保护审计信息包括识别和保护用于查看和操作日志数据的工具。保护审计工具对于防止对审计信息的未授权操作是必要的。
KubeOS 提供与审计信息交互的工具时，将利用用户权限和角色来识别访问工具的用户以及该用户享有的相应权限，从而对审计工具的访问做出访问决策。
审计工具包括但不限于供应商提供的和开源的审计工具，这些工具用于查看和操作审计信息系统活动和记录。审计工具还包括自定义查询和报告生成器。

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

### 17.12 Audispd 必须将审计记录从被审计的 KubeOS 系统卸载到不同的系统或介质上

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

存储在一个位置的信息容易受到意外或偶然性的删除或篡改。卸载是审计存储容量有限的信息系统中常见的处理流程。

**规则影响：**

无

**检查方法：**


执行grep remote_server /etc/audisp/audisp-remote.conf若返回值中存在remote_server = <ip_address>，则pass，否则fail

手动配置能够ping通的IP地址
ping <ip_address>

**修复方法：**


根据实际情况配置远端服务器IP，修改配置文件/etc/audit/audisp-remote.conf，增加或修改remote_server = <ip_address>

### 17.13 信息系统安全官（ISSO）和系统管理员（SA）至少必须配置邮件别名，以便在 KubeOS 审计处理失败时接收通知

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果系统存在无法按要求处理审计日志的风险，让相关人员及时知晓至关重要。若无此通知，安全人员可能无法察觉审计功能即将发生的故障，进而可能对系统运行产生不利影响。
审计处理故障包括软件/硬件错误、审计捕获机制故障，以及审计存储容量达到或超过上限。
此要求适用于每个审计数据存储库（即存储审计记录的独立信息系统组件）、组织的集中式审计存储容量（即所有审计数据存储库的总和），或两者兼而有之。

**规则影响：**

无

**检查方法：**


```bash
执行grep -i "^postmaster:" /etc/aliases | grep root，若返回值中postmaster的值为root，则pass，否则fail
```

**修复方法：**


修改/etc/aliases文件，添加或修改配置：
postmaster: root

### 17.14 信息系统安全官（ISSO）和系统管理员（SA）至少必须在 KubeOS 审计处理故障事件发生时收到告警

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果系统存在无法按要求处理审计日志的风险，让相关人员及时知晓至关重要。若无此通知，安全人员可能无法察觉审计功能即将发生的故障，进而可能对系统运行产生不利影响。
审计处理故障包括软件/硬件错误、审计捕获机制故障，以及审计存储容量达到或超过上限。
此要求适用于每个审计数据存储库（即存储审计记录的独立信息系统组件）、组织的集中式审计存储容量（即所有审计数据存储库的总和），或两者兼而有之。

**规则影响：**

无

**检查方法：**


执行grep action_mail /etc/audit/auditd.conf，若返回值为action_mail_acct = root，则pass，否则fail

**修复方法：**


修改/etc/audit/auditd.conf文件，添加或修改配置：
action_mail_acct = root

### 17.15 KubeOS 必须针对所有对 "chacl" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成针对组织安全和业务需求的特定审计记录，将难以建立、关联和调查与事件相关的事件，或识别事件责任人。
审计记录可由信息系统内的各个组件生成（例如，模块或策略过滤器）。

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

### 17.16 KubeOS 必须针对所有对 "chage" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成针对组织安全和业务需求的特定审计记录，将难以建立、关联和调查与事件相关的事件，或识别事件责任人。
审计记录可由信息系统内的各个组件生成（例如，模块或策略过滤器）。

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

### 17.17 KubeOS 必须针对所有对 "chcon" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成针对组织安全和业务需求的特定审计记录，将难以建立、关联和调查与事件相关的事件，或识别事件责任人。
审计记录可由信息系统内的各个组件生成（例如，模块或策略过滤器）。

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

### 17.18 KubeOS 必须针对所有对 "chfn" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果审计记录包含的信息不足，将无法进行有害事件的重构或取证分析。
组织至少必须对特权命令进行全文记录审计。组织必须维护足够详细的审计跟踪，以重构事件并确定入侵的原因和影响。

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

### 17.19 KubeOS 必须针对所有对 "chmod" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成针对组织安全和业务需求的特定审计记录，将难以建立、关联和调查与事件相关的事件，或识别事件责任人。
审计记录可由信息系统内的各个组件生成（例如，模块或策略过滤器）。

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

### 17.20 KubeOS 必须针对所有对 "chsh" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果审计记录包含的信息不足，将无法进行有害事件的重构或取证分析。
组织至少必须对特权命令进行全文记录审计。组织必须维护足够详细的审计跟踪，以重构事件并确定入侵的原因和影响。

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

### 17.21 KubeOS 必须针对所有对 "crontab" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成针对组织安全和业务需求的特定审计记录，将难以建立、关联和调查与事件相关的事件，或识别事件责任人。
审计记录可由信息系统内的各个组件生成（例如，模块或策略过滤器）。

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

### 17.22 KubeOS 必须针对所有对 "gpasswd" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果审计记录包含的信息不足，将无法进行有害事件的重构或取证分析。
组织至少必须对特权命令进行全文记录审计。组织必须维护足够详细的审计跟踪，以重构事件并确定入侵的原因和影响。

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

### 17.23 KubeOS 必须针对所有对 "insmod" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不具备生成审计记录的能力，将难以建立、关联和调查与事件相关的事件，或识别事件责任人。
审计记录可由信息系统内的各个组件生成（例如，模块或策略过滤器）。
审计事件列表是指需要生成审计的事件集合。该事件集合通常是系统能够生成审计记录的所有事件列表的一个子集。
美国国防部（DOD）已定义了以下 KubeOS 必须提供审计记录生成能力的事件列表：
1）成功和失败的访问、修改或删除权限、安全对象、安全级别或信息类别（如分类级别）的尝试；
2）访问行为，例如成功和失败的登录尝试、特权活动或其他系统级访问、用户访问系统的起止时间、来自不同工作站的并发登录、成功和失败的对象访问、所有程序启动以及所有对信息系统的直接访问；
3）所有账户的创建、修改、禁用和终止；以及
4）所有内核模块的加载、卸载和重启操作。

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

### 17.24 KubeOS 必须针对所有对 "kmod" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不具备生成审计记录的能力，将难以建立、关联和调查与事件相关的事件，或识别事件责任人。
审计记录可由信息系统内的各个组件生成（例如，模块或策略过滤器）。
审计事件列表是指需要生成审计的事件集合。该事件集合通常是系统能够生成审计记录的所有事件列表的一个子集。
美国国防部（DOD）已定义了以下 KubeOS 必须提供审计记录生成能力的事件列表：
1）成功和失败的访问、修改或删除权限、安全对象、安全级别或信息类别（如分类级别）的尝试；
2）访问行为，例如成功和失败的登录尝试、特权活动或其他系统级访问、用户访问系统的起止时间、来自不同工作站的并发登录、成功和失败的对象访问、所有程序启动以及所有对信息系统的直接访问；
3）所有账户的创建、修改、禁用和终止；以及
4）所有内核模块的加载、卸载和重启操作。

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

### 17.25 KubeOS 必须针对所有对 "modprobe" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不具备生成审计记录的能力，将难以建立、关联和调查与事件相关的事件，或识别事件责任人。
审计记录可由信息系统内的各个组件生成（例如，模块或策略过滤器）。
审计事件列表是指需要生成审计的事件集合。该事件集合通常是系统能够生成审计记录的所有事件列表的一个子集。
美国国防部（DOD）已定义了以下 KubeOS 必须提供审计记录生成能力的事件列表：
1）成功和失败的访问、修改或删除权限、安全对象、安全级别或信息类别（如分类级别）的尝试；
2）访问行为，例如成功和失败的登录尝试、特权活动或其他系统级访问、用户访问系统的起止时间、来自不同工作站的并发登录、成功和失败的对象访问、所有程序启动以及所有对信息系统的直接访问；
3）所有账户的创建、修改、禁用和终止；以及
4）所有内核模块的加载、卸载和重启操作。

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

### 17.26 KubeOS 必须针对所有对 "newgrp" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果审计记录包含的信息不足，将无法进行有害事件的重构或取证分析。
组织至少必须对特权命令进行全文记录审计。组织必须维护足够详细的审计跟踪，以重构事件并确定入侵的原因和影响。

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

### 17.27 KubeOS 必须针对所有对 "pam_timestamp_check" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成针对组织安全和业务需求的特定审计记录，将难以建立、关联和调查与事件相关的事件，或识别事件责任人。
审计记录可由信息系统内的各个组件生成（例如，模块或策略过滤器）。

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

### 17.28 KubeOS 必须针对所有对 "passwd" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果审计记录包含的信息不足，将无法进行有害事件的重构或取证分析。
组织至少必须对特权命令进行全文记录审计。组织必须维护足够详细的审计跟踪，以重构事件并确定入侵的原因和影响。

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

### 17.29 KubeOS 必须针对所有对 "rm" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成针对组织安全和业务需求的特定审计记录，将难以建立、关联和调查与事件相关的事件，或识别事件责任人。
审计记录可由信息系统内的各个组件生成（例如，模块或策略过滤器）。

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

### 17.30 KubeOS 必须针对所有对 "rmmod" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不具备生成审计记录的能力，将难以建立、关联和调查与事件相关的事件，或识别事件责任人。
审计记录可由信息系统内的各个组件生成（例如，模块或策略过滤器）。
审计事件列表是指需要生成审计的事件集合。该事件集合通常是系统能够生成审计记录的所有事件列表的一个子集。
美国国防部（DOD）已定义了以下 KubeOS 必须提供审计记录生成能力的事件列表：
1）成功和失败的访问、修改或删除权限、安全对象、安全级别或信息类别（如分类级别）的尝试；
2）访问行为，例如成功和失败的登录尝试、特权活动或其他系统级访问、用户访问系统的起止时间、来自不同工作站的并发登录、成功和失败的对象访问、所有程序启动以及所有对信息系统的直接访问；
3）所有账户的创建、修改、禁用和终止；以及
4）所有内核模块的加载、卸载和重启操作。

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

### 17.31 KubeOS 必须针对所有对 "setfacl" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成针对组织安全和业务需求的特定审计记录，将难以建立、关联和调查与事件相关的事件，或识别事件责任人。
审计记录可由信息系统内的各个组件生成（例如，模块或策略过滤器）。

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

### 17.32 KubeOS 必须针对所有对 "ssh-agent" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果审计记录包含的信息不足，将无法进行有害事件的重构或取证分析。
组织至少必须对特权命令进行全文记录审计。组织必须维护足够详细的审计跟踪，以重构事件并确定入侵的原因和影响。

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

### 17.33 KubeOS 必须针对所有对 "ssh-keysign" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果审计记录包含的信息不足，将无法进行有害事件的重构或取证分析。
组织至少必须对特权命令进行全文记录审计。组织必须维护足够详细的审计跟踪，以重构事件并确定入侵的原因和影响。

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

### 17.34 KubeOS 必须针对所有对 "su" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成针对组织安全和业务需求的特定审计记录，将难以建立、关联和调查与事件相关的事件，或识别事件责任人。
审计记录可由信息系统内的各个组件生成（例如，模块或策略过滤器）。

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

### 17.35 KubeOS 必须针对所有对 "sudo" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果审计记录包含的信息不足，将无法进行有害事件的重构或取证分析。
组织至少必须对特权命令进行全文记录审计。组织必须维护足够详细的审计跟踪，以重构事件并确定入侵的原因和影响。

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

### 17.36 KubeOS 必须针对所有对 "sudoedit" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成针对组织安全和业务需求的特定审计记录，将难以建立、关联和调查与事件相关的事件，或识别事件责任人。
审计记录可由信息系统内的各个组件生成（例如，模块或策略过滤器）。

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

### 17.37 KubeOS 必须针对所有对 "unix_chkpwd" 或 "unix2_chkpwd" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成针对组织安全和业务需求的特定审计记录，将难以建立、关联和调查与事件相关的事件，或识别事件责任人。
审计记录可由信息系统内的各个组件生成（例如，模块或策略过滤器）。

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

### 17.38 KubeOS 必须针对所有对 "usermod" 命令的使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成针对组织安全和业务需求的特定审计记录，将难以建立、关联和调查与事件相关的事件，或识别事件责任人。
审计记录可由信息系统内的各个组件生成（例如，模块或策略过滤器）。

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

### 17.39 KubeOS 必须针对所有影响 /etc/group 文件的账户创建、修改、禁用和终止事件生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

攻击者一旦获得系统的初始访问权限，往往会尝试创建一种持久化的方式以重新建立访问。实现该目的的一种简单方法就是创建新账户。对账户创建行为进行审计可以降低此类风险。
为满足访问需求，KubeOS 可与满足或超出访问控制策略要求的企业级认证/访问/审计机制集成。

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


### 17.40 KubeOS必须为影响/etc/security/opasswd的所有帐户创建、修改、禁用和终止事件生成审核记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

一旦攻击者建立了对系统的初始访问权限，攻击者通常会尝试创建一种持久的方法来重新建立访问权限。实现此目标的一种方法是，攻击者只需创建一个新帐户。审核账户创建可以降低这种风险。

为了满足访问需求，KbeOS可以与满足或超过访问控制策略要求的企业级身份验证/访问/审计机制集成。

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

### 17.41 KubeOS必须为所有影响/etc/passwd的帐户创建、修改、禁用和终止事件生成审计记录
**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

一旦攻击者建立了对系统的初始访问权限，攻击者通常会尝试创建一种持久的方法来重新建立访问权限。实现此目的的一种方法是，攻击者只需创建一个新帐户。审核账户创建可以降低这种风险。

为了满足访问需求，KbeOS可以与满足或超过访问控制策略需求的企业级身份验证/访问/审计机制集成。

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

### 17.42 KubeOS必须为影响/etc/shadow的所有帐户创建、修改、禁用和终止事件生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

一旦攻击者建立了对系统的初始访问权限，攻击者通常会尝试创建一种持久的方法来重新建立访问权限。实现此目标的一种方法是，攻击者只需创建一个新帐户。审核帐户创建可以降低这种风险。

为了满足访问需求，KbeOS可以与满足或超过访问控制策略需求的企业级身份验证/访问/审计机制集成。

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

### 17.43 KubeOS必须为“chmod”、“fchmod”和“fchmodat”系统调用的所有使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成特定于组织的安全和任务需求的审计记录，就很难建立、关联和调查与某个事件相关的事件，或确定事件的责任人。

审计记录可以从信息系统中的各种组件（例如，模块或策略过滤器）生成。系统调用规则加载到匹配的引擎中，该引擎拦截系统上所有程序发出的每个系统调用。因此，仅在绝对必要时使用syscall规则非常重要，因为这些规则会影响性能。规则越多，对性能的冲击越大。但是，只要有可能，通过将syscall合并到一个规则中可以帮助提高性能。

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

### 17.44 KubeOS必须为所有使用"chown"、"fchown"、"fchownat"和"lchown"系统调用生成审计记录
**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成特定于组织的安全和任务需求的审计记录，就很难建立、关联和调查与某个事件相关的事件，或确定事件的责任人。

审计记录可以从信息系统中的各种组件（例如，模块或策略过滤器）生成。系统调用规则加载到匹配的引擎中，该引擎拦截系统上所有程序发出的每个系统调用。因此，仅在绝对必要时使用syscall规则是非常重要的，因为这些会影响性能。规则越多，性能冲击越大。但是，尽可能将syscall合并到一个规则中，可以帮助提高性能。

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

### 17.45 KubeOS必须为“creat”、“open”、“open_by_handle_at”、“truncate”和“ftruncate”系统调用的所有使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成特定于组织的安全和任务需求的审计记录，就很难建立、关联和调查与某个事件相关的事件，或确定事件的责任人。

审计记录可以从信息系统中的各种组件（例如，模块或策略过滤器）生成。系统调用规则加载到匹配的引擎中，该引擎拦截系统上所有程序发出的每个系统调用。因此，仅在绝对必要时使用syscall规则是非常重要的，因为这些会影响性能。规则越多，对性能的冲击越大。但是，只要有可能，将syscall合并到一个规则中，可以帮助提高性能。

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

### 17.46 KubeOS必须为“delete_module”系统调用的所有使用生成审核记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成特定于组织的安全和任务需求的审计记录，就很难建立、关联和调查与某个事件相关的事件，或确定事件的责任人。

审计记录可以从信息系统中的各种组件（例如，模块或策略过滤器）生成。

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

### 17.47 KubeOS必须为“init_module”和“finit_module”系统调用的所有使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成特定于组织的安全和任务需求的审计记录，就很难建立、关联和调查与某个事件相关的事件，或确定事件的责任人。

审计记录可以从信息系统中的各种组件（例如，模块或策略过滤器）生成。系统调用规则加载到匹配的引擎中，该引擎拦截系统上所有程序发出的每个系统调用。因此，仅在绝对必要时使用syscall规则是非常重要的，因为这些会影响性能。规则越多，对性能的冲击越大。但是，尽可能将syscall合并到一个规则中，可以帮助提高性能。

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

### 17.48 KubeOS必须为“mount”系统调用的所有使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果审计记录不包含足够的信息，则无法重建有害事件或进行取证分析。

至少，组织必须审计特权命令的全文记录。组织必须维护足够详细的审计跟踪，以重构事件，以确定危害的原因和影响。

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

### 17.49 KubeOS必须为“setxattr”、“fsetxattr”、“lsetxattr”、“removexattr”、“fremovexattr”和“lremovexattr”系统调用的所有使用生成审计记录。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成特定于组织的安全和任务需求的审计记录，就很难建立、关联和调查与某个事件相关的事件，或确定事件的责任人。

审计记录可以从信息系统中的各种组件（例如，模块或策略过滤器）生成。系统调用规则加载到匹配的引擎中，该引擎拦截系统上所有程序发出的每个系统调用。因此，仅在绝对必要时使用syscall规则是非常重要的，因为这些会影响性能。规则越多，对性能的冲击越大。但是，只要有可能，将syscall合并到一个规则中，可以帮助提高性能。

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

### 17.50 KubeOS必须为“umount”系统调用的所有使用生成审计记录。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果审计记录不包含足够的信息，则无法重建有害事件或进行取证分析。

至少，组织必须审计特权命令的全文记录。组织必须维护足够详细的审计跟踪，以重构事件，以确定危害的原因和影响。

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

### 17.51 KubeOS必须为“unlink”、“unlinkat”、“重命名”、“rmdir”系统调用的所有使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成特定于组织的安全和任务需求的审计记录，就很难建立、关联和调查与某个事件相关的事件，或确定事件的责任人。

审计记录可以从信息系统中的各种组件（例如，模块或策略过滤器）生成。系统调用规则加载到匹配的引擎中，该引擎拦截系统上所有程序发出的每个系统调用。因此，仅在绝对必要时使用syscall规则是非常重要的，因为这些会影响性能。规则越多，对性能的冲击越大。但是，只要有可能，将syscall合并到一个规则中，可以帮助提高性能。

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

### 17.52 KubeOS必须为特权函数的所有使用生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

授权用户有意或无意地滥用特权功能，或未经授权的外部实体破坏了信息系统帐户，是一个严重且持续的问题，可能会对组织造成重大不利影响。对特权功能的使用进行审计是检测此类滥用并识别来自内部威胁和高级持续性威胁的风险的一种方法。

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

### 17.53 KubeOS必须为“lastlog”文件的所有修改生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成特定于组织的安全和任务需求的审计记录，就很难建立、关联和调查与某个事件相关的事件，或确定事件的责任人。

审计记录可以从信息系统中的各种组件（例如，模块或策略过滤器）生成。

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

### 17.54 KubeOS必须生成审计记录对“txt”文件的所有修改都必须生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成特定于组织的安全和任务需求的审计记录，就很难建立、关联和调查与某个事件相关的事件，或确定事件的责任人。

审计记录可以从信息系统中的各种组件（例如，模块或策略过滤器）生成。

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

### 17.55 KubeOS必须审计sudoers文件的所有使用以及“/etc/sudoers.d/”目录下的所有文件

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果审计记录不包含足够的信息，则无法重建有害事件或进行取证分析。

至少，组织必须审核特权访问命令的全文记录。组织必须维护足够详细的审计跟踪，以重构事件，以确定危害的原因和影响。

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

### 17.56 在KubeOS中成功/不成功使用“setfiles”必须生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果审计记录不包含足够的信息，则无法重建有害事件或进行取证分析。

至少，组织必须审计特权命令的全文记录。组织必须维护足够详细的审计跟踪，以重构事件，以确定危害的原因和影响。“setfiles”命令主要用于在一个或多个文件系统（或其中的一部分）上初始化安全上下文字段（扩展属性）。通常，它最初是作为SELinux安装过程的一部分（通常称为标记）运行的。

当用户登录时，AUID设置为正在进行身份验证的帐户的UID。守护进程不是用户会话，并且loginuid设置为“-1”。AUID表示是一个无符号的32位整数，等于“4294967295”。审计系统以相同的方式解释“-1”、“4294967295”和“unset”。

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

### 17.57 在KubeOS中使用“semanage”必须生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果审计记录不包含足够的信息，则无法重建有害事件或进行取证分析。

至少，组织必须审计特权命令的全文记录。组织必须维护足够详细的审计跟踪，以重构事件，以确定危害的原因和影响。“semanage”命令用于配置SELinux策略的某些元素，而不需要修改策略源或重新编译策略源。

当用户登录时，AUID设置为正在进行身份验证的帐户的UID。守护进程不是用户会话，并且loginuid设置为“-1”。AUID表示是一个无符号的32位整数，等于“4294967295”。审计系统以相同的方式解释“-1”、“4294967295”和“unset”。

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

### 17.58 在KubeOS中使用“setebool”必须生成审计记录。

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果审计记录不包含足够的信息，则无法重建有害事件或进行取证分析。

至少，组织必须审计特权命令的全文记录。组织必须维护足够详细的审计跟踪，以重构事件，以确定危害的原因和影响。“setebool”命令将特定SELinux布尔值或布尔值列表的当前状态设置为给定值。

当用户登录时，AUID被设置为正在进行身份验证的帐户的UID。守护进程不是用户会话，并且loginuid设置为“-1”。AUID表示是一个无符号的32位整数，等于“4294967295”。审计系统对“-1”、“4294967295”和“unset”的解释是相同的。

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

### 17.59 KubeOS必须为“/run/utmp文件”生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成特定于组织的安全和任务需求的审计记录，就很难建立、关联和调查与某个事件相关的事件，或确定事件的责任人。

审计记录可以从信息系统中的各种组件（例如，模块或策略过滤器）中生成。

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

### 17.60 KubeOS必须为“/var/log/btmp”文件生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成特定于组织的安全和任务需求的审计记录，就很难建立、关联和调查与某个事件相关的事件，或确定事件的责任人。

审计记录可以从信息系统中的各种组件（例如，模块或策略过滤器）生成。

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

### 17.61 KubeOS必须为“/var/log/wtmp”文件生成审计记录

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

如果不生成特定于组织的安全和任务需求的审计记录，就很难建立、关联和调查与某个事件相关的事件，或确定事件的责任人。

审计记录可以从信息系统中的各种组件（例如，模块或策略过滤器）生成。

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

### 17.62 KubeOS不能禁用syscall审计

**级别：** 要求（MEDIUM）

**适用版本：** ALL

**规则说明：**

默认情况下，KbeOS包含“-a task,never”审计规则作为默认的。此规则将禁止使用此规则启动的所有任务的系统调用审核。由于审计守护进程从上到下处理“audit.rules”文件，因此此规则取代了所有其他已定义的syscall规则；因此，操作系统上无法进行任何syscall审计。

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

### 17.63 KubeOS必须将所有帐户和/或帐户类型的并发会话数限制为10

**级别：** 建议（LOW）

**适用版本：** ALL

**规则说明：**

KubeOS管理包括控制使用KubeOS的用户和用户会话的数量的能力。限制每个用户允许的用户和会话的数量有助于降低与拒绝服务（DoS）攻击相关的风险。

此要求解决了信息系统帐户的并发会话，而不能解决单个用户通过多个系统帐户的并发会话。并发会话的最大数量应根据任务需求和每个系统的操作环境来定义。

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

### 17.64 KubeOS必须安装policycoreutils包

**级别：** 建议（LOW）

**适用版本：** ALL

**规则说明：**

如果没有对安全功能进行验证，安全功能可能无法正常运行，故障可能会被忽视。安全功能定义为信息系统的硬件、软件和/或固件，负责实施系统安全策略并支持保护所基于的代码和数据的隔离。安全功能包括但不限于建立系统帐户、配置访问授权（即权限、特权）、设置要审计的事件以及设置入侵检测参数。

Policycoreutils包含启用SELinux的系统的基本操作所需的策略核心实用程序。这些实用程序包括load_policy（用于加载SELinux策略）、setfile（用于标记文件系统）、newrole（用于切换角色）以及run_init（用于在适当的上下文中运行/etc/init.d脚本）。

**规则影响：**

无

**检查方法：**


```bash
执行如下命令，若该命令中包含"SELinux status"，则pass，否则fail
# sestatus -v
```

**修复方法：**


在镜像制作过程中安装policycoreutils

### 17.65 KubeOS审计事件多路复用器必须配置为使用Kerberos

**级别：** 建议（LOW）

**适用版本：** ALL

**规则说明：**

存储在一个位置的信息容易被意外或偶然的删除或更改。

允许设备和用户在不首先对其进行身份验证的情况下连接到系统或从系统连接到系统将导致不可信的访问，并可能导致危害或攻击。可能包含敏感数据的审计事件必须在传输之前加密。Kerberos提供了一种机制来为审计事件记录提供身份验证和加密。

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

### 18.1 KubeOS必须禁用x86 Ctrl-Alt-Delete键序列

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

在控制台按Ctrl-Alt-Delete组合键的本地登录用户可以重新引导系统。如果意外按下，就像在混合操作系统环境中可能发生的情况一样，这可能会造成由于意外重启而导致系统短期可用性丧失的风险。在图形用户界面环境中，Ctrl-Alt-Delete序列意外重启的风险会降低，因为在采取任何操作之前，会提示用户。

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

### 19.1 具有基本输入/输出系统(BIOS)的KubeOS在引导进入单用户和维护模式时必须要求身份验证

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

为降低已获得批准的PKI颁发证书的实体未经授权访问敏感信息的风险，所有系统（例如，Web服务器和Web门户）必须正确配置为包含访问控制方法，这些方法不完全依赖于拥有证书进行访问。成功的身份验证不能自动授予实体对资产或安全边界的访问权限。必须实施授权程序和控制，以确保每个经过身份验证的实体也具有经过验证的和当前的授权。授权是确定一个实体在通过身份验证后是否被允许访问特定资产的过程。信息系统使用访问控制策略和执行机制来实现这一要求。
访问控制策略包括基于身份的策略、基于角色的策略和基于属性的策略。访问实施机制包括访问控制列表、访问控制矩阵和密码学。应用程序必须采用这些策略和机制来控制信息系统中的用户（或代表用户行事的进程）与对象（例如，设备、文件、记录、进程、程序和域）之间的访问。

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

### 19.2 实现了统一可扩展固件接口(UEFI)的KubeOS必须在引导至单用户模式和维护时要求身份验证

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

如果系统允许用户在不进行身份验证的情况下引导至单用户或维护模式，则调用单用户或维护模式的任何用户都被授予对所有系统信息的特权访问权限。

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

### 20.1 KubeOS工具rpm必须开启gpgcheck。

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

对任何软件组件的更改都可能对KubeOS的整体安全性产生重大影响。此要求可确保软件未被篡改，且由可信供应商提供。

相应地，补丁、服务包、设备驱动程序或KubeOS组件必须使用组织认可和批准的证书进行签名。

在安装之前验证软件的真实性，可以验证从供应商处收到的补丁或升级的完整性。这可确保软件未被篡改，并且由受信任的供应商提供。此要求不允许使用自签名证书。

**规则影响：**

无

**检查方法：**


KubeOS无单包升级，无yum源，先和灵雀云确认升级方案

**修复方法：**


KubeOS无yum、单包升级场景，不涉及

## 21 telnet

### 21.1 KubeOS不能安装telnet-server包

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

KubeOS提供或默认安装超出需求或任务目标的功能是有害的。这些不必要的功能或服务经常会被忽视，因此可能会保持不安全。它们通过提供额外的攻击媒介增加了平台的风险。

KubeOS能够提供各种各样的功能和服务。默认提供的某些功能和服务可能不是支持基本组织运营（例如，关键任务和职能）所必需的。

非必要功能的示例包括但不限于与需求无关的软件包、工具和演示软件，或提供了并非每个任务都需要但不能禁用的广泛功能。

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

### 22.1 FIPS 140-2/140-3模式必须在KubeOS上启用

**级别：** 要求（HIGH）

**适用版本：** ALL

**规则说明：**

使用弱或未经测试的加密算法会破坏使用加密保护数据的目的。KubeOS必须实现符合联邦政府批准的更高标准的加密模块，因为这可以保证它们已经过测试和验证。

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
