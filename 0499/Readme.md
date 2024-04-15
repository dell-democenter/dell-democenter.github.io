># **FOR ACTUAL ISSUES SEE BELOW "actual issues"**

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
## lauchpad  
has the function to be the initial place for accessing the 0499 lab environment. it runs the AD controller role for "demo.local". 
it runs an **DHCP** and **DNS** server, the DHCP server updates all hostnames from any linux/windows to the DNS server. means if you deploy a linux or windows vm that hostname will listed and updated in the DNS server automatically. result is a pretty good name reolution in your lab.
any manual configured IP have to to manual maintained in the DNS server.
### mremoteNG
contains pre-configured SSH and RDP sessions for almost all workloads and systems
### browser
firefox "should" be used as default browser. it´s feature to pre-fill user and password fields makes it really easy and fast to access the UI of all systems. edge **MUST** be used for windows admin center.
### Data Studiio Client (IBM)
nice java UI for accessing the DB2 single node instance and the oracle dataguard cluster.
for those how wants to see that the a DB is alive/mounted and able to create entrys.
### SAP HANA Studio
nice java UI for accessing the HANA HXE single node instance.
###WinSCP 
well known tool for transferring files to linux systems.

## vcenter01 (by broadcomm)
use either "admin@vsphere.local" or "windows session authentication". user "administrator@vpshere.local" is locked due to license agreement with broadcomm. if you need to create "user accounts" in vsphere use AD user from "demo.local". 

## ansible
a linux host preconfigured to run automation with ansible playbooks.


## mail /exchange1 / exchange2
this exchange dag let you reiceive and send email just internally. it not (yet) relaying any SMTP mails.
the name of IP less DAG is "mail.demo.local" residing on two nodes "exchange1" and "exchange2"
some system are already configured to send email like ASUPś and so on. try to configure it for your workload.

## ppdm-1
is fresh installed and has just the "vcenter01" asset configured.
it is doing TSDM protection of all vsphere vm's, so if you mess something up, stay cool, you have a backup.
report and index engine are already deployed.

## nve-1
is fresh installed and has just the "vcenter01" configured.
it is doing VADP protection of all vsphere vm's, so if you mess something up, stay cool, you have a backup.

## ddmc
fresh deployed, nothing configured

## DPA
fresh deployed, nothing configured

## DPC
fresh deployed, nothing configured

## powerstore
fresh deployed, nothing configured
intend to be used when demonstrating synergy between dell primary storage <> backup storage, orchestrated by PowerProtect

## powerscale
fresh deployed, joinded the demo.local domain. has some file in the \\powerscale\data SMB share. intend to be used for demontrating the DNAS workload in PPDM.

## openshift
3 node openshuft cluster. running mysql and wordpress workload. intend to be used for demontrating the K8s workload in PPDM

## rancher 
single node rancher. running mysql and wordpress workload. intend to be used for demontrating the K8s workload in PPDM

## nas 
is running rocky linux and doing the NFS datstore jov for the esxi cluster.

## wac (windows admin center)
the "option" to have a nice HTML5 UI for managing windows systems.
**MUST** be access by edge, it's not working with firefox




# actual issues 
## openshift cluster not starting (SOLUTION)  
- start ansible vm
- login via ssh as admin into ansible vm
- export k8s envvironment and get new certificates
  
```bash
export KUBECONFIG=~/workspace/openshift/auth/kubeconfig
oc get csr -o go-template='{{range .items}}{{if not .status}}{{.metadata.name}}{{"\n"}}{{end}}{{end}}' | xargs oc adm certificate approve

```
- wait for at least 10 minutes

## vm's did not get DHCP ip adress
- networking issue in the underlying democenter infrastructure.
- cancel the lab and deploy a new one




