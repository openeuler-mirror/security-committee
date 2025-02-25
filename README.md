# 安全委员会

[English](docs\en\charter\README-EN) | 简体中文

本文档主要介绍安全委员会的职责、组织构成和运作方式，以及负责的相关流程。



## 安全委员会（SC）使命

openEuler安全委员会（SC）负责接收和响应openEuler相关的安全问题报告、提供社区安全指导，开展社区安全治理，具体职责和要求参见[openEuler社区安全保障策略总纲](security-strategy-overview.md)。安全委员会（SC）的使命：为openEuler用户提供最安全的产品和开发环境。



## 工作职责

+ 协助漏洞修复：确保及时修复已知漏洞。通过为软件包Maintainer们提供补丁帮助，帮助用户系统在成为攻击受害者之前进行漏洞修复，包括提供相关漏洞检测和修复工具。
+ 响应安全问题：响应上报的安全问题，跟踪安全问题的处理进展，并遵循安全问题披露策略对安全问题在社区内进行披露和公告。
+ 安全编码规则：普及安全编码知识是安全团队的目标。安全团队会努力创建文档或开发工具来帮助开发团队避免软件开发过程中的常见陷阱。安全团队还会尝试回答在开发和使用过程中遇到的任何问题。
+ 参与代码审核：安全团队希望能够通过代码审核帮助团队提前发现代码中的漏洞。



## 成员

安全委员会成员列表与职责在[MEMBERS](MEMBERS)里持续维护

## 会议时间

- 每双周三下午4:00~5:30，通过weLink视频会议召开



### 联系SC

SC和产品发布相关，所以和发布经理有很多工作关联，SC会负责让产品安全的发布。请参考使用正确的联系方式以获得最佳和最快的响应。

| 清单或群组                             | 类型    | 用途                                                         |
| -------------------------------------- | ------- | ------------------------------------------------------------ |
| openeuler-security@openeuler.org       | Private | openEuler安全披露邮箱。此列表由PSC密切监控和分类。详细信息请参考[安全披露指南](docs\zh\vulnerability-management-process\security-disclosure) |
| release-managers-private@openeuler.org | Private | 发布经理的私人交流邮件，所有成员都应订阅openeuler-security@openeuler.org。发布经理在发布过程中讨论安全问题的处理应使用该邮箱 |
| security-discuss-private@openeuler.org | Private | SC的私有内部讨论邮件，所有成员都需订阅openeuler-security@openeuler.org |



### 安全发布流程

关于如何上报安全问题，如何获取安全补丁等安全相关事宜，请参考[安全披露指南](docs\zh\vulnerability-management-process\security-disclosure)

了解openEuler社区的安全处理流程和安全策略，请参考[安全处理流程](docs\zh\vulnerability-management-process\security-process)



## 行为规范

+ 接受[openEuler行为规范](https://gitee.com/openeuler/community/blob/master/code-of-conduct.md)的约束。
+ 接受[openEuler安全委员会成员行为准则](docs\zh\charter\security-committee-rules.md)的约束。


## 仓库结构说明

openEuler安全委员会仓库通过以下结构分类维护仓库文件，提交代码时请参考以下约定将文件置于合适的地方，若涉及新增文件夹，需在```index.md```索引文件里添加基本描述。

```
security-committee
 ┣ assets
 ┣ docs
 ┣ sub-projects
 ┣ MEMBERS.md
 ┣ README.md
 ┗ security-strategy-overview.md
 ```

 + ```assets```：存放非工程文件的资源文件，包括文档使用的图片及安全委员会成员公钥文件等。
 + ```docs```：存放介绍社区安全流程的文档，包括社区章程，漏洞管理，漏洞上报流程等，通过中英文分类管理。
 + ```sub-projects```：存放通过安全委员会运作/孵化的子项目。若项目涉及存放工程文件或SIG化运作，推荐存放在```sub-projects```。若项目为纯文档构成，推荐存放在```docs```。
 + ```root```：安全委员会根目录，存放Readme，社区安全政策，安全委员会成员清单等基础信息，便于开发者掌握全景。

 ### index索引文件

 仓库在```assets```, ```docs```等次级文件夹root维护索引文件```index.md```，描述当前文件夹里的存放的文件信息