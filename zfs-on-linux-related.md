# config email alert
# NOT WORK ON CENTOS7
set your smtp config in /etc/mail.rc

`vim /etc/zfs/zed.d/zed.rc`
change following contents
```
ZED_EMAIL_ADDR="xxx@xx.com"
ZED_EMAIL_PROG="mail"
ZED_EMAIL_OPTS="-s '@SUBJECT@' @ADDRESS@ -r sender@xxx.com"
ZED_NOTIFY_VERBOSE=1
ZED_USE_ENCLOSURE_LEDS=1
```
`systemctl restart zed`

if not work, file an issue
