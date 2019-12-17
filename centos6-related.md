# install pure-ftpd
`yum install pure-ftpd -y`
`vim /etc/pure-ftpd/pure-ftpd.conf`
change as follows:
```
PureDB /etc/pure-ftpd/pureftpd.pdb
VerboseLog yes
NoAnonymous yes
PassivePortRange 48000 50000
```

```
sudo groupadd ftpgroup
sudo useradd -g ftpgroup -d /home/ftpuser -s /sbin/nologin ftpuser
sudo chown -R ftpuser:ftpgroup /home/ftpuser

sudo pure-pw useradd admin -u ftpuser -d /home/ftpuser
sudo pure-pw mkdb


chkconfig pure-ftpd on
/etc/init.d/pure-ftpd start
```
