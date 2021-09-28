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
* 请输入一级域名 :example.com（一定要登录阿里云云解析添加一个记录，脚本才能正确运行）
* 请输入二级域名 ：www
* 请输入记录的TTL(Time-To-Live)值：600
* 请输入阿里云AccessKey ID
* 请输入阿里云Access Key Secret

填写完成后，新版的AliDDNS 2.0如果没有激活专家模式，会直接进入执行流程;

###  定时任务
```bash
vi /etc/crotab
```
```bash
*/5 * * * * /usr/sbin/aliddnsv2.sh run >/dev/null 2>&1 &
```
添加完成后，保存退出。

