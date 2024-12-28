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
```


## 4、Linux 系统中 FTP 服务安全防护 
创建 test 普通用户，然后给其设置一个的密码，并修改该登录 shell 为/bin/sh；最后将该用户禁用
```
sudo useradd test
sudo passwd test
sudo usermod -s /bin/sh test
sudo usermod -L test
```
修改/etc/vstpd.conf 配置文件，关闭匿名用户，
修改/etc/vstpd.conf 配置文件，限制本地用户只能在自己的家目录中活动，
修改/etc/vstpd.conf 配置文件，设置每个 IP 的最大客户端连接数目为 5，
```
anonymous_enable=NO
local_enable=YES
chroot_local_user=YES
max_per_ip=5
```
修改/etc/vstpd.conf 配置文件，设置服务监听端口为2121，设置客户端同时最大连接数为 100，设置匿名用户的家目录为/ftphome，禁止所有用户进行写操作。 
```
listen_port=2121
max_clients=100
anon_root=/ftphome
write_enable=NO
```

## 5、url-rot13-base64 解密算法 
密码学用于解决信息安全中的保密性，完整性，认证和不可否认性等问题。最初主要用于解决保密性。随着密码学技术的发展，逐渐应用到其它领域。常见的密码算法有很多，下 
面按照要求对“VmFzYmV6bmd2YmElMjBGcnBoZXZnbCUyME5xenZhdmZnZW5nYmU=”进行解密。（没有特别说明加密算法采用默认设置）
```
1、打开cyberchef解码工具（或其他解码工具）https://cyberchef.org/
2、将密文VmFzYmV6bmd2YmElMjBGcnBoZXZnbCUyME5xenZhdmZnZW5nYmU=
复制到Input输入框中。
3、然后在左侧依次搜索 From base64、ROT13、URL Decode，然后拖动到中间位置
4、出现答案后，复制答案到提交框里提交。（注意看答案提交要求，可能会要求加flag{xxx}，或者把空格换成下划线等）
```


## 6、取证技术应用（重复） 
（1）登入虚拟机（0f3f0c_网络与信息安全管理员高级工级_Win7_x64），双击“c:\tools\ 
开启蜜罐.bat”，然后打开蜜罐设备管理页（https://miguan.test:4433/web/，账号密码： 
admin/HFish2021），请描述：1）攻击最频繁的攻击者 ip；2）列取被攻击的蜜罐服务；3） 
攻击者最常用的密码。 
（2）利用电子取证技术恢复虚拟磁盘（c:\files\test.vhd）唯一分区中被恶意删除的机密 
文件，并获取机密文件内容。 

## 7、Linux 系统中 FTP 服务安全防护（重复）
创建 test 普通用户，然后给其设置一个的密码，并修改该登录 shell 为/bin/sh；最后 
将该用户禁用，修改/etc/vstpd.conf 配置文件，关闭匿名用户，修改/etc/vstpd.conf 配 
置文件，限制本地用户只能在自己的家目录中活动，修改/etc/vstpd.conf 配置文件，设置 
每个 IP 的最大客户端连接数目为 5，修改/etc/vstpd.conf 配置文件，设置服务监听端口为 
2121，设置客户端同时最大连接数为 100，设置匿名用户的家目录为/ftphome，禁止所有用 
户进行写操作。 

## 8、url-rot13-base64 解密算法（重复）
密码学用于解决信息安全中的保密性，完整性，认证和不可否认性等问题。最初主要用 
于解决保密性。随着密码学技术的发展，逐渐应用到其它领域。常见的密码算法有很多，下 
面按照要求对 
“VmFzYmV6bmd2YmElMjBGcnBoZXZnbCUyME5xenZhdmZnZW5nYmU=”进行解密。（没有特别说明 
加密算法采用默认设置） 


## 9、数据安全管理 
启动虚拟机。该虚拟机安装了 MySQL 数据库（MySQL 绑定地址为 192.168.101.103，服 
务端口为 3306，用户名/密码为 root/root123），登录后请完成以下操作： 
### (1) 查看 DATABASE idatabase 中的 customer 表，写出表中有多少条记录并对查询过程进行截图。
``` 
select count(*) from idatabase.customer;
```

### (2) 查看上述 customer 表中按 name、idnum 去重后涉及多少条记录，写出结果并对查询过程进行截图。 
```
select count(*) from (select distinct name,idnum  from idatabase.customer)a;
```

### (3) 将 customer 表中的 idnum 替换为 MD5 值，写出操作命令，并对替换前、后的表数据进行截图（截图中替换前后的 idnum 要能对应上）。 
```
alter table oc_config modify userid varchar(256);
update customer set idnum=md5(idnum);
```

### (4) 将 customer 表中 phone 的第 4-7 位（从左边开始数）用“*”替换，写出操作命令， 并对替换前、后的表数据进行截图（截图中替换前后的 phone 要能对应上）。
```
select concat(left(`key`,3),'****',substr(`key`,8)), `key` from oc_config;
update customer set phone=concat(left(`key`,3),'****',substr(`key`,8)); 
ALTER TABLE table_name ADD COLUMN column_name data_type;
ALTER TABLE table_name DROP COLUMN column_name;
```


## 10、数据安全处置
请以管理员账户登录当前 Windows 系统，并按顺序执行以下操作： 
①将 Sysmon.7z 解压（解压密码为"3 级 SysMon",压缩包里有帮助文档“Sysmon 帮助文 
档.pdf”和 Sysmon 命令）。以管理员权限执行 Sysmon 命令，依次进行安装（Install）、更 
新（Update configuration）为默认配置，并对命令和执行情况进行截图。
sysmon -i
sysmon -c 

②打开 pdf 文档。在“事件查看器-应用程序和服务日志-Microsoft-Windows-Sysmon” 
（以下简称“事件日志”）中，找到打开日志并截图，同时写出该日志 Event ID。 

③关闭 pdf 文档。在事件日志中，找到关闭日志并截图，同时写出该日志 Event ID。 

④通过 Sysmon 命令更新配置（配置文件为压缩包中的“Q4.xml”）。配置更新后，在“C:/test” 
目录下创建、删除“OK.txt”文件。在事件日志中，找到创建、删除日志并截图（截图中应 
体现创建和删除的文件名）。 

## 11、Windows本地安全防护
禁止系统在未登录的情况下关闭：
在组策略编辑器（gpedit.msc）中，依次展开 “计算机配置” - “windows设置” - “安全设置” - “本地策略” - “安全选项”，双击 “关机：允许系统在未登录的情况下关闭”，选择 “已禁用”。

不显示最后登录的用户名：
在组策略编辑器中，依次展开 “计算机配置” - “Windows 设置” - “安全设置” - “本地策略” - “安全选项”，找到 “交互式登录：不显示最后的用户名”，设置为 “已启用”。

不允许 SAM 账户的匿名枚举：
在组策略编辑器中，同上，找到 “网络访问：不允许 SAM 账户的匿名枚举”，设置为 “已启用”。

设置只有 Administrators 组用户才能从网络访问此计算机：
在组策略编辑器中，同上，找到 “网络访问：本地账户的共享和安全模型”，设置为 “仅来宾 - 对本地用户进行身份验证，其身份为来宾”，然后在 “用户权限分配” 中找到 “从网络访问此计算机”，只保留 Administrators 组。

禁止从远程系统强制关闭计算机：
在组策略编辑器中，依次展开 “计算机配置” - “Windows 设置” - “安全设置” - “本地策略” - “用户权限分配”，找到 “从远程系统强制关机”删除所有用户组。

禁止将未加密的密码发送到第三方的 SMB 服务器：
在组策略编辑器中，依次展开 “计算机配置” - “Windows 设置” - “安全设置” - “本地策略” - “安全选项”，找到“Microsoft网络客户端：将未加密的密码发送到第三方SMB 服务器”，设置为 “已禁用”。

禁止软盘复制并访问所有的驱动器和所有文件夹：
在组策略编辑器中，同上，找到“恢复控制台：允许软盘复制并访问所有的驱动器和所有文件夹”，设置为 “已禁用”。

设置只有 Administrators 用户组才能关闭系统：
在组策略编辑器中，找到 “计算机配置” - “Windows 设置” - “安全设置” - “本地策略” - “用户权限分配” - “关闭系统”，只保留 Administrators 组。

设置用户在登录系统时应该有 “Hello,World!!!” 的提示信息：
在组策略编辑器中，依次展开 “计算机配置” - “Windows 设置” - “安全设置” - “本地策略” - “安全选项”，找到“交互式登录：试图登录的用户的消息标题”、“交互式登录：试图登录的用户的消息文本”，设置为“Hello,World!!!”  （win7测试有一个值为空都不会显示）

设置远程用户非活动会话连接超时为 5 分钟：
在组策略编辑器中，找到 “计算机配置” - “管理模板” - “Windows 组件” - “远程桌面服务” - “远程桌面会话主机” - “会话时间限制”，找到”设置活动但空闲的远程桌面服务会话的时间限制“设置相应的超时时间。

删除可远程访问的注册表路径：
在组策略编辑器中，找到 “计算机配置” - “Windows 设置” - “安全设置” - “本地策略” - “安全选项”，找到“网络访问：可远程访问的注册表路径”删除所有路径，下一条子路径最好也删除。


## 12、selinux 权限管理 
通过 selinux 设置文件权限，限制 web 访问，
在/var/www/html 下，新建 index.html、index2.html， 
使 index.html 可以通过 web 访问，index2.html 不可以通过 web 访问。

sestatus

### 查看文件的 SELinux 属性的方法
使用 ls -Z 命令
```
#getenforce
[root@localhost html]# getenforce
Enforcing
```
编辑/etc/selinux/config文件，将SELINUX=enforcing，然后重启系统使设置生效。

### 设置文件的上下文类型
在 SELinux 中，文件的访问权限不仅取决于传统的文件权限位，还取决于文件的上下文类型。对于/var/www/html目录下的文件，默认的上下文类型通常为httpd_sys_content_t，允许被 HTTP 服务访问。
对于index.html文件，保持其默认的上下文类型即可，确保它可以被正常访问。
对于index2.html文件，需要修改其上下文类型，使其不能被 HTTP 服务访问。可以使用命令
```
chcon -t public_content_t /var/www/html/index2.html
```
或者 default_t
，将其上下文类型更改为public_content_t，该类型通常不允许被 HTTP 服务访问。

可选：持久化上下文类型设置
上述使用chcon命令修改的文件上下文类型在系统重启后可能会恢复默认值。如果希望设置在重启后仍然有效，可以使用命令
semanage fcontext -a -t public_content_t /var/www/html/index2.html
，将修改后的上下文类型持久化到 SELinux 的策略中。
semanage fcontext -a -t httpd_sys_content_t "/var/www/html/index.html"
semanage fcontext -a -t default_t "/var/www/html/index2.html"
restorecon -Rv /var/www/html/
ls -Z
需要全路径，执行完之后再ls -Z
