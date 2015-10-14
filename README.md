# my-environments
我的桌面环境配置以及问题解决方案

# ubuntu unity dash 编辑
Install [Alacarte](apt:alacarte)

- /usr/share/applications/下的desktop文件
- /usr/share/applications/mimeinfo.cache文件
- ~/.local/share/applications下的desktop文件
- ~/.local/share/applications/default.list目录下的文件
- ~/.local/share/applications/mimeinfo.cache文件
- /etc/gnome/defaults.list文件

-----
＃ proftpd 无法启动

检查配置

May be you have error in your proftpd config file. You have to perform configuration check. The easiest way to do informative syntax check as is stated here is to run:
`proftpd -td10`
or you can run (less noise):
`proftpd -td5`

---

#PPTP 无法连接

提示：*/usr/sbin/pppd: Plugin /usr/lib/pptpd/pptpd-logwtmp.so is for pppd version 2.4.5, this is 2.4.6* ，应当是版本不对。搜索后提示，将 * /etc/pptpd.conf* 中的 *logwtmp* 注释掉，但是文件中没有发现这个配置选项。好吧其实有的，就在第二行，我没有注意到。
   - 报错 *No CHAP secret found for authenticating 用户名* ，发现在 */etc/ppp/* 中改错了密码文件，修正后可以连接
   - 然后目前无法上网，应该是 ubuntu 中 ufw 管理 iptables 写入的众多规则导致不能进行转发？尝试了众多配置，仍然不行，原因是对 iptables 不熟悉，在瞎改。学习下配置方法：也许要摒弃 ufw ？ [Google Search: iptables + ufw](https://www.google.com.hk/#safe=strict&q=iptables++ufw)
   - 看了一些资料，然后把 ufw / firewall 相关的 netfilter 管理工具全部卸载了，于是 iptables 就干净了，然后执行 pptp 一键安装脚本里面的 iptables 相关部分
```shell
#ubuntu has exit 0 at the end of the file.
sed -i '/^exit 0/d' /etc/rc.local

cat >> /etc/rc.local << END
echo 1 > /proc/sys/net/ipv4/ip_forward
#control channel
iptables -I INPUT -p tcp --dport 1723 -j ACCEPT
#gre tunnel protocol
iptables -I INPUT  --protocol 47 -j ACCEPT

iptables -t nat -A POSTROUTING -s 192.168.2.0/24 -d 0.0.0.0/0 -o eth0 -j MASQUERADE

#supposedly makes the vpn work better
iptables -I FORWARD -s 192.168.2.0/24 -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -j TCPMSS --set-mss 1356

END
sh /etc/rc.local
```
# Ingress
使用 wiresharks 进行抓包，不用 root 权限：` sudo setcap 'CAP_NET_RAW+eip CAP_NET_ADMIN+eip' /usr/bin/dumpcap`
非特权用户抓包 
- [tutorial](http://packetlife.net/blog/2010/mar/19/sniffing-wireshark-non-root-user/)
- [tutorial](https://blog.wireshark.org/2010/02/running-wireshark-as-you/)
- [tutorial](http://superuser.com/questions/319865/how-to-set-up-wireshark-to-run-without-root-on-debian)
```shell

# one way
$ sudo -s
# groupadd -g wireshark
# usermod -a -G wireshark gerald
# chgrp wireshark /usr/bin/dumpcap
# chmod 4750 /usr/bin/dumpcap

# add a user to a group
sudo usermod -a -G groupName userName
```
iptables 添加路由
```shell
sudo iptables -t nat -A PREROUTING -d 173.194.72.84  -j DNAT --to-destination 103.7.200.79
sudo iptables -t nat -A POSTROUTING -d 103.7.200.79 -j MASQUERADE
```

这个问题没有解决，另外除了收到 retransmission 包，还碰到了 GFW 的蜜罐 IP，有趣。
---

# iptables
看了一些文章，大体了解了其工作流程
- view input chain: `iptables -nvx -L INPUT`
- 

# Atom Editor
文本显示：中文乱码
可以修改 style.css
Atom China 网站上有个解决方案：[Linux 下中文显示成方块的解决方案](http://atom-china.org/t/linux/85)
另外 markdown preview 显示乱码，可以添加：
```css
.markdown-preview {
  font-family: @mono-font-family;
  atom-text-editor::shadow *{
    font-family: @mono-font-family;
  }
}
```
以及 [Atom 在Ubuntu 中文显示乱码（方块）解决方法](http://tikitoo.me/2015/07/23/ubuntu-atom-zh-display-squre/)

# Pandoc
- Convert markdown files to pdfs: `pandoc test.md  --latex-engine=xelatex -o hw03.pdf --template=pm-template.latex`
   其中 [pm-template.latex](https://gist.github.com/ceclinux/9116293) 是一个生成模板，如果要修改生成 pdf 中的字体、代码格式等，就要对这个模板进行修改。
