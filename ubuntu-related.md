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
