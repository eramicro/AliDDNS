安装必需组件：

For CentOS：
```bash
yum install -y wget curl cronie
```
For Debian 8+：
```bash
apt install -y wget curl cron
```
For Ubuntu/Debian 7：
```bash
apt-get install -y wget curl cron
```
然后下载AliDDNS脚本到你的服务器上：
```bash
wget -O /usr/sbin/aliddnsv2.sh https://raw.githubusercontent.com/eramicro/AliDDNS/master/aliddnsv2.sh
```
为脚本文件加上可执行属性：
```bash
chmod +x /usr/sbin/aliddnsv2.sh
```
执行脚本，开始配置：
```bash
/usr/sbin/aliddnsv2.sh
```
弹出启动菜单：

AliDDNS 工具 (阿里云云解析修改工具)

使用方法 (Usage)：
AliDDNS.sh run 配置并运行工具 (如果已有配置将会直接运行)
AliDDNS.sh config 仅配置工具
AliDDNS.sh clean 清理配置文件及运行环境
AliDDNS.sh version 显示版本信息

[Info] 选择你要使用的功能:

配置并运行 AliDDNS
仅配置 AliDDNS
清理环境
退出
输入数字以选择: _

在这里，我们输入 1 (数字1) ，后按下回车，开始进入AliDDNS配置向导：

[Info] 请输入一级域名 (比如 example.com)
(此项必须填写，查看帮助请输入“h”):
假如你需要设置AliDDNS的域名为ddns.example.com，那么请在这里输入 example.com

分解开就是 [ddns] . [example.com]

同时，登录阿里云云解析 https://dns.console.aliyun.com/，在需要DDNS的域名上，添加一个记录：

记录类型：A
主机记录：[请填写你的二级域名]
解析线路：默认
记录值：127.0.0.1 (或者随便填写一个IP地址)
TTL： [请根据实际需要选择合适的TTL]
同步默认线路：是 (勾选)
简单粗暴的，看都不看的复制粘贴，作者也有权拒绝回答任何问题！
完成后按下回车键，继续填写二级域名：

* [Info] 请输入二级域名 (比如 ddns)
(此项必须填写，查看帮助请输入“h”):
同上面的范例，我们输入 ddns ，之后按下回车键继续：

* [Info] 请输入记录的TTL(Time-To-Live)值：
(默认为600，查看帮助请输入“h”):

填写完成后，按下回车键继续：

* [Info] 请输入阿里云AccessKey ID
(此项必须填写，查看帮助请输入“h”):
AccessKey ID 和 AccessKey Secret 推荐使用 子用户AccessKey(访问控制台RAM) 分配的权限！这样最安全！

使用子用户AccessKey，请分配 AliyunDNSReadOnlyAccess(只读访问云解析(DNS)的权限) 和 AliyunDNSFullAccess(管理云解析(DNS)的权限) 这两个权限！

填写完成后，按下回车键继续：

* [Info] 请输入阿里云Access Key Secret
(此项必须填写，查看帮助请输入“h”):
同上，填写你的AccessKey ID对应的AccessKey Secret。获取你的AccessKey Secret属于账号高风险操作，请准备好用来接收阿里云验证码的手机！

填写完成后，新版的AliDDNS 2.0如果没有激活专家模式，会直接进入执行流程；如果启动了专家模式，以下参数请在你理解的基础上填写！否则请一律留空！

* [Info] 请输入获取本机IP使用的命令
(查看帮助请输入“h”):
输入获取本机IP地址使用的命令。如果你不懂或者不需要配置，请留空，直接回车！

* [Info] 请输入解析使用的DNS服务器
(此项必须填写，查看帮助请输入“h”):
输入nslookup命令解析使用的DNS服务器。如果你不懂或者不需要配置，请留空，直接回车！

之后，会自动开始DDNS(测试)运行过程：

* [Info] 检测到存在的配置，自动读取现有配置
如果你不需要，请通过菜单中的清理环境选项进行清除
[Info] 正在写入配置文件……
[Info] 正在获取本机IP……
[Info] 本机IP：...
[Info] 正在获取 ddns.example.com 的IP……
[Info] 解析结果：ddns.example.com -> 127.0.0.1
[Info] 正在生成时间戳……
[Info] 获取到RecordID：*
[Info] 正在更新解析记录……
{"RecordId":"","RequestId":"----"}
[Info] 已经更新RecordID：*
[Success] DDNS记录更新成功，新的IP为：...

出现最后的 DDNS记录更新成功 提示，即为DDNS记录同步成功，稍后等待DNS解析生效，即可完成DDNS域名更换！

###  定时任务
```bash
vi /etc/crotab
```
```bash
*/5 * * * * /usr/sbin/aliddnsv2.sh run >/dev/null 2>&1 &
```
添加完成后，保存退出。

