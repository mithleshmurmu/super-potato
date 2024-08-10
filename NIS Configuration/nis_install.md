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
```
vi /var/yp/securenets
```
```
# create new
# specify range of network you allow to access NIS clients
255.0.0.0       127.0.0.0
255.255.255.0   192.168.10.0
```
