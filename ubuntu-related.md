# install lastest openssh server on ubuntu 16.04
```
wget https://launchpadlibrarian.net/335526589/openssh-client_7.5p1-10_amd64.deb
wget https://launchpadlibrarian.net/298453050/libgssapi-krb5-2_1.14.3+dfsg-2ubuntu1_amd64.deb
wget https://launchpadlibrarian.net/298453058/libkrb5-3_1.14.3+dfsg-2ubuntu1_amd64.deb
wget https://launchpadlibrarian.net/298453060/libkrb5support0_1.14.3+dfsg-2ubuntu1_amd64.deb
sudo dpkg -i libkrb5support0_1.14.3+dfsg-2ubuntu1_amd64.deb
sudo dpkg -i libkrb5-3_1.14.3+dfsg-2ubuntu1_amd64.deb
sudo dpkg -i libgssapi-krb5-2_1.14.3+dfsg-2ubuntu1_amd64.deb
sudo dpkg -i openssh-client_7.5p1-10_amd64.deb
ssh -V
```

# install latest remmina
```
sudo apt-add-repository ppa:remmina-ppa-team/remmina-next
sudo apt update
sudo apt install remmina remmina-plugin-rdp remmina-plugin-secret remmina-plugin-spice
```


# install and configure pure-ftpd
```
sudo apt-get install pure-ftpd
sudo groupadd ftpgroup
sudo useradd -g ftpgroup -d /home/ftpuser -s /sbin/nologin ftpuser
sudo chown -R ftpuser:ftpgroup /home/ftpuser

sudo pure-pw useradd admin -u ftpuser -d /home/ftpuser
sudo pure-pw mkdb

cd /etc/pure-ftpd/conf
sudo echo 'no' > PAMAuthentication
sudo echo 'no' > UnixAuthentication
sudo echo '/etc/pure-ftpd/pureftpd.pdb' > PureDB
cd /etc/pure-ftpd/auth
unlink other soft links
sudo ln -s /etc/pure-ftpd/conf/PureDB /etc/pure-ftpd/auth/50pure

/etc/init.d/pure-ftpd restart
```
add user in chroot jail
`sudo pure-pw useradd $USERNAME -u ftpuser -d /home/ftpuser -m`

add user not in chroot jail
`sudo pure-pw useradd $USERNAME -u ftpuser -D /home/$USERNAME -m`

view user
`sudo pure-pw show $USERNAME`

change passwd
`sudo pure-pw passwd $USERNAME -m`

update existing user
`sudo pure-pw usermod $USERNAME $OPTIONS -m`

delete user
`sudo pure-pw userdel $USERNAME -m`

list all users
`sudo pure-pw list`

update password db
`sudo pure-pw mkdb`


clean remove pure-ftpd
```
sudo service pure-ftpd stop
sudo apt-get autoremove pure-ftpd
sudo apt-get purge pure-ftpd
sudo rm -r /etc/pure-ftpd
```

# install lftp 
`sudo apt install lftp -y`
`cat ~/.lftprc`
```
set ssl:verify-certificate false

```

# disable firewall
`sudo ufw disable `

# ubuntu 18.04 change apt source
```
cp /etc/apt/sources.list /etc/apt/sources.list.bak
sudo vim /etc/apt/sources.list
```
```
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

```
`apt update && apt updgrade -y`

# ubuntu 16.04 change apt source

```
deb http://mirrors.aliyun.com/ubuntu/ xenial main
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main

deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main

deb http://mirrors.aliyun.com/ubuntu/ xenial universe
deb-src http://mirrors.aliyun.com/ubuntu/ xenial universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates universe

deb http://mirrors.aliyun.com/ubuntu/ xenial-security main
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security universe
```

# ubuntu 18.04 enable screen sharing

- `cd /etc/NetworkManager`

- save `NetworkManager.conf` to `NetworkManager.orig` (as a backup)

- `sudo vi NetworkManager.conf`

  Change `managed=false` to `managed=true`

  New file looks like this:

  ```
  [main]
      plugins=ifupdown,keyfile
  [ifupdown]
      managed=true
  [device]
      wifi.scan-rand-mac-address=no
  ```

- `sudo service network-manager restart`

- `cd /etc/netplan`

- `sudo vi 01-netcfg.yaml`

  Add this line just below `network:`

  `renderer: NetworkManager`

  New file looks similar to this (ensure the `renderer` line is indented as shown):

  ```
  network:
      renderer: NetworkManager
      ethernets:
          enp3s0:
              addresses: []
              dhcp4: true
  version: 2
  ```

- save

- `sudo netplan apply`

- I had then to restart the computer for this to be effective.

- After restart the network will now show "wired-connected"

  If errors like: 

  **Server did not offer supported security type!**

  Then:

  ```
  sudo apt install dconf-editor
  ```

Launch dconf-edtor

Browse **org | gnome | desktop | remote-access**

Toggle off **Require Encryption**

Close Editor

