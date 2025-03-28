># **FOR ACTUAL ISSUES SEE BELOW [actual issues](./#actual-issues)**

# what is HOL-0499 ?  
### objective is to have a playground that represents a more or less “realistic” customer environment with several workloads.
### on this playground we give the chance to install and configure Dell Technolgies products from scratch on your own to gain experience !

# lab overview
### systems / workloads / connectivity 
![image](https://github.com/user-attachments/assets/f75558d2-88f3-4ca2-902e-6e43629a099c)

# FQDN and DNS suffix  
***.demo.local** is the usual DNS suffix   
naming policy is: use ALWAYS FQDN !!!

# account and passwords  
Username depends, as always, on the workload or the device you want to login or manage   
Rule of thumb for password is always: **Password123!**

Exceptions listed here: 
FDQN | account/password | workload  |  notes  
------|---------------------|------------|-----------  
vault-ppdm | root/changeme | ppdm in the vault | OS level
vault-ppdm | admin/@ppAdm1n | ppdm in the vault | app level
nve-1 | root passphrase "Password123" | NVE upgrade | without exclamation mark

# workloads
FDQN | account | workload  |  notes  
------|---------------------|------------|-----------  
vcenter01 | admin@vsphere.local | vsphere | The production vcenter 
esxi01+02+03 | root | esxi | The production hypervisors
nas | admin | NFS Datastore for vsphere | rocky linux and zfs
launchpad | administrator@demo.local | YOUR jumpbox is AD controller for “demo.local” and DHCP + DNSserver 
ansible | admin | RestAPI automation | filled with automation magic from karsten 
ddve-01 | sysadmin | PP DataDomain | primary protection target 
ddve-02 | sysadmin | PP DataDomain | secondary protection target 
sql01+02 | administrator@demo.local | SQL 2019 AAG  |	treated as bare-metal SQL DB 
ddmc |	sysadmin | ddmc | smart scale “bash hack” used 
dpa |	administrator  | DPA 	
dpc |	administrator@dpc.local  | DPC 	
exchange1+2  |		administrator@demo.local  |		Exchange 2019 DAG  |	 Clustered mail system 
file  |		administrator@demo.local  |		Win 2022 file server  |		Central file server 
hana01  |		H01adm  |		hana hxe 2.0 SPS02  |	 	SAP HANA  
openshift0+1+2  |		kubeadmin  |		openshift cluster  |	 	Kubernetes from RedHat  
powerscale  |		root  |		Isilon oneFS | treated as NAS workload / joined demo.local AD  
powerstore-1+2  |		admin  |	PowerOS | treated as NAS workload / joined demo.local AD  
scvmm  |		administrator@demo.local  |		system center virtual machine manager  |		old stlye UI fpr central managing vm´s  
wac  |		administrator@demo.local  |		Windows Admin Center  |		nice UI for central managing windows machines  
nve-1  |		administrator  |		Networker  |	legay dataprotection 	
nve-1-vproxy  |		admin  |		vproxy 	
ora1  |		root or oracle | Oracle 19c  |	DG active |		Data guard node 
ora2  |		root or oracle | Oracle 19c  |	DG standby |	Data guard node 
ppdm-1  | admin  | PP Data Manager  | next gen dataprotection with genAI
ppdm-1-search | NA | PPDM search node 	
ppdm-1-report | NA | PPDM report node 	
sql03+04 | administrator@demo.local | SQL 2022 AAG | treated as vm for app ware backup 
vault-esxi | root | esxi | for simulating a CR vault area  
vault-vcenter |	admin@vsphere.local | vSphere for the vault|The separated vcenter in the vault  
vault-ppdm | root/changeme | ppdm for the vault | ppdm is waiting in “install screen”  
vault-ddve | sysadmin | PPDD for the vault | the separated protection target  
cr | crso + cradmin | PPCR for the vault | vault CyberRecovery Manager 
cs | admin |	CyberSense  | Optional deep forensic  


# systems in detail  
## launchpad  
has the function to be the initial place for accessing the 0499 lab environment. it runs the AD controller role for "demo.local". 
it runs an **DHCP** and **DNS** server. DHCP Server lease pool is from 192.168.1.100 up to 192.168.1.199. all adresses below 192.168.1.100 are manual and YOU have to doublecheck for duplicate IP´s and correct DNS entrys. DHCP server updates all hostnames from any linux/windows to the DNS server if the client is able to transfer his hostname to DHCP. means if you deploy a linux or windows vm that hostname will listed and updated in the DNS server automatically, result is a working name reolution in your lab.
any manual configured IP have to to manual maintained in the DNS server.  

### XCA tool
is for generating your own SSL certificates. rootCA for democenter is alreday pushed via GPO. so every windows machine that is member of demo.local AD trust your self created SSL certificates. on linux boxes you have to this manually depended on the used distribution.   
how to use XCA: https://hohnstaedt.de/xca/  

### mremoteNG
contains pre-configured SSH and RDP sessions for almost all workloads and systems
### browser
firefox "should" be used as default browser. it´s feature to pre-fill user and password fields makes it really easy and fast to access the UI of all systems. edge **MUST** be used for windows admin center.
### Data Studio Client (IBM)
nice java UI for accessing the DB2 single node instance and the oracle dataguard cluster.
for those how wants to see that the a DB is alive/mounted and able to create entrys.
### SAP HANA Studio
nice java UI for accessing the HANA HXE single node instance.
###WinSCP 
well known tool for transferring files to linux systems.

## vcenter01 (by broadcom)
use either "admin@vsphere.local" or "windows session authentication". user "administrator@vpshere.local" is locked due to license agreement with broadcom. if you need to create "user accounts" in vsphere use AD user from "demo.local". 

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
is fresh installed and has just the "vcenter01" for image backup configured.
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

## nas 
is running rocky linux and doing the NFS datstore jov for the esxi cluster.

## scvmm (system center virtual machine manager)
the old style way to manage vm´s
can be pimped with our networker plugin ;-)

