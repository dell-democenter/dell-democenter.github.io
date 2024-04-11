# what is HOL-0499 ?  
### goal is to have a playground that represents a “realistic” customer environment with several workloads.
### goal is to install and configure all DELL solution from scratch to gain experience on your own !

# lab overview
### systems / workloads / connectivity 
![image](https://github.com/dell-democenter/dell-democenter.github.io/assets/37048842/c6c7933c-7002-4862-805c-c745375c549d)

# account and passwords  
Username depends, as always, on the workload or the device you want to login or manage   
Rule of thumb for password is always: **Password123!**

Exceptions listed here: 
FDQN | account/password | workload  |  notes  
------|---------------------|------------|-----------  
vault-ppdm | root/changeme | ppdm in the vault | OS level
vault-ppdm | admin/@ppAdm1n | ppdm in the vault | app level
cs.demo.local | admin/admin | cybersense | password policy is ...

# workloads
FDQN | account | workload  |  notes  
------|---------------------|------------|-----------  
vcenter01 | admin@vsphere.local | vsphere | The production vcenter 
esxi01 - 03 | root | esxi | The production hypervisors
nas | admin | NFS Datastore for vsphere | rocky linux and zfs
launchpad | administrator@demo.local | YOUR jumpbox is AD controller for “demo.local” and DHCP + DNSserver 
ansible | admin | RestAPI automation | filled with automation magic from karsten 
ddve-01 | sysadmin | PP DataDomain | primary protection target 
ddve-02 | sysadmin | PP DataDomain | secondary protection target 
sql01 - 02 | administrator@demo.local | SQL 2019 AAG  |	treated as bare-metal SQL DB 
ddmc |	sysadmin | ddmc | smart scale “bash hack” used 
dpa |	administrator  | DPA 	
dpc |	administrator@dpc.local  | DPC 	
exchange1 - 2  |		administrator@demo.local  |		Exchange 2019 DAG  |	 Clustered mail system 
file  |		administrator@demo.local  |		Win 2022 file server  |		Central file server 
hana01  |		H01adm  |		hana hxe 2.0 SPS02  |	 	SAP HANA 
openshift  |		kubeadmin  |		openshift cluster  |	 	Kubernetes from IBM 
powerscale  |		root  |		Isilon oneFS | treated as NAS workload / joined demo.local AD
powerstore  |		admin  |	PowerOS | treated as NAS workload / joined demo.local AD
K8scl1-pool1…  |		bobuser  |		Kubernetes cluster 	k8s  |	 workload with wordpress/mysql 
rancher  |		ubuntu  |		Rancher  |		K8s manager for cluster/pods  
nve-1  |		administrator  |		Networker  |	legay dataprotection 	
nve-1-vproxy  |		admin  |		vproxy 	
ora1  |		root or oracle | Oracle 19c  |	DG active |		Data guard node 
ora2  |		root or oracle | Oracle 19c  |	DG standby |	Data guard node 
ppdm-1  | admin  | PP Data Manager  | next gen dataprotection with genAI
ppdm-1-search | | PPDM search node 	
ppdm-1-report | | PPDM report node 	
sql03 - 04 | administrator@demo.local | SQL 2022 AAG | treated as vm for app ware backup 
vault-esxi | root | esxi | for simulating a CR vault area 
vault-vcenter |	admin@vsphere.local | vSphere for the vault|The separated vcenter in the vault 
vault-ppdm | root/changeme | ppdm for the vault | ppdm is waiting in “install screen” 
vault-ddve | sysadmin | PPDD for the vault | the separated protection target 
cr | rso | PPCR for the vault | vault CyberRecovery Manager 
cs | admin/admin |	CyberSense  | Optional deep forensic 



# systems in detail  
### lauchpad  
has the function to be the initial place for accessing the vapp. it runs the AD controller role doe "demo.local". it runs an DHCP and DNS server, the DHCP server updates all hostnames from any OS to the DNS server. that means if you deploy a linux or windows vm that hostname will listed and updated in the DNS server automatically. result is a pretty good name reolution in your lab.
if you give manual IP you have to manual maintain the DNS entrys.



## Header 1

### Header 2

##### Header 3
```bash
rm -rf /
```
```Powershell
get-help -module dau
```

