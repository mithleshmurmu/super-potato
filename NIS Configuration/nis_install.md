### NIS Configuration

### Master Node ( Rocky Linux 8.8 )
Stop and Disable firewalld

Disable the selinux file
```
hostnamectl set-hostname master.concept.lan
```
```
yum install ypserv rpcbind
```

set NIS domain
```
ypdomainname concept.lan
```
```
echo "NISDOMAIN=concept.lan" >> /etc/sysconfig/network
```