## wac (windows admin center)
the "option" to have a nice HTML5 UI for managing windows systems.
**MUST** be accessed by edge, it's not working with firefox


# actual issues 
## openshift cluster not starting (SOLUTION)  
- start ansible vm
- login via ssh as admin into ansible vm
- export k8s envvironment and get new certificates
  
```bash
export KUBECONFIG=~/workspace/openshift/auth/kubeconfig
oc get csr -o go-template='{{range .items}}{{if not .status}}{{.metadata.name}}{{"\n"}}{{end}}{{end}}' | xargs oc adm certificate approve

```
- wait for at least 5 minutes
- if openshift cluster is still not starting - repeat the CLI commands

## DPC login in UI not possible (SOLUTION)
- ssh into DPC via mremoteNG
```bash
sudo /usr/local/dpc/bin/dpc start
```
- check that all services are in state "active" (except FIPS ;-)
```bash
sudo /usr/local/dpc/bin/dpc status
```
- wait 1-2 minutes and try to login in UI again

## CR login in UI not possible (SOLUTION)
- ssh into CR via mremoteNG
```bash
sudo /opt/dellemc/cr/bin/crsetup.sh --verifypassword
```
- use always the default lab Password123!
- regenerate the expired ertificates
```bash
sudo /opt/dellemc/cr/bin/crsetup.sh --securereset
```
- wait 1-2 minutes and try to login in UI again

## WAC vm is not running
- open the failover cluster manager and navigate to hv-cluster > roles 
- right click "SCVMM wac resources" navigate to "more actions" > "delete saved state"
- delete the save state and start the wac vm
  
## vm's did not get DHCP ip adress
- networking issue in the underlying democenter infrastructure.
- cancel the lab and deploy a new one




