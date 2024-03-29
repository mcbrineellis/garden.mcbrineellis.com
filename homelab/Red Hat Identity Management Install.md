I followed this guide: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html-single/installing_identity_management/index#opening-the-ports-required-by-idm_preparing-the-system-for-ipa-server-installation

Configured Chronyd (edited `/etc/chrony.conf` to use the pfSense as an NTP server)
Checked the status using `chronyc tracking`.

Updated the firewall, using `firewall-cmd --permanent --add-service={freeipa-4,dns}` then ran `firewall-cmd --reload`.

Enabled the required repos:
```
subscription-manager repos --enable=rhel-9-for-x86_64-baseos-rpms
subscription-manager repos --enable=rhel-9-for-x86_64-appstream-rpms
```

Then installed the packages for IdM server with integrated DNS:
```
dnf install ipa-server ipa-server-dns
```

Then I completed the setup and whatnot....