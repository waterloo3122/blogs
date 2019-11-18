# 开启ssh远程登录
## 1 
vi /etc/inetd.conf
去掉ssh前的#

## 2
vi /etc/rc.conf
添加
sshd_enable="YES"

## 3
vi /etc/ssh/sshd_config

change
#PermitRootLogin no
to
PermitRootLogin yes

change
PasswordAuthentication no
to 
PasswordAuthentication yes

change 
#PermitEmptyPasswords no
to
PermitEmptyPasswords no

## 4
/etc/rc.d/sshd start

## 5
netstat -an
