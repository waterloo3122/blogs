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

# install tmux
pkg install tmux

vim ~/.tmux.conf

```
set-option -g history-limit 100000
set -g base-index 1
setw -g pane-base-index 1
#set display-panes-time 4000
set -s escape-time 0

# Remap window navigation to vim
unbind-key j
bind-key j select-pane -D
unbind-key k
bind-key k select-pane -U
unbind-key h
bind-key h select-pane -L
unbind-key l
bind-key l select-pane -R


bind -n M-k resize-pane -U 5
bind -n M-j resize-pane -D 5
bind -n M-h resize-pane -L 5
bind -n M-l resize-pane -R 5

```

# 更换国内源
## 更换pkg源

```
mkdir -p /usr/local/etc/pkg/repos
cat /usr/local/etc/pkg/repos/FreeBSD.conf
```

```
FreeBSD:{
    url: "pkg+http://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/quarterly",
}
```
or
```
FreeBSD: {
    url: "pkg+http://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/latest",
    mirror_type: "srv",  signature_type: "fingerprints",
    fingerprints: "/usr/share/keys/pkg",
    enabled: yes
}
```

`pkg update`
`pkg update -f`




## 更换portsnap源
vi /etc/portsnap.conf
change
SERVERNAME=portsnap.FreeBSD.org
to 
SERVERNAME=portsnap.cn.FreeBSD.org

portsnap fetch
portsnap extract
portsnap update

## 修改ports源
vim /etc/make.conf

```
# content of make.conf
FETCH_CMD=axel -n 10 -a
DISABLE_SIZE=yes
MASTER_SITE_OVERRIDE?=http://mirrors.ustc.edu.cn/freebsd-ports/distfiles/${DIST_SUBDIR}/
```

# install vim

```
pkg clean
rm -rf /var/cache/pkg/*
pkg update -f
pkg install vim

```


# install gnome3 on intel vga

`pkg install xf86-video-intel​​​​​​​`

`vim /etc/rc.conf`

add 

`linux_enable="YES"`

`kldload linux64`

`kldstat`

`pkg install xorg`
`startx`
`xrandr`

`pkg install gnome3`
`vim /etc/rc.conf`

add

```
gdm_enable="YES"
gnome_enable="YES"
dbus_enable="YES"
hald_enable="YES"
```
`reboot`

# install sudo
`pkg install sudo`
`visudo`
add
`pp ALL=(ALL) ALL`
below
`root ALL=(ALL) ALL`
