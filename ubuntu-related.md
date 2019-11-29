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
