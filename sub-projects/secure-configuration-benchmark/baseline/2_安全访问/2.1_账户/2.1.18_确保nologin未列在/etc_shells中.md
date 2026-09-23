### 2.1.18 确保nologin未列在/etc/shells中

**级别：** 建议

**适用版本：** 全部

**规则说明：**

/etc/shells文件列出了系统认可的有效登录shell。/sbin/nologin和/bin/false等shell的功能是拒绝用户登录，并非真正的交互式shell，不应列在/etc/shells中。如果nologin被列在/etc/shells中，某些使用/etc/shells作为有效shell判断依据的程序可能会产生误判，允许不应登录的用户通过某些认证方式登录。

**规则影响：**

无

**检查方法：**

使用如下命令检查/etc/shells中是否包含nologin或false，以下命令应无输出：

```bash
# grep -E "nologin|false" /etc/shells
```

**修复方法：**

编辑/etc/shells文件，删除包含nologin或false的行：

```bash
# vim /etc/shells
```
