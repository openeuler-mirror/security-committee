# openEuler安全设计指南
本规范主要参考业界广泛标准和开源最佳实践，结合openEuler业务发展趋势，制定出openEuler安全设计指南，用于指导开发者进行模块设计，避免出现高安全设计风险。

## 1. 身份认证&鉴权
1.1 所有跨网络传输的机机接口需要有接入认证机制， 且认证处理过程应放到服务端进行。  
**说明**：跨网络接口需要支持身份认证避免攻击者仿冒接入。  

1.2 应在服务器端对于每一个需要授权访问的请求核实用户身份是否被授权执行这个操作。  
**说明**：URL越权是典型的Web安全漏洞，利用该漏洞，攻击者可以很轻易地绕过系统的权限控制，未经授权地访问系统资源、使用系统的功能。为防止用户通过直接输入URL，越权请求并执行一些页面，在请求某一URL时，后台需对请求该URL的用户进行权限鉴定。  

1.3 在服务器端对所有来自不可信数据源进行大小、类型、长度、以及特殊字符等合法性校验，拒绝任何没有通过校验的数据。  
**说明**：避免攻击者通过代理拦截并篡改请求绕过客户端的合法性校验，所以需要在服务端进行数据校验。  

1.4 不应存在可绕过系统安全机制（认证、权限控制、日志记录）对系统或数据进行访问的功能。  
**说明**：若存在可绕过系统安全机制对系统或数据进行访问的功能，有可能被恶意人员知晓进行私自操作，从而给系统造成很大影响。  
**示例**：隐藏账号、隐藏口令、隐藏组合键访问方式、隐藏协议、隐藏的调试命令/端口等隐秘访问方式，用户不可管理的账号，这些均为反面的示例。openEuler默认为单用户模式要求使用密码登录，避免无密码获取root权限。  

1.5 根据权限最小化原则，运行软件程序的帐号要尽可能的使用操作系统低权限的帐号。  
**说明**：对于运行软件程序的操作系统帐号，不应使用“root”、“administrator”、“supervisor”等特权帐号或高级别权限帐号，应该使用普通权限的账号运行。  

## 2. 安全传输
2.1 在跨网络之间进行敏感数据或关键业务数据传输应采用安全传输协议或者加密后传输。  
**说明**：在跨网络进行传输时，容易遭受攻击者窃取或篡改，需要对重要数据进行保护。  
**示例**：iSulad项目中用户名密码加密后优先采用HTTPS传输。  

2.2 不应使用SSL2.0、SSL3.0、TLS1.0、TLS1.1协议进行安全传输，推荐使用更安全协议TLS1.2和TLS1.3。  
**说明**：SSL2.0和SSL3.0协议因存在很多安全问题已分别于2011年3月和2015年6月被IETF禁用。TLS1.0协议中对称加密算法仅支持RC4算法和分组加密算法的CBC模式，RC4已经公认不安全并被IETF在TLS所有版本协议中禁用，而对称算法的CBC模式存在IV可预测的问题，从而容易受到BEAST攻击。因此，推荐使用TLS1.2和TLS1.3协议。  

## 3. 敏感/隐私数据保护
3.1 认证凭据(如口令、密钥等)不允许明文存储在系统中，应该加密保护，在不需要还原明文的场景，应使用安全的不可逆算法加密，如果需要还原，则可以使用安全的对称加密算法和安全的加密模式加密。  
**示例**：iSulad项目使用AES256-CFB来加密保存用户名密码。openEuler默认设置使用SHA-512来加密用户口令。  

## 4. 加密算法&密钥管理
4.1 不应使用私有的、非标准的密码算法。  
**说明**：私有的、非标准的密码算法不应该被使用。一方面，非密码学专业人员设计的密码算法，其安全强度难以保证。另一方面，这类算法在技术上也未经业界分析验证，有可能存在未知的缺陷，并且这样的设计违背公开透明的安全设计原则，推荐使用业界常用的开源密码组件，如OpenSSL、OpenSSH等。  

4.2 不应使用不安全的密码算法，推荐使用强密码算法。  
**说明**：随着密码技术的发展以及计算能力的提升，一些密码算法已不再适合现今的安全领域，如不安全对称加密算法DES、RC4等，不安全非对称加密算法RSA 1024等，不安全哈希算法SHA-0、SHA-1、MD2、MD4、MD5等，不安全密钥协商DH-1024等，推荐使用AES-256、RSA-3072、SHA-256、PBKDF2或更强密码算法。  
**示例**：OpenSSH服务端设置的缺省数据传输加密算法为：AES128-CTR, AES192-CTR, AES256-CTR, CHACHA20-POLY1305, AES128-GCM, AES256-GCM。  

4.3 用于对数据进行加密的密钥不能在代码中硬编码。  
**说明**：密钥要支持替换，避免长期使用增加被泄露风险。  
**示例**：iSulad项目使用AES256时密钥可通过密钥文件配置更改。  

4.4 密码算法中使用到的随机数应是密码学意义上的安全随机数。  
**说明**：随机数用于生成IV、盐值、密钥等，都属于密码算法用途。不安全的随机数使得密钥、IV等可被全部或部分预测。密码学意义上的安全随机数，要求必须保证其不可预测性。安全随机数应通过安全的随机数发生器来产生。例如可以使用：OpenSSL库的RAND_bytes、Linux操作系统的/dev/random设备接口、TPM中的随机数产生器（RNG）模块。  