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
```
vi /etc/hosts
```
```
# add hosts that are in NIS domain (server/client)
192.168.10.1   master.concept.lan master
192.168.10.2   node01.concept.lan node01
```
```
systemctl enable --now rpcbind ypserv ypxfrd yppasswdd nis-domainname
```
update NIS databases
```
/usr/lib64/yp/ypinit -m
```
```
At this point, we have to construct a list of the hosts which will run NIS
servers.  dlp.srv.world is in the list of NIS server hosts.  Please continue to add
the names for the other hosts, one per line.  When you are done with the
list, type a <control D>.
        next host to add:  master.concept.lan
        next host to add:  ### # Ctrl + D key
The current list of NIS servers looks like this:

master.concept.lan

Is this correct?  [y/n: y]  y
We need a few minutes to build the databases...
Building /var/yp/srv.world/ypservers...
Running /var/yp/Makefile...
gmake[1]: Entering directory '/var/yp/concept.lan'
Updating passwd.byname...
Updating passwd.byuid...
Updating group.byname...
Updating group.bygid...
Updating hosts.byname...
Updating hosts.byaddr...
Updating rpc.byname...
Updating rpc.bynumber...
Updating services.byname...
Updating services.byservicename...
Updating netid.byname...
Updating protocols.bynumber...
Updating protocols.byname...
Updating mail.aliases...
gmake[1]: Leaving directory '/var/yp/concept.lan'

master.concept.lan has been set up as a NIS master server.

Now you can run ypinit -s master.concept.lan on all slave server.
```
=> If you add local user or local group, new hosts in [/etc/hosts] on NIS Server, then Apply changes to NIS databases like follows.
```
cd /var/yp
```
```
make
```

### On the Client Node01 Node :-
```
yum install ypbind rpcbind oddjob-mkhomedir
```
set NIS domain
```
ypdomainname concept.lan
```
```
echo "NISDOMAIN=concept.lan" >> /etc/sysconfig/network
```
```
vi /etc/yp.conf
```
```
# add to the end
# [domain (NIS domain) server (NIS server)]
domain concept.lan server master.concept.lan
```
```
vi /etc/hosts
```
```
# add hosts that are in NIS domain (server/client)
192.168.10.1   master.concept.lan master
192.168.10.2   node01.concept.lan node01
```
```
authselect select nis --force
```
```
Backup stored at /var/lib/authselect/backups/2019-10-17-01-18-26.zBmEkS
Profile "nis" was selected.
The following nsswitch maps are overwritten by the profile:
- aliases
- automount
- ethers
- group
- hosts
- initgroups
- netgroup
- networks
- passwd
- protocols
- publickey
- rpc
- services
- shadow

Make sure that NIS service is configured and enabled. See NIS documentation for more information.
```
set if you need (create home directory when initial login)
```
authselect enable-feature with-mkhomedir
```
```
systemctl enable --now rpcbind ypbind nis-domainname oddjobd
```
```
exit
```
```
CentOS Linux 8 (Core)
Kernel 4.18.0-80.7.1.el8_0.x86_64 on an x86_64

node01 login: redhat     # NIS user
Password:
[redhat@node01 ~]$       # just logined

#confirm binded NIS server
[redhat@node01 ~]$ ```ypwhich```
master.concept.lan

#change NIS password
[redhat@node01 ~]$ ```yppasswd```
```
