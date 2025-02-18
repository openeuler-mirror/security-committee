# 常见问题

### 1 “Fixed”指什么？

答：“Fixed”表示openEuler安全小组分析评估后确认该漏洞影响到了openEuler产品，相应的修复已经发布更新。

### 2 “Affected”指什么？

答：“Affected”表示经openEuler安全小组分析评估后确认该漏洞影响到了openEuler产品，正在努力准备相应的修复更新中。

##### 2.1 “Vulnerabilities are still analyzing”指什么？

答：“Vulnerabilities are still analyzing”表示openEuler安全小组分析评估后确认该漏洞影响到了openEuler产品，正在努力准备相应的修复方案。

##### 2.2 “No solution or patch”指什么？

答：“No solution or patch”表示openEuler安全小组分析评估后确认该漏洞影响到了openEuler产品，目前暂时没有相应的修复方案或补丁。

##### 2.3 “To be fixed through an upgraded version”指什么？

答：“To be fixed through an upgraded version”表示openEuler安全小组分析评估后确认该漏洞影响到了openEuler产品，目前等待版本升级后修复。

##### 2.4 “Out of support scope”指什么？

答：“超出范围”是指该漏洞涉及的软件包或对应的影响的版本不在openEuler产品服务支持范围。建议依据openEuler对该漏洞的影响评估，采用可能的缓解方案或升级到有支持的软件包或版本。

##### 2.5 为什么“Will not Fix”？

答：虽然该漏洞影响到了openEuler产品，但经openEuler安全小组分析评估后，决定暂不修复该漏洞。
我们建议：
• 优先考虑升级你的产品；升级到已经包括该漏洞修复的，或者不受该漏洞影响的openEuler产品。
• 如果该漏洞有缓解方案，可以尝试使用缓解方案。
• 向我们反馈你的诉求，请说明需要修复的理由。

### 3 为什么“Unaffected”？

答：“Unaffected”表示经openEuler安全小组分析评估，确认该漏洞不影响openEuler产品，无需修复更新。

##### 3.1 “Component_not_present”指什么？

答：“Component_not_present”表示openEuler安全小组分析评估后确认该漏洞对openEuler产品没有影响，openEuler相关产品中不存在漏洞影响的相关组件。

##### 3.2 “Inline_mitigations_already_exist”指什么？

答：“Inline_mitigations_already_exist”表示openEuler安全小组分析评估后确认该漏洞对openEuler产品没有影响，openEuler相关产品中已有内置的内联控制或缓解措施。

##### 3.3 “Vulnerable_code_cannot_be_controlled_by_adversary”指什么？

答：“Vulnerable_code_cannot_be_controlled_by_adversary”表示openEuler安全小组分析评估后确认该漏洞对openEuler产品没有影响，openEuler相关产品中漏洞代码不能被攻击者触发。

##### 3.4 “Vulnerable_code_not_in_execute_path”指什么？

答：“Vulnerable_code_not_in_execute_path”表示openEuler安全小组分析评估后确认该漏洞对openEuler产品没有影响，openEuler相关产品中漏洞代码不在执行路径。

##### 3.5 “Vulnerable_code_not_present”指什么？

答：“Vulnerable_code_not_present”表示openEuler安全小组分析评估后确认该漏洞对openEuler产品没有影响，openEuler相关产品中漏洞代码不存在。

### 4 “Under Investigation”指什么？

答：“Under Investigation”表示openEuler安全小组正在努力分析评估该漏洞对openEuler产品的影响，暂时还没有确认该漏洞是否影响到了openEuler相关产品。

### 5 什么是CVSS评分？为什么有些CVE openEuler评分与NVD评分不同？

答：CVSS是通用漏洞打分系统的简称，是由CVSS-SIG组织发布与更新的CVE评分标准。openEuler参考的是CVSSv3.1版本。CVSS只是一个通用的漏洞评价体系，不同机构、厂商或社区有不同的业务安全视角，因此对同一个CVE评估分数可能有出入。