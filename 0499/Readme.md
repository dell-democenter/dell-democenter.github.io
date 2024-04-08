# what is 0499 ?  
Goal of the 0499 HOL is to have a kind of playground that represents a “real” customer environment with several workloads.

# lab overview
systems / workloads / connectivity 
![image](https://github.com/dell-democenter/dell-democenter.github.io/assets/37048842/167698f2-0d6e-4a2f-9606-dae82823f6ce)

# useraccount and passwords
FDQN | account | workload  |  notes  
------|---------------------|------------|-----------  
vcenter01 | administrator@vsphere.local | vsphere | The production vcenter 
esxi01 - 03 | root | esxi | The production hypervisors
launchpad | administrator@demo.local | YOUR jumpbox is AD controller for “demo.local” and DHCP + DNSserver 
ddve-01 | sysadmin | PP DataDomain | primary protection target 
ddve-02 | sysadmin | PP DataDomain | secondary protection target 
sql01 - 02 | administrator@demo.local | SQL 2019 AAG  |	 treated as bare-metal SQL DB 
ddmc |	sysadmin | ddmc | smart scale “bash hack” used 
dpa |	administrator  |		DPA 	
exchange1 - 2  |		administrator@demo.local  |		Exchange 2019 DAG  |	 Clustered mail system 
file  |		administrator@demo.local  |		Win 2022 file server  |		Central file server 
hana01  |		H01adm  |		hana hxe 2.0 SPS02  |	 	SAP HANA  
isilon  |		root  |		Isilon oneFS 9.1  |	 	treated as NAS workload / joined demo.local AD 
K8scl1-pool1…  |		bobuser  |		Kubernetes cluster 	k8s  |	 workload with wordpress/mysql 
rancher  |		ubuntu  |		Rancher  |		K8s manager for cluster/pods  
nve-1  |		administrator  |		Networker  |	legay dataprotection 	
nve-1-vproxy  |		admin  |		vproxy 	
ora1  |		root or oracle | Oracle 19c  |	DG active |		Data guard node 
ora2  |		root or oracle | Oracle 19c  |	DG standby |		Data guard node 
ppdm-1  | admin  | PP Data Mmanager  	
ppdm-1-search | PPDM search node 	
ppdm-1-report | PPDM report node 	
sql03 - 04 | administrator@demo.local | SQL 2022 AAG | treated as vm for app ware backup 
vault-esxi | root | esxi | for simulating a CR vault area 
vault-vcenter |	administrator@vsphere.local | vSphere for the vault|The separated vcenter in the vault 
vault-ppdm | root/changeme | ppdm for the vault | ppdm is waiting in “install screen” 
vault-ddve | sysadmin | PPDD for the vault | the separated protection target 
cr | rso | PPCR for the vault | vault CyberRecovery Manager 
cs | admin/admin |	CyberSense  | Optional deep forensic 






## Header 1

### Header 2

##### Header 3
```bash
rm -rf /
```
```Powershell
get-help -module dau
```
