# set terminal time out
`echo "export TMOUT=300" >> ~/.bashrc`
OR
`echo "export TMOUT=300" >> /etc/profile`

# set history limit
```
echo "export HISTSIZE=1000" >> ~/.bashrc
echo "export HISTFILESIZE=1000" >> ~/.bashrc
```
OR
`echo "export HISTSIZE=1000" >> /etc/profile`

# modify file descriptor 文件描述符
`echo "* - nofile 65535" >> /etc/security/limits.conf`

# kernel 
`vim /etc/sysctl.conf`
add follows
```
vm.swappiness = 0
net.ipv4.neigh.default.gc_stale_time = 120

# see details in https://help.aliyun.com/knowledge_detail/39428.html
net.ipv4.conf.all.rp_filter = 0
net.ipv4.conf.default.rp_filter = 0
net.ipv4.conf.default.arp_announce = 2
net.ipv4.conf.lo.arp_announce = 2
net.ipv4.conf.all.arp_announce = 2
# see details in https://help.aliyun.com/knowledge_detail/41334.html
#net.ipv4.tcp_max_tw_buckets = 5000
#net.ipv4.tcp_syncookies = 1
#net.ipv4.tcp_max_syn_backlog = 1024
#net.ipv4.tcp_synack_retries = 2
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
kernel.sysrq = 1

net.ipv4.tcp_fin_timeout=2
net.ipv4.tcp_tw_reuse=1
net.ipv4.tcp_tw_recycle=1
net.ipv4.tcp_syncookies=1
net.ipv4.tcp_keepalive_time=600
net.ipv4.ip_local_port_range=4000 65000
net.ipv4.tcp_max_syn_backlog=16384
net.ipv4.tcp_max_tw_buckets=36000
net.ipv4.route.gc_timeout=100
net.ipv4.tcp_syn_retries=1
net.ipv4.tcp_synack_retries=1
net.core.somaxconn=16384
net.core.netdev_max_backlog=16384
net.ipv4.tcp_max_orphans=16384

#net.ipv4.tcp_window_scaling = 1
#net.ipv4.tcp_rmem = 4096 87380 4194304
#net.ipv4.tcp_wmem = 4096 16384 4194304
#net.ipv4.tcp_max_syn_backlog = 16384
#net.core.wmem_default = 8388608
#net.core.rmem_default = 8388608
#net.core.rmem_max = 16777216
#net.core.wmem_max = 16777216

#firewall related
#net.nf_conntrack_max=25000000
#net.netfilter.nf_conntrack_max=25000000
#net.netfilter.nf_conntrack_tcp_timeout_established=180
#net.netfilter.nf_conntrack_tcp_timeout_time_wait=120
#net.netfilter.nf_conntrack_tcp_timeout_close_wait=60
#net.netfilter.nf_conntrack_tcp_timeout_fin_wait=120
```
then
`sysctl -p`

# set welcome message when ssh login
`vim /etc/motd`
```
                                  _oo0oo_
                                 088888880
                                 88" . "88
                                 (| -_- |)
                                  0\ = /0
                               ___/'---'\___
                             .' \\\\|     |// '.
                            / \\\\|||  :  |||// \\
                           /_ ||||| -:- |||||- \\
                          |   | \\\\\\  -  /// |   |
                          | \_|  ''\---/''  |_/ |
                          \  .-\__  '-'  __/-.  /
                        ___'. .'  /--.--\  '. .'___
                     ."" '<  '.___\_<|>_/___.' >'  "".
                    | | : '-  \'.;'\ _ /';.'/ - ' : | |
                    \  \ '_.   \_ __\ /__ _/   .-' /  /
                ====='-.____'.___ \_____/___.-'____.-'=====
                                  '=---='


              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
              佛祖保佑                                永不死机
```
# lock import system files
`chattr +i /etc/passwd /etc/shadow /etc/group /etc/gshadow`
`lsattr /etc/passwd /etc/shadow /etc/group /etc/gshadow`
`chattr -i /etc/passwd /etc/shadow /etc/group /etc/gshadow`

# disable user which not used
edit /etc/passwd

