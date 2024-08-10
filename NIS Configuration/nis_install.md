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
