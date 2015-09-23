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
