# L3

## 1、数据安全防护 
### (1) 请计算字符串“This is aHMACtest.”的 HMAC 消息认证码值（输入为双引号中内容， 不含双引号，下同），密钥为“12345678”（UTF8 编码），哈希算法采用 MD5，并对该操作步骤和工具中参数配置情况进行截图。

工具：Cyberchef

HMAC 
key: 12345678    UTF-8    
hashing function: MD5

### (2) 请计算字符串“This is a TCP/IP Checksumalgorithm.”的 TCP/IP 校验和，并对该操作步骤和工具中参数配置情况进行截图。

工具：Cyberchef

TCP/IP Checksum

### (3) 请计算字符串“CRC-32 is a Checksum algorithm.”的 CRC-32 校验和。并对该操作步骤和工具中参数配置情况进行截图。 

工具：Cyberchef

CRC-32 Checksum 
 
### (4) 某同学在 Linux 环境中用以下命令计算上述步骤的 CRC-32 校验和。请分析并写出该方法的问题。 
echo "CRC-32 is a Checksum algorithm." >> 1.txt 
cksum 1.txt 
算法差异？输出格式？文件追加？有换行？
$ echo "CRC-32 is a Checksum algorithm." >> 1.txt
$ cksum 1.txt
2891329521 32 1.txt

命令选择：
cksum 使用的是基于 CRC 的算法，但并不是标准的 CRC-32 算法。它实现的是一个 32-bit 的循环冗余检查（CRC），但它使用的多项式和初始化值可能与标准的 CRC-32 不同。如果目标是计算符合通信协议或数据传输标准的 CRC-32 校验和，那么应该使用特定于 CRC-32 的工具或库。
输出格式：
cksum 输出的结果包括三个部分：校验和、字节数以及文件名。这可能不是用户想要的输出格式，特别是当他们只需要 CRC-32 值时。
换行符的影响：
echo 默认会在字符串末尾添加一个换行符 (\n)。这意味着即使对于相同的输入文本，不同平台上的 echo 行为可能会导致不同的输出结果（例如 Windows 和 Linux 的换行符不同）。这会改变文件的内容，进而影响 CRC-32 的计算结果。
环境差异：
如果是在脚本中使用此方法，并且脚本需要在多个环境中运行（比如不同版本的 Unix/Linux 或者跨平台），那么 cksum 实现的细节差异可能会导致不一致的结果。


## 2、ssh 登录用户权限管理
对通过 ssh 远程登录用户 tt，限制其只能执行 lsof 命令，tt 用户在本地登录不受此限制。 
**注意vi打开文件编辑，插入数据前需要输入: i**
**退出保存，先按esc键，然后 :wq 回车即可，下同**
sudo vi /etc/ssh/sshd_config
添加：
```
Match User tt
ForceCommand lsof
    AllowTcpForwarding no
```
执行：
sudo service sshd restart


## 3、取证技术应用 
### (1) 登入虚拟机（0f3f0c_网络与信息安全管理员高级工级_Win7_x64），双击“c:\tools\开启蜜罐.bat”，然后打开蜜罐设备管理页（https://miguan.test:4433/web/，账号密码： 
admin/HFish2021），请描述：1）攻击最频繁的攻击者ip；2）列取被攻击的蜜罐服务；3）攻击者最常用的密码。 
```
1、在浏览器访问https://miguan.test:4433/web/
2、输入账户密码admin/HFish2021
3、打开左侧威胁实体-攻击来源，查看“攻击总次数”最多的IP
4、列取被攻击的蜜罐服务。打开左侧威胁感知-攻击列表，即可查看
5、攻击者最常用的密码。打开左侧威胁实体-账号资源。即可查看常用密码
```
### (2) 利用电子取证技术恢复虚拟磁盘（c:\files\test.vhd）唯一分区中被恶意删除的机密文件，并获取机密文件内容。 
```
使用DiskGenius打开，选择磁盘—打开虚拟磁盘文件，点击恢复文件，注意要有恢复已删除文件，点击开始
文件夹前有垃圾桶的就是之前删除的文件