use mobo to vnc to your ubuntu



# ubuntu install aria2

`sudo apt install aria2`

`mkdir /data/dowloads`

`chown -R pp.pp /data/downloads`

`mkdir -p /home/pp/.aria2`

`vim /home/pp/.aria2/aria2.conf`

add following contents



```
dir=/data/downloads
disable-ipv6=true

enable-rpc=true
rpc-allow-origin-all=true
rpc-listen-all=true
#rpc-listen-port=6800
continue=true
input-file=/home/pp/.aria2/aria2.session
save-session=/home/pp/.aria2/aria2.session

max-concurrent-downloads=20
save-session-interval=120

# Http/FTP Ïà¹Ø
connect-timeout=120
#lowest-speed-limit=10K
max-connection-per-server=10
#max-file-not-found=2
min-split-size=10M

split=10
check-certificate=false
#http-no-cache=true
bt-tracker=http://asnet.pw:2710/announce,http://h4.trakx.nibba.trade:80/announce,http://pt.lax.mx:80/announce,http://share.camoe.cn:8080/announce,http://t.nyaatracker.com:80/announce,http://tr.cili001.com:8070/announce,http://tracker.files.fm:6969/announce,http://tracker.gbitt.info:80/announce,http://tracker.ipv6tracker.ru:80/announce,http://tracker.nyap2p.com:8080/announce,http://tracker.tfile.co:80/announce,http://tracker.trackerfix.com:80/announce,http://tracker.ygsub.com:6969/announce,http://tracker1.itzmx.com:8080/announce,http://tracker2.itzmx.com:6961/announce,http://tracker3.itzmx.com:6961/announce,http://tracker4.itzmx.com:2710/announce,http://vps02.net.orel.ru:80/announce,https://1337.abcvg.info:443/announce,https://tracker.nanoha.org:443/announce,https://tracker.opentracker.se:443/announce,https://tracker.parrotlinux.org:443/announce,udp://9.rarbg.me:2710/announce,udp://9.rarbg.to:2710/announce,udp://bt1.archive.org:6969/announce,udp://bt2.archive.org:6969/announce,udp://chihaya.toss.li:9696/announce,udp://denis.stalker.upeer.me:6969/announce,udp://exodus.desync.com:6969/announce,udp://ipv4.tracker.harry.lu:80/announce,udp://ipv6.tracker.harry.lu:80/announce,udp://ipv6.tracker.zerobytes.xyz:16661/announce,udp://open.demonii.com:1337,udp://open.demonii.si:1337/announce,udp://open.nyap2p.com:6969/announce,udp://open.stealth.si:80/announce,udp://opentor.org:2710/announce,udp://opentracker.i2p.rocks:6969/announce,udp://retracker.akado-ural.ru:80/announce,udp://retracker.lanta-net.ru:2710/announce,udp://retracker.netbynet.ru:2710/announce,udp://thetracker.org:80/announce,udp://tracker-udp.gbitt.info:80/announce,udp://tracker.0o.is:6969/announce,udp://tracker.birkenwald.de:6969/announce,udp://tracker.coppersurfer.tk:6969/announce,udp://tracker.cyberia.is:6969/announce,udp://tracker.doko.moe:6969/announce,udp://tracker.internetwarriors.net:1337/announce,udp://tracker.leechers-paradise.org:6969/announce,udp://tracker.moeking.me:6969/announce,udp://tracker.nyaa.uk:6969/announce,udp://tracker.openbittorrent.com:80/announce,udp://tracker.opentrackr.org:1337/announce,udp://tracker.sbsub.com:2710/announce,udp://tracker.tiny-vps.com:6969/announce,udp://tracker.torrent.eu.org:451/announce,udp://tracker.uw0.xyz:6969/announce,udp://tracker.zerobytes.xyz:1337/announce,udp://tracker.zum.bi:6969/announce,udp://tracker1.itzmx.com:8080/announce,udp://tracker2.itzmx.com:6961/announce,udp://tracker3.itzmx.com:6961/announce,udp://tracker4.itzmx.com:2710/announce,udp://valakas.rollo.dnsabr.com:2710/announce,udp://xd.dog:1337/announce,udp://xxxtor.com:2710/announce,udp://zephir.monocul.us:6969/announce,wss://tracker.btorrent.xyz,wss://tracker.fastcast.nz,wss://tracker.openwebtorrent.com
```

## firefox add on

Send to Aria2 WE

Aria2 Download Manager Integration

# ubunt 18 smb install and configure

`sudo apt isntall smaba smbclient`

`vim  /etc/samba/smb.conf`

add the following at the bottom:

```
[sambashare]
[sambashare]
    comment = Samba on Ubuntu
    path = /data/sambashare
    read only = no
    browsable = yes

```

` sudo smbpasswd -a pp` 

`mkdir /data/sambashare`

`chown -R pp.pp /data/sambashare`

`sudo systemctl restart smbd.service`

`sudo smbclient //192.168.88.106/sambashare -U pp`
