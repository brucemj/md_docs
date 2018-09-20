
### 命令行升级
```
# 查看状态
android-sdk-linux/tools/android list sdk
# 升级选定 Packages
android-sdk-linux/tools/android update sdk --no-ui --filter 1,2,3
```

### 自动升级，自动同意license
[如何纯命令行进行andorid SDK的更新及自动确认license的方法](http://blog.csdn.net/yygydjkthh/article/details/48263973)
```
# 自动进行license确认的脚本如下：

#!/usr/bin/expect
set timeout -1
spawn /usr/local/android-sdk-linux/tools/android update sdk -u -a -t 1,2,3,24,25,26,27,28,30,95,96,102,103,104,105,106,107
expect {
    "Do you accept the license" { exp_send "y\r" ; exp_continue }
    eof
}

```