# add grub password
on centos7,generate password
`grub2-mkpasswd-pbkdf2`
generated encrypted hash like
```
PBKDF2 hash of your password is grub.pbkdf2.sha512.10000.A97FD1982DA349F1FBFA9363119CEFAE18C0CCA4FC462C52F7F7C83C8731CC06C1DD928B53306B37AD455BD01AEE1FDAC22A5217E6528F7BA6693C0C79F47029.53EEDC14116DACA2F67FFB17578BD5984B78233332BD50B37521F78C7CAA07FEFB1FE8E4788169DAD8A5B1A681AFC9FD48BB585FD80630135612FE78731F1A29

```
`cp /etc/grub.d/40_custom /etc/grub.d/40_custom.backup`
`vi /etc/grub.d/40_custom   # Edit the GRUB Custom Menu`
```
#!/bin/sh
exec tail -n +3 $0
# This file provides an easy way to add custom menu entries.  Simply type the
# menu entries you want to add after this comment.  Be careful not to change
# the 'exec tail' line above.
password_pbkdf2 root grub.pbkdf2.sha512.10000.A97FD1982DA349F1FBFA9363119CEFAE18C0CCA4FC462C52F7F7C83C8731CC06C1DD928B53306B37AD455BD01AEE1FDAC22A5217E6528F7BA6693C0C79F47029.53EEDC14116DACA2F67FFB17578BD5984B78233332BD50B37521F78C7CAA07FEFB1FE8E4788169DAD8A5B1A681AFC9FD48BB585FD80630135612FE78731F1A29
```
`cp /boot/grub2/grub.cfg /boot/grub2/grub.cfg.backup`

`grub2-mkconfig -o /boot/grub2/grub.cfg`

Check if effect
`cat /boot/grub2/grub.cfg`

# disable ping
```
echo "net.ipv4.icmp_echo_ingore_all=1" >> /etc/sysctl.conf
sysctl -p
```

# change yum source to aliyun
```
yum install -y epel-release
cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak  
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo 

mv  /etc/yum.repos.d/epel.repo  /etc/yum.repos.d/epel.repo.bak
mv  /etc/yum.repos.d/epel-testing.repo  /etc/yum.repos.d/epel-testing.repo.bak
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

yum clean all
yum makecache
```

# wget usage
```
wget -O path_to_your_downloaded url
-T 指定超时时间
--tries 指定重试的次数
-q 静默下载
```

# tree usage
```
tree -L 1 / #指定显示的层数
```

# ssh connect slow
`vim /etc/ssh/sshd_config`
add 
`useDNS no`
`systemctl restart ssh`


# basic rules
不用root管理
更改远程连接端口，禁止root连接，可以只监听内网端口
定时通过ntp更新服务器时间
关闭selinux和iptables
调整文件描述符的数量
定时清理邮件目录垃圾文件，防止innode被占满
精简并保留必要的开机服务
内核优化
更改字符集
锁定关键文件
清理多余的系统帐号
grub添加密码
禁止被ping
配置更新源

 
# print file line number
```
cat -n
nl
```

# clean journal log
`journalctl --vacuum-time=2d`
or
`journalctl --vacuum-size=500M
`
or
`echo "1 1 * * * /usr/bin/journalctl --vacuum-time=2d"`

# mount nfs on boot
edit /etc/rc.local
/etc/fstab does not work in centos7.4,don't konow why

# install and configure smtp

`yum install -y mailx`
`vim /etc/mail.rc`
add follows
```
set from=op@licaimofang.com
set smtp=smtps://smtp.ym.163.com:994
set smtp-auth-user=xx@xxxx.com
set smtp-auth-password=xxxxxx
set smtp-auth=login
set ssl-verify=ignore
set nss-config-dir=/etc/pki/nssdb/

```
test
`echo "This is a mail for test" | mail -v -s "Test Email Suject" xx@xxxx.com`


# install  php7
```
su - root
yum install epel-release
rpm -ivh https://mirrors.aliyun.com/remi/enterprise/remi-release-7.rpm
yum install yum-utils
sed -i 's/http:\/\/cdn.remirepo.net/https:\/\/mirrors.aliyun.com\/remi/g'  /etc/yum.repos.d/remi*; 
sed -i 's/http:\/\/rpms.remirepo.net/https:\/\/mirrors.aliyun.com\/remi/g'  /etc/yum.repos.d/remi*; 
sed -i 's/#baseurl/baseurl/g' /etc/yum.repos.d/remi*; 

cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak  
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo  

mv  /etc/yum.repos.d/epel.repo  /etc/yum.repos.d/epel.repo.bak
mv  /etc/yum.repos.d/epel-testing.repo  /etc/yum.repos.d/epel-testing.repo.bak
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

yum clean all
yum makecache

yum-config-manager --enable  remi-php73

yum install -y php php-common php-opcache php-mcrypt php-cli php-gd php-curl php-mysqlnd php-fpm php-pear php-devel php-devel php-devel php-devel php-devel php-devel php-devel php-devel php-devel  composer vim git unzip tmux net-tools
su - bae
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
```
