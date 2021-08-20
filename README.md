- A script to rotate filling logs that just don't seem to want to work with logrotate or auditd native function.
- Installs as a service, or can be used to manually run from auditd.conf exec

Minimal configuration parameters are held in /etc/audrotate/auditlog_rotate.conf

-----

Point where you would like files to be rotated to with "targetDir"
Temporary file location and order used is specified via "targetPartitions".
Valye sizes are in MB. 

audirFreeLimit=2500
targetDir=/var/log/audit_rotate
targetDirPadLimit=800
targetPartitions="/tmp /usr /opt /home"

-----
Option 1 : Install RPM
```
curl -vsL https://github.com/Sky-Bly/auditlog_rotate/raw/main/auditlog-rotate-1-01.el7.x86_64.rpm > auditlog-rotate-1-01.el7.x86_64.rpm
curl -vsL https://raw.githubusercontent.com/Sky-Bly/auditlog_rotate/main/RPM-GPG-KEY-rpm > /etc/pki/rpm-gpg/RPM-GPG-KEY-audlog
rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-audlog
yum localinstal auditlog-rotate-1-01.el7.x86_64.rpm
systemctl enable auditlog-rotate.service
systemctl start auditlog-rotate.service
```

Option 2 : Setup as 'exec' in auditd.conf ( usually requires reboot ):
```
curl -vsL https://raw.githubusercontent.com/Sky-Bly/auditlog_rotate/main/auditlog_rotate > /usr/bin/auditlog_rotate
mkdir /etc/audrotate/
curl -vsL https://raw.githubusercontent.com/Sky-Bly/auditlog_rotate/main/auditlog_rotate.conf > /etc/audrotate/auditlog_rotate.conf
sed -i  's/space_left_action.*/space_left_action = exec \/usr\/bin\/auditlog_rotate/g' /etc/audit/auditd.conf
```
