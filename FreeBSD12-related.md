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

# install vim tmux
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
